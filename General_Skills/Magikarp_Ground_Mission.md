# Magikarp Ground Mission

### Solution

ssh tới server của pico: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/General_Skills/images/image17.png?raw=true)

Mật khẩu đã được pico cung cấp

=> giải thích lệnh ssh ctf-player@venus.picoctf.net -p 57077 - khởi tạo 1 kết nối ssh (secure shell) đến 1 server từ xa.

- ssh: khởi tạo phiên ssh (ssh - giao thức mạng giúp thực hiện hoạt động như login từ xa, thực hiện lệnh trên server từ xa 1 cách an toàn, bảo mật)

- ctf-player: tên username trên server từ xa

- @venus.picoctf.net: tên miền (hoặc ip) của server

-p 57077: chỉ định cổng (port) sử dụng cho ssh, mặc định ssh sử dụng cổng 22, lần này sử dụng 57077

Sử dụng lệnh ls để liệt kê file/folder và lệnh cat để đọc file .txt:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/General_Skills/images/image18.png?raw=true)

=> Thu được đoạn đầu flag: picoCTF{xxsh_

Sử dụng lệnh cd / để tới thư mục root trên server: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/General_Skills/images/image19.png?raw=true)

=> ls và cat, đọc được phần giữa của flag: 0ut_0f_\/\/4t3r_

Sử dụng lệnh cd ~ để trở về thư mục home trên server: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/General_Skills/images/image20.png?raw=true)

=> ls và cat, lấy được phần cuối của flag: 71be5264}

**flag:  picoCTF{xxsh_0ut_0f_\/\/4t3r_71be5264}**


