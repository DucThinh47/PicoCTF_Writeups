# OperationOrchid
![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image9.png?raw=true)

### Solution

Dùng **wget** tải file disk image: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image10.png?raw=true)

Kiểm tra file format và giải nén: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image11.png?raw=true)

=> sử dụng lệnh **mmls disk.flag.img** để xem định dạng phân vùng của ảnh đĩa: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image12.png?raw=true)

=> Nhận thấy phân vùng có desc là Linux (0x83) có start dài nhất và length ko đồng đều, có thể là phân vùng chính: 

=> dùng lệnh **fls -o 411648 disk.flag.img** để liệt kê tệp tin và thư mục trong disk image disk.flag (chưa thêm phân vùng), tùy chọn -o để chỉ định dữ liệu phân vùng bắt đầu từ sector thứ 411648: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image13.png?raw=true)

=> thư mục home có phân vùng 460, root có phân vùng 472, kiểm tra 2 thư mục này: 

=> dùng lệnh **fls -o 411648 disk.flag.img 460** để liệt kê tệp tin và thư mục trong /home: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image14.png?raw=true)

=> Không có gì

=> dùng lệnh **fls -o 411648 disk.flag.img 472** để liệt kê tệp tin và thư mục trong /root: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image15.png?raw=true)

=> Sử dụng lệnh **icat -o 411648 disk.flag.img 1875** (inode của file .ash_history) để đọc nội dung file .ash_history (lệnh icat sẽ trích xuất file từ ảnh đĩa - trích xuất dữ liệu từ ảnh đĩa disk.flag.img tại offset 411648 và inode 1875):

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image16.png?raw=true)

=> Output ko phải 1 file .txt thông thường, mà là 1 chuỗi các lệnh shell:

- touch flag.txt: tạo 1 file rỗng tên là flag.txt

- nano flag.txt: mở trình soạn thảo để chỉnh sửa file

- apk get nano: cài đặt nano

- apk –help: hiển thị hướng dẫn sử dụng apk

- apk add nano: cài đặt nano

- nano flag.txt: mở lại file flag.txt để chỉnh sửa

- openssl: khởi động công cụ mã hóa ssl

- openssl aes256 -salt -in flag.txt -out flag.txt.enc -k 

- unbreakablepassword1234567: mã hóa nội dung file flag.txt bằng thuật toán AES-256 với mật khẩu và lưu kết quả vào file flag.txt.enc

- ls -al: liệt kê các file với nội dung chi tiết

- halt: Tắt hệ thống

=> Có thể thấy rằng file flag.txt đã được mã hóa thành file flag.txt.enc với mật khẩu

=> Sử dụng lệnh **icat -o 411648 disk.flag.img 1782 > flag.txt.enc** (1782 là inode của file flag.txt.enc) để trích xuất nội dung file flag.txt.enc từ disk image và lưu nội dung file này vào file flag.txt.enc ở thư mục hiện tại:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image17.png?raw=true)

=> Sử dụng lệnh **openssl aes256 -d -in flag.txt.enc -out decrypt.txt** để giải mã nội dung file flag.txt.enc, nội dung mới được lưu vào file decrypt.txt 
(openssl: tool dùng để bảo mật, mã hóa, giải mã và chứng thực; aes256: thuật toán giải mã được sử dụng trong lệnh; -d: tùy chọn decrypt; -in: chỉ định file đầu vào; -out: chỉ định file đầu ra)

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image18.png?raw=true)

Sau khi giải mã, đây là nội dung file decrypt.txt: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image19.png?raw=true)




