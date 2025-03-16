# GDB baby step 3

![img](91)

Download file cần debug: 

![img](92)

Kiểm tra `file format`: 

![img](93)

=> Là một file `ELF` (Executable and Linkable Format), file thực thi trên Linux.

Khởi chạy `GDB`: 

![img](94)

Chuyển đổi cú pháp sang `Intel`:

![img](95)

Xem danh sách các hàm trong mã: 

![img](96)

Cần phân tích hàm `main`. `Disassemble` hàm main để xem mã assembly:

![img](97)

Chú ý lệnh chuyển giá trị vào vị trí nhớ:

    mov    DWORD PTR [rbp-0x4],0x2262c96b

Giá trị `0x2262c96b` được lưu tại địa chỉ `[rbp-0x4]`.

Thiết lập `breakpoint` tại hàm `main`:

![img](98)

Chạy chương trình:

![img](99)

Quan sát mã lắp ráp trực tiếp khi thực thi, sử dụng lệnh 
    
    layout asm

![img](100)

Quan sát chương trình tạm dừng tại `breakpoint`, có thể thấy giá trị cuối cùng trong `thanh ghi EAX` được tìm nạp từ bộ nhớ. Để xác nhận, đặt một `breakpoint` khác tại địa chỉ lệnh `0x40111f` (trong đó `EAX` được đặt): 

![img](101)

Tiếp tục chạy chương trình:

![img](102)

Xem giá trị của `thanh ghi EAX`:

![img](103)

Giá trị này không phải là flag. Vì chương trình sử dụng định dạng `little-endian` (byte được lưu theo thứ tự ngược), cần kiểm tra nội dung tại địa chỉ `[rbp-0x4]` (theo mô tả thử thách):

![img](104)

=> **Flag: picoCTF{0x6bc96222}**