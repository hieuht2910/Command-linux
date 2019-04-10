# Install Nginx - Centos 7

1. Add Nginx Repository
```sh
$ sudo yum install epel-release
```
2. Install Nginx
```sh
$ sudo yum install nginx
```
3. Start Nginx
```sh
$ sudo systemctl start nginx
$ firewall-cmd --zone=public --add-service=http
```
