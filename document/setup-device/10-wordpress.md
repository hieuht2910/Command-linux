# Lệnh cài đặt wordpress, tạo database #
--------------------------------------------------
## Cài đặt máy chủ Apache hoặc Nginx (ở đây hướng dẫn cài Apache) ##
Lệnh cài đặt máy chủ Apache:

    sudo apt install apache2
    
Sau khi cài đặt, chạy lệnh sau để vô hiệu quá danh sách thư mục (directory listing)

    sudo sed -i "s/Options Indexes FollowSymLinks/Options FollowSymLinks/" /etc/apache2/apache2.conf

Các lệnh tương tác với máy chủ

    sudo systemctl stop apache2.service
    sudo systemctl start apache2.service
    sudo systemctl enable apache2.service
    
## Cài đặt Mysql hoặc MariaDB ##

    sudo apt-get install mariadb-server mariadb-client
    
Các lệnh tương tác với máy chủ

    sudo systemctl stop mariadb.service
    sudo systemctl start mariadb.service
    sudo systemctl enable mariadb.service

Cấu hình Mariadb

    sudo mysql_secure_installation
    
Nhập cấu hình

    Enter current password for root (enter for none): Just press the Enter
    Set root password? [Y/n]: Y
    New password: Enter password
    Re-enter new password: Repeat password
    Remove anonymous users? [Y/n]: Y
    Disallow root login remotely? [Y/n]: Y
    Remove test database and access to it? [Y/n]:  Y
    Reload privilege tables now? [Y/n]:  Y
    

Khởi động lại MariaDB

    sudo systemctl restart mariadb.service
    
Cài đặt PHP

    sudo apt install php libapache2-mod-php php-common php-mbstring php-xmlrpc php-soap php-gd php-xml php-intl php-mysql php-cli php-mcrypt php-ldap php-zip php-curl
     
Sau khi cài xong, mở file cấu hình

    sudo nano /etc/php/7.1/apache2/php.ini # Ubuntu 17.10

Thay đổi cấu hình theo bên dưới

    file_uploads = On
    max_execution_time = 180
    memory_limit = 256M
    upload_max_file_size = 64M
      
Tạo database cho wordpress
  
    sudo mysql -u root -p
    CREATE DATABASE wordpress;
    CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'new_password_here';
    GRANT ALL ON wordpress.* TO 'wordpressuser'@'localhost' IDENTIFIED BY 'user_password_here' WITH GRANT OPTION;
    GRANT ALL ON wordpress.* TO 'wordpressuser'@'localhost' IDENTIFIED BY 'user_password_here' WITH GRANT OPTION;
    FLUSH PRIVILEGES;
    EXIT;
    
## Tải wordpress ##

    cd /tmp && wget https://wordpress.org/latest.tar.gz
    tar -zxvf latest.tar.gz
    sudo mv wordpress /var/www/html/wordpress
    sudo chown -R www-data:www-data /var/www/html/wordpress/
    sudo chmod -R 755 /var/www/html/wordpress/

Config Apache

    sudo nano /etc/apache2/sites-available/wordpress.conf

Thêm đoạn sau

    <VirtualHost *:80>
     ServerAdmin admin@example.com
     DocumentRoot /var/www/html/wordpress/
     ServerName example.com
     ServerAlias www.example.com
     <Directory /var/www/html/wordpress/>
        Options +FollowSymlinks
        AllowOverride All
        Require all granted
     </Directory>
     ErrorLog ${APACHE_LOG_DIR}/error.log
     CustomLog ${APACHE_LOG_DIR}/access.log combined
    </VirtualHost>
    
Cho phép Wordpress và các module lquan

    sudo a2ensite wordpress.conf
    sudo a2enmod rewrite
    
    sudo systemctl restart apache2.service
    
Cấu hình wordpress và chạy

    sudo mv /var/www/html/wordpress/wp-config-sample.php /var/www/html/wordpress/wp-config.php
    
    sudo nano /var/www/html/wordpress/wp-config.php
    
Điền các thông số 
    
     db name
     db user
     db password
     db host
     
## Sửa lỗi chỉnh đường dẫn theo bài viết (trong wordpress) ##

Mở file /etc/apache2/apache2.conf

Sửa đoạn tất cả AllowOverride thành All

```
