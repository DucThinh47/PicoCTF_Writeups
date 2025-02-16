# First Find

### Solution

Sử dụng lệnh wget để tải file:

![img](6)

Sử dụng lệnh unzip để giải nén file: 

![img](7)

=> Cần tìm file ‘uber-secret.txt’

![img](8)

=> Không thấy!

=> Sử dụng lệnh man find để tìm xem có công cụ nào tên là find không? 

![img](9)

=> Có công cụ find, được dùng để tìm kiếm tệp tin

Sử dụng lệnh cd để cd vào thư mục files: 

Sử dụng lệnh find . -name uber-secret.txt:

![img](10)

. : đại diện cho folder hiện tại, lệnh sẽ tìm từ folder hiện tại và lướt qua tất cả folder con

-name: chỉ định lệnh find tìm tệp có tên là uber-secret.txt

=> Tìm ra path chứa file .txt

=> cd dần vào theo path: 

![img](11)

**Flag: picoCTF{f1nd_15_f457_ab443fd1}**
