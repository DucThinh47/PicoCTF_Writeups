# GDB baby step 3

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image91.png?raw=true)

Download file cần debug: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image92.png?raw=true)

Kiểm tra `file format`: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image93.png?raw=true)

=> Là một file `ELF` (Executable and Linkable Format), file thực thi trên Linux.

Khởi chạy `GDB`: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image94.png?raw=true)

Chuyển đổi cú pháp sang `Intel`:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image95.png?raw=true)

Xem danh sách các hàm trong mã: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image96.png?raw=true)

Cần phân tích hàm `main`. `Disassemble` hàm main để xem mã assembly:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image97.png?raw=true)

Chú ý lệnh chuyển giá trị vào vị trí nhớ:

    mov    DWORD PTR [rbp-0x4],0x2262c96b

Giá trị `0x2262c96b` được lưu tại địa chỉ `[rbp-0x4]`.

Thiết lập `breakpoint` tại hàm `main`:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image98.png?raw=true)

Chạy chương trình:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image99.png?raw=true)

Quan sát mã lắp ráp trực tiếp khi thực thi, sử dụng lệnh 
    
    layout asm

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image100.png?raw=true)

Quan sát chương trình tạm dừng tại `breakpoint`, có thể thấy giá trị cuối cùng trong `thanh ghi EAX` được tìm nạp từ bộ nhớ. Để xác nhận, đặt một `breakpoint` khác tại địa chỉ lệnh `0x40111f` (trong đó `EAX` được đặt): 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image101.png?raw=true)

Tiếp tục chạy chương trình:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image102.png?raw=true)

Xem giá trị của `thanh ghi EAX`:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image103.png?raw=true)

Giá trị này không phải là flag. Vì chương trình sử dụng định dạng `little-endian` (byte được lưu theo thứ tự ngược), cần kiểm tra nội dung tại địa chỉ `[rbp-0x4]` (theo mô tả thử thách):

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image104.png?raw=true)

=> **Flag: picoCTF{0x6bc96222}**