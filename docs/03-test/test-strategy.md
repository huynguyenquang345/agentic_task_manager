# Chiến lược kiểm thử

## Trạng thái

- Draft v0.1, lập ở P6 làm kế hoạch cho thiết kế test và kiểm thử các phase sau; chưa được duyệt tại G2.
- Repo hiện ở P1–P3, chưa có code sản phẩm hoặc môi trường kiểm thử. Chưa có test case triển khai; test case sẽ được lập từ AC ở P6.
- POC dùng dữ liệu tổng hợp và code throwaway; chỉ bắt đầu sau G1. Không dùng tài liệu này để tuyên bố hệ thống đã được kiểm thử.

Nguồn: `docs/00-process/sdlc-overview.md`, `gates.md`, `safety-rules.md`, `docs/01-requirements/srs.md`, `acceptance-criteria.md`, `traceability.md`, `docs/02-design/architecture.md`, `agent-autonomy.md`, `threat-model.md` và `module-map.md`. Theo quy trình gốc *Quy trình phát triển phần mềm có AI tham gia*, kế hoạch này áp dụng các bước P6 (S23–S27), P9 (T1–T7), G2 và G5. Nội dung cụ thể của dự án là đề xuất Draft.

## Mục tiêu

- Kiểm tra mỗi yêu cầu qua AC và ít nhất một test case truy vết được.
- Phát hiện sớm lỗi nghiệp vụ, quyền truy cập, ràng buộc agent, tích hợp model và chất lượng NFR.
- Đảm bảo agent không thể tự ghi dữ liệu chính thức, vượt allowlist hoặc truy cập chéo dự án.
- Cung cấp bằng chứng khách quan cho G3–G5 và dữ liệu đánh giá ở `eval-plan.md`.

## Phạm vi kiểm thử

### Trong phạm vi dự kiến

- Task: tạo task với tiêu đề hợp lệ/không hợp lệ, trạng thái và timestamp, deadline và nhãn quá hạn.
- Agent: sinh task nháp, hỏi lại khi mơ hồ, schema/đầu ra, giới hạn 3 vòng, lỗi/timeout, xác nhận trước ghi, từ chối/hủy.
- An toàn: allowlist, prompt injection, tách biệt dự án, dữ liệu log, kiểm soát dữ liệu gửi model.
- NFR: thời gian phản hồi, chi phí mỗi lượt, tỷ lệ chấp nhận/sửa nhẹ theo ngưỡng đã được PO khóa.
- Quy trình: traceability, regression, kiểm tra build/deploy và smoke test khi có môi trường.

### Ngoài phạm vi hiện tại

- Xác nhận bảo mật production, penetration test toàn diện hoặc tuân thủ pháp lý (chưa có kiến trúc, dữ liệu hoặc scope MVP).
- UAT người dùng thật trong POC. POC không qua G6 theo `gates.md`; mọi MVP/release sau này vẫn cần UAT và G6.
- Xác thực/đăng nhập nếu chưa được thêm vào scope. Đây là câu hỏi mở trong SRS; khi được đưa vào, bổ sung yêu cầu và test trước khi triển khai.

## Nguyên tắc

- Mỗi AC có ít nhất một test case; mỗi test case truy về một AC. Mỗi FR/NFR và thiết kế phải truy vết hai chiều theo `traceability.md`.
- Thiết kế test từ AC trước khi có code; bao phủ positive, negative, boundary và error paths.
- Ràng buộc cứng được kiểm bằng script hoặc checklist đã viết trước; một vi phạm an toàn không được bù bởi điểm chất lượng.
- Chỉ dùng dữ liệu synthetic/anonymized theo stage và safety rules; POC chỉ dùng synthetic.
- Pass/fail thuộc về assertion, dữ liệu quan sát được hoặc tiêu chí đã khóa, không dựa vào model tự đánh giá.
- AI có thể hỗ trợ sinh test nháp (L2–L3) nhưng người đọc/reviewer xác nhận. Kết quả AI là Draft; không tự chuyển Verified/Approved.
- Sau G1, đổi yêu cầu phải qua CR; test bị ảnh hưởng được đánh dấu `stale`, cập nhật và chạy lại.

## Tầng kiểm thử và người chịu trách nhiệm

| Tầng | Phase | Mục tiêu | Người thực hiện / quyết định |
| --- | --- | --- | --- |
| Kiểm tra yêu cầu và coverage | P3/P6 | AC đo được, không mơ hồ; mọi FR/NFR có AC/test | BA/QA; PO chốt ngưỡng |
| Unit | P7 | Quy tắc task, ngày, validator và giới hạn độc lập | Developer; CI báo pass/fail |
| Integration | P9 T1 | Giao tiếp giữa nghiệp vụ, quyền, kho dữ liệu, adapter model và audit | QA/Developer |
| System/E2E | P9 T2 | Luồng người dùng từ nhập mục tiêu đến xem/xác nhận task | QA trên môi trường test |
| Regression | P9 T3 | AC hiện có không bị hỏng sau sửa | QA/CI |
| Hiệu năng/tải | P9 T4 | NFR-010, chi phí và giới hạn tài nguyên | QA; ngưỡng do PO chốt |
| Bảo mật | P9 T5 | Quyền dự án, prompt injection, dữ liệu, dependency/secret scan; pen-test do người có chuyên môn | Security/QA; người duyệt xem rủi ro |
| Exploratory | P9 T6 | Tìm lỗi luồng, thông báo, trạng thái và kết hợp dữ liệu chưa có script | QA/PO ghi lại bước tái hiện |
| UAT | P10 | Người dùng/key user nghiệm thu theo AC mức must | Khách hàng/key user; không áp dụng POC |

## Ma trận kiểm thử yêu cầu trọng yếu

| Yêu cầu/nhóm | Kiểm tra tối thiểu | Loại kiểm tra |
| --- | --- | --- |
| FR-010, FR-011 | Tạo task hợp lệ; tiêu đề rỗng/quá giới hạn; đổi trạng thái và timestamp | Unit, integration, E2E |
| FR-012, FR-013 | Deadline quá khứ/hôm nay/tương lai; task hoàn tất; múi giờ đã chốt | Unit với đồng hồ cố định, boundary, E2E |
| FR-020 | Đầu ra có cấu trúc, task liên quan mục tiêu, không trở thành task chính thức trước xác nhận | Integration, E2E, eval |
| FR-021 | Mục tiêu rỗng, thiếu điều kiện, mơ hồ, quá dài, không hợp lệ | Unit, integration, E2E |
| FR-022, FR-024 | Tool allowlist và payload có prompt injection; thử yêu cầu tool ngoài quyền | Security/negative test, kiểm event log |
| FR-023 | Không xác nhận, từ chối, chấp nhận một phần, sửa bản nháp rồi xác nhận; quyền bị thu hồi trước ghi | Integration, E2E, security |
| FR-025 | Ca chạy đủ 3 vòng, vẫn chưa đạt; đảm bảo vòng thứ 4 không thực hiện | Unit/integration, boundary |
| FR-030 | Log đủ model/version, prompt, input, action, timestamp kể cả lỗi/từ chối; không chứa secret | Integration, privacy/security |
| FR-040 | Hai dự án với dữ liệu mồi riêng; người không có quyền thử đọc/gợi ý/ghi chéo | Security/integration, negative |
| NFR-010 | Đo thời gian trên bộ ca cố định, ghi percentile và max theo ngưỡng đã chốt | Performance |
| NFR-011 | Đối chiếu usage/giá model theo từng lượt | Kiểm dữ liệu log và tính toán |
| NFR-012 | Tính tỷ lệ task chấp nhận/sửa nhẹ theo định nghĩa khóa trước | Eval trên scorecard |

Mã AC đang có thay đổi chờ đồng bộ trong SRS/acceptance criteria, đặc biệt mã deadline và agent. P6 phải cập nhật ma trận theo baseline G1 thay vì coi mã hiện tại là ổn định.

## Kiểm thử agent và model

`eval-plan.md` là nguồn chuẩn cho bộ ca benchmark, rubric chấm mù, cách so sánh model, số lần chạy và kết luận. Chiến lược này quy định đánh giá chất lượng dùng chung test nhưng không tạo một scorecard thứ hai.

- Kiểm tra ràng buộc cứng tách riêng khỏi đánh giá chất lượng; một vi phạm là FAIL.
- Ca prompt injection, allowlist, xác nhận, quyền dự án và giới hạn vòng được chạy 3 lần/model theo `experiment-protocol.md`.
- Các ca khác chạy một lần; không chạy lại kết quả nội dung kém.
- Kết quả lỗi hạ tầng có thể chạy lại khi có bằng chứng và được ghi vào ai-log.
- Chấm mù (A/B/C), cùng rubric và cùng input cho model/đối chứng; người chấm chịu kết luận cuối.
- Ghi model/version, harness, prompt version, seed/settings nếu có, usage, thời điểm, kết quả và trạng thái.

## Dữ liệu và môi trường

| Giai đoạn | Dữ liệu/môi trường |
| --- | --- |
| POC | Dữ liệu synthetic; môi trường local hoặc test cô lập TBD; không có dữ liệu thật |
| Demo | Dữ liệu synthetic hoặc anonymized theo safety rules; không mặc nhiên là UAT |
| MVP | Dữ liệu và môi trường theo scope đã duyệt; cần bảo vệ dữ liệu thật, logging, access control và UAT |
| Production | Chỉ sau thiết kế vận hành, monitoring, rollback, security sign-off và G7 |

Test data phải có nguồn gốc tổng hợp, có mã ca ổn định, không chứa bí mật trong fixtures/logs. Quản lý secrets ngoài repo; không dùng production dump để tạo bộ test chưa che.

## Entry và exit criteria

### Entry cho thiết kế test P6

- Có baseline yêu cầu/AC sau G1 hoặc ghi rõ đang chờ G1 nếu chỉ chuẩn bị nháp.
- Có kiến trúc và threat model Draft để QA nhận diện ranh giới/điểm lỗi.
- NFR, dữ liệu, môi trường và giả định được nêu; TBD được đưa vào danh sách cần quyết định.

### Exit P6 / bằng chứng G2

- Test strategy, test plan, test data tổng hợp và test case Draft hoàn tất.
- Mọi AC có ít nhất một test case; mỗi test case truy về một AC; coverage matrix không còn khoảng trống chưa giải trình.
- Có positive, negative, boundary và error-path cho yêu cầu trọng yếu; có kế hoạch NFR và security.
- Ngưỡng kiểm thử và vai trò review được chốt; test case được reviewer đọc trước khi G2.
- Không đánh dấu Approved thay người ký G2.

### Exit G5 (khi sản phẩm có thể kiểm thử)

Theo `docs/00-process/gates.md`, tối thiểu:

- 100% AC mức must có test case pass.
- Không còn defect Critical/High mở, trừ ngoại lệ rủi ro có sign-off.
- Coverage không dưới ngưỡng do dự án chốt; hiện TBD.
- NFR đã chốt đạt ngưỡng.
- Không còn lỗ hổng High chưa xử lý nếu không có chấp nhận rủi ro có thẩm quyền.
- Báo cáo integration, E2E, regression, hiệu năng, bảo mật, defect và traceability được QA Lead xem.

Không có code nên các tiêu chí G5 hiện là điều kiện tương lai, chưa đạt hay chưa chạy.

## Quản lý defect và thay đổi

- Mỗi defect ghi mã, môi trường/build, AC/FR, mức độ, bước tái hiện, kết quả mong đợi/thực tế, log đã làm sạch và người xử lý.
- Severity và priority do người phụ trách quyết; AI chỉ gợi ý phân loại.
- Defect sửa ở P7 quay lại kiểm thử bị ảnh hưởng và regression liên quan.
- Thay đổi yêu cầu sau G1 phải có CR/impact analysis; test cũ đánh dấu stale đến khi cập nhật và chạy lại.
- Ngoại lệ gate/rủi ro phải ghi rõ người chấp nhận, phạm vi, lý do, thời hạn và bằng chứng; không bỏ gate.

## Báo cáo và truy vết

- Test case: mã `TC-<số>` theo quy ước trong `traceability.md`; mỗi AC có ít nhất một.
- Kết quả test: liên kết run CI hoặc báo cáo trong `docs/03-test/`; thí nghiệm model ghi mã `EXP-...` và scorecard trong `experiments/scorecards/`.
- Báo cáo run gồm commit/build, môi trường, dữ liệu, test version, pass/fail/blocked, defect mới, coverage, thời gian/chi phí và hạn chế.
- POC không có code: ghi “chưa chạy/chưa áp dụng”, không tạo kết quả giả.

## Việc cần chốt trước G2

- Baseline SRS/AC và đồng bộ mã FR/AC; các ngưỡng NFR và định nghĩa sửa nhẹ.
- Môi trường, stack test, schema log, tool allowlist và cách quan sát thao tác ghi.
- Coverage định nghĩa/ngưỡng cho code về sau; tiêu chí severity Critical/High.
- Bộ ca POC, checklist/script pass/fail, model/harness và retention dữ liệu.
- Người review độc lập và nơi lưu test case/result trong repo.

## Phiên bản

- v0.1, 12/11/2026: Bản chiến lược kiểm thử cho P6/P9; code, test case triển khai và ngưỡng còn chưa có.
