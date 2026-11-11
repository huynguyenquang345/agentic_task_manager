## 2026-11-02, phản biện labs/README.md

- Model: (tên và phiên bản đúng như hiển thị trong ô chọn model, không ghi "Auto")
- Mức: L1, chỉ đọc, không sửa file
- Prompt: (dán nguyên văn)
- Kết quả: 7 phát hiện; hữu ích: điểm 3 và 7; chỉ là chỗ trống khung: điểm 1, 5, 6
- Bỏ sót (tôi tự phát hiện): tách hai loại mức AI; ràng buộc an toàn của hai lab
- Việc tôi làm: sửa README theo điểm 2, 3, 4, 7
- Trạng thái: Draft (phản biện) → tôi duyệt và áp dụng

## 2026-11-02, soạn labs/README.md

- Model: Claude (ghi đúng tên và phiên bản bạn thấy trong giao diện)
- Mức: L2, soạn nháp mục đích và câu hỏi POC; phần còn lại do tôi chốt trước đó
- Việc tôi làm: đọc lại, sửa (ghi chỗ đã sửa nếu có)
- Trạng thái: Draft → Approved bởi tôi

## 2026-11-03, hoàn thiện docs/00-process/experiment-protocol.md v1

- Model: Claude và GPT (ghi đúng tên và phiên bản hiển thị trong giao diện)
- Mức: L2, soạn nháp từ hai bản; tôi gộp và duyệt
- Việc tôi làm: bỏ thang 0-5, bỏ phần trùng với file khác, giữ thang 0-2
- Trạng thái: Draft → Approved bởi tôi

## 2026-11-03, hoàn thiện experiment-protocol.md v1.1

- Model: Claude Sonnet 5.5 (soạn nháp bản vá) sau phản biện của chính model này
- Mức: L2, tôi đọc lại, điền harness và chỉnh ngưỡng
- Việc tôi làm: Thêm các mục đã được phản biện bởi claude sonnet 5.5
- Trạng thái: Draft → Approved bởi tôi

## 2026-11-04, soạn docs/00-process/gates.md

- Model: Claude Sonnet 5.5 (chat web)
- Mức: L2, soạn nháp theo mục 3.1 của PDF; tôi điền vai trò và duyệt
- Việc tôi làm: (ghi chỗ bạn đã sửa)
- Trạng thái: Draft → Approved bởi tôi

## 2026-11-05, soạn docs/00-process/safety-rules.md

- Model: Claude Sonnet 5.5 (chat web)
- Mức: L2, soạn nháp theo mục 4.1 và 4.2; tôi điền bảng công cụ và duyệt
- Việc tôi làm: viết bản nháp cho safety-rules.md
- Trạng thái: Draft → Approved bởi tôi

## 2026-11-06, soạn docs/00-process/sdlc-overview.md

- Model: Claude Sonnet 5.5 (chat web)
- Mức: L2, soạn nháp theo mục 1 đến 4 của PDF; tôi điền cycle đầu tiên và duyệt
- Việc tôi làm: create draft to compare
- Trạng thái: Draft → Approved bởi tôi

## 2026-11-06, hợp nhất sdlc-overview.md từ hai bản nháp

- Model: Claude Sonnet 5.5 (chat web) và GPT-6 (ghi đúng tên hiển thị)
- Mức: L2; tôi gộp, điền cycle đầu tiên và duyệt
- Điều kiện: prompt không hoàn toàn giống nhau, không chấm mù (điểm dữ liệu tham khảo)
- Nhận xét: bản GPT mạnh ở mô tả phase và quản lý thay đổi nhưng nhóm phase lệch PDF và trùng
  lặp với file khác; bản Claude chính xác hơn nhưng quá gọn
- Việc tôi làm: compare both reply version of GPT-6 and Claude then unite
- Trạng thái: Draft → Approved bởi tôi

## 2026-11-10, hoàn thiện docs/00-process/safety-rules.md v1.1

- Model: Claude Sonnet 5.5 (chat web)
- Mức: L2, viết lại theo kết quả đối chiếu với PDF mục 4.1, 4.2 và các mục liên quan; tôi điền bảng công cụ và duyệt
- Việc tôi làm: viết ngày chỉnh sửa ở cuối file markdown
- Trạng thái: Draft → Approved bởi tôi

## 2026-11-10, soạn nháp docs/01-requirements/acceptance-criteria.md

- Model: Claude Sonnet 5.5 (chat web)
- Mức: L2, soạn nháp khung và bộ AC theo tài liệu gốc; chưa có BRD và SRS của tôi
- Việc tôi làm: Soạn nháp bản markdown của file acceptance-criteria để đối chiếu với GPT-6
- Trạng thái: Draft (chờ FR được duyệt)
- Ghi chú: mã FR là dự kiến, phải đồng bộ với srs.md
