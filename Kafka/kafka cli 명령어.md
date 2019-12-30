# kafka cli 명령어

## 서버

- 시작
    - zookeeper -> kafka 순으로 작동
    - `-d` 옵션은 daemon으로 생성 

    ```
    bin/zookeeper-server-start.sh config/zookeeper.properties
    bin/kafka-server-start.sh config/server.properties
    ```

- 종료 
    - kafka -> zookeeper 순으로 종료

    ```
    bin/kafka-server-stop.sh config/server.properties
    bin/zookeeper-server-stop.sh config/zookeeper.properties
    ```

- 서버 로그 체크
    ```
    tail /usr/local/bin/kafka/logs/server.log 
    ```

## 토픽

참고 서버를 `--bootstrap-server svr:9092`로 지정할 수도 있고, `--zookeeper zkp:2181`로 지정할 수도 있음

- 토픽 리스트

    ```
    bin/kafka-topics.sh --zookeeper zk1:2181 --list 
    ```

- 토픽 생성

    ```
    bin/kafka-topics.sh --zookeeper zk1:2181 --create \
      --replication-factor 1 \
      --partitions 1 \
      --topic topic1
    ```

- 토픽 삭제

    ```
    bin/kafka-topics.sh --zookeeper localhost:2181 --delete \
      --topic topic1
    ```

## 컨슈머그룹

- 컨슈머그룹 체크

    ```
    bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 \
    --list
    ```

- 컨슈머 옵셋 체크

    ```
    bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 \
    --group sr --describe
    ```

- 컨슈머그룹 삭제

    ```
    bin/kafka-consumer-groups.sh --zookeeper localhost:2181 --delete --group <group_name>
    ```

- 토픽 리더 체크
    ```
    bin/kafka-topics.sh --zookeeper localhost:2181 --topic my_topic --describe
    ```

## 컨슈머

- 특정 토픽의 값 (처음부터) 조회
    ```
    bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 \
      --from-beginning \
      --topic topic1
    ```

- 특정 

    ```
    bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 \
      --from-beginning \
      --partition 1 \
      --topic numtest
    ```

