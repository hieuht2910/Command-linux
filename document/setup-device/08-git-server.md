# Lệnh cài đặt server git #
------------
## Bước Cài đặt trên Server:
Tạo 1 thư mục chứa git (khởi tạo, mã nguồn): 
VD: mkdir hkt.git

Di chuyển đến thư mục vừa tạo: cd hkt.git

Khởi tạo git: git init --bare

## Bước Cài đặt trên máy Client:
Tạo 1 thư mục chứa mã nguồn code: mkdir project

Di chuyển đến thư mục vừa tạo: cd project

Khởi tạo git: git init

Tạo 1 file text

Thêm remote: git remote add origin ssh://git@remote-server/path-on-server.git

Thực hiện commit và push lên server

==> DONE

## Bước lấy mã nguồn về (test xem đã tạo kho git thành công chưa)
Thực hiện lệnh: git clone git@remote-server:/path-on-server.git

