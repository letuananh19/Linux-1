# DHCP Configuration ( Basic )
## **1) Các file/thư mục quan trọng của dịch vụ DHCP**
- `/etc/dhcp/dhcpd.conf` : file cấu hình dịch vụ DHCP
- `/var/lib/dhcpd/dhcp.leases` : file chứa thông tin các IP động đang cấp qua DHCP
- `/var/log/message` : log mặc định chứa quá trình DHCP 4 bước
## **2) Cấu hình dịch vụ DHCP Server**
### **Mô hình thực tế**
<img src=https://i.imgur.com/IVzTiUR.png>

### **Mô hình LAB**
<img src=https://i.imgur.com/60smOhu.png>

| | IP | Ethernet |
|-|----|---------|
| **DHCP_Server** | `172.16.0.5/24` | `ens37` |
| **Ubuntu 19.04** | `?` | `ens37` |
| **Windows 10** | `?` | `ens37` |
### **Các bước triển khai**
- **B1 :** Kiểm tra xem gói chương trình `dhcp` đã được cài đặt chưa , nếu chưa thì cài bằng `yum` :
    ```
    # rpm -qa | grep dhcp
    # yum install -y dhcp
    ```
    <img src=https://i.imgur.com/GFPGMkj.png>

- **B2 :** Cấu hình dịch vụ DHCP
    - **B2.1** : Chỉnh sửa file cấu hình `dhcpd.conf` :

        <img src=https://i.imgur.com/VLcP36j.png>

        - File cấu hình DHCP gồm 2 phần :
            - **Cấu hình toàn cục ( `global` )** : quy định những thông tin giá trị mặc định cho các khai báo lớp mạng ( subnet ) cấp pháp IP động DHCP .
            - **Cấu hình lớp mạng cấp phát IP động ( `scope` )** : quy định những giá trị thông tin cho việc cấp phát IP động thông qua DHCP .
        - Cấu hình **`global`** :
            ```
            option domain-name "nhanhoa.com";               # Tên Domain
            option domain-name-servers  172.16.1.3;         # Khai báo DNS
            option router 172.16.1.1;                       # Khai báo gateway
            default-lease-time 600;                         # Thời gian mặc định 1 IP DHCP tồn tại
            max-lease-time 7200;                            # Thời gian tối đa 1 IP được cấp cho client
            lease-file-name "/var/lib/dhcpd.leases";        # File chứa thông tin về địa chỉ đã được cấp phát qua DHCP
            authoritative;                                  # Chỉ định đây là DHCP Server master
            ```
        - Cấu hình **`scope`** :
            ```
            subnet 172.16.0.0 netmask 255.255.255.0 {
                range 172.16.11 
             
