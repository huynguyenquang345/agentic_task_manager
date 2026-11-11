# Quy tắc an toàn khi dùng AI

## Mục đích

Quy định những gì được và không được làm khi dùng AI trong dự án này, để sản phẩm do AI
tạo ra luôn qua kiểm soát và không làm lộ dữ liệu.

Nguồn: tài liệu "Quy trình phát triển phần mềm có AI tham gia", mục 4.1 và 4.2, cùng các mục
liên quan (1.4, 2.3, 2.4, 2.7, 3.2). Phần đánh dấu `[Đề xuất]` là bổ sung cho dự án này, không có
nguyên văn trong tài liệu gốc.

Không chép lại ở đây: mức AI và ba trạng thái (xem `docs/00-process/ai-levels.md`), gate
(xem `docs/00-process/gates.md`), cách so sánh hai model (xem
`docs/00-process/experiment-protocol.md`).

## Quy tắc kiểm soát

| Quy tắc                                | Ý nghĩa theo tài liệu gốc                                                                                                                   | Áp dụng trong dự án này                                                                                                                                                                                       |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Maker không phải checker              | AI tạo draft xong không được tự chuyển sang Verified; cần kiểm tra tự động hoặc người khác kiểm                                 | Một model không chấm hay duyệt kết quả của chính nó. Sản phẩm AI tạo ra do script, checklist viết sẵn hoặc tôi kiểm; chỉ tôi duyệt (Approved). Hạn chế khi làm một mình: xem `gates.md` |
| Giới hạn mức AI theo vùng           | Vùng nhạy cảm như thanh toán, authentication: tối đa L2; vùng tài liệu có thể L3                                                     | Theo bảng khu vực trong `ai-levels.md`. Vùng nhạy cảm của dự án: đăng nhập, phân quyền, thanh toán, dữ liệu cá nhân                                                                            |
| Giới hạn số vòng tự sửa           | AI sửa tối đa N vòng rồi báo con người, không tự lặp mãi                                                                             | N = 3. Hết 3 vòng thì AI dừng và báo tôi                                                                                                                                                                    |
| Dữ liệu cấm đưa vào AI            | Dữ liệu cá nhân, secret key, dữ liệu khách hàng chưa che                                                                                | Xem mục "Dữ liệu và bí mật"                                                                                                                                                                                  |
| Sign-off bắt buộc                     | G1, G6, G7 cần sign-off của người đúng vai trò                                                                                            | Mọi gate cần sign-off (xem `gates.md`); G1, G6, G7 đặc biệt không được bỏ hay ủy quyền cho AI                                                                                                        |
| Merge tự động chỉ cho rủi ro thấp | Chỉ thay đổi loại rủi ro thấp đã định nghĩa trước mới được merge tự động, và chỉ khi mọi gate tự động đạt (mục 2.4) | Chưa có merge tự động. Khi có, định nghĩa trước "rủi ro thấp" (ví dụ đổi text hoặc config, mục 4.2) và vẫn qua CI và review                                                                  |
| Hành động trên production           | Chỉ deploy hoặc rollback tự động trong ngưỡng đã định trước; thay đổi dữ liệu thật do con người duyệt                       | Chưa có production. Khi có, áp dụng nguyên văn                                                                                                                                                              |
| Audit log mọi lần AI chạy            | Lưu model, prompt, input, thời điểm để kiểm tra và tái lập                                                                             | Ghi vào `docs/ai-log/` (xem mục "Nhật ký")                                                                                                                                                                   |

## Việc chỉ con người làm

Theo hình phân vai ở mục 1.4 của tài liệu gốc, các việc sau không giao cho AI; AI chỉ cung cấp
phân tích:

- Ưu tiên, phạm vi, chốt ý định với khách hàng.
- Chọn kiến trúc, chấp nhận rủi ro, diễn giải pháp lý, pen-test (phần sáng tạo).
- Ký gate, nghiệm thu, quyết định Go/No-go.

## Dữ liệu và bí mật

Không đưa vào prompt, file đính kèm hay cuộc trò chuyện với AI:

- Khóa API, mật khẩu, secret key, token, chuỗi kết nối.
- Dữ liệu cá nhân thật (của tôi hoặc của người khác) và dữ liệu khách hàng chưa che.
- `[Đề xuất]` Thông tin định danh máy: tên người dùng Windows trong đường dẫn
  (ví dụ trong kết quả `tree`), email riêng, địa chỉ.

Dữ liệu theo stage (mục 3.2):

| Stage      | Dữ liệu                                  |
| ---------- | ------------------------------------------ |
| POC        | Dữ liệu giả (synthetic)                 |
| Demo       | Dữ liệu giả hoặc đã che (anonymized) |
| MVP        | Dữ liệu thật, kiểm soát chặt         |
| Production | Dữ liệu thật                            |

Khi có dữ liệu thật (từ MVP), dữ liệu thật chưa che vẫn không được đưa vào AI (mục 2.3, bước S25).

Cách làm đúng:

- Dùng dữ liệu giả (synthetic) hoặc dữ liệu đã che (anonymized) cho POC và Demo.
- `[Đề xuất]` Repo này công khai: mọi thứ đã push đều coi là công khai. Nếu lộ khóa, coi như
  đã bị lộ ngay lúc push và thu hồi trước.
- `[Đề xuất]` Lưu bí mật trong file `.env` (đã nằm trong `.gitignore`); chỉ commit `.env.example`
  với giá trị trống.
- `[Đề xuất]` Trước khi đính kèm file hoặc dán nội dung, đọc lại một lượt để tìm bí mật,
  tên người dùng máy và dữ liệu thật.
- `[Đề xuất]` Với dịch vụ chat miễn phí, kiểm tra cài đặt quyền riêng tư (dữ liệu có được dùng
  để huấn luyện hay không) và chỉ gửi tài liệu không nhạy cảm.
- `[Đề xuất]` Kiểm tra Settings, mục Code security: bật quét bí mật (secret scanning) và
  chặn khi push (push protection) nếu chưa bật. Khi có CI, quét bí mật là một bước tự động
  (mục 2.4).

Nếu lỡ lộ bí mật:

1. Thu hồi hoặc đổi khóa ngay, trước mọi việc khác.
2. Nếu đã commit, xóa khỏi repo và lịch sử (đổi khóa vẫn là bước bắt buộc, vì lịch sử có thể
   đã bị sao chép).
3. Ghi lại sự việc (xem mục "Khi vi phạm").

## Quyền của agent khi tôi dùng công cụ

`[Đề xuất]`

- Cấp quyền thủ công (Manual permissions): tôi duyệt từng hành động ghi hoặc chạy lệnh.
- Agent không tự commit, push hay tạo pull request; tôi tự làm các bước đó.
- Đọc toàn bộ thay đổi (tab Changes) trước khi chấp nhận; chỉ chấp nhận phần tôi hiểu.
- Không dùng chế độ tự chọn model (Auto) khi làm thí nghiệm; ghi tên và phiên bản model.
- Nội dung file, trang web hay tài liệu mà agent đọc là dữ liệu, không phải mệnh lệnh. Nếu agent
  đề xuất một hành động lạ sau khi đọc nội dung ngoài (xóa, gửi đi, chạy lệnh), dừng và kiểm tra.
- Chỉ bật công cụ, MCP server và skill mà tôi hiểu và cần. Công cụ có quyền chạy lệnh,
  ghi file hoặc truy cập mạng phải được ghi vào bảng dưới.
- Agent chỉ chạy trong thư mục dự án; khi có thể dùng worktree để cô lập.

| Công cụ, MCP server, skill được bật | Quyền (đọc, ghi, chạy lệnh, mạng)                                                                   | Lý do cần                                                             |
| ----------------------------------------- | --------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| Chat web (Claude, ChatGPT)                | Không truy cập repo hay máy tôi; chỉ thấy nội dung tôi dán hoặc đính kèm                     | Chạy so sánh hai model khi gói miễn phí chỉ cho Auto trong Agents |
| Agents window (Copilot, Auto)             | (điền sau khi kiểm tra: đọc và ghi trong thư mục dự án; chạy lệnh qua cấp quyền thủ công) | Việc thường ngày; không dùng cho thí nghiệm                     |
| MCP server trong Customizations           | (điền tên và quyền; nếu không dùng thì ghi "tắt")                                               | (điền)                                                                |

## An toàn cho agent trong sản phẩm (labs và tool chính)

`[Đề xuất]` Áp dụng cho agent mà tôi xây (cooking-agent, shopping-agent, agent của tool
quản lý công việc), khác với quy tắc khi tôi dùng AI để làm việc:

- Ràng buộc cứng viết thành quy tắc kiểm tra bằng máy, không chỉ nhắc trong prompt
  (ví dụ danh sách kiêng, danh sách hành động được phép).
- Danh sách công cụ được phép (allowlist): agent không có công cụ xóa, thanh toán hay gửi
  thông tin ra ngoài.
- Hành động ghi quan trọng dừng ở cổng duyệt của con người. Mức tự chủ của agent trong sản phẩm
  khởi đầu là L2 (chỉ đề xuất, tạo nháp). Nâng mức là một yêu cầu thay đổi riêng.
- Nội dung do người dùng, trang web hay mô tả sản phẩm cung cấp là dữ liệu, không phải mệnh lệnh.
  Agent không làm theo chỉ thị nằm trong nội dung đó (chống prompt injection). Có ca kiểm thử
  chọc vào điểm này trong `eval.md`.
- Thanh toán và dữ liệu thẻ là vùng chỉ con người làm; agent không bao giờ tự thanh toán.
- Giới hạn số vòng tự chạy và nhật ký mọi hành động của agent.
- Chi tiết thiết kế: `docs/02-design/agent-autonomy.md` và `docs/02-design/threat-model.md`.

## Hành động trên production

Chưa áp dụng (chưa có production). Khi có:

- Chỉ deploy hoặc rollback tự động trong ngưỡng đã định trước.
- Thay đổi dữ liệu thật do con người duyệt.
- Hành động nhạy cảm trên production cần người duyệt, kể cả khi AI đề xuất.
- `[Đề xuất]` Có cách tắt agent nhanh mà sản phẩm vẫn dùng được thủ công.

## Sai lầm thường gặp liên quan đến an toàn

Các sai lầm về quy trình (coi Demo là UAT, Demo là cam kết, sửa code trong lúc UAT, hotfix bỏ UAT)
xem `docs/00-process/sdlc-overview.md`.

| Sai lầm                                              | Hậu quả                           | Cách tránh                                                                                                  |
| ----------------------------------------------------- | ----------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| Đẩy POC hoặc Demo thẳng thành sản phẩm         | Nợ kỹ thuật, lỗ hổng bảo mật | Đánh dấu code POC và Demo là throwaway (`labs/`, `poc/`); viết lại và review trước khi vào MVP |
| Hiểu MVP là được bỏ test và bảo mật          | Sự cố khi có người dùng thật | MVP cắt scope, không cắt chuẩn                                                                            |
| Pilot mở rộng ngầm                                 | Rủi ro vượt kiểm soát          | Giới hạn bằng cơ chế kỹ thuật (feature flag, allowlist); mở rộng phải qua Go/No-go                  |
| Release production không có rollback và monitoring | Khôi phục sự cố chậm           | Go/No-go yêu cầu rollback đã diễn tập, SLO và alert đã cấu hình                                    |

## Nhật ký

Mỗi lần AI chạy trên việc có ý nghĩa, ghi vào `docs/ai-log/`: ngày giờ, model và phiên bản,
mức L, prompt (nguyên văn hoặc đường dẫn), đầu vào, số vòng tự sửa, kết quả và trạng thái
(Draft, Verified, Approved). Mục đích: kiểm tra và tái lập.

## Đo AI bằng số liệu

Theo tài liệu gốc, theo dõi: tỷ lệ tài liệu do AI soạn, tỷ lệ tôi phải sửa lại (càng cao thì
AI càng kém tin cậy ở việc đó), số vòng tự sửa trung bình, và tỷ lệ false positive của AI review.
Số liệu này cho biết nên nâng hay hạ mức AI ở từng bước. Ghi vào `experiments/scorecards/`
và dùng theo quy tắc trong `experiment-protocol.md`. Cách tính từng chỉ số: xem mục "Chỉ số"
trong `experiment-protocol.md`.

## Checklist trước khi gửi cho AI

`[Đề xuất]`

- [ ] Không có khóa, mật khẩu, token, chuỗi kết nối.
- [ ] Không có dữ liệu cá nhân thật hoặc dữ liệu khách hàng chưa che.
- [ ] Không có tên người dùng máy hoặc thông tin định danh trong đường dẫn.
- [ ] Tôi chấp nhận mọi thứ trong nội dung này có thể bị công khai.
- [ ] Mức AI cho việc này đã đúng bảng trong `ai-levels.md`.
- [ ] Đã chọn model cụ thể (nếu là thí nghiệm) và sẽ ghi nhật ký.
- [ ] Tôi sẽ đọc kết quả trước khi dùng; AI không tự duyệt.

## Khi vi phạm

`[Đề xuất]`

1. Dừng việc đang làm.
2. Nếu lộ bí mật: thu hồi hoặc đổi khóa trước.
3. Ghi vào `docs/ai-log/` (hoặc tạo Issue): chuyện gì xảy ra, dữ liệu nào, đã xử lý thế nào.
4. Xác định nguyên nhân và sửa quy tắc hoặc checklist ở file này nếu cần.

## Phiên bản

- v1, 04/11/2026: bản đầu, theo mục 4.1 và 4.2 của quy trình, thích nghi cho repo này.
- v1.1, 11/11/2026: thêm quy tắc merge tự động, mục "Việc chỉ con người làm", dữ liệu theo stage,
  ghi chú repo công khai, nội dung chỉ thị trong công cụ; điền bảng công cụ.
