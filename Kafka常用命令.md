### 主题列表
```
kafka-topics --zookeeper localhost:2181 --list
```

### 创建topic
```
kafka-topics --zookeeper localhost:2181 --create --replication-factor 1 --partitions 1 --topic charles
```
```
kafka-topics –-zookeeper localhost:2181 –-alter -–partitions 20 –-topic iptvlog
```
```
kafka-topics --zookeeper localhost:2181 --create --replication-factor 3 --partitions 3 --topic test
```

### 删除topic
```
kafka-topics --zookeeper localhost:2181 --delete --topic charles
```

### 查看topic的明细
```
kafka-topics --describe --zookeeper localhost:2181 --topic charles
```

### kafka生产者
```
kafka-console-producer --broker-list localhost:9092 --topic charles
```

### 消费者命令
```
kafka-console-consumer --bootstrap-server localhost:9092 --from-beginning --topic charles
```
```
kafka-console-consumer --bootstrap-server 192.168.2.160:9092,192.168.2.161:9092,192.168.2.162:9092 --from-beginning --topic charles
```


