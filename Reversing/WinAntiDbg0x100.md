# WinAntiDbg0x100

![img](8)

# Solution

Download file: 

![img](9)

Giải nén file này: 

![img](10)

Xem thử nội dung file *config.bin*: 

![img](11)

=> Nội dung có vẻ đã được mã hóa, không thu được gì. 

Thử chạy file *WinAntiDbg0x100.exe*:

![img](12)

=> File yêu cầu chạy trên 1 debugger. 

Sử dụng IDA để chạy file .exe này: 

![img](13)

Xem qua hàm main, tìm được code kiểm tra debugger có hoạt động không: 

![img](14)

![img](15)

Phân tích: 

- ***call ds:OutputDebugStringW***: Gửi một chuỗi ký tự ra Debugger (trình gỡ lỗi) nếu có. Nếu không có trình gỡ lỗi, hàm này không tạo ra bất kỳ kết quả nào.
- ***call ds:IsDebuggerPresent***: Kiểm tra xem chương trình có đang chạy trong trình gỡ lỗi không. Thanh ghi ***EAX = 1*** nếu đang chạy trong trình gỡ lỗi, ngược lại = 0 nếu không có trình gỡ lỗi. 
- ***test eax, eax***: Kiểm tra xem giá trị của EAX có bằng 0 không.
- ***jz (Jump if Zero) short loc_D161B***: Nếu EAX = 0, nhảy đến loc_D161B.

Trong TH này, giá trị thanh ghi EAX **đang không bằng 0**, nên chương trình dừng ở đây:

![img](16)

Nếu có thể jump đến loc_D161B thì sẽ lấy được flag:

![img](17)

=> Cần thay đổi mã để nhảy đến loc_D161B ngay cả khi thanh ghi EAX không bằng 0. Click vào dòng ***jz short loc_D161B*** => Edit => Patch program => Change byte...: 

![img](18)

Chuyển giá trị Hex từ 0x74 (jz) thành 0x75 (jnz - jump if not zero):

![img](19)

Click OK => Edit => Patch program => Apply patches to input file để áp dụng thay đổi. Sau đó chạy lại chương trình: 

![img](20)

=> **Flag: picoCTF{d3bug_f0r_th3_Win_0x100_cc0ff664}**









