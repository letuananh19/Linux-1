# File System Format
- **File system** là những thành phần quan trọng trong 1 hệ điều hành , được sử dụng để điều khiển cách dữ liệu được lưu trữ và truy vấn...
- Linux là hệ điều hành có khả năng hỗ trợ nhiều loại **file system** nhất hiện nay
## **Cơ chế Jounaling**
## **Các kiểu file system trên CentOS 7**
### **1) `xfs`**       <img src=https://i.imgur.com/vJhHWfd.jpg align=right width=30%>
- Đây là **file system** mặc định trên CentOS 7
- Được phát triển bởi ***Silicon Graphics*** từ nằm 1993
- Có các đặc điểm :
    - Hạn chế được tình trạng phân mảnh dữ liệu
    - Hỗ trợ file system lên đến `6 exabytes` tương đương `18 triệu terabytes`
    - Hỗ trợ file lên đến `8 exabyte`
    - Cấu trúc thư mục chứa hàng chục triệu đối tượng
    - Kích thước tối đa của 1 `xfs partition` là `500TB`
### **2) `ext4`**
- File system này ra đời từ phiên bản `2.6.28` của Linux kernel ( 25-12-2008 )
- Nó kế thừa và phát huy các điểm mạnh của `ext3` , đồng thời `ext4` có thể giảm bớt hiện tượng phân mảnh dữ liệu trong ổ cứng , hỗ trợ file system và file có dung lượng lớn và có tốc độ hoạt động nhanh .
- Hỗ trợ kích thước tối đa của 1 file system `ext4` trên CentOS là `50TB` .
