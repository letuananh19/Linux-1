# SSH Tunneling
## **1) Khái niệm**
- **SSH Tunneling** ( hay còn được gọi là **SSH port forwarding** ) là hình thức thực hiện **SSH** từ máy client đến Server thông qua Internet dưới dạng **tunnel** ( đường hầm ) .
- **Tunneling** có thể được sử dụng để tăng cường bảo mật , vượt tường lửa , đôi khi dùng làm ***backdoor*** cho 1 mạng nội bộ .
## **2) Các hình thức SSH tunneling**
### **2.1) Local Forwarding**
- **Local Forwarding** được sử dụng để forward 1 port từ client đến SSH Server .
- Về cơ bản , SSH client lắng nghe kết nối trên 1 port đã được cấu hình , khi nhận được kết nối , nó bắt đầu thiết lập **tunnel** tới SSH Server .
- Server kết nối tới 1 port đích đã được cấu hình...
- Các trường hợp sử dụng **local port forwarding** thường bao gồm :
    - Kết nối **tunnel** cho session và trao đổi file thông qua **jump servers** .
    - Kết nối đến 1 service trong mạng nội bộ từ bên ngoài Internet
    - Kết nối đến 1 file được chia sẻ qua Internet
- Có khá nhiều doanh nghiệp cho phép tất cả các truy cập **SSH** thông qua 1 **jump server** . Server này có thể là 1 server Linux/Unix tiêu chuẩn , thường được tăng cường bảo mật , phát hiện xâm nhập , có hệ thống ghi log , hoặc có thể là 1 giải pháp **jump server** mang tính thương mại . <br>**VD :** **CryptoAuditor** có thể hoạt động như 1 **jump server** , ghi lại tất cả các phiên truy cập , các nội dung truy cập để phân tích và đưa ra cảnh báo sớm về các hoạt động đáng ngờ .
- Nhiều **jump server** chỉ cho phép **port forwarding** khi kết nối đã được xác thực . Làm như vậy khá là thuận tiện , bởi vì điều đó cho phép những người am hiểu công nghệ có thể sử dụng tài nguyên nội bộ 1 cách rõ ràng .<br>**VD :** Họ có thể forward port trên máy của họ tới web server nội bộ của công ty , tới mail server nội bộ của công ty , tới file server nội bộ thông qua port **`445`** hoặc **`139`** hoặc tới mọi thứ trong mạng nội bộ .
- Cấu trúc lệnh : sử dụng options `-L` :
    ```
    # ssh -L 80:intra.example.com:80 gw.example.com
    ```
    => Lệnh này cho phép mở kết nối tới **jump server** `gw.example.com` và forward bất kì kết nối nào từ port `80` của máy local đến port `80` của máy `intra.example.com` .


# LAB : SSH Tunneling
## **1) Mô hình**
<img src=https://i.imgur.com/r3UbzpS.png>

| | IP | LAN/WAN |
|-|----|---------|
| **SSH_Client** | `10.0.0.10/24` | WAN ( `ens33` ) |
| **Jump_Server** | `10.0.0.20/24` | WAN ( `ens33` ) |
|             | `172.16.0.20/24` | LAN ( `ens37` ) |
| **SSH_Server** | `172.16.0.10/24` | LAN ( `ens33` ) |
> ### **Mục tiêu 1** : **SSH local port forwarding**<br> Từ **SSH CLient** thực hiện kết nối SSH đến **SSH Server** thông qua **Jump Server**
- **B1 :** Đặt địa chỉ IP chính xác :
    - Trên **SSH Client** :
        ```
        # ifconfig ens33 10.0.0.10 netmask 255.255.255.0 up
        ```
        <img src=https://i.imgur.com/tH2KBW6.png>
    - Trên **Jump Server** :
        ```
        # ifconfig ens33 10.0.0.20 netmask 255.255.255.0 up
        # ifconfig ens33 172.16.0.20 netmask 255.255.255.0 up
        ```
        <img src=https://i.imgur.com/FLfybQa.png>
    - Trên **SSH Server** :
        ```
        # ifconfig ens33 172.16.0.10 netmask 255.255.255.0 up
        ```
        <img src=https://i.imgur.com/9UmnQUo.png>

- **B2 :** Trên **SSH_Client** thực hiện SSH vào **SSH_Server** qua **Jump_Server** :
    ```
    # ssh -L 2222:172.16.0.10:22 root@10.0.0.20
    ```