# Picker II

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image51.png?raw=true)

Thử kết nối chương trình thông qua `netcat`:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image52.png?raw=true)

Không có gì. Download source code: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image53.png?raw=true)

Xem source code file picker-II.py: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image54.png?raw=true)

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image55.png?raw=true)

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image56.png?raw=true)

Hàm main: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image57.png?raw=true)

Thử kết nối lại `netcat` và gọi hàm `win`trực tiếp: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image58.png?raw=true)

Thử gọi hàm tên các hàm trực tiếp: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image59.png?raw=true)

=> Hàm `win` là hàm hợp lệ. Xem lại hàm nội dung hàm `win`, để ý dòng `flag = open('flag.txt', 'r').read()`, kết nối `netcat` và thử dùng lệnh `print()`: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image60.png?raw=true)

=> **Flag: picoCTF{f1l73r5_f41l_c0d3_r3f4c70r_m1gh7_5ucc33d_0b5f1131}**




