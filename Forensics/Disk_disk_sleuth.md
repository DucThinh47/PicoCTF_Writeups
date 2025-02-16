# Disk, disk, sleuth!
![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image.png?raw=true)

### Solution

Dùng lệnh **wget** để tải file disk image: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image1.png?raw=true)

Kiểm tra file format: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image2.png?raw=true)

=> là file **gzip** (.gz). DÙng **gunzip** để giải nén:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image3.png?raw=true)

=> Sử dụng lệnh **mmls dds2-alpine.flag.img** để hiển thị cấu trúc phân vùng của disk image:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image4.png?raw=true)

=> Có thể phân vùng 2 là phân vùng chính **(desc: Linux(0x83))** vì bắt đầu từ sector lớn nhất và có độ dài không đều!

=> Thử lệnh **fls -o 2048 dds2-alpine.flag.img** để liệt kê các tệp tin và thư mục trong disk image dds2-alpine.flag (chưa thêm phân vùng đã chọn), tùy chọn -o để chỉ định dữ liệu phân vùng bắt đầu tại sector 2048:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image5.png?raw=true)

=> Kiểm tra thư mục home, liệt kê các tệp tin và thư mục trong thư mục /home có inode là 26417: fls -o 2048 dds2-alpine.flag.img 26417:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image6.png?raw=true)

=> Không có gì.

Kiểm tra, liệt kê các tệp tin và thư mục trong thư mục /root có inode là 18290:  **fls -o 2048 dds2-alpine.flag.img 18290**:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image7.png?raw=true)

=> Phát hiện file text: **down-at-the-bottom.txt**

Dùng lệnh **icat -o 2048 dds2-alpine.flag.img 18291** để đọc file (18291 là inode của file text):

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image8.png?raw=true)

**Flag: picoCTF{f0r3ns1c4t0r_n0v1c3_0d9d9ecb}**



