# Setup centos 7

#### 1. DHClient
If centos can not retrieve from central repository. Type command:

```sh
$ sudo dhclient
```

#### 2. IPv4 address
If centos can not get ipv4, SET ONBOOT is set to yes in /etc/sysconfig/network-scripts/ifcfg-eno1677736

```sh
$ vi /etc/sysconfig/network-scripts/ifcfg-eno1677736
```
Then save the file and make it executable:
```sh
$ chmod +x /etc/rc.d/rc.local
```

Install net-tools
```sh
$ sudo yum install net-tools
```

Then, reboot machine

#### 3. SSH (Optional)
Install Open ssh

```sh
$ yum install openssh openssh-server openssh-clients openssl-libs
```

Default port is 22. 
Config port:
```sh
$ vi /etc/ssh/sshd_config
```
Make change 'Port 22' to other

#### 4. Java (Optional)
Download java, copy to /opt/resources/java

Open ~/.bash_profile, copy text below

```sh
$ JAVA_HOME="/opt/resources/java/jdk1.8.0_201"
$ PATH=$PATH:$JAVA_HOME/bin
$ export JAVA_HOME
```

Refresh file ~/.bash_profile 

```sh
$ source ~/.bash_profile
```

