# Checklist phát hành

## Trạng thái

- Draft v0.1, mẫu cho P11/G7; chưa có release package hoặc production hiện tại.
- Repo đang ở P1–P3, chưa có code sản phẩm. Checklist này chưa được chạy; không có mục nào được xem là hoàn tất chỉ vì mẫu đã được điền.
- Dùng cho từng release của MVP/Production sau khi scope, môi trường và người ký được xác định.

Nguồn: `docs/00-process/sdlc-overview.md`, `docs/00-process/gates.md`, `docs/00-process/safety-rules.md`, `docs/03-test/test-strategy.md`, `docs/03-test/eval-plan.md` và `CONTRIBUTING.md`. Tham khảo quy trình gốc *Quy trình phát triển phần mềm có AI tham gia*, các mục P11–P12, G6–G8, release, rollback và monitoring. Checklist áp dụng cụ thể là đề xuất cho repo này.

## Thông tin release

| Trường | Giá trị |
| --- | --- |
| Release/version | TBD |
| CR và phạm vi | TBD |
| Commit/build artifact | TBD |
| Môi trường đích | TBD |
| Người phụ trách release | TBD |
| Ngày/giờ dự kiến và múi giờ | TBD |
| Change ticket | TBD |
| Quyết định G7 | Chưa xem xét |
| Người ký và thời điểm | Chưa ký |

## Cách dùng

- Tạo một bản checklist cho mỗi release; điền người chịu trách nhiệm và liên kết bằng chứng.
- Đánh dấu `[x]` chỉ khi bằng chứng đã được kiểm tra. Ghi `N/A` kèm lý do cho mục không áp dụng; không xóa mục để che thiếu sót.
- Mục chưa đạt phải có defect, người xử lý, tác động và quyết định của người có thẩm quyền.
- AI có thể chuẩn bị tài liệu hoặc tóm tắt trạng thái; AI không ký G6/G7, không chấp nhận rủi ro và không tự quyết Go/No-go.
- Trạng thái ban đầu mọi mục là chưa kiểm tra. Chỉ người có thẩm quyền mới ghi Approved.

## 1. Phạm vi và baseline

- [ ] CR, phạm vi release và acceptance criteria liên quan được xác định.
- [ ] Baseline SRS/thiết kế được duyệt và các thay đổi sau baseline có CR/traceability.
- [ ] Phiên bản phát hành truy về CR đã đóng/được phép, task, PR và test result trong `docs/01-requirements/traceability.md`.
- [ ] Release note nêu tính năng, sửa lỗi, thay đổi tương thích, hạn chế đã biết và hành động cần người dùng.
- [ ] Không có nội dung ngoài scope hoặc tính năng thử nghiệm/throwaway từ `poc/` hay `labs/` được đưa thẳng vào sản phẩm.

Bằng chứng/liên kết: TBD

## 2. Build và chất lượng (G3–G5)

- [ ] Commit/tag và artifact được cố định; artifact được tạo từ nguồn đã review.
- [ ] Pull request đã được reviewer con người approve; không có góp ý chặn.
- [ ] CI xanh: build, lint, unit test, security/dependency/secret scan theo pipeline đã duyệt.
- [ ] Build cài/triển khai được lên môi trường đích; smoke test pass (G4).
- [ ] Báo cáo integration, system/E2E, regression, NFR và security đã được QA xem (G5).
- [ ] 100% AC mức `must` có test case pass; coverage đạt ngưỡng đã chốt.
- [ ] Không còn defect Critical/High mở, trừ ngoại lệ có sign-off đúng thẩm quyền.
- [ ] NFR và các ràng buộc cứng agent đạt ngưỡng đã khóa; không có vi phạm được che bởi điểm chất lượng.
- [ ] Mọi kết quả test, defect và giới hạn đã biết được lưu tại `docs/03-test/` và truy vết.

Bằng chứng/liên kết: TBD

## 3. UAT và G6

- [ ] UAT hoàn tất bởi khách hàng/key user; kết quả gắn với build/phiên bản đang phát hành.
- [ ] 100% kịch bản UAT mức `must` pass hoặc ngoại lệ riêng có sign-off.
- [ ] Defect/CR phát sinh được phân loại; yêu cầu mới không âm thầm đưa vào release.
- [ ] Người dùng/key user đã xác nhận nghiệm thu; PO xác nhận theo quy tắc G6 trong `gates.md`.
- [ ] Build được đóng băng trong UAT; mọi thay đổi sau đó đã chạy lại phần test bị ảnh hưởng và UAT lại khi cần.
- [ ] Với hotfix: có test tái hiện lỗi, review con người, kế hoạch rollback, UAT tối thiểu do PO/key user xác nhận và lịch UAT bổ sung sau deploy.

Bằng chứng UAT/sign-off: TBD

Quy tắc: UAT bắt buộc cho mọi release MVP/Production. Demo không thay UAT. Checklist này không áp dụng G6 cho POC (`gates.md`).

## 4. Bảo mật, dữ liệu và quyền riêng tư

- [ ] Threat model được cập nhật theo thay đổi release; rủi ro còn lại có người chấp nhận và bằng chứng.
- [ ] Không có secret, dữ liệu cá nhân thật hoặc dữ liệu khách hàng chưa che trong repo, log, prompt, artifact hoặc release note.
- [ ] Quyền truy cập dự án, allowlist công cụ và xác nhận trước hành động ghi đã được kiểm tra trên build này.
- [ ] Prompt injection, đầu ra model sai schema, lỗi provider và truy cập chéo dự án đã có kết quả kiểm tra phù hợp với scope.
- [ ] Secret, cấu hình và quyền dịch vụ được cấp theo quy trình; không đóng gói giá trị bí mật vào artifact.
- [ ] Lỗ hổng High/Critical đã xử lý hoặc có ngoại lệ được người có thẩm quyền ký; pen-test do con người thực hiện khi nằm trong scope.

Bằng chứng/liên kết: TBD

## 5. Triển khai và rollback

- [ ] Kế hoạch deploy nêu thứ tự bước, người thực hiện, thời lượng, điều kiện dừng và kiểm tra sau deploy.
- [ ] Kế hoạch rollback nêu trigger, người ra quyết định, thao tác phục hồi, kiểm tra sau rollback và điểm liên hệ.
- [ ] Rollback đã diễn tập trong môi trường phù hợp; kết quả và thời gian phục hồi được lưu.
- [ ] Database/schema/config migration có kế hoạch tương thích và phục hồi; backup/restore đã xác minh khi áp dụng.
- [ ] Feature flag/canary/blue-green và giới hạn pilot được cấu hình nếu dùng; điều kiện mở rộng/đóng lại rõ ràng.
- [ ] Change ticket, cửa sổ triển khai và thông báo tới bên liên quan đã sẵn sàng.
- [ ] Có phương án tắt agent nhanh và luồng quản lý task thủ công nếu cần (thiết kế chi tiết cần chốt ở P4/G2).

Bằng chứng/liên kết: TBD

## 6. Vận hành và quan sát

- [ ] SLO/health check, dashboard, alert và người trực đã được xác định trước khi production.
- [ ] Log/metric không chứa dữ liệu nhạy cảm ngoài chính sách; quyền truy cập và thời hạn lưu được xác định.
- [ ] Có cách theo dõi lỗi model, timeout, số vòng, usage/chi phí và lỗi xác nhận/ghi.
- [ ] Runbook incident và escalation/contact list đã sẵn sàng.
- [ ] Có cách đối chiếu phiên bản chạy thực tế với artifact/tag được duyệt.
- [ ] Kế hoạch theo dõi sau deploy và mốc đánh giá G8 được lên lịch.

Bằng chứng/liên kết: TBD

## 7. Go/No-go — G7

Người Release Manager tổng hợp bằng chứng và ý kiến PO, QA, Security, DevOps. Người có thẩm quyền quyết định cuối; AI chỉ có thể tạo bảng sẵn sàng.

- [ ] Release package đầy đủ và định danh được.
- [ ] G5 đạt hoặc mọi ngoại lệ có sign-off.
- [ ] G6 đã Approved.
- [ ] Kế hoạch deploy, rollback và change ticket đầy đủ; rollback đã diễn tập.
- [ ] SLO/alert, người phụ trách và hỗ trợ sau triển khai sẵn sàng.
- [ ] Tác động, rủi ro còn lại và phương án dừng được người ký xem.

| Quyết định | Người ký | Ngày/giờ | Lý do và điều kiện |
| --- | --- | --- | --- |
| Chưa quyết định (Go / No-go) | TBD | TBD | TBD |

Chỉ quyết định `Go` mới cho phép triển khai theo kế hoạch. `No-go` yêu cầu ghi việc cần xử lý và lịch xem xét lại.

## 8. Sau triển khai và đóng vòng — G8

- [ ] Smoke test và health check sau deploy pass.
- [ ] Metric/error/alert được theo dõi trong khoảng thời gian đã định; kết quả ghi lại.
- [ ] Không có incident mức chặn; nếu có, incident được xử lý theo runbook và rollback khi cần.
- [ ] Người dùng/đội hỗ trợ được thông báo về trạng thái và vấn đề đã biết.
- [ ] Chỉ số sau release, phản hồi, defect và CR liên quan được cập nhật.
- [ ] Retrospective ghi điều tốt, điều sai và hành động cải thiện có người phụ trách.
- [ ] Các CR của cycle đã đóng hoặc được chuyển thành CR mới với chủ sở hữu.
- [ ] PO ký đóng vòng G8; bug/yêu cầu mới quay về P1.

Bằng chứng G8 dự kiến lưu ở `docs/05-retro/` và liên kết từ issue/release.

## Phiên bản

- v0.1, 12/11/2026: Bản checklist Draft cho P11/G7 và theo dõi G8; chưa chạy cho release nào.
