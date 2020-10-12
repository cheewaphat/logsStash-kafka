# secuPI Log to Kafka
1. filebeat -> kafka
2. filebeat -> logStash -> kafka


## 1.filebeat -> kafka


## 2. logStash -> kafka

view all topisc
```sh
docker exec -it kafka /opt/kafka/bin/kafka-topics.sh --list --zookeeper zookeeper:2181
```

view kafka msg
```sh
docker exec -it kafka /opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server kafka:9092 --topic logstash_logs
```

Test Json MSG HTTP --> logStash (port 10000) --> Kafka (local)
```sh
curl -X POST -d "{msg:12144}" http://localhost:10000 --header "Content-Type:application/json"
```

# ref
docker kafka images [link](https://hub.docker.com/r/wurstmeister/kafka)
example post [tutorial](https://logz.io/blog/filebeat-tutorial/)