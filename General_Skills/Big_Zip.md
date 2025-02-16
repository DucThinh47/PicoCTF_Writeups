# Big Zip

### Solution

Sử dụng lệnh wget để tải file:

![img]()

Sử dụng lệnh unzip để giải nén file big-zip-files.zip: 

![img](1)

Sử dụng lệnh cd vào directory big-zip-files và lệnh ls: 

![img](2)

=> Có quá nhiều file và folder, chưa thể sử dụng lệnh find vì không biết folder nào chứa flag!

Sử dụng lệnh grep –help để tìm hiểu thêm về grep: 

![img](3)

=> Phát hiện ra tùy chọn -r, –recursive: tùy chọn này có nghĩa là sẽ tìm kiếm đệ quy tất

Sử dụng câu lệnh grep ‘picoCTF’ -r (khi đã cd vào directory big-zip-files, nếu chưa cd phải thêm grep ‘picoCTF’ -r big-zip-files)

![img](4)

![img](5)

=> Tìm ra flag nằm trong path: folder_pmbymkjcya/folder_cawigcwvgv/folder_ltdayfmktr/folder_fnpfclfyee/whzxrpivpqld.txt

**Flag: picoCTF{gr3p_15_m4g1c_ef8790dc}**

