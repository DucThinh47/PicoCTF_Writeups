# Disk, disk, sleuth!
![img](0)

### Solution

Dùng lệnh **wget** để tải file disk image: 

![img](1)

Kiểm tra file format: 

![img](2)

=> là file **gzip** (.gz). DÙng **gunzip** để giải nén:

![img](3)

=> Sử dụng lệnh **mmls dds2-alpine.flag.img** để hiển thị cấu trúc phân vùng của disk image:

![img](4)

=> Có thể phân vùng 2 là phân vùng chính **(desc: Linux(0x83))** vì bắt đầu từ sector lớn nhất và có độ dài không đều!

=> Thử lệnh **fls -o 2048 dds2-alpine.flag.img** để liệt kê các tệp tin và thư mục trong disk image dds2-alpine.flag (chưa thêm phân vùng đã chọn), tùy chọn -o để chỉ định dữ liệu phân vùng bắt đầu tại sector 2048:

![img](5)

=> Kiểm tra thư mục home, liệt kê các tệp tin và thư mục trong thư mục /home có inode là 26417: fls -o 2048 dds2-alpine.flag.img 26417:

![img](6)

=> Không có gì.

Kiểm tra, liệt kê các tệp tin và thư mục trong thư mục /root có inode là 18290:  **fls -o 2048 dds2-alpine.flag.img 18290**:

![img](7)

=> Phát hiện file text: **down-at-the-bottom.txt**

Dùng lệnh **icat -o 2048 dds2-alpine.flag.img 18291** để đọc file (18291 là inode của file text):

![img](8)

**Flag: picoCTF{f0r3ns1c4t0r_n0v1c3_0d9d9ecb}**



