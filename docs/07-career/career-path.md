# Lộ trình phát triển năng lực cá nhân

## Trạng thái

- Draft v0.1, đề xuất gắn việc học với các đầu ra của dự án; chưa phải cam kết nghề nghiệp hay lịch học.
- Người làm dự án hiện đảm nhận nhiều vai trong dự án một người (PO, Architect/Tech Lead, QA, PM/Release, Security); reviewer độc lập tham gia tại các gate được nêu trong `gates.md`.
- Chưa có thông tin về vai trò nghề nghiệp đích, kinh nghiệm hiện tại hoặc thời lượng học; các mục tiêu dưới đây là khung có thể điều chỉnh, không gán trình độ hay ngày hạn.

Nguồn: `docs/00-process/sdlc-overview.md`, `gates.md`, `ai-levels.md`, `safety-rules.md`, roadmap, BRD/SRS, test/eval plan và cấu trúc repo hiện tại. Tài liệu quy trình gốc *Quy trình phát triển phần mềm có AI tham gia* cung cấp các phase, vai trò, gate và mức tự chủ làm khung cho lộ trình.

## Mục đích

Dùng dự án như một chuỗi bài thực hành có bằng chứng: mỗi năng lực gắn với đầu ra thật trong repo, review hoặc kết quả đo. Không dùng số lượng tài liệu/code do AI tạo làm thước đo năng lực; người thực hiện cần giải thích được quyết định, rủi ro và bằng chứng.

## Nguyên tắc học

- Ưu tiên tự làm bản nháp hoặc quyết định trước; dùng AI để phản biện/soạn draft trong giới hạn L0–L4 của `ai-levels.md`.
- Lưu bài học, lỗi và mức sửa trong `docs/ai-log/` hoặc retrospective; không đưa dữ liệu cá nhân thật/secret vào prompt.
- Người tạo không tự duyệt mọi sản phẩm: dùng checklist định trước và reviewer độc lập ở G1/G2 khi khả thi.
- Mỗi năng lực chỉ đánh dấu hoàn thành khi có đầu ra hoặc bằng chứng kiểm chứng được và người học tự trình bày được.
- Không đặt lịch hoặc chứng chỉ mục tiêu cho đến khi người sở hữu lộ trình xác nhận nhu cầu.

## Năng lực theo phase

| Phase / chủ đề | Năng lực cần luyện | Bài thực hành trong dự án | Bằng chứng hoàn thành |
| --- | --- | --- | --- |
| P1–P3: phân tích yêu cầu | Làm rõ nhu cầu, viết scope/FR/NFR, AC Given–When–Then và câu hỏi mở | Hoàn thiện CR/BRD/SRS/AC/traceability; phân biệt giả định với quyết định | G1 review; mọi FR truy về CR, có AC; giải thích được scope và TBD |
| P4: kiến trúc và threat modeling | So sánh phương án, ghi trade-off, nhận diện tài sản/ranh giới/đe dọa | Hoàn thiện architecture, module map, threat model và ADR | G2 review; mỗi FR có thiết kế; rủi ro/kiểm soát truy vết được |
| P5: lập kế hoạch | Phân rã công việc, ước lượng, phụ thuộc và phân loại `ai-eligible`/`human-only` | Tạo backlog gắn FR và điều kiện hoàn tất | Mọi task truy về FR; PO/Tech Lead giải thích được thứ tự và rủi ro |
| P6/P9: kiểm thử và đánh giá AI | Thiết kế test tích cực/tiêu cực/biên, NFR, benchmark và rubric chấm mù | Test strategy, eval-plan, dữ liệu tổng hợp, scorecard | Coverage AC; kết quả tái lập trong giới hạn; không vi phạm hard constraint |
| P7–P8: phát triển và review | Viết code/test, đọc diff, review độc lập, CI và dependency/security hygiene | POC throwaway sau G1; sau đó chỉ code sản phẩm khi gates cho phép | PR có traceability, test/CI, review findings được xử lý; tự giải thích code |
| P10: UAT | Điều phối nghiệm thu mà không dẫn dắt kết quả thay người dùng | Kịch bản UAT từ AC mức must và quản lý phản hồi | G6 do khách hàng/key user ký; AI không nghiệm thu thay |
| P11–P12: release và vận hành | Chuẩn bị rollback, monitoring, xử lý incident, học từ sau release | Checklist release, runbook, retrospective | G7/G8 và bằng chứng deploy/rollback/metric khi đến giai đoạn đó |
| Quản trị AI xuyên suốt | Chọn mức tự chủ theo rủi ro, bảo vệ dữ liệu, audit và giới hạn quyền | Ai-log, allowlist, review output, ghi số vòng và sửa đổi | Không có dữ liệu cấm; trạng thái Draft/Verified/Approved dùng đúng; quyết định do người chịu trách nhiệm |

## Mốc học gắn với roadmap

1. **Hiện tại — G1:** luyện đặc tả và truy vết; ưu tiên làm rõ các placeholder/NFR thay vì vội triển khai.
2. **Sau G1 — G2:** luyện kiến trúc và threat modeling; so sánh lựa chọn rồi ghi quyết định và rủi ro.
3. **POC/M1:** luyện thiết kế thí nghiệm có đối chứng, bộ ca khóa trước, chấm mù và kết luận đúng giới hạn mẫu.
4. **Nếu được duyệt MVP:** luyện phát triển theo PR/CI, regression, xử lý defect và test quyền/security.
5. **Khi có release:** luyện UAT, rollback, monitoring, incident và retrospective qua G6–G8.

Không nhảy sang mốc sau khi gate trước chưa Approved. POC/labs là thực hành throwaway; không đưa code thẳng vào MVP.

## Theo dõi tiến bộ

| Mục theo dõi | Cách ghi |
| --- | --- |
| Năng lực/phase | Chọn hàng trong bảng năng lực |
| Bằng chứng | Liên kết file, PR, scorecard, review hoặc gate issue |
| Tự đánh giá | Chưa thử / Đang luyện / Có thể tự làm / Có thể giải thích và review |
| Phản hồi | Ghi góp ý cụ thể từ reviewer hoặc người dùng |
| Bài học tiếp theo | Một hành động cụ thể cho phase sau |

Tự đánh giá chỉ phục vụ học tập, không thay sign-off chất lượng hoặc quyết định gate. Có thể ghi retrospective tại `docs/05-retro/` sau mỗi mốc.

## Câu hỏi cá nhân cần bổ sung

- Vai trò nghề nghiệp hoặc hướng chuyên môn muốn theo đuổi là gì?
- Kinh nghiệm hiện tại ở BA/QA/kiến trúc/phát triển/bảo mật/vận hành ra sao?
- Mỗi tuần có thể dành bao nhiêu thời gian và có mốc thời gian nào cần đáp ứng?
- Có mục tiêu portfolio, chứng chỉ hoặc kỹ năng cụ thể nào không?

## Phiên bản

- v0.1, 12/11/2026: Khung học tập theo phase/gate của dự án; chưa cá nhân hóa theo mục tiêu nghề nghiệp hoặc lịch.
