BÁO CÁO XỬ LÝ QUAN HỆ NHIỀU - NHIỀU (MANY-TO-MANY) - RIKKEI CINEMA
Phần 1: Phân tích nguyên nhân lỗi & Mất dấu giá vé
Lý do không liên kết trực tiếp N-N giữa BOOKING và SEAT: Quan hệ đặt ghế phụ thuộc vào từng suất chiếu cụ thể; nếu nối trực tiếp mà không có ngữ cảnh suất chiếu (ShowtimeID), hệ thống không thể phân biệt ghế được đặt cho khung giờ nào, dẫn đến sự cố bán trùng ghế (Double Booking) giữa các ca chiếu.
Lý do mất dấu giá vé: Giá ghế (VIP/Standard) biến động theo thời gian hoặc chính sách; nếu không có bảng chi tiết để lưu giá tại thời điểm giao dịch, hệ thống phải truy vấn giá hiện tại của ghế, khiến báo cáo doanh thu các vé đã bán trong quá khứ bị sai lệch khi giá thay đổi.
Phần 2: Thiết kế thực thể trung gian TICKET (BOOKING_DETAIL)
Phân rã quan hệ N-N giữa BOOKING và SEAT thành 2 quan hệ 1-N thông qua thực thể trung gian TICKET (hoặc BOOKING_DETAIL):

Tên thực thể: TICKET
Khóa chính (PK): TicketID (hoặc khóa phức hợp BookingID, SeatID, ShowtimeID)
Khóa ngoại (FK):
BookingID (FK tham chiếu BOOKING.BookingID): Xác định vé thuộc đơn đặt nào.
SeatID (FK tham chiếu SEAT.SeatID): Xác định vị trí ghế được đặt.
ShowtimeID (FK tham chiếu SHOWTIME.ShowtimeID): Chống Double Booking theo từng suất chiếu.
Cột giá lịch sử: Price (decimal/numeric) dùng để đóng băng giá tại thời điểm hoàn tất giao dịch.
Phần 3: Phân tích vi phạm dạng chuẩn 2NF đối với SeatType
Nếu thêm cột SeatType vào thực thể trung gian TICKET:

Có vi phạm dạng chuẩn 2NF. Bởi vì SeatType là thuộc tính không khóa chỉ phụ thuộc hàm vào một phần của khóa (chỉ phụ thuộc vào SeatID), không phụ thuộc đầy đủ vào toàn bộ khóa chính của vé (BookingID, SeatID, ShowtimeID), do đó phải được giữ nguyên tại thực thể SEAT.
