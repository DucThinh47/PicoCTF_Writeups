# WinAntiDbg0x200

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image21.png?raw=true)

Download challenge: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image22.png?raw=true)

Giải nén file: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image23.png?raw=true)

Đọc nội dung file config.bin: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image24.png?raw=true)

=> Không thu được gì.

Thử chạy file .exe: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image25.png?raw=true)

Cần sử dụng debugger để chạy chương trình. Mở IDA:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image26.png?raw=true)

=> Tìm ra đoạn mã dẫn đến thông báo *Oops! The debugger was detected. Try to bypass this check to get the flag!*:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image27.png?raw=true)

Thử thay đổi ***jnz*** thành ***jz*** và chạy lại chương trình xem có còn nhảy đến hàm thông báo *Oops! The debugger was detected. Try to bypass this check to get the flag!* nữa không: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image28.png?raw=true)

Kết quả: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image29.png?raw=true)

Không được. Khi thay đổi ***jnz*** thành ***jz*** ở hàm loc_FE1807, hàm đã không nhảy đếm hàm thông báo *Oops! The debugger was detected. Try to bypass this check to get the flag!*, thay vào đó nhảy đến hàm này:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image30.png?raw=true)

Hàm này lại một lần nữa kiểm tra xem tiến trình hiện tại có đang chạy dưới một debugger hay không. Từ hàm này có thể nhảy đến hàm thông báo flag: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image31.png?raw=true)

Hàm này sẽ kiểm tra nếu thanh ghi EAX = 0 thì nhảy đến hàm loc_241847, dẫn đến hàm thông báo flag, thử thay đổi ***jz*** thành ***jnz***: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image32.png?raw=true)

Lưu và chạy lại chương trình: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image33.png?raw=true)

=> **Flag: picoCTF{0x200_debug_f0r_Win_7853c59f}**


