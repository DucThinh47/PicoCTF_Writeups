# Picker I

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image42.png?raw=true)

Kết nối chương trình với netcat: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image43.png?raw=true)

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image44.png?raw=true)

Download source code: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image45.png?raw=true)

Xem source code:

Hàm main:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image46.png?raw=true)

Hàm getRandomNumber: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image47.png?raw=true)

Hàm win:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image48.png?raw=true)

Kết nối lại netcat, lần này thay vì nhập hàm getRandomNumber, thử nhập hàm win: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image49.png?raw=true)

=> Tìm được giá trị được mã hóa dưới dạng hex, thử giải mã bằng Cyberchef: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image50.png?raw=true)

=> **Flag: picoCTF{4_d14m0nd_1n_7h3_r0ugh_ce4b5d5b}**

