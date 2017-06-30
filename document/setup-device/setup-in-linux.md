# Cài đặt phần mềm trên linux#
------------
## Bước 1: Cài đặt biến môi trường
Cài đặt biến JAVA_HOME, MVN_HOME

* Download jdk: [Link download jdk](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) Tải bản 64 bit .tar.gz
* Download maven: [Link download maven](https://maven.apache.org/download.cgi) Tải bản binary .tar.gz

Truy cập /opt tạo folder tên "java"
Giải nén 2 bản vừa download, đổi tên

* "Bản jdk" -> "jdk1.8"
* "Bản maven" -> "maven"

Copy 2 folder vừa đổi tên vào /opt/java
(Khi truy cập /opt phải dùng quyền sudo)

Mở file bashrc qua lệnh: vi ~/.bashrc và thêm các dòng sau (cài đặt đường path với đúng cấu trúc folder vừa tạo)

    JAVA_HOME="/opt/java/jdk1.8
    MVN_HOME="/opt/java/maven
    PATH=$PATH:$JAVA_HOME/bin:$MVN_HOME/bin
    export JAVA_HOME MVN_HOME

Để kiểm tra xem đã nhận biến môi trường chưa, dùng các lệnh sau:

    which java
    which maven
    $JAVA_HOME
    $MVN_HOME

## Bước 2: Cài đặt Spring tool suite
Cài đặt STS: [link download](https://spring.io/tools/sts/all) bản tar.gz

Giải nén và đổi tên folder vừa giải nén thành "sts-bundle"

## Bước 3: Cài đặt và lấy mã nguồn HKT như trên window

## DONE