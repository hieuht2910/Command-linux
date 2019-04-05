# Install Elasticsearch

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