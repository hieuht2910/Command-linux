# Lệnh cài đặt server gitlab, cấu hình send mail bằng postfix #
--------------------------------------------------
## Cài đặt và cấu hình mail server postfix ##

Cài đặt postfix và các pakage cần thiết

    sudo apt-get install postfix mailutils libsasl2-2 ca-certificates libsasl2-modules

Mở file postfix config

    vim /etc/postfix/main.cf

Thêm các dòng sau

    relayhost = [smtp.gmail.com]:587
    smtp_sasl_auth_enable = yes
    smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
    smtp_sasl_security_options = noanonymous
    smtp_tls_CAfile = /etc/postfix/cacert.pem
    smtp_use_tls = yes

Mở file 

    vim /etc/postfix/sasl_passwd

Thêm dòng sau

    [smtp.gmail.com]:587    USERNAME@gmail.com:PASSWORD

Chỉnh lại quyền truy cập và cập nhập thông tin trong sasl_passwd

    sudo chmod 400 /etc/postfix/sasl_passwd
    sudo postmap /etc/postfix/sasl_passwd

Chứng thực hợp lệ để tránh xảy ra lỗi, thực hiện:

    cat /etc/ssl/certs/Thawte_Premium_Server_CA.pem | sudo tee -a /etc/postfix/cacert.pem
(Nếu không chạy được lệnh trên, thay "Thawte_Premium_Server_CA.pem" = "thawte_Primary_Root_CA.pem"

Restart postfix

    sudo /etc/init.d/postfix restart

Test xem đã gửi được mail chưa, chạy lệnh xong check mail

    echo "Test mail from postfix" | mail -s "Test Postfix" you@example.com

## Postfix Command ##

See all mail in queue

	mailq
	postqueue -p
	
To remove all mail from the queue:

	postsuper -d ALL
	
To remove all mails in the deferred queue:

	postsuper -d ALL deferred
	
Remove Specific Email by Id:

	sudo postsuper -d <mail ID>

To send all mails in queue:

	sudo postfix flush
	
Show mails content by id:

	sudo postcat -q <mail ID>

to find script that send email (mail.add_x_header = On in php.ini)

	sudo postcat -q <mail ID> | grep php
	

To trace and monitor process resource use:

lsof -p 11121,10108,11122,10111

postqueue -p | grep "email@example.com"

--------------------------------------------------
## Cài đặt gitlab và cấu hình gitlab tương thích với nginx ##

Tải gitlab

    curl -LJO https://packages.gitlab.com/gitlab/gitlab-ce/packages/ubuntu/xenial/gitlab-ce_9.3.0-ce.0_amd64.deb/download

Cài đặt gitlab

    sudo dpkg -i gitlab-ce_9.3.0-ce.0_amd64.deb

Config gitlab

    sudo gitlab-ctl reconfigure

--------------------------------------------------------------

Cấu hình gitlab tương thích với nginx và đổi cổng gitlab sang 8181

Mở file gitlab.rb

    sudo vi /etc/gitlab/gitlab.rb

Thêm các dòng sau

    nginx['enable'] = false
    web_server['external_users'] = ['www-data']
    gitlab_rails['trusted_proxies'] = [ '192.168.1.0/24', '192.168.2.1', '2001:0db8::/32' ]
    gitlab_workhorse['listen_network'] = "tcp"
    gitlab_workhorse['listen_addr'] = "0.0.0.0:8181"

Reconfig lại

    sudo gitlab-ctl reconfigure

-------------------------------------------------------------

Thay đổi hostname gitlab

    /etc/gitlab/gitlab.rb
    Thay đổi external_url
    Sau đó chạy lệnh sudo gitlab-ctl reconfigure
    

==> DONE















