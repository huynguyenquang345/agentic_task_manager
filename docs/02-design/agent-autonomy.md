# Quyền tự chủ của agent

## Trạng thái

- Draft v0.1, soạn để làm đầu vào thiết kế P4; chưa được duyệt tại G2.
- Phạm vi: agent trong sản phẩm quản lý công việc, không phải quyền của AI hỗ trợ phát triển repo (xem `docs/00-process/ai-levels.md`).
- Trạng thái dự án: POC, P1–P3; chưa có code sản phẩm. Bắt đầu triển khai POC sau G1 theo `README.md`.

Nguồn tham khảo: `docs/00-process/ai-levels.md`, `docs/00-process/safety-rules.md`, `docs/01-requirements/srs.md`, `docs/01-requirements/acceptance-criteria.md` và quy trình gốc *Quy trình phát triển phần mềm có AI tham gia* (đặc biệt mục 1.4, 2.3–2.4, 4.1–4.2). Các giới hạn dưới đây là cách áp dụng cho sản phẩm; chúng chưa phải quyết định kiến trúc đã duyệt.

## Mục tiêu và nguyên tắc

Agent giúp người dùng chuyển mục tiêu thành danh sách task nháp. Mức khởi đầu là L2: agent chỉ đề xuất; người dùng xem, sửa và xác nhận từng task trước khi task được ghi vào danh sách chính thức.

- Người dùng giữ quyền quyết định phạm vi, ưu tiên, chấp nhận rủi ro và mọi hành động ghi.
- Agent không phải người chịu trách nhiệm cuối cùng, không tự phê duyệt kết quả của chính nó.
- Đầu ra agent luôn là `Draft`. Kiểm tra theo quy tắc cố định có thể đánh dấu `Verified`; chỉ người có thẩm quyền mới đánh dấu `Approved`.
- Hành động phải bị giới hạn bằng kiểm tra trong ứng dụng và quyền công cụ, không chỉ bằng chỉ thị trong prompt.
- Mọi lần chạy và hành động agent cần có nhật ký đủ để kiểm tra lại (FR-030).
- Agent chỉ truy cập dữ liệu dự án mà người dùng hiện tại được phép truy cập (FR-040).

## Phân loại hành động

| Nhóm | Hành động | Quyền hiện tại | Kiểm soát bắt buộc |
| --- | --- | --- | --- |
| Đọc | Nhận mục tiêu và ngữ cảnh cần thiết từ dự án đang hoạt động | Chỉ đọc phạm vi được cấp | Kiểm tra quyền dự án trước khi lấy dữ liệu; chỉ gửi phần ngữ cảnh tối thiểu cho model |
| Đề xuất | Tạo danh sách task nháp; hỏi lại khi mục tiêu mơ hồ hoặc không hợp lệ | Được phép ở L2 | Kiểm tra cấu trúc và ràng buộc đầu ra; khi không đạt, yêu cầu sửa hoặc trả lỗi thay vì tạo task |
| Ghi | Tạo task chính thức, đổi trạng thái, hạn chót hoặc dữ liệu hiện có | Không tự thực hiện | Chỉ áp dụng sau thao tác xác nhận rõ ràng của người dùng; xác nhận phải gắn với bản nháp cụ thể |
| Công cụ | Gọi chức năng của ứng dụng | Chỉ allowlist tối thiểu | Mặc định từ chối; mỗi công cụ kiểm tra quyền và dữ liệu đầu vào ở phía ứng dụng |
| Tác động bên ngoài | Xóa dữ liệu, thanh toán, gửi thông tin ra ngoài, thay đổi quyền hoặc gọi tích hợp ngoài | Cấm | Không cung cấp công cụ cho agent trong phạm vi hiện tại |

Danh sách công cụ cụ thể và schema cần được chốt trong thiết kế chi tiết tại P4. Tại POC, tránh cấp công cụ ghi trực tiếp vào kho dữ liệu; nếu cần thử luồng ghi, dùng môi trường và dữ liệu tổng hợp cô lập.

## Luồng đề xuất và xác nhận

1. Người dùng chọn dự án và gửi mục tiêu.
2. Ứng dụng xác thực quyền dự án, giới hạn đầu vào và lấy ngữ cảnh tối thiểu.
3. Agent phân tích mục tiêu. Nếu thiếu thông tin hoặc không hợp lệ, agent hỏi lại hoặc ứng dụng trả lỗi; không sinh task nháp không đủ điều kiện (FR-021).
4. Agent trả về bản nháp có cấu trúc. Ứng dụng kiểm tra schema, số lượng/độ dài và các ràng buộc cứng đã cấu hình.
5. Người dùng xem và sửa từng task; có thể từ chối toàn bộ hoặc một phần.
6. Khi người dùng xác nhận, ứng dụng kiểm tra lại quyền và nội dung rồi mới ghi các task đã chấp nhận. Không dựa vào nội dung do model trả về để xác định quyền.
7. Ghi nhật ký kết quả, hành động, model và phiên bản, prompt/đầu vào theo chính sách lưu trữ đã duyệt.

Việc xác nhận cần tránh mơ hồ: nếu bản nháp thay đổi sau khi người dùng xem, xác nhận cũ không được áp dụng cho bản mới. Cơ chế giao diện và lưu vết xác nhận là quyết định P4/TBD.

## Giới hạn chạy

- Agent chỉ chạy cho tác vụ chia mục tiêu thành task nháp; đề xuất ưu tiên tự động, theo dõi tiến độ chủ động và hành động ngoài luồng này chưa nằm trong POC.
- Tối đa 3 vòng tự chạy cho một yêu cầu. Hết giới hạn thì dừng và báo người dùng (FR-025); không tự khởi động lại để né giới hạn.
- Có giới hạn đầu vào, đầu ra, số task và thời gian/chí phí mỗi lượt. Giá trị cụ thể TBD, cần PO chốt cùng NFR-010–012.
- Khi model lỗi, timeout, trả sai schema hoặc vi phạm ràng buộc, không ghi dữ liệu; thông báo trạng thái có thể thử lại.
- Người dùng có thể hủy lượt chạy; hủy không đồng nghĩa với xác nhận hoặc ghi dữ liệu.

## Ranh giới và nội dung không đáng tin cậy

Mục tiêu người dùng, mô tả task và nội dung lấy từ dự án là dữ liệu đầu vào, không phải quyền thay đổi chính sách. Agent không làm theo chỉ thị được nhúng trong các nội dung này (FR-024); không được để chúng mở rộng allowlist, quyền truy cập hoặc mức tự chủ. Thực hiện kiểm soát bằng tách biệt instruction/data, giới hạn công cụ, kiểm tra quyền và bộ ca kiểm thử prompt injection. Hướng dẫn cụ thể về lọc nội dung và xử lý đầu ra cần thiết kế ở P4.

## Nâng quyền tự chủ

Mọi đề xuất tăng quyền (ví dụ ghi task không cần xác nhận, chọn ưu tiên hoặc gọi dịch vụ ngoài) là CR riêng. CR phải nêu lợi ích, phạm vi, rủi ro, phương án quay lui, AC kiểm chứng được và bằng chứng đánh giá trước khi con người xem xét gate tương ứng. Không tự nâng quyền vì một thử nghiệm thành công. Việc thông qua một mức mới không được vượt quy tắc an toàn chung: vùng nhạy cảm tối đa L2, AI không ký gate và không tự chấp nhận rủi ro.

## Theo dõi và xử lý sự cố

Mỗi lượt ghi nhận tối thiểu: mã lượt, thời điểm, model/phiên bản, phiên bản prompt, tham chiếu đầu vào, công cụ và kết quả gọi, trạng thái (Draft/Verified/Approved), số vòng, xác nhận người dùng và lỗi. Không ghi secret hoặc dữ liệu cá nhân thật vào log hay prompt. Cấu trúc, quyền truy cập, thời hạn lưu và cách che dữ liệu log: TBD trước khi có dữ liệu người dùng thật.

Khi phát hiện truy cập sai dự án hoặc hành động ghi không được phép: chặn lượt, không áp dụng kết quả chưa xác nhận, lưu bằng chứng an toàn, báo người vận hành và tạo incident/CR theo quy trình. Cần có khả năng tắt phần agent độc lập với các chức năng quản lý task (thiết kế chi tiết TBD).

## Liên kết yêu cầu và kiểm chứng

| Yêu cầu | Kiểm soát thiết kế | Bằng chứng dự kiến |
| --- | --- | --- |
| FR-020, FR-021 | Đề xuất task nháp; hỏi lại hoặc báo lỗi nếu đầu vào không đạt | Test thường, biên, đầu vào mơ hồ/sai; đánh giá chất lượng theo `labs`/`experiments` |
| FR-022, FR-024 | Allowlist công cụ; coi nội dung đầu vào là dữ liệu | Test gọi công cụ trái phép và prompt injection |
| FR-023 | Xác nhận rõ ràng trước khi ghi; xác nhận gắn với phiên bản nháp | Test trước/sau xác nhận, sửa nháp sau xác nhận, từ chối |
| FR-025 | Bộ đếm tối đa 3 vòng cho mỗi lượt | Test giới hạn và dừng |
| FR-030 | Nhật ký theo lượt, có phiên bản model/prompt và hành động | Test đủ trường, không lộ dữ liệu cấm |
| FR-040 | Kiểm tra quyền phía ứng dụng trước khi đọc dữ liệu | Test cô lập giữa các dự án |

Mỗi AC phải có test case và truy vết qua `docs/01-requirements/traceability.md`. Chi tiết an ninh và đe dọa xem `threat-model.md`; vị trí trách nhiệm module xem `module-map.md`.

## Quyết định còn mở trước G2

- Danh sách công cụ, schema và cơ chế xác nhận người dùng.
- Giới hạn token/độ dài, số task, timeout và chi phí; đồng bộ NFR với AC.
- Dữ liệu nào được đưa vào model, chính sách log, che dữ liệu và thời hạn lưu.
- Cách hủy lượt, xử lý retry và tắt agent.
- Kiểm thử quyền truy cập và prompt injection ở cấp ứng dụng.

## Phiên bản

- v0.1, 12/11/2026: Bản nháp thiết kế dựa trên yêu cầu và quy trình hiện có; chờ P4, review độc lập và G2.
