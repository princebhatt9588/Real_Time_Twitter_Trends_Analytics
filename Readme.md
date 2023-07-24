# Real-Time Data Pipeline for Twitter Trends Analysis

![](https://github.com/akshitvjain/realtime-twitter-trends-analytics/blob/master/images/realtime-twitter-dashboard.gif)

## Motivation

In the current era, real-time data analysis holds critical significance for both SMEs and Large Corporations, spanning industries such as Financial services, Legal services, IT operation management services, Marketing, and Advertising. This entails the examination of extensive real-time and historical data to facilitate informed business decisions.

Distinguished by its velocity, volume, and variety, Big data differs from regular data. The development of a distributed data pipeline becomes essential for processing, storing, and analyzing real-time data, distinguishing it from traditional Big data applications.

This personal project aims to apply the principles of large-scale parallel data processing (CS 6240 - NEU) to create a real-time processing pipeline using open source tools. The goal is to efficiently capture, process, store, and analyze substantial data from diverse sources, ensuring scalability and effectiveness.

## Project Description

Leveraging Twitter streaming trends popularity and sentiment analysis proves to be an exceptional choice for constructing a distributed data pipeline. On a daily basis, an impressive volume of approximately 500 million tweets, sourced globally, emerges (as of October, 2019). Out of this vast number, approximately 1%, equivalent to 5 million tweets, becomes publicly accessible.

The data pipeline is ingeniously designed, employing the following components: <b>Apache Kafka</b> as the data ingestion system, <b>Apache Spark</b> as the real-time data processing system, <b>MongoDB</b> for distributed storage and retrieval, and <b>Apache Drill</b> to establish connectivity between MongoDB and <b>Tableau</b> for real-time analytics.

The Twitter data is obtained through the utilization of the Twitter Streaming API and is efficiently streamed to Apache Kafka, enabling seamless accessibility for Apache Spark, which then undertakes data processing and sentiment classification, storing the resultant data into MongoDB. The analysis of trends' popularity and sentiment is conducted through a comprehensive Tableau dashboard.

<b>Note:</b> Apache Drill plays a crucial role in connecting MongoDB with Tableau, thereby facilitating a smooth and cohesive integration. Further elaboration on Apache Drill will follow.

## Data Architecture

![link](https://github.com/princebhatt9588/eal_Time_Twitter_Trends_Analytics/assets/117750531/d4946d32-578f-4663-ac9d-a926881e750architecture.png)

In this data pipeline architecture, the Twitter streaming producer utilizes Kafka to publish real-time tweets to the 'tweets-1' topic within an Apache Kafka broker. Subsequently, the Apache Spark Streaming Context subscribes to the 'tweets-1' topic, enabling the ingestion of tweets for further processing.

The Spark engine efficiently leverages Spark Streaming to conduct batch processing on the incoming tweets. Prior to storing the processed data, Spark performs sentiment classification on the tweets. The processed results are then persistently stored in MongoDB, ensuring distributed storage and retrieval.

To facilitate the integration of MongoDB with Tableau, Apache Drill serves as the connector, establishing a seamless connection between the two systems. By tapping into real-time data from MongoDB, a live dashboard is created using Tableau, offering comprehensive insights into the popularity and sentiment of trending topics on Twitter. This live dashboard provides a powerful tool for analyzing and understanding the dynamics of popular topics as they unfold on the platform.

## System Design

### Kafka Twitter Streaming Producer:

The Kafka Twitter Streaming Producer is a crucial component responsible for publishing real-time tweets to the 'tweets-1' topic in the central Apache Kafka broker. Utilizing the twitter4j library for Twitter API integration, this producer captures streaming tweets written in English from various locations worldwide.

### Apache Kafka:

Apache Kafka serves as a distributed publish-subscribe messaging system and a robust queue, adept at handling high volumes of data. Its primary role is to facilitate the seamless transmission of messages between endpoints. Supporting both offline and online message consumption, Kafka ensures data persistence on disk and replicates messages within the cluster to ensure data integrity. It integrates seamlessly with Apache Spark for real-time streaming data analysis.

#### Kafka's Dependency: Apache Zookeeper

A critical dependency of Apache Kafka is Apache Zookeeper, which acts as a distributed configuration and synchronization service. Zookeeper acts as the coordination interface between Kafka brokers and consumers. Storing essential metadata, such as information about topics, brokers, consumer offsets, and more, Zookeeper enables Kafka to maintain a robust and fault-tolerant state, even in the event of broker or Zookeeper failure.

### Apache Spark:

Apache Spark, a high-speed and versatile distributed cluster computing framework, is the foundation of the project. It offers a rich set of higher-level tools, including Spark SQL for SQL and structured data processing, MLlib for machine learning, GraphX for graph processing, and Spark Streaming for real-time data analysis.

#### Spark Core:

The core component of Apache Spark, Spark Core, revolves around Resilient Distributed Datasets (RDDs) as the primary data abstraction. RDDs represent immutable, partitioned collections of elements that can be processed in parallel with fault tolerance. Spark's RDD lineage graph allows for re-computation of missing or damaged partitions due to node failures, ensuring fault-tolerance.

#### Spark Streaming:

Leveraging Spark Core, Spark Streaming performs real-time streaming analysis. The key abstraction used is Discretized Stream or DStream, representing continuous data streams. DStreams consist of a continuous series of RDDs, each containing data from a specific interval. The data processing involves both stateless and stateful transformations on the raw streaming tweets, preparing them for sentiment classification. 

### MongoDB:

MongoDB serves as the distributed storage and retrieval system for the processed data from Spark. The results of sentiment classification are stored persistently in MongoDB, ensuring efficient data management and scalability.

### Apache Drill:

Apache Drill, an open-source SQL execution engine, enables SQL queries on non-relational databases and file systems. In this project, Drill plays a vital role in connecting MongoDB with Tableau, facilitating smooth data integration.

### Tableau:

Tableau, a powerful data visualization tool, utilizes the real-time data stored in MongoDB to create an interactive live dashboard. This dashboard offers a comprehensive analysis of trending topics on Twitter, presenting valuable insights into their popularity and sentiment in real-time.

## Instructions to Setup Data Pipeline and DashboardCertainly! Here are the rephrased instructions without the image links:

1. **Download Required Components:**
   - Download [Zookeeper](https://www.apache.org/dyn/closer.lua/zookeeper/zookeeper-3.5.7/apache-zookeeper-3.5.7-bin.tar.gz), [MongoDB](https://docs.mongodb.com/guides/server/install/), [Apache Kafka](https://archive.apache.org/dist/kafka/2.4.0/kafka_2.12-2.4.0.tgz), [Apache Spark](https://spark.apache.org/downloads.html), and [Apache Drill](https://drill.apache.org/docs/installing-drill-on-linux-and-mac-os-x/).

2. **Optional: Instructions to Setup Spark Development Environment:**
   - Optionally, follow the instructions [here](https://kaizen.itversity.com/setup-development-environment-intellij-and-scala-big-data-hadoop-and-spark/) to set up Spark Development Environment.

3. **Clone Project Repository:**
   - Clone the project repository to your local machine.

4. **Create a Twitter Developer Account:**
   - Sign up for a Twitter developer account [here](https://developer.twitter.com/en/apply-for-access).

5. **Update Twitter API Tokens:**
   - Locate the 'oAuth-tokens.txt' file in the input directory of the project repository.
   - Update the file with your respective Twitter API keys and tokens.

6. **Start Zookeeper Server:**
   - Open a terminal and run the following command:
     ```bash
     /usr/local/zookeeper/bin/zkServer.sh start
     ```

7. **Start Kafka Server:**
   - Open another terminal and run the following command:
     ```bash
     /usr/local/kafka/bin/kafka-server-start.sh /usr/local/kafka/config/server.properties
     ```

8. **Create Kafka Topic:**
   - In a terminal, create a topic named "tweets-1" in Kafka using the following command:
     ```bash
     /usr/local/kafka/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic tweets-1
     ```

9. **Verify Topic Creation:**
   - To verify if the topic has been successfully created, run the following command:
     ```bash
     /usr/local/kafka/bin/kafka-topics.sh --list --zookeeper localhost:2181
     ```

10. **Start MongoDB Server:**
    - Start MongoDB server on your local machine.

11. **Start Apache Drill in Distributed Mode:**
    - Follow the instructions [here](https://drill.apache.org/docs/starting-drill-in-distributed-mode/) to start Apache Drill in distributed mode.

12. **Enable MongoDB Storage Plugin in Apache Drill:**
    - Configure MongoDB as a storage plugin for Apache Drill using the instructions found [here](https://drill.apache.org/docs/mongodb-storage-plugin/).

13. **Run KafkaTwitterProducer.java:**
    - Execute the KafkaTwitterProducer.java with the appropriate arguments.

14. **Run KafkaSparkProcessor.scala:**
    - Run the KafkaSparkProcessor.scala with the appropriate arguments.

15. **Configure Tableau to Connect with MongoDB via Apache Drill:**
    - Follow the instructions [here](https://help.tableau.com/current/pro/desktop/en-us/examples_apachedrill.htm) to configure Tableau and connect it to MongoDB using Apache Drill.

## Tools + IDE

Here are the tools and IDE with their download links.

- [Apache Kafka 2.4.0](https://kafka.apache.org/)
- [Apache Spark 2.4.1](https://spark.apache.org/)
- [Apache Drill 1.17.0](https://drill.apache.org/)
- [MongoDB](https://www.mongodb.com/)
- [Tableau Desktop](https://www.tableau.com/)
- [IntelliJ IDEA](https://www.jetbrains.com/idea/)
- [Java 8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)
- [Scala 2.11.12](https://www.scala-lang.org/download/2.11.12.html)

