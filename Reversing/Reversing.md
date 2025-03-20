# Reversing

## Content

- [Safe Opener 2](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/Reversing.md#safe-opener-2)

- [Transformation](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/Reversing.md#transformation)

- [WinAntiDbg0x100](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/Reversing.md#winantidbg0x100)

- [WinAntiDbg0x200](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/Reversing.md#winantidbg0x200)

- [packer](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/Reversing.md#packer)

- [Picker I](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/Reversing.md#picker-i)

- [Picker II](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/Reversing.md#picker-ii)

- [Picker III](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/Reversing.md#picker-iii)

- [GDB baby step 1](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/Reversing.md#gdb-baby-step-1)

- [GDB baby step 2](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/Reversing.md#gdb-baby-step-2)

- [GDB baby step 3](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/Reversing.md#gdb-baby-step-3)

- [GDB baby step 4](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/Reversing.md#gdb-baby-step-4)

- [ASCII FTW](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/Reversing.md#ascii-ftw)

- [timer](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/Reversing.md#timer)

### GDB baby step 1

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image76.png?raw=true)

Download file: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image77.png?raw=true)

Kiểm tra định dạng file: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image78.png?raw=true)

=> Là một file `Executable and Linkable Format (ELF)`. 

Sử dụng `gdb` - một công cụ giúp dịch ngược mã. Nghĩa là chia một chương trình thành các phần nhỏ hơn, một tính năng tiện dụng khi làm việc với các ngôn ngữ cấp thấp hoặc kiểm tra các chương trình biên dịch:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image79.png?raw=true)

Sử dụng lệnh `info functions` để xem danh sách tất cả các hàm có trong file:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image80.png?raw=true)

=> Xác nhận file này không bị `stripped` (tức là chưa bị xóa các ký hiệu gỡ lỗi), tất cả functions đều được hiển thị rõ ràng. 

Chú ý vào hàm `main` có địa chỉ là `0x1129`. 

GDB mặc định sử dụng cú pháp AT&T để hiển thị mã lệnh Assembly. Nếu quen với cú pháp Intel hơn, có thể chuyển đổi bằng lệnh `set disassembly-flavor intel`

Tiến hành dịch ngược hàm `main` để xem mã Assembly của nó, sử dụng lệnh `disassemble main`:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image81.png?raw=true)

Theo mô tả thử thách, cần khám phá giá trị thanh ghi `EAX`.

Dựa vào dòng `mov eax, 0x86342`, có nghĩa là giá trị `0x86342` được chuyển vào thanh ghi `EAX`.

Mở 1 terminal khác và in ra giá trị integer của `0x86342`: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image82.png?raw=true)

=> **Flag: picoCTF{549698}**

### GDB baby step 2

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image83.png?raw=true)

Download file: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image84.png?raw=true)

Kiểm tra file format: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image85.png?raw=true)

Sử dụng `gdb` để dịch ngược file: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image86.png?raw=true)

Liệt kê các hàm trong file bằng lệnh `info functions`: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image87.png?raw=true)

Theo mô tả thử thách, chú ý hàm `main` có địa chỉ `0x401106`.

Sử dụng lệnh `disassemble main` xem mã Assembly của hàm `main`: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image88.png?raw=true)

Phân tích luồng thực thi của hàm:

**1. Thiết lập Stack Frame**

```
0x0000000000401106 <+0>:	endbr64
0x000000000040110a <+4>:	push   %rbp
0x000000000040110b <+5>:	mov    %rsp,%rbp
```

- `endbr64`: Đây là một lệnh mới trong kiến trúc Intel CET (Control-flow Enforcement Technology) để bảo vệ chống lại các tấn công ROP (Return-Oriented Programming).
- `push   %rbp`: Lưu giá trị của thanh ghi `rbp` (Base Pointer) vào stack.
- `mov    %rsp,%rbp`: Đặt Base Pointer (rbp) bằng Stack Pointer (rsp). Thiết lập khung stack (stack frame) cho hàm main.

**2. Lưu tham số và khởi tạo biến**

```
0x000000000040110e <+8>:	mov    %edi,-0x14(%rbp)
0x0000000000401111 <+11>:	mov    %rsi,-0x20(%rbp)
0x0000000000401115 <+15>:	movl   $0x1e0da,-0x4(%rbp)
0x000000000040111c <+22>:	movl   $0x25f,-0xc(%rbp)
0x0000000000401123 <+29>:	movl   $0x0,-0x8(%rbp)
```

- `mov %edi, -0x14(%rbp)`: Lưu tham số `argc` (số lượng tham số) vào vị trí `-0x14(%rbp)`.
- `mov %rsi, -0x20(%rbp)`: Lưu tham số `argv` (mảng tham số) vào vị trí `-0x20(%rbp)`.
- `movl $0x1e0da, -0x4(%rbp)`: Đặt giá trị `0x1e0da` (123226) vào vị trí `-0x4(%rbp)`.
- `movl $0x25f, -0xc(%rbp)`: Đặt giá trị `0x25f` (607) vào vị trí `-0xc(%rbp)`.
- `movl $0x0, -0x8(%rbp)`: Đặt giá trị 0 vào vị trí `-0x8(%rbp)`.

**3. Vòng lặp**

***Nhảy đến kiểm tra điều kiện:***

```
0x000000000040112a <+36>:	jmp    0x401136 <main+48>
```

- Nhảy đến vị trí kiểm tra điều kiện vòng lặp.

***Thân vòng lặp:***

```
0x000000000040112c <+38>:	mov    -0x8(%rbp),%eax
0x000000000040112f <+41>:	add    %eax,-0x4(%rbp)
0x0000000000401132 <+44>:	addl   $0x1,-0x8(%rbp)
```

- `mov -0x8(%rbp), %eax`: Lấy giá trị từ `-0x8(%rbp)` (biến đếm) vào thanh ghi `eax`.
- `add %eax, -0x4(%rbp)`: Cộng giá trị `eax` vào biến tại `-0x4(%rbp)` (tổng cộng dồn).
- `addl $0x1, -0x8(%rbp)`: Tăng biến đếm lên 1.

***Điều kiện kiểm tra vòng lặp:***

```
0x0000000000401136 <+48>:	mov    -0x8(%rbp),%eax
0x0000000000401139 <+51>:	cmp    -0xc(%rbp),%eax
0x000000000040113c <+54>:	jl     0x40112c <main+38>
```

- `mov -0x8(%rbp), %eax`: Lấy giá trị của biến đếm vào thanh ghi `eax`.
- `cmp -0xc(%rbp), %eax`: So sánh với giá trị tại `-0xc(%rbp)` (607).
- `jl (Jump if Less)`: Nếu eax nhỏ hơn 607, nhảy về `main+38` để lặp lại.

**4: Kết thúc hàm**

```
0x000000000040113e <+56>:	mov    -0x4(%rbp),%eax
0x0000000000401141 <+59>:	pop    %rbp
0x0000000000401142 <+60>:	ret
```

- `mov -0x4(%rbp), %eax`: Lấy giá trị tổng cộng dồn từ `-0x4(%rbp)` và đưa vào thanh ghi `eax`.
- `pop %rbp`: Khôi phục giá trị ban đầu của `rbp` từ stack.
- `ret`: Trả về, kết thúc hàm.

=> Cần lấy giá trị của thanh ghi `EAX` trước khi chương trình thoát, tức là ngay tại lệnh `pop %rbp` (dòng +59). (vì biến `rbp` lưu giá trị tổng cộng dồn của thanh ghi `EAX`). 

Đặt `breakpoint` tại địa chỉ lệnh này và chạy chương trình, chương trình sẽ dừng lại ngay trước khi thực thi lệnh `pop %rbp`:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image89.png?raw=true)

Tiếp theo kiểm tra giá trị của thanh ghi `EAX`, dùng lệnh `info registers eax`:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image90.png?raw=true)

=> **Flag: picoCTF{307019}**

### GDB baby step 3 

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

### packer

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image34.png?raw=true)

Download file binary: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image35.png?raw=true)

Kiểm tra định dạng file out: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image36.png?raw=true)

Thử liệt kê các chuỗi trong file bằng lệnh ***strings out*** và tìm được: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image37.png?raw=true)

=> UPX là một kiểu nén tệp thực thi nâng cao. UPX thường sẽ giảm kích thước tệp của các chương trình và DLL khoảng 50%-70%, do đó giảm không gian đĩa, thời gian tải mạng, thời gian tải xuống và chi phí phân phối và lưu trữ khác.

Để giải nén file được nén UPX, sử dụng lệnh ***upx -d ./out***: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image38.png?raw=true)

Kiểm tra lại định dạng file out: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image39.png?raw=true)

=> Sau khi giải nén thu được nhiều thông tin về định dạng file hơn. 

Liệt kê lại các chuỗi trong file nhưng lần này lọc ra chuỗi 'flag', dùng lệnh ***strings out | grep 'flag'***: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image40.png?raw=true)

=> Tìm được chuỗi flag nhưng được mã hóa: 7069636f4354467b5539585f556e5034636b314e365f42316e34526933535f35646565343434317d

Dịch chuỗi này, sử dụng CyberChef: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image41.png?raw=true)

=> **Flag: picoCTF{U9X_UnP4ck1N6_B1n4Ri3S_5dee4441}**. 

### Picker I

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

### Picker II

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

### Picker III

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

### Safe Opener 2

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image.png?raw=true)

Download file mà thử thách cung cấp: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image1.png?raw=true)

Kiểm tra format file: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image2.png?raw=true)

=> Là 1 class Java. 

Thử dùng lệnh **strings** để lấy ra các chuỗi trong file: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image3.png?raw=true)

**Flag: picoCTF{SAf3_0p3n3rr_y0u_solv3d_it_de45efd6}**

### Transformation

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image4.png?raw=true)

Download file **enc** mà thử thách cung cấp và xem nội dung: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image5.png?raw=true)

=> Nội dung bị mã hóa. 

Như mô tả thử thách, có đoạn code python phía sau, có vẻ là gợi ý cách giải mã: **<enc_content>''.join([chr((ord(flag[i]) << 8) + ord(flag[i + 1])) for i in range(0, len(flag), 2)])**

Giải thích đoạn code: 

- **flag** được giả định là input string.

- **for i in range(0, len(flag),2)**: Lặp qua các cặp ký tự (2 ký tự 1 lần) trong chuỗi flag.

- **ord(flag[i]) và ord(flag[i+1])**: ord() sẽ chuyển ký tự tại vị trí i và i + 1 trong flag thành giá trị số nguyên (mã Unicode của ký tự đó)

- **(ord(flag[i]) << 8>>) + ord(flag[i+1])**: Dịch trái mã Unicode của ký tự đầu tiên trong cặp 8 bit (tức là nhân với 256). Sau đó cộng với mã Unicode của ký tự thứ hai để ghép thành 1 số nguyên 16 bit.

- **chr(...)**: chuyển số nguyên vừa tính được thành ký tự Unicode.

- **''.join(...)**: nối tát cả các ký tự lại thành 1 chuỗi kết quả.

Ví dụ:

Nếu flag = "ABCD" thì:

- Cặp ký tự: **['AB', 'CD']**.

- Chuyển đổi: **'AB'**: (ord('A') << 8) + ord('B') = (65 << 8) + 66 = 16706. Ký tự Unicode là **chr(16706) = '愀'**. **'CD'**: (ord('C') << 8) + ord('D') = (67 << 8) + 68 = 17220. Ký tự Unicode là **chr(17220) = '搀'**

=> Kết quả: **'愀搀'**

=> Viết 1 đoạn mã python để decode nội dung file enc:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image6.png?raw=true)

=> Run code và tìm ra chuỗi đã được decode:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image7.png?raw=true)

**Flag: picoCTF{16_bits_inst34d_of_8_26684c20}**

### WinAntiDbg0x100

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image8.png?raw=true)

Download file: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image9.png?raw=true)

Giải nén file này: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image10.png?raw=true)

Xem thử nội dung file *config.bin*: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image11.png?raw=true)

=> Nội dung có vẻ đã được mã hóa, không thu được gì. 

Thử chạy file *WinAntiDbg0x100.exe*:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image12.png?raw=true)

=> File yêu cầu chạy trên 1 debugger. 

Sử dụng IDA để chạy file .exe này: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image13.png?raw=true)

Xem qua hàm main, tìm được code kiểm tra debugger có hoạt động không: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image14.png?raw=true)

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image15.png?raw=true)

Phân tích: 

- ***call ds:OutputDebugStringW***: Gửi một chuỗi ký tự ra Debugger (trình gỡ lỗi) nếu có. Nếu không có trình gỡ lỗi, hàm này không tạo ra bất kỳ kết quả nào.
- ***call ds:IsDebuggerPresent***: Kiểm tra xem chương trình có đang chạy trong trình gỡ lỗi không. Thanh ghi ***EAX = 1*** nếu đang chạy trong trình gỡ lỗi, ngược lại = 0 nếu không có trình gỡ lỗi. 
- ***test eax, eax***: Kiểm tra xem giá trị của EAX có bằng 0 không.
- ***jz (Jump if Zero) short loc_D161B***: Nếu EAX = 0, nhảy đến loc_D161B.

Trong TH này, giá trị thanh ghi EAX **đang không bằng 0**, nên chương trình dừng ở đây:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image16.png?raw=true)

Nếu có thể jump đến loc_D161B thì sẽ lấy được flag:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image17.png?raw=true)

=> Cần thay đổi mã để nhảy đến loc_D161B ngay cả khi thanh ghi EAX không bằng 0. Click vào dòng ***jz short loc_D161B*** => Edit => Patch program => Change byte...: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image18.png?raw=true)

Chuyển giá trị Hex từ 0x74 (jz) thành 0x75 (jnz - jump if not zero):

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image19.png?raw=true)

Click OK => Edit => Patch program => Apply patches to input file để áp dụng thay đổi. Sau đó chạy lại chương trình: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image20.png?raw=true)

=> **Flag: picoCTF{d3bug_f0r_th3_Win_0x100_cc0ff664}**

### WinAntiDbg0x200

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

### GDB baby step 4

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image105.png?raw=true)

Download file debug: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image106.png?raw=true)

Kiểm tra định dạng file này: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image107.png?raw=true)

=> File có dạng `ELF 64-bit LSB`: là file thực thi 64-bit ở định dạng ELF và `Not stripped`: file chưa bị xóa thông tin debug, có thể phân tích.

Mở file bằng `gdb`: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image108.png?raw=true)

Xem danh sách các hàm: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image109.png?raw=true)

Để ý hàm `func1` và `main`, mô tả thử thách ám chỉ đến một hàm nhân giá trị của EAX với một hằng số. Đây có thể là chức năng của hàm `func1`. 

Chạy chương trình và quan sát sự khác biệt giữa `step into` và `step over` trong hành động. Đặt kiểu hiển thị mã lệnh thành Intel:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image110.png?raw=true)

Đặt `breakpoint` tại hàm `main`:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image111.png?raw=true)

Thiết lập giao diện dễ đọc hơn bằng lệnh: 

    layout asm

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image112.png?raw=true)

Chạy chương trình:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image113.png?raw=true)

=> Chương trình sẽ dừng tại điểm bắt đầu của hàm `main`.

Sử dụng lệnh `ni` (Next Instruction) để di chuyển qua các lệnh một cách tuần tự, để ý khi dùng `ni` liên tục, chương trình sẽ bỏ qua lệnh `call`, lệnh mà sẽ bước vào hàm `func1`:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image114.png?raw=true)

Vì vậy khi đến lệnh `call`, sử dụng lệnh `si` (Step Into) để bước vào hàm `func1`:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image115.png?raw=true)

Lệnh `imul` trong hàm `func1` sẽ thực hiện phép nhân: 

    EAX = EAX * 0x3269

Chuyển đổi giá trị hex sang thập phân bằng lệnh: 

    print/d 0x3269
    
Kết quả là số thập phân tương ứng với hằng số hex `0x3269`.

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image116.png?raw=true)

=> **Flag: picoCTF{12905}**

### ASCII FTW

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image117.png?raw=true)

Download file: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image118.png?raw=true)

Kiểm tra format file này: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image119.png?raw=true)

Là file `ELF` - Executable and Linkable Format, một định dạng `tệp nhị phân` phổ biến chứa các chương trình hoặc thư viện phần mềm, chủ yếu được sử dụng cho các hệ điều hành giống `Unix`. Bằng cách giải mã đúng định dạng này, có thể tìm ra flag.

Hiểu rõ tệp `ELF` là rất quan trọng vì nó cung cấp cái nhìn sâu sắc về `cách chương trình hoạt động` và cuối cùng là những manh mối để giải mã flag. Tệp `ELF` chứa mọi thứ mà hệ thống cần để chuẩn bị thực thi một chương trình, bao gồm các hướng dẫn ngôn ngữ máy, cách sắp xếp bộ nhớ và điểm bắt đầu của chương trình.

Tuy nhiên, các tệp này không dễ đọc đối với con người – chúng được viết dưới dạng `nhị phân`, một ngôn ngữ bao gồm các con số `1` và `0` mà máy móc hiểu được, chứ không phải con người.

Thử thực thi chương trình này: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image120.png?raw=true)

Tìm được gợi ý `The flag starts with 70`.

Sử dụng `gdb` để bắt đầu debug: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image121.png?raw=true)

Liệt kê các hàm có trong chương trình: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image122.png?raw=true)

Khi một chương trình được biên dịch, nó sẽ chuyển từ mã nguồn (C, C++, Python,...) sang mã máy (dạng nhị phân - các bit 0 và 1). Quá trình `disassemble` là việc dịch ngược từ mã máy thành mã assembly để phân tích hoặc kiểm tra.

Thử disassemble hàm `main`: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image123.png?raw=true)

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image124.png?raw=true)

Để ý các lệnh movb, các lệnh này sẽ lưu trữ các giá trị hex vào vùng nhớ cụ thể, từ gợi ý thu được từ việc thực thi chương trình `The flag starts with 70`, trích xuất được dãy hex: 

    7069636f4354467b41534349495f49535f454153595f38393630463041467d

Thử dùng Cyberchef, giải mã chuỗi hex này: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image125.png?raw=true)

=> **Flag: picoCTF{ASCII_IS_EASY_8960F0AF}**

### timer

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image126.png?raw=true)

Download file: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image127.png?raw=true)

Kiểm tra file format: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image128.png?raw=true)

Là file `.apk`. Thử giải nén file này bằng lệnh:

    unzip timer.apk

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image129.png?raw=true)

Ra được một loạt file và folder khác nhau. Thử tìm kiếm chuỗi `picoCTF` trong tất cả các file và folder: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image130.png?raw=true)

Có vẻ flag nằm ở file `classes3.dex`. Thử lọc ra chuỗi có chứa `picoCTF` trong file này: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Reversing/images/image131.png?raw=true)

=> **Flag: picoCTF{t1m3r_r3v3rs3d_succ355fully_17496}**






