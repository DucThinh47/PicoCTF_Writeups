# WinAntiDbg0x200

![img](21)

Download challenge: 

![img](22)

Giải nén file: 

![img](23)

Đọc nội dung file config.bin: 

![img](24)

=> Không thu được gì.

Thử chạy file .exe: 

![img](25)

Cần sử dụng debugger để chạy chương trình. Mở IDA:

![img](26)

=> Tìm ra đoạn mã dẫn đến thông báo *Oops! The debugger was detected. Try to bypass this check to get the flag!*:

![img](27)

Thử thay đổi ***jnz*** thành ***jz*** và chạy lại chương trình xem có còn nhảy đến hàm thông báo *Oops! The debugger was detected. Try to bypass this check to get the flag!* nữa không: 

![img](28)

Kết quả: 

![img](29)

Không được. Khi thay đổi ***jnz*** thành ***jz*** ở hàm loc_FE1807, hàm đã không nhảy đếm hàm thông báo *Oops! The debugger was detected. Try to bypass this check to get the flag!*, thay vào đó nhảy đến hàm này:

![img](30)

Hàm này lại một lần nữa kiểm tra xem tiến trình hiện tại có đang chạy dưới một debugger hay không. Từ hàm này có thể nhảy đến hàm thông báo flag: 

![img](31)

Hàm này sẽ kiểm tra nếu thanh ghi EAX = 0 thì nhảy đến hàm loc_241847, dẫn đến hàm thông báo flag, thử thay đổi ***jz*** thành ***jnz***: 

![img](32)

Lưu và chạy lại chương trình: 

![img](33)

=> **Flag: picoCTF{0x200_debug_f0r_Win_7853c59f}**


