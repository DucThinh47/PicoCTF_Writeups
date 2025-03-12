# Picker III

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image61.png?raw=true)

Download và xem source code: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image62.png?raw=true)

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image63.png?raw=true)

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image64.png?raw=true)

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image65.png?raw=true)

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image66.png?raw=true)

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image67.png?raw=true)

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image68.png?raw=true)

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image69.png?raw=true)

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image70.png?raw=true)

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image71.png?raw=true)

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image72.png?raw=true)

Khi chạy chương trình, có thể chọn 4 hàm để thực thi:

- Hàm `print_table` sẽ in ra bảng function hiện tại.  

- Hàm `read_variable` sẽ nhận giá trị của biến.

- Hàm `write_variable` sẽ cho phép ghi lên bất kỳ biến nào. 

- Hàm `getRandomNumber` lại in ra 1 số ngẫu nhiên. 

=> Ở đầu source code, có thể thấy các hàm mà người dùng có thể gọi được lưu trong một biến tên là `func_table`, với:

- Số lượng hàm: 4
- Độ dài chuỗi hàm: 32 ký tự

    USER_ALIVE = True
    FUNC_TABLE_SIZE = 4
    FUNC_TABLE_ENTRY_SIZE = 32

=> Có thể thực hiện khai thác bằng cách ghi đè giá trị của biến `func_table` để mọi hàm đều trở thành `win`.

=> Ghi đè biến `func_table` với nội dung `'win                             win                             win                             win                             '`

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image73.png?raw=true)

Sau khi thực hiện ghi đè, bảng hàm mới sẽ trông như sau:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image74.png?raw=true)

=> Tìm ra chuỗi hex: `0x70 0x69 0x63 0x6f 0x43 0x54 0x46 0x7b 0x37 0x68 0x31 0x35 0x5f 0x31 0x35 0x5f 0x77 0x68 0x34 0x37 0x5f 0x77 0x33 0x5f 0x67 0x33 0x37 0x5f 0x77 0x31 0x37 0x68 0x5f 0x75 0x35 0x33 0x72 0x35 0x5f 0x31 0x6e 0x5f 0x63 0x68 0x34 0x72 0x67 0x33 0x5f 0x61 0x31 0x38 0x36 0x66 0x39 0x61 0x63 0x7d` 

=> Sử dụng CyberChef giải mã chuỗi này: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image75.png?raw=true)

=> **Flag: picoCTF{7h15_15_wh47_w3_g37_w17h_u53r5_1n_ch4rg3_a186f9ac}**




