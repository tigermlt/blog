- Architecture that cause problem
  - if you have 4 source systems and 6 target systems, you need to write 24 integrations
  - Each integration comes with difficulties around
    - protocol - how data is transported (TCP, HTTP etc)
    - Data format - how data is parsed (binary, CSV, JSON etc)
    - Data schema & evolution - how the data is shaped and may change
  - each source system will have an increasesd load from the connections
  - Apache Kafka comes in: source systems -> kafka -> target systems
    - Kafka is a high-throughput distributed messaging system
    - can scale to 100s of brokers, can scale to millions of messages per second
    - high performance (latency of less than 10ms) - real time
    - usage cases:
      - messaging system
      - activity tracking
      - gather metrics from many different locations
      - application logs gathering
      - stream processing
      - De-coupling of system dependencies
      - Integration with Spark, Flink, Storm, Hadoop and many other big data technologies
- Kafka theory
  - Topics: a particular stream of data
    - similar to a table in database (without all the constraints)
    - you can have as many topics as you want
    - A topic is identified by its name
  - partitions: topics are split in partitions
    - each partition is ordered (order is guaranteed only within a partition not across partitions)
    - each message within a partition gets an incremental id called **offset** (each partition has its own offset)
    - Data is kept only for a limited time (default is one week)
    - Once the data is written to a partition, it can't be changed (immutability)
  - Brokers
    - A kafka cluster is composed of multiple brokers (servers)
    - Each broker is identified with its ID (integer)
    - Each broker contains certain topic partitions
    - After connecting to any broker, you will be connected to the entire cluster
  - Topic replication factor
    - topics should have a replication factor > 1
    - if a broker is down, another broker can serve the data
    - At any time, only one broker can be a leader for a given partition. Only that leader can receive and serve data for a partition. The other brokers will synchronize the data. Therefore each partition has one leader and multiple ISR (in-sync replica)
  - Producers
    - Producers write data to topics
    - Producers automatically know to which broker and partition to write to
    - In case of broker failures, producers will automatically recover
    - Producers can choose to receive acknowledgement of data writes:
      - acks=0: producer won't wait for acknowledgement
      - acks=1: producer will wait for leader acknowledgement
      - acks=all: leader + replicas acknowledgement
    - message keys:
      - producers can choose to send a key with the message
      - if key=null, data is sent round robin
      - if a key is sent, then all messages for that key will always go to the same partition
  - Consumers
    - Consumers read data from a topic
    - consumers know which broker to read from
    - In case of broker failures, consumers know how to recover
    - Data is read in order **within each partitions**, but no specific order across partitions
    - Consumer groups:
      - consumers read data in consumer groups
      - each consumer within a group reads from exclusive partitions
      - if you have more consumers than partitions, some consumers will be inactive
  - Consumer offsets
    - kafka stores the offsets at which a consumer group has been reading (like a checkpoint)
    - the offsets committed live in a kafka topic named __consumer_offsets
    - when a consumer in a group has processed data received from kafka, it should committing the offsets
    - if a consumer dies, it will be able to read back from where it left off based on the offsets
    - consumers choose when to commit offsets
    - 3 delivery semantics:
      - At most once: offsets are commited as soon as the message is received; if the processing goes wrong, the message will be lost
      - At least once (preferred): offsets are commited after the message is processed. If the processing goes wrong, the message will be read again. This could result in duplicate processing of messages. Make sure the processing is idempotent (ie.e. processing again the messages won't impact your system
      - Exactly once: can only be achieved for kafka => kafka workflows using afka streams API
  - Kafka Broker Discovery
    - Every kafka broker is also called "bootstrap server" which means you only need to connct to one broker and you will be connected to the entire cluster
    - Each broker knows about all brokers, topics and partitions (metadata)
    - kafka client sends connection + metadata request, broker gives back list of all brokers, client can connect to the needed brokers
  - Zookeeper
    - it manages brokers (keeps a list of them) (connects to kafka broker)
    - it helps in performing leader election for partitions
    - it sends notifications to kafka in case of changes (e.g. new topic, broker dies etc)
    - kafka can't work without zookeeper
    - zookeeper by design operates with an odd number of servers
    - zookeeper has a leader (handle writes), the rest of the servers are followers (handle reads)
    - zookeeper does not store consumer offsets with kafka
- CLI
  - Topics:
    - create topic: ```kafka-topics.sh --zookeeper 127.0.0.1:2181 --topic first_topic --create --partitions 3 --replication-factor 1```
    - view topic: ```kafka-topics.sh --zookeeper 127.0.0.1:2181 --list```
    - view more detail of a specific topic: ```kafka-topics.sh --zookeeper 127.0.0.1:2181 --topic <topic_name> --describe```
    - delete topic: ``` kafka-topics.sh --zookeeper 127.0.0.1:2181 --topic <topic_name> --delete```
  - Producer
    - send to kafka topic: ```kafka-console-producer.sh --broker-list 127.0.0.1:9092 --topic <topic_name>```
    - can also specify properties while launching producer: ```kafka-console-producer.sh --broker-list 127.0.0.1:9092 --to <topic_name> --producer-property acks=all```
    - producer to a topic that not exist before will create the topic with default setting (partitioncount=1, replicationfactor=1 etc). The default value can be set in server.properties
  - Consumer
    - consume a topic for the new message: ```kafka-console-consumer.sh --bootstrap-server 127.0.0.1:9092 --topic <topic_name>```
    - consume a topic from the beginning (including old message): ```kafka-console-consumer.sh --bootstrap-server 127.0.0.1:9092 --topic <topic_name> --from-beginning```
    - like mentioned before since message goes into different partitions, if I do the above consume command multiple times, the order I read might not be the same as the order I sent. But the order I read each time seems to be consistent.
    - consumer group:
      - start the consumer group and read: ```kafka-console-consumer.sh --bootstrap-server 127.0.0.1:9092 --topic <topic_name> --group <group_name>```
      - If I use multiple screen (process) locally and read as the same consumer group, the message I produced will be split among groups (appear only in one screen)
      - After I close the consumer group, it will commit and increase offset so I will not be able to read old message anymore using the same consumer group even if I do ```--from-beginning```
      - check the status of consumer group:
        - list all consumer groups: ```kafka-consumer-groups.sh --bootstrap-server localhost:9092  --list```
        - see more detail of consumer group: ```kafka-consumer-groups.sh --bootstrap-server localhost:9092  --describe --group <group_name>```
          - it will show PARTITION, CURRENT-OFFSET, LOG-END-OFFSET, LAG. LAG means the number of offsets that have not been read by this consumer group (it is equal to LOG-END-OFFSET - CURRENT-OFFSET)
      - reset offset for consumer group: 
        - reset to the earlist offset: ```kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group <group_name>  --reset-offsets --to-earliest  --execute  --topic <topic_name>```
        - rest to some offsets: ```kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group <group_name> --reset-offsets --shift-by 2  --execute  --topic <topic_name>``` it will shift left 2 offsets of every partition. Ideally when I launch the consumer again, I should get 6 messages (3 partiions and each have 2 offsets left). But I didn't. I found the LOG-END-OFFSET also got shift by 2, so the lag is still 0 rather 2 for each partiion. Not sure if this is a bug or new feature of kafka.
- Kafka Java programming:
  - producer without key: Notice the send data is asynchronous so need to flush or close 
  ```
      // psvm short hand for main method
      public static void main(String[] args) {

          // create producer propertis
          Properties properties = new Properties();
          properties.setProperty(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "127.0.0.1:9092");
          properties.setProperty(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());
          properties.setProperty(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());

          // create the producer
          KafkaProducer<String, String> producer = new KafkaProducer<String, String>(properties);

          // create a producer record
          ProducerRecord<String, String> record = new ProducerRecord<String, String>("first_topic", "hello world");

          // send data - asynchronous
          producer.send(record);
          // flush data
          producer.flush();
          // flush and close producer
          producer.close();
      }
    ```
  - add callback and logger to monitor the status after sending message to kafka
  ```
      public static void main(String[] args) {
        final Logger logger = LoggerFactory.getLogger(ProducerDemo.class);

        // create producer propertis
        Properties properties = new Properties();
        properties.setProperty(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "127.0.0.1:9092");
        properties.setProperty(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());
        properties.setProperty(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());

        // create the producer
        KafkaProducer<String, String> producer = new KafkaProducer<String, String>(properties);

        // create a producer record
        ProducerRecord<String, String> record = new ProducerRecord<String, String>("first_topic", "hello world");


        // send data - asynchronous
        producer.send(record, new Callback() {
            // execute every time a record is successfully sent or an exception is thrown
            public void onCompletion(RecordMetadata recordMetadata, Exception e) {
                if (e == null) {
                    // the record was successfully sent
                    logger.info("Received new metadata. \n" +
                                "Topic: " + recordMetadata.topic() + "\n" +
                                "Partition: " + recordMetadata.partition() + "\n" +
                                "Offset: " + recordMetadata.offset() + "\n" +
                                "Timestamp: " + recordMetadata.timestamp());
                } else {
                    logger.error("Error while producing", e);
                }
            }
        });

        // flush data
        producer.flush();
        // flush and close producer
        producer.close();
  ```
  - Add key while sending message to kafka. The purpose is that the same key will always go to the same partition
  ```
      public static void main(String[] args) throws ExecutionException, InterruptedException {
        final Logger logger = LoggerFactory.getLogger(ProducerDemo.class);

        // create producer propertis
        Properties properties = new Properties();
        properties.setProperty(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "127.0.0.1:9092");
        properties.setProperty(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());
        properties.setProperty(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());

        // create the producer
        KafkaProducer<String, String> producer = new KafkaProducer<String, String>(properties);

        for (int i=0; i<10; i++) {
            String topic = "first_topic";
            String value  = "hello world " + i;
            String key = "id_" + i;

            // create a producer record
            ProducerRecord<String, String> record = new ProducerRecord<String, String>(topic, key, value);

            logger.info("Key: " + key);

            // send data - asynchronous
            producer.send(record, new Callback() {
                // execute every time a record is successfully sent or an exception is thrown
                public void onCompletion(RecordMetadata recordMetadata, Exception e) {
                    if (e == null) {
                        // the record was successfully sent
                        logger.info("Received new metadata. \n" +
                                    "Topic: " + recordMetadata.topic() + "\n" +
                                    "Partition: " + recordMetadata.partition() + "\n" +
                                    "Offset: " + recordMetadata.offset() + "\n" +
                                    "Timestamp: " + recordMetadata.timestamp());
                    } else {
                        logger.error("Error while producing", e);
                    }
                }
            });
        }

        // flush data
        producer.flush();
        // flush and close producer
        producer.close();
    }
  ```
  - consumer, even the autoreset to earlist, the same consumer group can only read data once
  ```
      public static void main(String[] args) {

        Logger logger = LoggerFactory.getLogger(ConsumerDemo.class.getName());

        // create consumer configs
        Properties properties = new Properties();
        properties.setProperty(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, "127.0.0.1:9092");
        properties.setProperty(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
        properties.setProperty(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
        properties.setProperty(ConsumerConfig.GROUP_ID_CONFIG, "my-first-application");
        // can also be latest -> read only new message, none -> throw an error if there is no message has been saved
        properties.setProperty(ConsumerConfig.AUTO_OFFSET_RESET_CONFIG, "earliest");

        // create consumer
        KafkaConsumer<String, String> consumer = new KafkaConsumer<String, String>(properties);

        // subscribe consumer to out topics
        // .singleton means only subscribe to one topic. If multiple, we can do Arrays.asList("topic1", "topic2" ...)
        consumer.subscribe(Collections.singleton("first_topic"));

        // poll for new data
        while (true) {
            ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(100));

            for (ConsumerRecord<String, String> record : records) {
                logger.info("Key: "+ record.key() + ", value: " + record.value());
                logger.info("Partition: " + record.partition() + ", Offset: " + record.offset());
            }
        }
    }
  ```
  - consumer group
    - if started multiple consumer in the same consumer group, each consumer will rebalance (each deal with some part of partitions)
    - rebalance happens when the number of consumer changes
  
