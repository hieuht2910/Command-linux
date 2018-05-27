# Cài đặt máy ảo Virtual-box và cài ubuntu desktop#
------------

### Bước 1: Cài đặt máy ảo

Tải máy ảo virtual box mới nhất: [Link virtualbox](https://www.virtualbox.org/wiki/Downloads)  --> Cài đặt

Trong quá trình cài đặt ubuntu. thực hiện các bước 2

### Bước 2: Cài đặt ubuntu (ubuntu server)
Chọn New

* Name = "ubuntu-minimal", Type = "Linux", Version = "Ubuntu 64bit"

* Memory size = 1024MB (1Gb)

* Hard disk: "Create virtual hard disk now"

* Hard disk type: VDI

* Storage: Dynamically allocated

* Chọn file cài đặt + dung dượng set: 60GB
----> Setup xong, bắt đầu cài đặt

* Chọn file chứa ubuntu.iso

Trong quá trình setup, hệ thống sẽ yêu cầu người dùng cài đặt 1 số thông số như:

* Ngôn ngữ: English
* Ngôn ngữ bàn phím: English
* Tên tài khoản và mật khẩu: hkt

Sau quá trình cài đặt, đăng nhập vào hệ thống với tên tk và mật khẩu vừa tạo: hkt

### Bước 3: Cài đặt xfce4 (ubuntu desktop)
Để cài xfce4, chạy lệnh sau
    
    sudo apt-get install xfce4
    
Sau khi cài đặt xong, chạy lệnh sau để khởi động ubuntu desktop

    startx
    

###DONE