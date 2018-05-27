# Forward url apache server #
--------------------------------------------------
Tạo file .conf trong /etc/apache2/mods-enabled


<VirtualHost *:80>
  ServerName servername (tên cuối)
  ProxyRequests Off
  <Proxy *>
    Order allow,deny
    Allow from all
  </Proxy>
  ProxyPass / http://(địa chỉ cần alias)/
  ProxyPassReverse / http://(địa chỉ cần alias)/
  <Location />
    Order allow,deny
    Allow from all
  </Location>
</VirtualHost>

ví dụ 

<VirtualHost *:80>
  ServerName demo.hktsoft.com
  ProxyRequests Off
  <Proxy *>
    Order allow,deny
    Allow from all
  </Proxy>
  ProxyPass / http://192.168.1.2:3030/
  ProxyPassReverse / http://192.168.1.2:3030/
  <Location />
    Order allow,deny
    Allow from all
  </Location>
</VirtualHost>


Restart apache server    

==> DONE















