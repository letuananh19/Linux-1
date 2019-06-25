# SSH - Secure Shell
## **1) Khái niệm**
- **SSH - Secure Shell** là một giao thức mạng dùng để thiết lập kết nối mạng một cách bảo mật .
- **SSH** có sử dụng cơ chế mã hoá đủ mạnh nhằm ngăn chặn các hiện tượng nghe trộm , đánh cắp thông tin trên đường truyền . Các giao thức trước đây như **rlogin** , **telnet** không hỗ trợ mã hóa .
- **SSH** hoạt động ở tầng **Application** trong mô hình **TCP/IP**
- Các công cụ **SSH** ( như là **OpenSSH**,...) cung cấp cho người dùng cách thức để thiết lập kết nối mạng được mã hoá để tạo một kênh kết nối riêng tư .
## **2) Cấu trúc lệnh SSH**
- Nếu đang sử dụng Linux hoặc Mac OS , kết nối **SSH** khá đơn giản bằng cách sử dụng **Terminal** . Nếu sử dụng Windows , cần thêm 1 chương trình khác để mở kết nối **SSH** . Trình kết nối **SSH** được sử dụng phổ biến nhất cho Windows là **PuTTY** .
- Với Mac OS và Linux , mở **Terminal** và gõ lệnh theo cấu trúc sau :
    ```
    # ssh user@[host/IP]
    ```
    - Trong đó :
        - **user** : user local trên máy cần ssh
        - **host/IP** : hostname ( VD : `www.xyzdomain.com` ) hoặc IP của máy cần kết nối SSH ( VD : `244.235.23.19` )
- Sau khi thực hiện lệnh , máy đầu xa sẽ yêu cầu password của user sử dụng **SSH** .
## **3) Các kỹ thuật mã hóa**
- Một lợi thế quan trọng của **SSH** so với các giao thức tiền nhiệm trước là nó sử dụng mã hóa để đảm bảo kết nối bảo mật giữa ***host*** và ***client*** .
    - ***Host*** là server đầu xa .
    - ***Client*** là máy tính đang thực hiện truy cập từ xa vào ***host*** .
- Có 3 công nghệ mã hóa khác nhau được sử dụng trong **SSH** là : ***symmetric encryption*** , ***asymmetric encryption*** và ***hashing*** .
### **3.1) Symmetric Encryption ( *mã hóa đối xứng* )**
- **Symmetric Encryption** là 1 dạng mã hóa mà sử dụng 1 **secret key** cho cả việc mã hóa và giải mã gói tin bởi ***host*** và ***client*** . Do vậy , bất cứ ai có được **secret key** đều có thể giải mã gói tin .

    <img src=https://i.imgur.com/54cAASp.png>


