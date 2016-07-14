How to deploy a kafka and zookeeper cluster at AWS
==================================================

Make sure that python at environment is 2.7

1. Install dependencies
    ```
    pip install -r requirements.txt
    ```
Make sure that ansible has version >= 2.1

2. Install ansible galaxy dependencies
    ```
    ansible-galaxy install -r requirements.yaml
    ```

3. Import environment variables:
    ```
    export AWS_ACCESS_KEY=XXXXX
    export AWS_ACCESS_KEY_ID=XXXXX
    export AWS_SECRET_KEY=YYYYY
    export AWS_SECRET_ACCESS_KEY=YYYYY
    export AWS_DEFAULT_REGION=us-west-2
    ```

4. Provision servers for kafka and zookeeper
    ```
    ansible-playbook -i inventory/aws aws_zookeeper_provision.yaml
    ansible-playbook -i inventory/aws aws_kafka_provision.yaml
    ```

5. Deploy kafka and zookeeper
    ```
    ansible-playbook -i inventory/aws aws_zookeeper_deploy.yaml
    ansible-playbook -i inventory/aws aws_kafka_deploy.yaml
    ```

6. Check cluster example
    ```
    login to any kafka server
    ssh -i keys/kafka ubuntu@<ip>
    sudo su
    cd /home/kafka/kafka
    bin/kafka-topics.sh --create --zookeeper 172.31.47.47:2181,172.31.46.127:2181,172.31.33.99:2181 --replication-factor 3 --partitions 1 --topic kafkatest
    bin/kafka-console-producer.sh --broker-list 172.31.31.191:9092,172.31.26.114:9092,172.31.27.93:9092 --topic kafkatest
    bin/kafka-console-consumer.sh --zookeeper 172.31.47.47:2181,172.31.46.127:2181,172.31.33.99:2181 --topic kafkatest --from-beginning
    ```
