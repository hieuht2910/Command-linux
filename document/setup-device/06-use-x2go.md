# Cài đặt x2go server và x2go client#
------------
## Bước 1: Cài đặt x2go server
Dùng các lệnh sau để cài đặt

    add-apt-repository ppa:x2go/stable
    apt-get update
    apt-get install x2goserver x2goserver-xsession
    reboot
    
## Bước 2: Cài đặt x2go client

    sudo apt-add-repository ppa:x2go/stable
    sudo apt-get update
    sudo apt-get install x2goclient

## Bước 3: Truy cập máy ảo linux từ máy thật (window)

Nhập lệnh
    ifconfig
trên linux để lấy địa chỉ ip: giả sử là 192.168.1.60

Tạo thêm 1 tài khoản linux (cho window truy cập)

    adduser nameuser

Mở x2go client trên window, cài đặt session với các thông số
* host: địa chỉ ip máy linux
* login: tên tài khoản user mới tạo
* password: mật khẩu tài khoản user mới tạo

##DONE