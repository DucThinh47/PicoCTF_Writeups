# Packets Primer
![img](20)

### Solution

Tải file **network-dump.flag.pcap** và mở trong Wireshark:

![img](21)

=> Flag thường khó tìm thấy trong ARP message vì các ARP message chỉ liên quan đến địa IP address và MAC address 

=> lọc các ARP message: !arp

![img](22)

=> Để ý 3 packet đầu là quá trình bắt tay 3 bước - TCP handshake, có thể được xác định bằng các cờ trong gói tin. Đầu tiên cờ ‘SYN’ được gửi từ host A, host B gửi lại ‘SYN, ACK’, sau đó cuối cùng là ‘ACK’ từ host A. SYN là viết tắt của đồng bộ hóa, ACK là xác nhận. Cả 2 bên đồng bộ hóa và xác nhận.

=> Để ý 2 packet còn lại, packet thứ 4 có cờ ‘PSH’ nghĩa là có dữ liệu cho ứng dụng trong packet.

=> Kiểm tra packet có cờ PSH, tìm thấy flag: 

![img](23)

**Flag: picoCTF{p4ck37_5h4rk_b9d53765}**
