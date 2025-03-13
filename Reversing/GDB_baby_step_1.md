# GDB baby step 1

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


