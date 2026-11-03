# Gates (cổng duyệt)

## Mục đích

Gate là điểm dừng bắt buộc: chưa có sign-off của người có thẩm quyền thì chưa được đi tiếp.
File này ghi 8 gate (G1 đến G8), 4 milestone gate (M1 đến M4), ai ký, điều kiện, bằng chứng
cần có và nơi lưu bằng chứng trong repo.

Nguồn: tài liệu "Quy trình phát triển phần mềm có AI tham gia", mục 1.2, 1.3, 2.1 đến 2.7,
3.1, 3.2, 4.1 và 4.3. Phần đánh dấu `[Đề xuất]` là bổ sung cho dự án này, không có nguyên văn
trong tài liệu gốc.

Ba chỗ tài liệu gốc không thống nhất, và cách xử lý ở đây:

- Hình tổng quan ghi "5 gate chính", bảng mục 3.1 liệt kê 8 gate: dùng 8 gate.
- G6: mục 3.1 ghi Khách hàng ký, mục 2.6 ghi Khách hàng ký và PO xác nhận: dùng cả hai.
- G2: mục 2.2 đặt G2 sau P4 đến P5, hình tổng quan đặt sau P6: dùng P4 đến P6.

## Nguyên tắc

- Gate không phải thủ tục hình thức: người ký chịu trách nhiệm về kết quả.
- AI không bao giờ là người chịu trách nhiệm cuối cùng (Accountable). "AI đã kiểm tra"
  không thay được sign-off.
- Chỉ ký khi đã thấy bằng chứng (báo cáo kiểm tra, ma trận truy vết, kết quả test).
  AI giúp chuẩn bị bằng chứng nhưng không ký thay.
- Pass hay fail của điều kiện tự động do quy tắc cố định quyết định, không do AI "cảm thấy" đạt.
- Mọi yêu cầu, thiết kế, task, code, test và release phải truy vết được với nhau.
- Không có đường đi từ G5 thẳng tới release mà bỏ qua G6. Hotfix chỉ được rút gọn UAT,
  không được bỏ.
- Ngoại lệ chỉ áp dụng cho một điều kiện cụ thể (ví dụ lỗ hổng mức High đã được chấp nhận rủi ro,
  kịch bản UAT có ngoại lệ). Mỗi ngoại lệ phải có sign-off và lý do, và không cho phép bỏ gate.
- Sản phẩm của AI luôn qua Draft, Verified, Approved (xem `docs/00-process/ai-levels.md`).
  Sign-off tương ứng trạng thái Approved.

## Vòng đời một gate

1. Chuẩn bị sản phẩm của gate (Draft).
2. Chạy kiểm tra tự động hoặc checklist đã viết sẵn (Verified).
3. Thu thập bằng chứng.
4. Người có thẩm quyền xem xét.
5. Ký (Approved) hoặc từ chối.
6. Nếu chưa đạt: sửa rồi kiểm tra lại; không đi tiếp khi gate chưa Approved.

`[Đề xuất]` Trạng thái của một gate:

| Trạng thái     | Nghĩa là                               |
| ---------------- | ---------------------------------------- |
| Chưa bắt đầu | Chưa chuẩn bị gì                     |
| Đang chuẩn bị | Đang làm sản phẩm hoặc kiểm tra    |
| Chờ duyệt      | Kiểm tra xong, bằng chứng sẵn sàng  |
| Approved         | Người có thẩm quyền đã ký        |
| Rejected         | Người duyệt từ chối, quay lại sửa |

Chỉ người có thẩm quyền chuyển gate sang Approved. Kiểm tra tự động chỉ đưa gate tới Chờ duyệt.

## Tám gate

| Gate | Nhóm phase                  | Câu hỏi gate trả lời                          | Ai ký                                                               | Điều kiện kiểm tra được                                                      |
| ---- | ---------------------------- | ------------------------------------------------- | -------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| G1   | Yêu cầu (P1 đến P3)      | Yêu cầu đã đúng và đủ chưa?             | PO ký, Khách hàng đồng ý                                       | Mọi FR có AC; mọi FR truy về CR; không còn finding mức blocker               |
| G2   | Thiết kế (P4 đến P6)     | Thiết kế và kế hoạch có khả thi không?    | Architect, Tech Lead (Security tham gia)                             | Mọi FR có thiết kế; threat model xong; mọi task trong backlog truy về một FR |
| G3   | Build và test (P7 đến P8) | Thay đổi code đã đủ tốt để merge chưa?  | Tech Lead                                                            | CI xanh; review đã approve; coverage không giảm                                 |
| G4   | Build và test               | Build đã sẵn sàng để test tích hợp chưa? | QA Lead                                                              | Build deploy được lên môi trường test; smoke test pass                       |
| G5   | Kiểm thử (P9)              | Chất lượng đã đạt chưa?                   | QA Lead (Security tham gia)                                          | Exit criteria ở mục G5                                                            |
| G6   | UAT (P10)                    | Khách hàng đã nghiệm thu chưa?              | Khách hàng ký, PO xác nhận                                      | 100% kịch bản UAT mức must pass, hoặc ngoại lệ có sign-off                   |
| G7   | Release (P11)                | Có nên release không?                          | PM hoặc Release Manager, sau khi tham vấn PO, QA, Security, DevOps | Release package đầy đủ; có kế hoạch rollback; có change ticket              |
| G8   | Vận hành (P12)             | Cycle đã đóng chưa?                          | PO                                                                   | Đã thu chỉ số sau release; các CR liên quan đã đóng                       |

## Chi tiết từng gate

Mục "Người duyệt xem xét" là `[Đề xuất]`.

### G1: Duyệt yêu cầu

- Bằng chứng cần có: CR, BRD hoặc PRD, SRS (FR, NFR), AC, ma trận truy vết, kết quả kiểm tra chất lượng yêu cầu.
- Điều kiện kiểm tra được: 100% FR có AC; 100% FR truy về CR; không còn finding mức blocker.
- Người duyệt xem xét: phạm vi và ưu tiên; giả định và hạn chế đã được ghi.
- Kết quả: SRS thành baseline. Mọi thay đổi sau đó phải đi qua CR mới.

### G2: Duyệt thiết kế và kế hoạch

- Bằng chứng cần có: kiến trúc, thiết kế chi tiết (API, data model), threat model, impact analysis,
  backlog và nhãn `ai-eligible` hoặc `human-only`, test strategy.
- Điều kiện kiểm tra được: mọi FR có thiết kế; threat model xong; mọi task truy về một FR.
- Người duyệt xem xét: quyết định kiến trúc và đánh đổi; rủi ro bảo mật; cách chia task.
- Kết quả: thiết kế và kế hoạch được duyệt.

### G3: Duyệt merge code

- Bằng chứng cần có: pull request, test, kết quả CI, ghi chú review.
- Điều kiện kiểm tra được: CI xanh (build, lint, unit test, quét bảo mật, quét phụ thuộc,
  quét bí mật); review đã approve; coverage không giảm.
- Người duyệt xem xét: thay đổi nằm trong phạm vi task; không còn góp ý chặn.
- Kết quả: code được merge. Chỉ thay đổi rủi ro thấp đã định nghĩa trước mới được merge tự động.

### G4: Build sẵn sàng để test

- Bằng chứng cần có: phiên bản build và commit, kết quả deploy lên môi trường test, báo cáo smoke test.
- Điều kiện kiểm tra được: build deploy được lên môi trường test; smoke test pass.
- Người duyệt xem xét: các hạn chế đã biết của build.
- Kết quả: build được nhận vào P9.

### G5: Chất lượng đạt

- Bằng chứng cần có: báo cáo integration, E2E, regression, hiệu năng, bảo mật; bảng defect; báo cáo coverage.
- Điều kiện kiểm tra được (exit criteria mẫu, có thể cấu hình):
  - 100% AC mức must có test case pass.
  - Không còn defect Critical hoặc High đang mở.
  - Code coverage không thấp hơn ngưỡng đã đặt.
  - NFR nằm trong ngưỡng.
  - Không còn lỗ hổng mức High chưa xử lý, trừ khi có sign-off chấp nhận rủi ro.
- Người duyệt xem xét: defect còn lại và rủi ro đã chấp nhận.
- Kết quả: hệ thống sẵn sàng cho UAT.

### G6: Nghiệm thu

- Bằng chứng cần có: kế hoạch và kịch bản UAT từ AC, kết quả UAT, phản hồi, bản ghi defect và CR.
- Điều kiện kiểm tra được: 100% kịch bản UAT mức must pass, hoặc ngoại lệ có sign-off.
- Người duyệt xem xét: người dùng thật tự thao tác và tự đánh giá; AI chỉ hỗ trợ (L1 đến L2),
  không nghiệm thu thay người dùng.
- Kết quả: bản được khách hàng chấp nhận.

Độ sâu UAT theo loại release:

| Loại release              | Độ sâu UAT                                                                                                              |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| MVP hoặc tính năng lớn | Đầy đủ: mọi AC mức must, luồng E2E; không được rút gọn                                                        |
| Thay đổi vừa hoặc nhỏ | Theo impact: AC bị ảnh hưởng và regression nghiệp vụ                                                                |
| Hotfix khẩn               | Tối thiểu: PO hoặc key user xác nhận lỗi đã hết; bắt buộc ghi ngoại lệ và lên lịch UAT bổ sung sau deploy |

Quy tắc: UAT bắt buộc cho mọi release; phản hồi từ buổi Demo không thay thế UAT;
build được đóng băng khi đang UAT, nếu phải sửa thì sửa qua quy trình có phiên bản
rồi UAT lại phần bị ảnh hưởng.

### G7: Go/No-go

- Bằng chứng cần có: release package, báo cáo G5, sign-off G6, release note, kế hoạch deploy và rollback,
  change ticket.
- Điều kiện kiểm tra được: release package đầy đủ; có kế hoạch rollback; có change ticket.
- Người duyệt xem xét: rollback đã diễn tập; SLO và alert đã cấu hình (mục 4.3); ý kiến của
  PO, QA, Security, DevOps.
- Kết quả: quyết định Go hoặc No-go được ghi lại. AI chỉ trình bày bảng sẵn sàng, không tự quyết.

### G8: Đóng vòng

- Bằng chứng cần có: chỉ số sau release, báo cáo giám sát, tóm tắt sự cố và defect, phản hồi người dùng,
  retrospective.
- Điều kiện kiểm tra được: đã thu chỉ số sau release; các CR liên quan đã đóng.
- Người duyệt xem xét: kết quả sản phẩm; việc còn lại được ghi và xếp ưu tiên.
- Kết quả: cycle đóng. Bug hoặc yêu cầu mới từ vận hành quay lại P1 dưới dạng CR mới.

## Milestone gate (chuyển stage)

M1 đến M4 chuyển sản phẩm giữa các stage. G6 vẫn là gate của từng cycle và luôn nằm trước
mọi release, kể cả release MVP.

| Milestone | Chuyển                                  | Ai ký                                        |
| --------- | ---------------------------------------- | --------------------------------------------- |
| M1        | POC sang Demo (quyết định tiếp tục) | Tech Lead, PO                                 |
| M2        | Demo sang MVP (chốt scope)              | PO, Sponsor                                   |
| M3        | Go/No-go MVP                             | (tài liệu gốc không nêu rõ người ký) |
| M4        | Go/No-go cho mỗi release Production     | (tài liệu gốc không nêu rõ người ký) |

| Stage      | UAT và G6                       | Ghi chú                                                           |
| ---------- | -------------------------------- | ------------------------------------------------------------------ |
| POC        | Không                           | Code thường bỏ; dữ liệu giả                                  |
| Demo       | Không                           | Demo không phải cam kết; chỉ SRS đã qua G1 mới là cam kết |
| MVP        | Bắt buộc, UAT đầy đủ       | MVP cắt scope, không cắt chuẩn                                 |
| Production | Bắt buộc, độ sâu theo scope | Hotfix: UAT tối thiểu, vẫn qua G6                               |

## AI tại các gate

Mức AI theo `docs/00-process/ai-levels.md`. Riêng ở gate:

- AI được: soạn tài liệu và bằng chứng (L2), phân tích (L1 đến L2), sinh test case (L2 đến L3),
  chạy kiểm tra đã định nghĩa trước (L3 đến L4), tóm tắt kết quả CI và test.
- AI không được: ký gate, chấp nhận rủi ro, nghiệm thu thay người dùng, quyết định Go/No-go cuối cùng.
- Sản phẩm AI tạo ra giữ trạng thái Draft cho đến khi được kiểm tra độc lập.
- Không đưa bí mật hoặc dữ liệu cá nhân, dữ liệu khách hàng chưa che vào AI
  (xem `docs/00-process/safety-rules.md`).

## Áp dụng trong dự án này

### Vai trò

Dự án một người, nên một người có thể đóng nhiều vai.

| Vai trò theo tài liệu   | Ai đảm nhận trong dự án này             |
| -------------------------- | --------------------------------------------- |
| PO                         | (bạn điền)                                 |
| Khách hàng hoặc Sponsor | (bạn điền: người đóng vai nghiệm thu) |
| Architect, Tech Lead       | (bạn điền)                                 |
| QA Lead                    | (bạn điền)                                 |
| PM, Release Manager        | (bạn điền)                                 |
| Security                   | (bạn điền)                                 |

### Khu vực nào đi qua gate nào

- `docs/` (tool chính): đi qua G1 đến G8 theo từng cycle.
- `labs/` và `poc/`: ở mức POC, không đi qua G1 đến G8 (không UAT, không G6). `[Đề xuất]` Dùng ba mốc:
  yêu cầu và AC đã duyệt trước khi nhờ AI viết code; ngưỡng trong `eval.md` đã khóa trước khi chạy;
  scorecard đã được tôi ký.
- `experiments/`: theo `docs/00-process/experiment-protocol.md`.

### Chứng cứ của từng gate trong repo

| Gate | Chứng cứ                                                                   | Vị trí                                            |
| ---- | ---------------------------------------------------------------------------- | --------------------------------------------------- |
| G1   | SRS, AC, ma trận truy vết, kết quả kiểm tra                             | `docs/01-requirements/`; tag `srs-v1.0`         |
| G2   | Thiết kế, threat model, bản đồ module, bảng quyền hạn agent, backlog | `docs/02-design/`; tag `design-v1.0`            |
| G3   | Pull request có CI xanh và checklist review                                | Pull request, `.github/PULL_REQUEST_TEMPLATE.md`  |
| G4   | Biên bản build lên môi trường test và smoke test                      | `docs/03-test/`                                   |
| G5   | Kết quả test, bảng defect, exit criteria                                  | `docs/03-test/test-strategy.md`, `eval-plan.md` |
| G6   | Kịch bản và kết quả UAT, ngoại lệ có sign-off                        | `docs/03-test/`                                   |
| G7   | Checklist phát hành, kế hoạch rollback, change ticket                    | `docs/04-release/release-checklist.md`            |
| G8   | Chỉ số sau release, tổng kết, CR đã đóng                             | `docs/05-retro/`                                  |

### Điều kiện nào kiểm tự động được hiện nay

- Có: kiểm tra định dạng Markdown (`docs-check`).
- Chưa có: "mọi FR có AC", "mọi FR truy về CR", "mọi task truy về FR". `[Đề xuất]` Trước mắt kiểm
  bằng checklist viết sẵn trước khi làm (trạng thái Verified bằng checklist, xem
  `docs/00-process/experiment-protocol.md`); về sau viết script đọc `traceability.md`.

### Cách ghi sign-off

`[Đề xuất]`

1. Mỗi gate là một Issue tạo từ mẫu `.github/ISSUE_TEMPLATE/gate-review.md` (nhãn `gate`,
   thuộc milestone tương ứng).
2. Khi đủ bằng chứng, tôi điền quyết định, người ký, ngày và liên kết bằng chứng, rồi đóng Issue.
   Đóng Issue là sign-off.
3. Sau G1 và G2, gắn tag (`srs-v1.0`, `design-v1.0`) để đóng băng baseline.
4. Nếu AI giúp chuẩn bị bằng chứng, ghi vào `docs/ai-log/`.

## Hạn chế khi làm một mình

- Người làm cũng là người ký, nên nguyên tắc "người tạo không tự duyệt chính sản phẩm đó" chỉ
  được giữ một phần: sản phẩm do AI tạo thì tôi duyệt; sản phẩm do tôi tạo thì cần checklist
  hoặc script viết sẵn và, khi có thể, một người ngoài dự án xem ở G1 và G2.
- Bảo vệ nhánh `main` có thể không dùng được với repo riêng tư gói miễn phí. Khi đó giữ kỷ luật:
  mọi thay đổi qua pull request, CI xanh rồi mới merge.

## Tài liệu liên quan

- `docs/00-process/sdlc-overview.md`: vòng đời 12 phase.
- `docs/00-process/ai-levels.md`: mức AI L0 đến L4 và ba trạng thái.
- `docs/00-process/safety-rules.md`: quy tắc an toàn khi dùng AI.
- `docs/00-process/experiment-protocol.md`: quy tắc so sánh hai model.
- `docs/06-roadmap/plan.md`: kế hoạch và việc cần làm cho gate tiếp theo.

## Phiên bản

- v1, (ngày): bản đầu, gộp từ hai bản nháp (Claude và GPT) và đối chiếu với tài liệu gốc.
