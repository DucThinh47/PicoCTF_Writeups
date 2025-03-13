# GDB baby step 2

![img](83)

Download file: 

![img](84)

Kiểm tra file format: 

![img](85)

Sử dụng `gdb` để dịch ngược file: 

![img](86)

Liệt kê các hàm trong file bằng lệnh `info functions`: 

![img](87)

Theo mô tả thử thách, chú ý hàm `main` có địa chỉ `0x401106`.

Sử dụng lệnh `disassemble main` xem mã Assembly của hàm `main`: 

![img](88)

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

![img](89)

Tiếp theo kiểm tra giá trị của thanh ghi `EAX`:

![img](90)

=> **Flag: picoCTF{307019}**



