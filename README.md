# 🚀 Hadoop Multi-node in Docker

## 🔧 Docker Group Permissions

```bash
sudo usermod -aG docker $USER
newgrp docker

```

### 🔍 Let's break it down:

-   `sudo usermod -aG docker $USER`  
    Adds your current user to the `docker` group.
    
-   `-aG` = append the user to the Group (`docker` group in this case).
    
-   `$USER` = the current logged-in username (e.g., `aparna` on your system).
    

✅ This allows your user to run Docker commands without needing `sudo`.

----------

## 🌐 Create a Docker Network

```bash
docker network create hadoop

```

### ✅ Why create a Docker network?

When running multiple Docker containers (e.g., in a Hadoop cluster: **NameNode**, **DataNode**, **ResourceManager**, etc.), you want them to:

-   Communicate by **container name** (e.g., `namenode` can ping `datanode`).
    
-   Stay **isolated** from other containers or networks on your machine.
    
-   Use **Docker's internal DNS** for easy hostname resolution.
    

----------

## 🧠 Example Use Case — Hadoop Cluster Setup

```bash
# Create a shared network for Hadoop components
docker network create hadoop

# Start NameNode container on that network
docker run -d --name namenode --network hadoop hadoop-namenode-image

# Start DataNode container on the same network
docker run -d --name datanode --network hadoop hadoop-datanode-image

```

✅ Now, `datanode` can connect to `namenode:8020` without needing IP addresses.

----------

## 🌉 Network Features Comparison

Feature

bridge (default)

User-defined (like `hadoop`)

Containers talk by name

❌ No

✅ Yes

DNS-based service discovery

❌ No

✅ Yes

Manual control

❌ Limited

✅ Full

----------

## 🔍 To verify your network:

```bash
docker network ls

```

Sample output:

```
NETWORK ID     NAME      DRIVER    SCOPE
abcdefgh1234   hadoop    bridge    local

```

----------

## 📁 1. Project Folder Structure

```bash
mkdir hadoop-docker-cluster
cd hadoop-docker-cluster

```

----------

## 🛠️ 2. `docker-compose.yml` File

Create a file named `docker-compose.yml`:

```yaml
version: '3'

services:
  namenode:
    image: bde2020/hadoop-namenode:2.0.0-hadoop3.2.1-java8
    container_name: namenode
    ports:
      - "9870:9870"   # Web UI
      - "9000:9000"   # IPC
    environment:
      - CLUSTER_NAME=HadoopCluster
    volumes:
      - hadoop_namenode:/hadoop/dfs/name
    networks:
      - hadoop

  datanode:
    image: bde2020/hadoop-datanode:2.0.0-hadoop3.2.1-java8
    container_name: datanode
    ports:
      - "9864:9864"   # Web UI
    environment:
      - CLUSTER_NAME=HadoopCluster
      - CORE_CONF_fs_defaultFS=hdfs://namenode:9000
    volumes:
      - hadoop_datanode:/hadoop/dfs/data
    networks:
      - hadoop
    depends_on:
      - namenode

volumes:
  hadoop_namenode:
  hadoop_datanode:

networks:
  hadoop:
    driver: bridge

```

----------

## ▶️ 3. Start the Cluster

```bash
docker-compose up -d

```

----------

## 🌐 4. Access Hadoop Web UI

Open your browser:

-   **NameNode UI:** [http://localhost:9870](http://localhost:9870/)
    
-   **DataNode UI:** [http://localhost:9864](http://localhost:9864/)
    

----------

## 💾 5. Interact with HDFS

Enter the NameNode container:

```bash
docker exec -it namenode bash

```

Run HDFS commands:

```bash
hdfs dfs -mkdir /test
hdfs dfs -put /etc/hosts /test
hdfs dfs -ls /test

```

----------

## 📊 6. Run MapReduce WordCount Job

Still inside the `namenode` container:

```bash
cd $HADOOP_HOME
hdfs dfs -mkdir /input
hdfs dfs -put etc/hadoop/*.xml /input

hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-*.jar wordcount /input /output

hdfs dfs -cat /output/part-r-00000

```

----------

## 🧹 7. Tear Down

To stop and remove everything:

```bash
docker-compose down

```

To remove volumes too:

```bash
docker-compose down -v

```

----------