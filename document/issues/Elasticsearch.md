# Issues of Elasticsearch

### Elasticsearch

##### 1. Max virtual memory areas vm.max_map_count [65530]

```sh
$ sudo sysctl -w vm.max_map_count=262144
```

##### 2. Max file descriptors [4096] for elasticsearch process is too low, increase to at least [65535]
```sh
$ sudo sh -c "ulimit -n 65535 && exec su $LOGNAME"
```
Link describe: https://gist.github.com/ntamvl/017ce61db2d3c6b9c595f2fac8eeac3c


### Kibana
##### 1. Kibana active but show message "Kibana server is not ready yet"

Open kibana.yml
```sh
$ sudo vi /etc/kibana/kibana.yml
```

Set config
```sh
elasticsearch.hosts: ["http://localhost:9200", "http://192.168.1.233"]
```


