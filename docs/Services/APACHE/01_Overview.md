# Tổng quan về Web Server
## **1) Web Server là gì**
- **Web Server** là 1 dịch vụ mạng hướng nội dung của người dùng lên giao diện Web .
- **Web Server** còn được gọi là **HTTP server** và chúng sử dụng giao thức **HTTP - hypertext transport protocol** .
- Các **Web Server** có thể cài lên CentOS 7 là :
    - **Apache HTTP Server**
    - **nginx**
## **2) Apache HTTP Server** <img src=https://i.imgur.com/aeHvD7d.png align=right width=35%>
- Trang chủ : http://httpd.apache.org/
- **Apache** là phần mềm máy chủ mã nguồn mở đa nền tảng miễn phí .
- Ra đời năm `1995` bởi **Robert McCool**
- Phiên bản ổn định : `2.4.39` ( `tháng 4-2019` ) , tuy nhiên phiên bản `2.2` vẫn còn được sử dụng .
- **Apache** được phát triển và duy trì bởi một cộng đồng các nhà phát triển dưới sự bảo trợ của **Quỹ phần mềm Apache ( Apache Software Foundation )** . 
- Phần lớn các phiên bản **Apache** chạy trên bản phân phối Linux , nhưng các phiên bản hiện tại cũng chạy trên Microsoft Windows và nhiều hệ thống tương tự Unix . Các phiên bản trước đây cũng chạy trên OpenVMS , NetWare và các hệ điều hành khác .
## **3) Cài đặt Apache Server**
- **B1 :** Cài đặt repo `Epel` :
    ```
    # yum install -y epel-release
    ```
- **B2 :** Cài đặt gói `httpd` :
    ```
    # yum install -y httpd
    ```
    - Mặc định khi cài qua `yum` , trên CentOS `6` sẽ cài Apache `2.2` , còn trên CentOS `7` sẽ cài `2.4` .
- **Gỡ cài đặt Apache**
    ```
    # yum remove httpd -y
    ```