# LVM - Logical Volume Manager
- **Logical Volume Manager ( LVM )** là phương pháp cho phép ấn định không gian đĩa cứng thành những ***Logical Volume*** khiến cho việc thay đổi kích thước trở nên dễ dàng hơn so với ***partition*** .
- Với kỹ thuật **LVM** , có thể thay đổi kích thước phân vùng mà không cần phải sửa lại table của OS . Điều này rất hữu ích với trường hợp đã sử dụng hết phần bộ nhớ còn trống của partition và muốn mở rộng dung lượng của nó .
- Vai trò của **LVM** : **LVM** là kỹ thuật quản lý việc thay đổi kích thước lưu trữ của ổ cứng :
    - Không để hệ thống bị gián đoạn hoạt động
    - Không làm hỏng dịch vụ
    - Có thể kết hợp với cơ chế ***hot swapping*** ( phương pháp thay thế nóng các thành phần bên trong máy tính )
## **1) Mô hình các thành phần trong LVM**
### **1.1) Hard drives - Drives**
- Là các thiết bị lưu trữ dữ liệu , có dạng `/dev/xxxx`
### **1.2) Partition**
- Là các phân vùng của **hard drives** , mỗi **hard drive** có `4` **partition** trong đó **partition** bao gồm 2 loại là ***primary partition*** và ***extended partition*** :
- **Primary Partition** : 
    - Là phân vùng chính , có thể boot
    - Mỗi đĩa cứng có thể có tối đa `4` phân vùng này
- **Extended Partition** :
    - Là phân vùng mở rộng chứa dữ liệu , trong nó có thể tạo các **logical partition**
### **1.3) Physical Volume** 
- Là 1 tên gọi khác của **partition** trong kỹ thuật **LVM** , nó là những thành phần cơ bản được sử dụng bởi **LVM**
- Một **Physical Volume** không thể mở rộng ra ngoài phạm vi 1 ổ đĩa .
- Có thể kết hợp nhiều **Physical Volume** thành một **Volume Group** .
### **1.4) Volume Group**
- Nhiều **Physical Volume** trên những ổ đĩa khác nhau kết hợp lại thành 1 **Volume Group**
- **Volume Group** được dùng để tạo ra các **Logical Volume** , trong đó người dùng có thể tạo , thay đổi kích thước , gỡ bỏ và sử dụng .
> ***Lưu ý :** Boot Loader không thể đọc `/boot` khi nó nằm trên **Logical Volume Group** . Do đó không thể sử dụng **LVM** với **boot mount point*** .
### **1.5) Logical Volume**
- **Volume Group** được chia nhỏ thành các **Logical Volume** , mỗi **Logical Volume** có ý nghĩa tương tự 1 partition . Nó được dùng cho các mount point và được format với những định dạng khác nhau như `ext2` , `ext3` , `ext4` , `xfs` ,...
- Khi dung lượng của **Physical volume** được sử dụng hết ta có thể đưa thêm ổ đĩa mới bổ sung cho **Volume Group** và do đó tăng được dung lượng của **Logical Volume** .<br>**VD :** Có thể tạo ra 4 ổ đĩa mỗi ổ `5GB` , kết hợp thành 1 **Volume Group** `20G`  , có thể tạo ra 2 **Logical Volume** mỗi cái `10G` .
### **1.6) File Systems**
- Tổ chức và kiểm soát các tập tin
- Được lưu trữ trên ổ đĩa cho phép truy cập nhanh chóng và an toàn
- Sắp xếp dữ liệu đĩa cứng trên máy tính
- Quản lý vị trí vật lý của mọi thành phần dữ liệu
### **1.7) Physical Extend ( PE )**
- Là 1 đại lượng thể hiện 1 khối dữ liệu dùng làm đơn vị tính dung lượng của **Logical Volume** .
## **2) Ưu điểm và nhược điểm của LVM**
### **2.1) Ưu điểm**
- Có thể gom nhiều đĩa cứng vật lý lại thành 1 đĩa ảo dung lượng lớn
- Có thể tạo ra các vùng dung lượng lớn nhỏ tùy ý
- Có thể thay đổi các vùng dung lượng đó dễ dàng , linh hoạt
### **2.2) Nhược điểm**
- Các bước thiết lập phức tạp , khó khăn hơn
- Càng gắn nhiều đĩa cứng và thiết lập càng nhiều **LVM** thì hệ thống khởi động càng lâu
- Khả năng mất dữ liệu khi 1 trong các đĩa cứng vật lý bị hỏng
- Windows không thể nhận ra vùng dữ liệu của **LVM** . Nếu *dual-boot* Windows sẽ không thể truy cập dữ liệu chứa trong **LVM**
## **3) Các lệnh trong LVM**
- **Physical Volume** :
    - `pvcreate` : tạo **physical volume**
    - `pvdisplay` , `pvs` : xem **physical volume** đã tạo
    - `pvremove` : xóa **physical volume**
- **Volume Group** :
    - `vgcreate` : tạo **volume group**
    - `vgdisplay` , `vgs` : xem **volume group** đã tạo
    - `vgremove` : xóa **volume group**
    - `vgextend` : tăng dung lượng của **volume group**
    - `vgreduce` : giảm dung lượng của **volume group**
- **Logical Volume** :
    - `lvcreate` : tạo **logical volume**
    - `lvdisplay` , `lvs` : xem **logical volume** đã tạo
    - `lvremove` : xóa **logical volume**
    - `lvextend` : tăng dung lượng **logical volume**
    - `lvreduce` : giảm dung lượng **logical volume**
## **4) Các bước tạo LVM**
- Thêm 4 ổ cứng `sdb` , `sdc` , `sdd` , `sde` vào Server
- **B1 :** Kiểm tra các hard drive có trên hệ thống :
    ```
    # lsblk
    ```
    hoặc
    ```
    # fdisk -l
    ```
    <img src=https://i.imgur.com/fYW0gzw.png>

- **B2 :** Tạo **partition** :
    - Từ các hard disk trên hệ thống , tạo ra các partition .
    - Dùng lệnh `fdisk` ( có thể dùng `parted` )
        ```
        # fdisk /dev/sdb
        ```
        <img src=https://i.imgur.com/7MCR1ml.png>

        - `n` : tạo mới partition

            <img src=https://i.imgur.com/qu3evFC.png>

        - `p` : tạo partition `primary`
        - `1` : tạo partition `primary` thứ nhất
        - Dòng "`First sector (2048-41943039,defaul 2048)`" để **default**
        - Dòng "`Last sector,+sector or size {K,M,G} (2048-41943039),default 41943039`"<br>=> `+10G` để tạo ra phân vùng `10GiB`

            <img src=https://i.imgur.com/UOW9Rae.png>

        - `t` : tùy chọn thày đổi định dạng partition
        - `8e` : thay đổi về định dạng **LVM**
        - `w` : lưu lại và thoát
    - Tương tự tạo thêm partition với các hard disk khác .

        <img src=https://i.imgur.com/uKTlRLr.png>

- **B3 :** Tạo **physical volume** :
    - Tạo các **physical volume** là `/dev/sdb1` , `/dev/sdc1` , `/dev/sdd1` , `/dev/sde1` :
        ```
        # pvcreate /dev/sdb1 /dev/sdc1 /dev/sdd1 /dev/sde1
        ```
        <img src=https://i.imgur.com/jdc5MCL.png>

    - Kiểm tra lại bằng lệnh `pvs` hoặc `pvdisplay` :

        <img src=https://i.imgur.com/fTGUtA6.png>
- **B4 :** Tạo **volume group** :
    - Nhóm các **physical volume** thành 1 **volume group** bằng câu lệnh
        ```
        # vgcreate vg-demo1 /dev/sdb1 /dev/sdc1 /dev/sdd1 /dev/sde1    ( vg-demo1 là tên volume group )
        ```
        <img src=https://i.imgur.com/OIWku0W.png>
    - Kiểm tra lại bằng lệnh `vgs` hoặc `vgdisplay` :
        
        <img src=https://i.imgur.com/JLJlbQQ.png>

- **B5 :** Tạo **logical volume** :
    - Từ 1 **volume group** , có thể tạo ra các **logical volume** bằng lệnh :
        ```
        # lvcreate -L 25G -n lv-demo1 vg-demo1
        ```
        - Trong đó :
            - `-L` : chỉ ra dung lượng của **logical volume**
            - `-n` : chỉ ra tên của **logical volume**

        <img src=https://i.imgur.com/rCpo27M.png>

    - Có thể tạo nhiều **logical volume** từ 1 **volume group**
    - Kiểm tra lại bằng lệnh `lvs` hoặc `lvdisplay` :

        <img src=https://i.imgur.com/jPNJKpL.png>
    
- **B6 :** Format **logical volume**
    ```
    # mkfs.xfs /dev/vg-demo1/lv-demo1
    ```
    <img src=https://i.imgur.com/KxV0vlZ.png>

- **B7 :** Mount và sử dụng :
    ```
    # mkdir demo1
    # mount /dev/vg-demo1/lv-demo1 demo1
    # df -h => kiểm tra
    ```
    <img src=https://i.imgur.com/9OQPhz4.png>

- **B8 :** Lưu cấu hình vào file `/etc/fstab`
    ```
    # echo /dev/vg-demo1/lv-demo1 /demo1 xfs defaults 0 0
    ```
## **5) Cách thay đổi dung lượng Logical volume trên LVM**
- **B1 :** Kiểm tra các thông tin hiện có : 
    ```
    # pvs
    # vgs
    # lvs
    ```
    - Giả sử `lv-demo1` đã đầy và cần tăng kích thước .
    - Để tăng kích thước , phải kiểm tra xem **volume group** còn dư dung lượng để kéo giãn **logical volume** không . **Logical volume** thuộc 1 **volume group** nhất định , nếu **volume group** đã cấp phát hết thì **logical volume** cũng không tăng dung lượng lên được .
    - Để kiểm tra , dùng lệnh `vgdisplay`
    - Chú ý 2 trường thông tin :
        - "`VG Status     resizeable`"  => có thể co dãn được
            <img src=https://i.imgur.com/SrFM4pJ.png>
        - "`Free PE / Size    3836 /14.98 GiB`"   
            <img src=https://i.imgur.com/2xRx0ny.png>  


- **B2.1 :** Tăng kích thước dung lượng **logical volume** :
    ```
    # lvextend -L +5G /dev/vg-demo1/lv-demo1
    ```
    - `-L` : tùy chọn để thay đổi kích thước
    - Sau khi tăng kích thước **logical volume** thì dung lượng đã được tăng nhưng file system trên **volume** này vẫn chưa thay đổi :
        ```
        # resize2fs /dev/vg-demo1/lv-demo1
        ```
- **B2.2 :** Giảm kích thước **logical volume** :
    - Trước tiên phải unmount **logical volume** muốn giảm :
        ```
        # umount /dev/vg-demo1/lv-demo1
        ```
    - Giảm kích thước của **logical volume** :
        ```
        # lvreduce -L 5G /dev/vg-demo1/lv-demo1
        ```
    - Format lại **logical volume** :
        ```
        # mkfs.xfs /dev/vg-demo1/lv-demo1
        ```
    - Mount lại **logical volume** :
        ```
        # mount /dev/vg-demo1/lv-demo1 demo1
        ```
- **B3 :** Kiểm tra kết quả :
    ```
    # df -h
    ```
## **6) Cách thay đổi dung lượng Volume Group trên LVM**
- Chính là việc nhóm thêm **physical volume** hay bỏ nhóm **physical volume** ra khỏi **volume group** .
- **B1 :** Kiểm tra thông tin partition :
    ```
    # vgdisplay
    ```
    hoặc
    ```
    # lsblk
    ```
- **B2.1 :** Nhóm thêm 1 partition vào **volume group** :
    ```
    # vgextend /dev/vg-demo1 /dev/sdb2
    ```
    ( hệ thống sẽ tự động chuyển `/dev/sdb2` thành **physical volume** )
- **B2.2 :** Cắt 1 partition ra khỏi **volume group**
    ```
    # vgreduce /dev/vg-demo1 /dev/sdb2
    ```
## **7) Cách xóa Logical Volumes , Volume Group , Physical Volume**
### **7.1) Xóa Logical Volume**
- **B1 :** Unmount **logical volume** :
    ```
    # umount /dev/vg-demo1/lv-demo1
    ```
- **B2 :** Xóa **logical volume** :
    ```
    # lvremove /dev/vg-demo1/lv-demo1
    ```
### **7.2) Xóa Volume Group**
- Trước khi xóa **volume group** , phải đảm bảo xóa hết **logical volume** :
- Xóa **volume group** bằng lệnh :
    ```
    # vgremove /dev/vg-demo1
    ```
### **7.3) Xóa Physical Volume**
- ```
  # pvremove /dev/sdb2
  ```


