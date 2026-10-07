# Kịch bản 10–15 phút — Màn tổng → Collateral Register → Quy trình

Không đọc nguyên văn. Chọn các tab theo số 1 → 2 → 3 → 4 → 5; trang 0 là bối cảnh dự phòng, không cần trình bày riêng.

## 0:00–3:00 · Màn 1: số lãnh đạo cần

- Chọn snapshot mẫu **30/09/2026 23:59**. Màn tổng trả lời: “Còn bao nhiêu tài sản Tự do tại đúng thời điểm này?”.
- KPI mẫu: tổng GT cha 140, phân bổ còn hiệu lực 90, Tự do 50 tỷ; theo loại là 30 + 5 + 15.
- Công thức: `Tự do tại thời điểm xem = Σ GT cha tại thời điểm đó − Σ phân bổ con còn hiệu lực tại cùng thời điểm`.
- Không trừ phân bổ đã hết hiệu lực/lịch sử. Tái phân bổ chỉ tính ở đích hiện hành một lần, không cộng cả bản cũ lẫn bản mới.
- Đây là số theo sổ, chưa haircut/LTV; Tự do không đồng nghĩa ngân hàng chắc chắn cấp thêm hạn mức.

## 3:00–6:00 · Màn 2: Collateral Register

- Mở TS-BDS-001: GT cha 100; VCB/HĐ-001 phân bổ 40 active; BIDV/HĐ-002 phân bổ 30 active. Chi tiết 70 khớp “Đã cầm cố”; Tự do 30 khớp màn tổng.
- Chỉ vào kỳ hiệu lực và trạng thái tại snapshot. Chỉ dòng active, có hiệu lực tại 30/09 mới bị trừ; released/expired vẫn tra được nhưng không bị trừ.
- Lịch sử BIDV: tạo AL-002 20 tỷ ngày 01/06; điều chỉnh 20 → 30 tỷ, hiệu lực từ 15/08; tại snapshot 30/09, mức hiện hành 30 được tính.
- Phân biệt **ngày ghi nhận thay đổi** để audit ai nhập lúc nào với **ngày hiệu lực** để dựng lại số tại ngày được xem.
- “Collateral register là sổ theo dõi tài sản gốc và các phân bổ bảo đảm, ngân hàng/hợp đồng, giá trị, trạng thái/kỳ hiệu lực và lịch sử. Nó giúp truy ngược số tổng về từng khoản có bằng chứng.”

Ví dụ tái phân bổ nếu bị hỏi: B1 20 vừa giải chấp thì Tự do tạm tăng 60 → 80; khi 20 được phân bổ mới sang B2, B2 thành 40 và Tự do về 60. Giữ dòng B1 Released để audit; chỉ phân bổ hiện hành được tính.

## 6:00–9:00 · Flowchart và rule

- Cổng cha: GT gốc > 0, mã không trùng. Cổng phân bổ: có NH/HĐ bảo đảm và ngày hiệu lực.
- Cổng sức chứa: tổng phân bổ còn hiệu lực tại thời điểm giao dịch không vượt GT cha. Lịch sử/hết hiệu lực không chiếm sức chứa.
- Giải chấp: giữ lịch sử nhưng giải phóng sức chứa kể từ ngày hiệu lực được xác nhận. “Ngưng hiệu lực” không mặc định là đã hết ràng buộc pháp lý.
- Tính lại theo cùng mốc thời gian. Backend cần kiểm tra nguyên tử để tránh hai user cùng phân bổ một phần headroom; HTML chỉ minh họa một phiên.
- Câu chốt: “Màn tổng cho biết số; Collateral Register cho biết số đến từ đâu; flowchart bảo vệ vòng đời làm thay đổi số.”

## 9:00–10:00 · Use case

- Ban Lãnh đạo xem tổng; Finance truy xuất hợp đồng/lịch sử; User Vận hành tạo/cập nhật phân bổ. Đây là persona theo công việc, không phải phân quyền; mọi user có cùng CRUD theo constraint đề.
- **User story — Cổng 3:** “Là User Vận hành, tôi muốn gán 50 tỷ vào tài sản cha 100 tỷ đang có phân bổ 40 + 30 tỷ để ghi nhận HĐ mới.” Acceptance: tổng dự kiến 120 > 100, hệ thống chặn lưu, báo vượt 20 tỷ, không tạo một phần và số Tự do 30 tỷ không đổi.
- **User story — Cổng 4:** “Khi nhận xác nhận B1 đã giải chấp 20 tỷ, tôi muốn ghi ngày hiệu lực và lý do để số Tự do được tính lại mà vẫn truy nguyên được lịch sử.” Hệ thống giữ dòng cũ ở Released, không xóa cứng; từ ngày hiệu lực đó, phân bổ không còn bị trừ.
- Sửa GT cha/con thì chạy lại Cổng 3 và ghi before/after. Nếu GT cha mới 60 nhưng phân bổ active là 70, chặn hoặc đánh dấu ngoại lệ theo rule Finance xác nhận; không phát hành số Tự do như số sẵn sàng vay.
- “Ngưng” không đồng nghĩa giải chấp: nếu tài sản còn ràng buộc pháp lý thì vẫn tính là active. Xóa cứng không dùng để giải chấp; nếu cần thao tác Xóa theo CRUD thì xác nhận soft delete và giữ audit.
- Quan hệ UML: Lọc loại và mở dòng xem NH/HĐ là `extend` tùy chọn từ luồng xem snapshot; mũi tên `extend` hướng về use case gốc.

## 10:00–12:00 · Rủi ro

Đề cho “không phân quyền”. Không phản đối suông — nhận constraint, chỉ ra lỗ hổng số liệu:

| Hành vi | Số Tự do trên báo cáo sếp |
| --- | --- |
| Xóa con cầm cố | Tăng ảo → sếp tưởng còn tài sản, thế chấp trùng |
| Sửa GT cha giảm / GT con tăng | Tự do âm hoặc vượt sức chứa |
| Sửa tay ô Tự do | Mất single source of truth |
| Hai người sửa cùng lúc | Cộng hai lần / mất một HĐ |

Gói 4 kiểm soát **không cần IAM đầy đủ**:

1. **Tự do là field tính** — khóa nhập.
2. **Cấm xóa cứng** — chỉ giải chấp / ngưng hiệu lực, bắt buộc lý do.
3. **Nhật ký trước–sau** (ai, lúc nào, giá trị cũ/mới).
4. **Chốt số báo cáo sếp** — snapshot cuối ngày; thao tác vận hành không tự sửa số đã gửi lãnh đạo. Maker–checker 1 bước nếu Finance đồng ý (user nhập, Kế toán trưởng duyệt mới lên snapshot).

Câu chốt: “Rủi ro không phải ‘ai được vào module’, mà là ‘số Tự do có bị viết đè không kiểm soát’.”

## 12:00–15:00 · Đóng + hỏi ngược

Ba câu hỏi (chọn 2):

1. Sếp nhìn Tự do theo **giá sổ** hay **giá sau haircut từng ngân hàng**?
2. Một tài sản cha có được cầm cố **cấp 2** (second charge) không?
3. Định giá lại định kỳ: khi GT cha giảm dưới Σ con đang cầm cố, business rule là chặn hay cảnh báo + tạo việc định giá?

Nếu họ kéo sang KPI/PnL (sau take-home, không xen vào 15 phút): Gross ≠ Net; COGS = 4.1; GP = Net hàng + net ship − COGS; promotion 2.1 ≠ voucher 6.x. Cầu: cùng identity + anti-double-count. Chi tiết miệng: `environment/dialog_kpi_pnl.md`.

Dừng. Không xin thêm thời gian.
