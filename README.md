# Log to Kafka
1. logStash -> kafka ( example-logStash )
2. filebeat -> kafka ( example-filebeat-kafka )
3. filebeat -> logStash -> kafka ( example-filebeat )


## 1. logStash -> kafka

Run Begin 
```sh
cd example-logStash
docker-compose up -d -f docker-compose.yml
```

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
curl -X POST -d@test-msg.json http://localhost:10000 --header "Content-Type:application/json"
```

## 2. filebeat -> kafka 
Run Begin 
```sh
cd example-logStash
docker-compose up -d -f docker-compose.yml
```

Test filebeat
```sh
# test putput
docker exec -it filebeat filebeat test config -e -c /usr/share/filebeat/filebeat.yml
docker exec -it filebeat filebeat test output -e -c /usr/share/filebeat/filebeat.yml
```

view all topisc
```sh
docker exec -it kafka /opt/kafka/bin/kafka-topics.sh --list --zookeeper zookeeper:2181
```

view kafka msg
```sh
docker exec -it kafka /opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server kafka:9092 --topic filebeat_logs
```

Test sent msg into kafka (filebeat_logs)
```sh
kafkacat -P -b 192.168.1.106:9092 -t filebeat_logs 
```

Test Json MSG HTTP --> logStash (port 10000) --> Kafka (local)
```sh
curl -X POST -d "{msg:12144}" http://localhost:10000 --header "Content-Type:application/json"
curl -X POST -d@test-msg.json http://localhost:10000 --header "Content-Type:application/json"
```

## 3. filebeat -> logStash -> kafka 

Run Begin 
```sh
cd example-filebeat
docker-compose up -d -f docker-compose.yml
```

view all topisc
```sh
docker exec -it kafka /opt/kafka/bin/kafka-topics.sh --list --zookeeper zookeeper:2181
```

view kafka msg
```sh
docker exec -it kafka /opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server kafka:9092 --topic filebeat_logs
```

Test Json MSG HTTP --> logStash (port 10000) --> Kafka (local)
```sh
curl -X POST -d@test-msg.json http://localhost:10000 --header "Content-Type:application/json"
```

Test filebeat
```sh
# test putput
docker exec -it filebeat filebeat test config -e -c /usr/share/filebeat/filebeat.yml
docker exec -it filebeat filebeat test output -e -c /usr/share/filebeat/filebeat.yml
```


# ref
- docker kafka images [link](https://hub.docker.com/r/wurstmeister/kafka)
- example post [tutorial](https://logz.io/blog/filebeat-tutorial/)
- [file beat configuration-kerberos.](https://www.elastic.co/guide/en/beats/filebeat/current/configuration-kerberos.html)