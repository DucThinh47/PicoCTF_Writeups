# Sleuthkit Intro
![img](24)

### Solution

Sử dụng lệnh wget để tải disk image vào thư mục tmp: 

![img](25)

Sử dụng lệnh gunzip disk.img.gz để giải nén file gzip:

![img](26)

Sử dụng lệnh mmls disk.img:
mmls: là lệnh hiển thị cấu trúc phân vùng hình ảnh đĩa

![img](27)

Sử dụng lệnh nc saturn.picoctf.net 60561:
nc (netcat): kết nối mạng từ máy tính của đến một máy chủ từ xa, problem là truy nhập chương trình kiểm tra

saturn.picoctf.net: tên miền

60561: port

![img](28)

**Flag: picoCTF{mm15_f7w!}**



