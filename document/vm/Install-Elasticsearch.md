# Install Elasticsearch - Kibana - Logstash

## 1. Install Elasticsearch
Check Java
```sh
$ java -version
```

Install Elastic

```sh
$ wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.7.1.tar.gz
$ wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.7.1.tar.gz.sha512
$ shasum -a 512 -c elasticsearch-6.7.1.tar.gz.sha512
$ tar -xzf elasticsearch-6.7.1.tar.gz
$ cd elasticsearch-6.7.1/
```

Run Elastic
``` sh
Normal run
$ ./elasticsearch
or
Run with claster-name and node-name
$ ./elasticsearch -Ecluster.name=my_cluster_name -Enode.name=my_node_name
or
Run with daemon
$ ./bin/elasticsearch -d -p pid
```

Check Elastic is running
```sh
$ curl -X GET "localhost:9200/"
```

Shutdown Elastic
```sh
$ pkill -F pid
```

## 2. Install Kibana

** You have to install Nginx

Add RPM package
```sh
$ sudo rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
```

Install Kibana

```sh
$ sudo yum install kibana
```

Start Kibana

```sh
$ sudo systemctl start kibana
```

Use openssl, create an administrative Kibana
```sh
$ echo "kibanaadmin:`openssl passwd -apr1`" | sudo tee -a /etc/nginx/htpasswd.users
```

Config Kibana in Nginx

```sh
$ sudo vi -p /etc/nginx/conf.d/example.com.conf
```

Add 
```sh
server {
    listen 80;

    server_name example.com www.example.com;

    auth_basic "Restricted Access";
    auth_basic_user_file /etc/nginx/htpasswd.users;

    location / {
        proxy_pass http://localhost:5601;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```
Check syntax and restart nginx
```sh
$ sudo nginx -t
$ sudo systemctl restart nginx
```

By default, SELinux security policy is set to be enforced. Run the following command to allow Nginx to access the proxied service
```sh
$ sudo setsebool httpd_can_network_connect 1 -P
```

Change config in kibana.yml

```sh
$ sudo vi /etc/kibana/kibana.yml
```

```sh
server.port: 5601
server.host: IP_NEED_TO_CONNECT
```

Test kibana: http://IP:5601/app/kibana

## 3. Install Logstash



Install Logstah
```sh
$ sudo yum install logstash
```

Logstash run as pipeline, 3 Step
- input
- filter
- output

Config file

##### Input
```sh
$ sudo vi /etc/logstash/conf.d/02-beats-input.conf
```

Add
```sh
input {
  beats {
    port => 5044
  }
}
```

##### Filter
```sh
$ sudo vi /etc/logstash/conf.d/10-syslog-filter.conf
```

Add
```sh
filter {
  if [fileset][module] == "system" {
    if [fileset][name] == "auth" {
      grok {
        match => { "message" => ["%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} sshd(?:\[%{POSINT:[system][auth][pid]}\])?: %{DATA:[system][auth][ssh][event]} %{DATA:[system][auth][ssh][method]} for (invalid user )?%{DATA:[system][auth][user]} from %{IPORHOST:[system][auth][ssh][ip]} port %{NUMBER:[system][auth][ssh][port]} ssh2(: %{GREEDYDATA:[system][auth][ssh][signature]})?",
                  "%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} sshd(?:\[%{POSINT:[system][auth][pid]}\])?: %{DATA:[system][auth][ssh][event]} user %{DATA:[system][auth][user]} from %{IPORHOST:[system][auth][ssh][ip]}",
                  "%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} sshd(?:\[%{POSINT:[system][auth][pid]}\])?: Did not receive identification string from %{IPORHOST:[system][auth][ssh][dropped_ip]}",
                  "%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} sudo(?:\[%{POSINT:[system][auth][pid]}\])?: \s*%{DATA:[system][auth][user]} :( %{DATA:[system][auth][sudo][error]} ;)? TTY=%{DATA:[system][auth][sudo][tty]} ; PWD=%{DATA:[system][auth][sudo][pwd]} ; USER=%{DATA:[system][auth][sudo][user]} ; COMMAND=%{GREEDYDATA:[system][auth][sudo][command]}",
                  "%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} groupadd(?:\[%{POSINT:[system][auth][pid]}\])?: new group: name=%{DATA:system.auth.groupadd.name}, GID=%{NUMBER:system.auth.groupadd.gid}",
                  "%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} useradd(?:\[%{POSINT:[system][auth][pid]}\])?: new user: name=%{DATA:[system][auth][user][add][name]}, UID=%{NUMBER:[system][auth][user][add][uid]}, GID=%{NUMBER:[system][auth][user][add][gid]}, home=%{DATA:[system][auth][user][add][home]}, shell=%{DATA:[system][auth][user][add][shell]}$",
                  "%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} %{DATA:[system][auth][program]}(?:\[%{POSINT:[system][auth][pid]}\])?: %{GREEDYMULTILINE:[system][auth][message]}"] }
        pattern_definitions => {
          "GREEDYMULTILINE"=> "(.|\n)*"
        }
        remove_field => "message"
      }
      date {
        match => [ "[system][auth][timestamp]", "MMM  d HH:mm:ss", "MMM dd HH:mm:ss" ]
      }
      geoip {
        source => "[system][auth][ssh][ip]"
        target => "[system][auth][ssh][geoip]"
      }
    }
    else if [fileset][name] == "syslog" {
      grok {
        match => { "message" => ["%{SYSLOGTIMESTAMP:[system][syslog][timestamp]} %{SYSLOGHOST:[system][syslog][hostname]} %{DATA:[system][syslog][program]}(?:\[%{POSINT:[system][syslog][pid]}\])?: %{GREEDYMULTILINE:[system][syslog][message]}"] }
        pattern_definitions => { "GREEDYMULTILINE" => "(.|\n)*" }
        remove_field => "message"
      }
      date {
        match => [ "[system][syslog][timestamp]", "MMM  d HH:mm:ss", "MMM dd HH:mm:ss" ]
      }
    }
  }
}
```

##### Output
```sh
$ sudo vi /etc/logstash/conf.d/30-elasticsearch-output.conf
```

Add

```sh
output {
  elasticsearch {
    hosts => ["localhost:9200"]
    manage_template => false
    index => "%{[@metadata][beat]}-%{[@metadata][version]}-%{+YYYY.MM.dd}"
  }
}
```

Test logstash

```sh
$ sudo -u logstash /usr/share/logstash/bin/logstash --path.settings /etc/logstash -t
```





























