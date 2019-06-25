# SSH - Secure Shell
## **1) Khái niệm**
- **SSH - Secure Shell** là một giao thức mạng dùng để thiết lập kết nối mạng một cách bảo mật .
- **SSH** có sử dụng cơ chế mã hoá đủ mạnh nhằm ngăn chặn các hiện tượng nghe trộm , đánh cắp thông tin trên đường truyền . Các giao thức trước đây như **rlogin** , **telnet** không hỗ trợ mã hóa .
- **SSH** hoạt động ở tầng **Application** trong mô hình **TCP/IP**
- Các công cụ **SSH** ( như là **OpenSSH**,...) cung cấp cho người dùng cách thức để thiết lập kết nối mạng được mã hoá để tạo một kênh kết nối riêng tư .
## **2) Cấu trúc lệnh SSH**

## **3) Các kỹ thuật mã hóa**
- Một lợi thế quan trọng của **SSH** so với các giao thức tiền nhiệm trước là nó sử dụng mã hóa để đảm bảo kết nối bảo mật giữa ***host*** và ***client*** .
    - ***Host*** là server đầu xa .
    - ***Client*** là máy tính đang thực hiện truy cập từ xa vào ***host*** .
- Có 3 công nghệ mã hóa khác nhau được sử dụng trong **SSH** là : ***symmetric encryption*** , ***asymmetric encryption*** và ***hashing*** .
### **3.1) Symmetric Encryption ( *mã hóa đối xứng* )**
- **Symmetric Encryption** là 1 dạng mã hóa mà sử dụng 1 **secret key** cho cả việc mã hóa và giải mã gói tin bởi ***host*** và ***client*** . Do vậy , bất cứ ai có được **secret key** đều có thể giải mã gói tin .

    <img src=https://i.imgur.com/54cAASp.png>

- **Symmetric Encryption** thường được gọi là **shared-key** hoặc **shared-secret** encryption . Thường chỉ có 1 **key** được sử dụng hoặc đôi khi là 1 cặp **key** trong khi từ **key** này có thể tính toán được ra **key** kia .
- **Symmetric keys** được sử dụng để mã hóa toàn bộ giao tiếp giữa 2 máy trong suốt phiên **SSH** . Cả server và client truy gốc **secret key** bằng việc sử dụng 1 thuật toán đồng nhất , và **key** sau khi được tổng hợp sẽ không được tiết lộ với bất kỳ bên thứ 3 nào . Quá trình tạo ra **symmetric key** được tiến hành bởi *thuật toán trao đổi key mã hóa* ( ***key exchange algorithm*** ) . Điều làm cho thuật toán này đặc biệt an toàn là **key** không bao giờ được trao đổi giữa ***client*** và ***host*** . Thay vào đó , 2 máy tính trao đổi 1 phần dữ liệu công khai và sau đó thực hiện tính toán độc lập ra **secret key** . Kể cả khi có 1 máy khác bắt được phần dữ liệu công khai này , nó cũng không thể tính toán được **secret key** bởi không biết *thuật toán trao đổi key* .
- Có rất nhiều thuật toán mã hóa ***( cypher )*** tồn tại , bao gồm **AES ( Advanced Encryption Standard )** , **CAST128** , **Blowfish** ,... Trước khi thiết lập một kết nối bảo mật , ***client*** và ***host*** sẽ quyết định xem dùng thuật toán mã hóa nào bằng cách xuất ra 1 danh sách các thuật toán mã hóa được hỗ trợ theo thứ tự ưu tiên . Thuật toán được ưu tiên nhất từ phía ***client*** và có xuất hiện trong danh sách thuật toán hỗ trợ của ***host*** sẽ được sử dụng làm thuật toán mã hóa chung . <br>**VD :** Ubuntu 14.04 LTS khi thực hiện kết nối với máy khác thông qua **SSH** , chúng sẽ sử dụng **aes128-ctr** là thuật toán mặc định .
### **3.2) Asymmetric Encryption ( *mã hóa bất đối xứng* )**
- Không giống **symmectric encryption** , **asymmetric encryption** sử dụng 2 **key** riêng biệt để mã hóa và giải mã . Hai loại **key** được biết đến với tên gọi là **public key** và **private key** . Cùng với nhau , chúng tạo thành 1 cặp **key-pair** .

    <img src=https://i.imgur.com/BjpfaFG.png>

- **Public key** được phân phối và chia sẻ với các bên khác . Trong khi nó được liên kết với **private key** theo 1 cách nào đó , **private key** không thể bị tính toán ra từ **public key** . Mối quan hệ giữa 2 loại **key** này khá phức tạp : gói tin được mã hóa bởi **public key** của máy nào thì chỉ được giải mã bằng chính **private key** của nó . Mối quan hệ 1 chiều này có nghĩa **public key** không thể giải mã gói tin của chính nó , và cũng không thể giải mã bất cứ thứ gì được mã hóa bởi **private key** .
- **Private key** sẽ ở lại với máy chủ của nó để kết nối được bảo mật , sẽ không có bên thứ 3 nào biết nó .
- Điều làm cho kết nối được an toàn giữa 2 máy là việc **private key** sẽ không bao giờ bị lộ , đó là khả năng duy nhất để giải mã gói tin sử dụng **public key** của nó . Do đó , bất kỳ bên nào có khả năng giải mã gói tin public phải sở hữu **private key** tương ứng .
- **Asymmetric encryption** không được sử dụng để mã hóa toàn bộ phiên **SSH** . Thay vào đó , nó chỉ được sử dụng trong *thuật toán trao đổi key* của **symmetric encryption** . Trước khi khởi tạo 1 kết nối bảo mật , cả 2 bên tạo ra 1 cặp **public-private keys** tạm thời , sau đó chia sẻ **private keys** của chúng để tạo ra **shared secret key** .
- Trong khi quá trình **symmetric** được thiết lập , server sẽ sử dụng **public key** của client để tạo ra và thử thách và truyền nó tới client để xác thực . Nếu client có thể giải mã thành công gói tin , có nghĩa là nó giữ **private key** được yêu cầu cho kết nối <br>**=>** Phiên làm việc **SSH** được bắt đầu .
### **3.3) Hashing**
- **Hashing** một chiều là 1 dạng mã hóa khác được sử dụng bởi **SSH** .
- Chức năng của chúng khác với 2 dạng mã hóa trên theo hướng rằng chúng sẽ không bao giờ bị giải mã .
- Chúng tạo ra 1 giá trị độc nhất cho phần đuôi được thêm vào của mỗi đoạn dữ liệu khiến chúng không thể bị khai thác . Điều này khiến **hash** hoàn toàn không thể bị đảo ngược

    <img src=https://i.imgur.com/S97o5ia.png>

- Có thể dễ dàng **hash** một đầu vào cho sẵn , nhưng không thể giải mã được đầu vào bị **hash** đó . Điều này có nghĩa nếu 1 client giữ đoạn dữ liệu chính xác , nó có thể giải mã **hash** và so sánh các giá trị của chúng để xác thực rằng liệu chúng có sở hữu dữ liệu chuẩn hay không .
- **SSH** sử dụng **hash** để đảm bảo tính xác thực của các gói tin . Nó được thực hiện bằng cách sử dụng **HMACs ( Hash-based Message Authentication Codes )** . Điều này đảm bảo các lệnh mà máy đầu xa nhận được sẽ không bị giả mạo theo bất cứ cách nào .
- Trong quá trình lựa chọn thuật toán mã hóa **symmetric** , một thuật toán xác thực gói tin cũng sẽ được lựa chọn theo đúng cách lựa chọn **symmetric** .
- Mỗi gói tin được truyền đi phải chứa đựng 1 **MAC** được tính toán dựa trên **symmetric key** , **packet sequence number** và nội dung gói tin .
### **3.4) Cách kết hợp cả 3 công nghệ trên của SSH**
- **SSH** sử dụng mô hình **Client-Server** để cho phép xác thực giữa 2 máy đầu xa và mã hóa dữ liệu truyền qua chúng .
- **SSH** hoạt động mặc định trên port `22` của giao thức **TCP** ( có thể thay đổi được )
- ***Host*** ( server ) lắng nghe trên port `22` chờ kết nối đến .
- ***Host*** sẽ thiết lập kết nối bảo mật bằng việc xác thực ***client*** và mở 1 môi trường **shell** tương ứng khi việc xác thực thành công .

    <img src=https://i.imgur.com/oiBeWh5.png>

- ***Client*** sẽ phải bắt đầu kết nối **SSH** bằng việc bắt tay 3 bước **TCP** với ***host*** , đảm bảo kết nối bảo mật **symmetric** , xác thực định danh trong server đúng với các bản ghi trước ( thường đuợc lưu trữ trong file **RSA** ) , và trình diện đúng user được ủy quyền trên ***host*** để xác thực phiên kết nối .