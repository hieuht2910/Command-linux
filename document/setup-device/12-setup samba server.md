# Setup Samba server #
--------------------------------------------------
Update app
   sudo apt update
Install Samba 
   sudo apt install samba
Check samba đã được install chưa
   whereis samba

Kết Quả (nếu thành công)
   samba: /usr/sbin/samba /usr/lib/samba /etc/samba /usr/share/samba /usr/share/man/man7/samba.7.gz /usr/share/man/man8/samba.8.gz

Tạo 1 folder muốn share hoặc chọn 1 folder có sẵn để share

Vào file config
   sudo vi /etc/samba/smb.conf

Add những dòng sau vào cuối file

	[tên truy cập folder] ví dụ [sambashare] thì địa chỉ truy cập sẽ là (prefix/ip máy chủ/sambashare)
   	   comment = Samba on Ubuntu
    	   path = /đường/dẫn/đến folder/muốn/share
    	   read only = no
    	   browsable = yes
Tạo user 

    sudo smbpasswd -a (youruser name)

Restart samba server
    sudo service smbd restart
==> DONE















