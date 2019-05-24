# Change host name

Get current hostname
```sh
$ hostnamectl
```

### Set static hostname
```sh
$ hostnamectl set-hostname my.new-hostname.server
```

After set static hostname, edit the /etc/hosts file
```sh
$ sudo vim /etc/hosts
```

```
127.0.0.1  localhost localhost.localdomain localhost 4 localhost4.localdomain4 old.hostname
```
Change old.hostname to my.new-hostname.server

Reboot and check

### [Optional] Set transient/pretty/static hostname
```sh
$ sudo hostnamectl –transient set-hostname temporary.hostname
```

Can use this cmd to change Static. Pretty ..
```sh
$ sudo hostnamectl --prettyset-hostname “Pretty Hostname”
or
$ sudo hostnamectl --staticset-hostname temporary.hostname
```

