# Cài đặt phần mềm trên linux#
------------
**Lưu ý: Cấu trúc thư mục, tải phần mềm và đổi tên đúng theo cấu trúc

/opt/
  - java
    - jdk1.8
    - gradle
    - maven
    - sts-bundle

## Bước 1: Cài đặt biến môi trường
Tải Maven, Gradle, Spring tool suite
* Maven: [link Maven Download!](https://maven.apache.org/download.cgi?Preferred=ftp%3A%2F%2Fmirror.reverse.net%2Fpub%2Fapache%2F)
* STS: [link STS Download!](https://spring.io/tools/sts/all) bản zip
* Gradle : [link gradle download!](https://gradle.org/releases/) bản complete
        
**Lưu ý: nhớ để các công cụ vào đúng cấu trúc thư mục 

## Bước 2:
Mở file bashrc qua lệnh: vi ~/.bashrc và thêm các dòng sau (cài đặt đường path với đúng cấu trúc folder vừa tạo)

    JAVA_HOME="/opt/java/jdk1.8"                    (path java) 
    MVN_HOME="/opt/java/maven"                      (path maven)
    GRADLE_HOME="/opt/java/gradle"                  (path gradle)
    PATH=$PATH:$MVN_HOME/bin:$GRADLE_HOME/bin
    export JAVA_HOME MVN_HOME GRADLE_HOME

Để kiểm tra xem đã nhận biến môi trường chưa, dùng các lệnh sau:

    which java
    which maven
    $JAVA_HOME
    $MVN_HOME

## Bước 3: Cài đặt Spring tool suite
Cài đặt STS: [link download](https://spring.io/tools/sts/all) bản tar.gz

Giải nén và đổi tên folder vừa giải nén thành "sts-bundle" (chuyển vào đúng cấu trúc thư mục)

## Bước 3: Cài đặt và lấy mã nguồn HKT như trên window

## NOTE: Để chạy Spring tool suite có 2 cách
1. Chạy bằng cách click chuột vào icon
2. Chạy bằng lệnh: ./STS (STS là tên của phần mềm)

## DONE