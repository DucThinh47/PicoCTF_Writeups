# Sleuthkit Intro
![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image24.png?raw=true)

### Solution

Sử dụng lệnh wget để tải disk image vào thư mục tmp: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image25.png?raw=true)

Sử dụng lệnh gunzip disk.img.gz để giải nén file gzip:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image26.png?raw=true)

Sử dụng lệnh mmls disk.img:
mmls: là lệnh hiển thị cấu trúc phân vùng hình ảnh đĩa

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image27.png?raw=true)

Sử dụng lệnh nc saturn.picoctf.net 60561:
nc (netcat): kết nối mạng từ máy tính của đến một máy chủ từ xa, problem là truy nhập chương trình kiểm tra

saturn.picoctf.net: tên miền

60561: port

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image28.png?raw=true)

**Flag: picoCTF{mm15_f7w!}**



