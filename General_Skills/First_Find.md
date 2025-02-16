# First Find

### Solution

Sử dụng lệnh wget để tải file:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/General_Skills/images/image6.png?raw=true)

Sử dụng lệnh unzip để giải nén file: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/General_Skills/images/image7.png?raw=true)

=> Cần tìm file ‘uber-secret.txt’

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/General_Skills/images/image8.png?raw=true)

=> Không thấy!

=> Sử dụng lệnh man find để tìm xem có công cụ nào tên là find không? 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/General_Skills/images/image9.png?raw=true)

=> Có công cụ find, được dùng để tìm kiếm tệp tin

Sử dụng lệnh cd để cd vào thư mục files: 

Sử dụng lệnh find . -name uber-secret.txt:

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/General_Skills/images/image10.png?raw=true)

. : đại diện cho folder hiện tại, lệnh sẽ tìm từ folder hiện tại và lướt qua tất cả folder con

-name: chỉ định lệnh find tìm tệp có tên là uber-secret.txt

=> Tìm ra path chứa file .txt

=> cd dần vào theo path: 

![img](https://github.com/DucThinh47/PicoCTF_Writeups/blob/main/General_Skills/images/image11.png?raw=true)

**Flag: picoCTF{f1nd_15_f457_ab443fd1}**
