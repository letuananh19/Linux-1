# DHCP - Dynamic Host Configuration Protocol
## **1) Khái niệm**
- **DHCP - Dynamic Host Configuration Protocol** là 1 giao thức mạng tự động cung cấp các thông tin TCP/IP cho client . Mỗi **DHCP Client** kết nối tới 1 **DHCP Server** trong mạng và được trả về các thông tin như `địa chỉ IP` , `subnet-mask` , `gateway` , `dns-server` ,...
- Các thuật ngữ trong **DHCP Server** :
    - **Options** : các thông tin như `địa chỉ IP` , `subnet-mask` , `gateway` , `dns-server` sẽ được **DHCP Server** cung cấp đầy đủ .
    - **Scope** : một dãy hay 1 đoạn địa chỉ IP đã được quy hoạch và chỉ định rõ ràng cho **DHCP Server** được phép cấp phát IP động thông cho **DHCP Client** .
    - **Reservation** : là những địa chỉ IP đã được "để dành" , tức là dùng để gán cố định cho 1 số client .
    - **Lease** : thời gian cho "thuê" hoặc tồn tại của IP động đối với mỗi máy Client .
## **2) Các trạng thái DHCP ( state transition of DHCP Client )**
### **2.1) 

