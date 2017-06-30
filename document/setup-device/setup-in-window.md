# Cài đặt Cygwin và lấy mã nguồn HKT#
------------

Cài đặt Cygwin, các packages của Cygwin và cách lấy mã nguồn project HKT về máy tính

### Bước 1:
Cài đặt cygwin. Sau khi cài cygwin, cài package: ssh, vim, git

* Cài ssh: Search openssh -> install folder .NET
    * Hướng dẫn: [link Youtube](https://www.youtube.com/watch?v=ISvWN5yPaHw)
* Cài vim: Search vim -> folder editors -> install "vim: Vi Improved - enhenced vi editor"
    * Hướng dẫn: [link Web](https://kiranshashi-cygwin.blogspot.com/2011/06/installing-vim-in-cygwin-part-1.html)
* Cài git: Search git  -> folder Devel -> install all
    * Hướng dẫn: [link Stackoverflow](https://stackoverflow.com/questions/15692890/running-git-through-cygwin-from-windows)
        
###Bước 2:
Thêm đường dẫn path cho biến JAVA_HOME và MVN_HOME
* Truy cập ~/.bashrc: để mở file dùng lệnh "vi ~/.bashrc"
        * File nằm trong đường dẫn "cygwin64/home/{name.user}/bashrc" của máy tính
* Thêm các dòng sau:

    JAVA_HOME="/cygdrive/c/Program Files\Java\jdk1.8.0_121" (path java)
    MVN_HOME="/cygdrive/c/hkt/tools/maven" (path maven)
    PATH=$PATH:$MVN_HOME/bin
    export JAVA_HOME MVN_HOME
    
###Bước 3:
Cài đặt public/private key cho .ssh: Dùng để tạo key mới, chưa cần dùng bước này vì đã được cung cấp key

###Bước 4:
Clone mã nguồn HKT về máy tính

* Lệnh: git clone git@bitbucket.org:hktc/hkt.git
* Sau khi clone mã nguồn, đổi từ nhánh master sang nhánh tuan08: git checkout tuan08
    
###Bước 5:
Cài đặt Maven và Spring tool suite (Cài bản zip, không cần cài đặt):

* Maven: [link Maven Download!](https://maven.apache.org/download.cgi?Preferred=ftp%3A%2F%2Fmirror.reverse.net%2Fpub%2Fapache%2F)
* STS: [link STS Download!](https://spring.io/tools/sts/all)
    
Dùng lệnh "mvn clean install" trong cygwin
* Chú ý: khi dùng lệnh phải vào project file "hkt/porm.xml" và block thẻ trong module rồi dùng 
* Khi dùng lệnh xong lần 1, vào "hkt/porm.xml" mở block ra và dùng tiếp lần 2.
    
###Bước 6:
* Chạy mvn clean install -Dtest.skip=false để kiểm tra

###Bước 7:
* Generate eclipse config với lệnh: mvn eclipse:eclipse

###Bước 8:
Thêm 1 số cấu hình cho git và loại bỏ các file thừa (những file không quản lý)

* Config email:
    * git config --global user.email "email@example.com"
* Chạy lệnh "vi ~/.gitignore" và thêm các dòng sau vào file
    
    build
    target 
    .*
    
* Chạy lệnh: git config --global core.excludesfile '~/.gitignore'
    
### Bước 9:
Import code vào STS và chạy phần mềm

### Bước 10:
Một số cài đặt trong STS

* Chuyển tab thành 2 space, hiển thị số dòng

* Chọn Window -> Preferences -> General -> Editors -> Text Editors
    * Chọn "Insert spaces for tab", "Show line number", "Show whitespace characters"
    * Chọn Window -> Preferences -> Java -> Formatter -> New Active profile -> Chỉnh sửa: 
        Tab policy: "spaces only"
        Indentation size: 2
        Tab size: 2

* Tắt hết các tab ko dùng cho gọn màn hình

###DONE