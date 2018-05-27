# Cài đặt Cygwin, Java, Gradle, Maven và lấy mã nguồn HKT#
------------

**Lưu ý: Cấu trúc folder chứa project và các phần mềm liên quan như sau:

tên ổ 
  - hkt 
      - project (clone project trên git vào folder này)
      - resours (chứa các tài liệu liên quan đến công việc)
      - tool (chứa các phần mềm liên quan đến công việc) như java, gradle, maven, spring tool...
      - workspaces (chứa cài đặt của spring tool suite)

### Bước 1:
Cài đặt cygwin. Sau khi cài cygwin, cài package của cygwin: ssh, vim, git

* Cài ssh: Search openssh -> install folder .NET
    * Hướng dẫn: [link Youtube](https://www.youtube.com/watch?v=ISvWN5yPaHw)
* Cài vim: Search vim -> folder editors -> install "vim: Vi Improved - enhenced vi editor"
    * Hướng dẫn: [link Web](https://kiranshashi-cygwin.blogspot.com/2011/06/installing-vim-in-cygwin-part-1.html)
* Cài git: Search git  -> folder Devel -> install all
    * Hướng dẫn: [link Stackoverflow](https://stackoverflow.com/questions/15692890/running-git-through-cygwin-from-windows)
       
### Bước 2 
Tải Maven, Gradle, Spring tool suite
* Maven: [link Maven Download!](https://maven.apache.org/download.cgi?Preferred=ftp%3A%2F%2Fmirror.reverse.net%2Fpub%2Fapache%2F)
* STS: [link STS Download!](https://spring.io/tools/sts/all) bản zip
* Gradle : [link gradle download!](https://gradle.org/releases/) bản complete
        
**Lưu ý: nhớ để các công cụ vào đúng cấu trúc thư mục        

###Bước 3:
Thêm đường dẫn path cho biến JAVA_HOME và MVN_HOME
* Truy cập ~/.bashrc: để mở file dùng lệnh "vi ~/.bashrc"
        * File nằm trong đường dẫn "cygwin64/home/{name.user}/bashrc" của máy tính
* Thêm các dòng sau (mẫu thôi, thay đổi cho đúng với từng máy):

    JAVA_HOME="/cygdrive/c/Program Files/Java/jdk1.8.0_121"   (path java) 
    MVN_HOME="/cygdrive/c/hkt/tools/maven"                    (path maven)
    GRADLE_HOME="/cygdrive/c/hkt/tools/gradle"                (path gradle)
    PATH=$PATH:$MVN_HOME/bin:$GRADLE_HOME/bin
    export JAVA_HOME MVN_HOME GRADLE_HOME

###Bước 4: Clone project HKT về máy

Liên hệ với a Tuấn / Hiếu để có tài khoản git lab

- add key ssh cho tài khoản và máy tính (làm theo hướng dẫn trên gitlab)
- clone project về mà không cần nhập nick, pass

* Lệnh: "git clone git@bitbucket.org:hktc/hkt.git" (trong " " là đường dẫn)

###Bước 5: Tải thử viện và cài đặt trong eclipse với project sample
Vào project sample: (với đường dẫn sau: projects/sample)

Chạy các lệnh sau (chạy lần lượt nhé):

mvn clean install -Dtest.skip=false

mvn eclipse:eclipse
    
###Bước 6:
Thêm 1 số cấu hình cho git và loại bỏ các file thừa (những file không quản lý)

* Config email:
    * git config --global user.email "email@example.com"
* Chạy lệnh "vi ~/.gitignore" và thêm các dòng sau vào file
    
    build
    target 
    .*
    
* Chạy lệnh: git config --global core.excludesfile '~/.gitignore'
    
### Bước 7:
Import code project sample vào STS và chạy phần mềm

### Bước 8:
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