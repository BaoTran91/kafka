https://medium.com/@at_ishikawa/getting-started-with-kafka-on-mac-f6aa8924fcda

$ brew install kafka -> use install-from-source if  doesn't work
$ brew install zookeeper
$ zkServer start
$ vim /usr/local/etc/kafka/server.properties
edit -> listeners=PLAINTEXT://:9092 to listeners=PLAINTEXT://localhost:9092

$ kafka-server-start /usr/local/etc/kafka/server.properties

### Create a topic
    $ kafka-topics --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
Here we have created a topic name test

### Send a message
Now we will initialize the Kafka producer console, which will listen to localhost at port 9092 at topic test :
    $ kafka-console-producer --broker-list localhost:9092 --topic test

>HELLO Kafka
### Receive a message
    $ kafka-console-consumer --bootstrap-server localhost:9092 --topic test --from-beginning
HELLO Kafka

##########################################
# UI 
##########################################


https://docs.confluent.io/platform/current/quickstart/ce-quickstart.html#ce-quickstart

export CONFLUENT_HOME=<path-to-confluent>
export PATH=$PATH:$CONFLUENT_HOME/bin


docker pull landoop/kafka-topics-ui
docker run --rm -it -p 8000:8000 \
-e "KAFKA_REST_PROXY_URL=http://localhost:9092" \
-e "PROXY=true" \
landoop/kafka-topics-ui

##########################################
# DOCKER 
##########################################

https://medium.com/big-data-engineering/hello-kafka-world-the-complete-guide-to-kafka-with-docker-and-python-f788e2588cfc
> git clone https://github.com/wurstmeister/kafka-docker.git 
> cd kafka-docker

### Update KAFKA_ADVERTISED_HOST_NAME inside 'docker-compose.yml',
### For example, set it to 172.17.0.1
> vim docker-compose.yml 
> docker-compose up -d

### Optional - Scale the cluster by adding more brokers (Will start a single zookeeper instance)
> docker-compose scale kafka=3

### You can check the proceses running with:
> docker-compose ps

### Destroy the cluster when you are done with it
> docker-compose stop


# Kafka Shell
## To start it just run the command:
    > ./start-kafka-shell.sh <DOCKER_HOST_IP/KAFKA_ADVERTISED_HOST_NAME>
### In my case:
    > ./start-kafka-shell.sh 172.17.0.1
Hello Topic
From within the Kafka Shell, run the following to create and describe a topic:
    > $KAFKA_HOME/bin/kafka-topics.sh --create --topic test \
    --partitions 4 --replication-factor 2 \
    --bootstrap-server `broker-list.sh`
    > $KAFKA_HOME/bin/kafka-topics.sh --describe --topic test \
    --bootstrap-server `broker-list.sh`
Hello Producer
Initialize the producer and write messages to Kafka’s brokers.
    > $KAFKA_HOME/bin/kafka-console-producer.sh --topic=test \
    --broker-list=`broker-list.sh`
    >> Hello World!
    >> I'm a Producer writing to 'hello-topic'
Hello Consumer
Initialize the consumer from another Kafka terminal and it will start reading the messages sent by the producer.
    > $KAFKA_HOME/bin/kafka-console-consumer.sh --topic=test \
    --from-beginning --bootstrap-server `broker-list.sh`



##########################################
# NIFI 
##########################################

export JAVA_HOME=`/usr/libexec/java_home -v 11.0`

https://medium.com/big-apps-tech/apache-nifi-for-data-flow-and-real-time-streaming-1c2e128d190d
export NIFI=/Users/baotran/Desktop/tech/NIFI/nifi-1.12.1
export PATH=$NIFI/bin:$PATH
