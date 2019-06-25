# Fail2ban
- Đây là 1 công cụ bảo mật cho rất nhiều dịch vụ như **SSH** , **FTP** , **SMTP** , **Apache** và các dịch vụ khác nữa chống lại nhưng vị khách không mời mà đến .
- Chương trình **Fail2ban** sẽ chạy nền dịch vụ và quét file **logs** được chỉ định bởi user hoặc sử dụng mẫu cấu hình mặc định . Sau đó **Fail2ban** sẽ sử dụng các khuôn mẫu mà **fail2ban** quy định để lọc và trích xuất các thông tin cần thiết . Từ những thông tin đó mà **Fail2ban** sẽ quyết định khóa truy cập IP đang thực hiện những hoạt động đáng ngờ , như đăng nhập **SSH** sai quá mật khẩu nhiều lần ,....
- Về cơ bản thì **Fail2ban** sẽ tạo ra 1 bộ rule tường lửa **iptables** để chặn truy cập từ địa chỉ IP đã xác định trong 1 khoảng thời gian nhất định . Như vậy , **Fail2ban** có khả năng giảm thiểu rất nhiều số hoạt động đăng nhập không chính xác vào dịch vụ hệ thống .
- Rất hữu dụng để chống lại khả năng tấn công của **Brute Force** , tuy nhiên không thể giải quyết các vấn đề như mật khẩu quá yếu , không đủ mạnh , phức tạp .
- Mặc định , **Fail2ban** không được cài đặt trên CentOS 7 .
## **1) Cài đặt Fail2ban**
- Cài đặt repo **EPEL** :
    ```
    # yum install -y epel-release
    ```
- Cài đặt gói **Fail2ban** :
    ```
    # yum install -y fail2ban
    ```
## **2) Cấu hình Fail2ban**
- Mở file cấu hình **Fail2ban** :
    ```
    # vi /etc/fail2ban/jail.conf
    ```
