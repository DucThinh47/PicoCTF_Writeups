# packer

![img](34)

Download file binary: 

![img](35)

Kiểm tra định dạng file out: 

![img](36)

Thử liệt kê các chuỗi trong file bằng lệnh ***strings out*** và tìm được: 

![img](37)

=> UPX là một kiểu nén tệp thực thi nâng cao. UPX thường sẽ giảm kích thước tệp của các chương trình và DLL khoảng 50%-70%, do đó giảm không gian đĩa, thời gian tải mạng, thời gian tải xuống và chi phí phân phối và lưu trữ khác.

Để giải nén file được nén UPX, sử dụng lệnh ***upx -d ./out***: 

![img](38)

Kiểm tra lại định dạng file out: 

![img](39)

=> Sau khi giải nén thu được nhiều thông tin về định dạng file hơn. 

Liệt kê lại các chuỗi trong file nhưng lần này lọc ra chuỗi 'flag', dùng lệnh ***strings out | grep 'flag'***: 

![img](40)

=> Tìm được chuỗi flag nhưng được mã hóa: 7069636f4354467b5539585f556e5034636b314e365f42316e34526933535f35646565343434317d

Dịch chuỗi này, sử dụng CyberChef: 

![img](41)

=> **Flag: picoCTF{U9X_UnP4ck1N6_B1n4Ri3S_5dee4441}**. 