# Stock Market Real-Time Processing with Apache Kafka

## Project Overview
This project implements a real-time stock market data processing system using **Apache Kafka**. The system ingests stock market data, processes it, and enables real-time consumption.

### **Architecture**
![Project Architecture](Architecture.jpg)

We are going to use different technologies such as Python, Amazon Web Services (AWS), S3 (Simple Storage Service), Apache Kafka, Glue Crawler, Glue Catalog, EC2, Athena, and SQL.

## **Dataset Used**
https://github.com/Chaimaaorg/Real-Time-Stock-Analysis/blob/main/stockData.csv

## **Setup Instructions**
This guide provides installation and configuration steps for both **Windows** and **Linux (EC2 instances)**.

---
## **1. Install Java (Required for Kafka)**
### **Windows:**
1. Download and install Java 8 from [Oracle](https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html) or [AdoptOpenJDK](https://adoptopenjdk.net/).
2. Verify installation:
   ```sh
   java -version
   ```

### **Linux (EC2 - Amazon Linux 2/Ubuntu):**
```sh
sudo yum install java-1.8.0-openjdk -y  # Amazon Linux
sudo apt install openjdk-8-jdk -y       # Ubuntu/Debian
java -version
```

---
## **2. Download & Extract Kafka**
### **Windows:**
1. Download Kafka:
   ```powershell
   Invoke-WebRequest -Uri "https://downloads.apache.org/kafka/3.3.1/kafka_2.12-3.3.1.tgz" -OutFile "kafka.tgz"
   ```
2. Extract Kafka (Use 7-Zip or WinRAR):
   ```powershell
   tar -xvf kafka.tgz
   ```
3. Navigate to Kafka directory:
   ```powershell
   cd kafka_2.12-3.3.1
   ```

### **Linux (EC2):**
```sh
wget https://downloads.apache.org/kafka/3.3.1/kafka_2.12-3.3.1.tgz
tar -xvf kafka_2.12-3.3.1.tgz
cd kafka_2.12-3.3.1
```

---
## **3. Start ZooKeeper**
ZooKeeper is required for Kafka to run.

### **Windows:**
```powershell
bin\windows\zookeeper-server-start.bat config\zookeeper.properties
```

### **Linux (EC2):**
```sh
bin/zookeeper-server-start.sh config/zookeeper.properties
```

---
## **4. Start Kafka Broker**
### **Windows:**
```powershell
bin\windows\kafka-server-start.bat config\server.properties
```

### **Linux (EC2):**
```sh
export KAFKA_HEAP_OPTS="-Xmx256M -Xms128M"
bin/kafka-server-start.sh config/server.properties
```

> **Note:** To make Kafka accessible via public IP, edit `config/server.properties` and update `ADVERTISED_LISTENERS` to your EC2 instance's public IP:
```sh
nano config/server.properties
# Change
advertised.listeners=PLAINTEXT://your-public-ip:9092
```
Restart Kafka after making this change.

---
## **5. Create a Kafka Topic**
### **Windows:**
```powershell
bin\windows\kafka-topics.bat --create --topic stock_market_data --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1
```

### **Linux (EC2):**
```sh
bin/kafka-topics.sh --create --topic stock_market_data --bootstrap-server your-public-ip:9092 --replication-factor 1 --partitions 1
```

---
## **6. Start a Producer**
### **Windows:**
```powershell
bin\windows\kafka-console-producer.bat --topic stock_market_data --bootstrap-server localhost:9092
```

### **Linux (EC2):**
```sh
bin/kafka-console-producer.sh --topic stock_market_data --bootstrap-server your-public-ip:9092
```
> **Type messages and press Enter** to send data.

---
## **7. Start a Consumer**
### **Windows:**
```powershell
bin\windows\kafka-console-consumer.bat --topic stock_market_data --bootstrap-server localhost:9092
```

### **Linux (EC2):**
```sh
bin/kafka-console-consumer.sh --topic stock_market_data --bootstrap-server your-public-ip:9092
```
> The consumer will display messages sent by the producer in real-time.

---
## **8. Stop Services**
### **Windows:**
```powershell
bin\windows\kafka-server-stop.bat
bin\windows\zookeeper-server-stop.bat
```

### **Linux (EC2):**
```sh
bin/kafka-server-stop.sh
bin/zookeeper-server-stop.sh
```

---
## **Troubleshooting**
- If Kafka fails to start, check logs in `logs/kafka-server.log`.
- Ensure Java is installed and properly set in `JAVA_HOME`.
- If Kafka is not accessible externally, verify `ADVERTISED_LISTENERS`.

---
## **Conclusion**
This setup enables real-time stock market data processing using Kafka. You can extend this project by integrating it with **Spark Streaming, Flink, or a Database for further analysis**.

Happy coding! 🚀

