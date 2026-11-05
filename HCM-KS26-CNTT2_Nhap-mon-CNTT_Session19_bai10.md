## **Bài 10: Đánh Giá Đa Giải Pháp Xác Thực Đa Yếu Tố (2FA)** 

## **Phần 1: Phân tích 2 Giải pháp Xác thực**

Phân tích 3 yếu tố xác thực danh tính:

\-Something You Know (điều bạn biết): Mật khẩu, mã PIN
\-Something You Have (Thứ bạn sở hữu): Thiết bị hoặc vật thể bạn nắm giữ (Điện thoại cài Authenticator App, Khóa bảo mật phần cứng YubiKey, thẻ từ, SIM,).  
\-Something You Are (Thứ thuộc về bạn): Đặc điểm sinh trắc học (Vân tay, FaceID,quét giọng nói).

Trong mô hình 2FA:  
\-Mật khẩu (Password):đại diện cho yếu tố Something You Know.  
\-Mã OTP: đại diện cho yếu tố Something You Have (chứng minh mình đang sở hữu chiếc điện thoại trong tay để xác thực OTP).

### 2\. Giải thích nguyên lý hoạt động của Thuật toán TOTP của Authenticator khi Offline

\-Dùng mật mã chung: Khi sử dụng Authenticator quét mã QR lần đầu,ứng dụng và điện thoại sẽ lưu chung 1 "mật mã bí mật".  
\-Đồng bộ giờ: Cả hai thiết bị bên ứng dụng và điện thoại đồng bộ cùng dùng chung đồng hồ hệ thống (thời gian thực).  
\-App và thiết bị tính toán độc lập: Cứ 30 giây, app lấy mật mã bí mật kết hợp với giờ hiện tại để tính ra 6 chữ số. điện thoại ở phía bên kia cũng tính theo đúng cách đó nhờ đồng bộ dùng chung (thời gian thực) nên App không cần Wi-Fi/4G/SMS vì mọi quy trình đều diễn ra trực tiếp trên điện thoại và máy chủ Authenticator .

## **Phần 2: Bảng So sánh Trade-off & Ra quyết định**

### 3\. Bảng so sánh SMS OTP và Authenticator TOTP

| Tiêu chí đánh giá                          | Giải pháp 1: SMS OTP                                                                                          | Giải pháp 2: TOTP Authenticator                                                                                                                   |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1\. Khả năng chống SIM Swapping & SS7      | Kém — Dễ bị kẻ xấu lừa nhà mạng cướp SIM hoặc nghe lén tin nhắn.                                              | Tuyệt đối — Mã lưu an toàn trong chip điện thoại, cướp SIM cũng không lấy được mã.                                                                |
| 2\. Tính sẵn sàng khi Offline / Nước ngoài | Phụ thuộc sóng di động — Mất sóng, đi máy bay hoặc ra nước ngoài chưa Roaming sẽ không nhận được mã.          | Hoạt động 100% Offline — Sinh mã chính xác mọi lúc mọi nơi, kể cả Chế độ Máy bay.                                                                 |
| 3\. Tiện dụng & Chi phí vận hành           | Dễ dùng nhưng tốn phí — Không cần cài app, nhưng doanh nghiệp tốn tiền mua dịch vụ SMS cho mỗi lần đăng nhập. | Miễn phí 90% (tùy ứng dụng, trừ 1password bắt trả phí) & Nhanh chóng — Doanh nghiệp không mất phí viễn thông, thao tác lấy mã trên app cực nhanh. |
| 4\. Rủi ro khi mất điện thoại              | Dễ khôi phục nhưng rủi ro cao — Làm lại SIM là lấy lại mã, nhưng kẻ trộm SIM có thể chiếm tài khoản ngay.     | Bảo mật cao — Cần dùng Bộ Mã Khôi phục (Backup Codes) để lấy lại quyền truy cập.                                                                  |

### 4\. Kết luận lựa chọn cho PixelForge Studio

Lựa chọn: dùng TOTP Authenticator là hiệu quả tối ưu nhất

Lý do chọn:

1. Khả năng chống SIM Swapping & SS7:Mã lưu an toàn trong chip điện thoại, cướp SIM cũng không lấy được mã.
2. Tính sẵn sàng khi Offline / Nước ngoài:Hoạt động 100% Offline — Sinh mã chính xác mọi lúc mọi nơi, kể cả Chế độ Máy bay khác Sim cần có sóng.
3. Tiện dụng & Chi phí vận hành: tối ưu chi phí cho doanh nghiệp không tốn tiền như mua dịch vụ SMS cho mỗi lần đăng nhập
4. khi mất điện thoại:Cần dùng Bộ Mã Khôi phục (Backup Codes) để lấy lại quyền truy cập với bảo mật cao, còn Sim thì làm lại vẫn bị kẻ trộm SIM có thể chiếm tài khoản ngay.

## **Phần 3: Quản lý Rủi ro & Backup Codes**

### 5\. Quản lý Bộ 10 Mã Khôi phục (Backup Codes)

\-Mục đích: Bộ mã dùng 1 lần này là "chìa khóa dự phòng" duy nhất giúp người dùng đăng nhập lại tài khoản để cài lại 2FA khi bị mất, hỏng điện thoại hoặc lỡ xóa app Authenticator.  
2 vị trí lưu an toàn tuyệt đối:

\-Trình quản lý mật khẩu doanh nghiệp: Lưu dạng Encrypted Note trên 1Password hoặc Bitwarden.  
\-In ra giấy: In bộ mã cất vào két sắt hoặc tủ tài liệu có khóa tại nhà/văn phòng.  
1 sai lầm tai hại cần tránh:  
\-Sai lầm rủi ro: không được chụp ảnh màn hình hoặc lưu file plaintext  (backup\_codes.txt) trên điện thoại, máy tính, hoặc Google Drive cá nhân.  
\-Hậu quả: Nếu thiết bị nhiễm virus hoặc tài khoản Cloud bị lộ, kẻ xấu sẽ lấy trọn 10 mã này và vượt qua hoàn toàn lớp bảo vệ 2FA.  
