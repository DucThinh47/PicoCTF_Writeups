# Picker II

![img](51)

Thử kết nối chương trình thông qua `netcat`:

![img](52)

Không có gì. Download source code: 

![img](53)3

Xem source code file picker-II.py: 

![img](54)

![img](55)

![img](56)

Hàm main: 

![img](57)

Thử kết nối lại `netcat` và gọi hàm `win`trực tiếp: 

![img](58)

Thử gọi hàm tên các hàm trực tiếp: 

![img](59)

=> Hàm `win` là hàm hợp lệ. Xem lại hàm nội dung hàm `win`, để ý dòng `flag = open('flag.txt', 'r').read()`, kết nối `netcat` và thử dùng lệnh `print()`: 

![img](60)

=> **Flag: picoCTF{f1l73r5_f41l_c0d3_r3f4c70r_m1gh7_5ucc33d_0b5f1131}**




