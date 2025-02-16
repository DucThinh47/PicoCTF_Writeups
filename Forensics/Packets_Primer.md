# Packets Primer
![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image20.png?raw=true)

### Solution

Tải file **network-dump.flag.pcap** và mở trong Wireshark:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image21.png?raw=true)

=> Flag thường khó tìm thấy trong ARP message vì các ARP message chỉ liên quan đến địa IP address và MAC address 

=> lọc các ARP message: !arp

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image22.png?raw=true)

=> Để ý 3 packet đầu là quá trình bắt tay 3 bước - TCP handshake, có thể được xác định bằng các cờ trong gói tin. Đầu tiên cờ ‘SYN’ được gửi từ host A, host B gửi lại ‘SYN, ACK’, sau đó cuối cùng là ‘ACK’ từ host A. SYN là viết tắt của đồng bộ hóa, ACK là xác nhận. Cả 2 bên đồng bộ hóa và xác nhận.

=> Để ý 2 packet còn lại, packet thứ 4 có cờ ‘PSH’ nghĩa là có dữ liệu cho ứng dụng trong packet.

=> Kiểm tra packet có cờ PSH, tìm thấy flag: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/Forensics/images/image23.png?raw=true)

**Flag: picoCTF{p4ck37_5h4rk_b9d53765}**
