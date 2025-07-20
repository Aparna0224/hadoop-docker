# 🐘 Hadoop Multi-Node Cluster with Docker Compose

Set up a fully functional **Hadoop cluster** using **Docker** and **Docker Compose** on your local machine. This setup includes:

✅ 1 NameNode  
✅ 2 DataNodes  
✅ HDFS access and MapReduce job execution  
✅ Web UIs for monitoring Hadoop services  

🎯 **Perfect for** students, researchers, and developers learning Big Data.

---

## 📂 Repository Structure

hadoop-docker-cluster/
│
├── docker-compose.yml # Cluster configuration
├── README.md # Project documentation

yaml
Copy
Edit

---

## 🔧 Prerequisites

Ensure you have the following installed:

- Docker  
- Docker Compose  
- Linux system (Recommended: Ubuntu 22.04+)

---

## 🚀 Quick Start

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/your-username/hadoop-docker-cluster.git
cd hadoop-docker-cluster
2️⃣ Create a Custom Docker Network
bash
Copy
Edit
docker network create hadoop
3️⃣ Launch the Cluster
bash
Copy
Edit
docker-compose up -d
This starts:

🧠 NameNode at http://localhost:9870

💾 DataNode 1 at http://localhost:9864

💾 DataNode 2 at http://localhost:9865

To verify:

bash
Copy
Edit
docker ps
🌐 Hadoop Web Interfaces
Component	Port	URL
NameNode	9870	http://localhost:9870
DataNode 1	9864	http://localhost:9864
DataNode 2	9865	http://localhost:9865

🛠️ Interacting with HDFS
Step 1: Access the NameNode Container
bash
Copy
Edit
docker exec -it namenode bash
Step 2: Run Basic HDFS Commands
bash
Copy
Edit
hdfs dfs -mkdir /test
hdfs dfs -put /etc/hosts /test
hdfs dfs -ls /test
🧪 Run WordCount MapReduce Job
Inside the NameNode container:

bash
Copy
Edit
cd $HADOOP_HOME

# Prepare input
hdfs dfs -mkdir /input
hdfs dfs -put etc/hadoop/*.xml /input

# Run job
hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-*.jar wordcount /input /output

# View output
hdfs dfs -cat /output/part-r-00000
🧹 Shutting Down the Cluster
To stop the cluster:

bash
Copy
Edit
docker-compose down
To remove volumes as well:

bash
Copy
Edit
docker-compose down -v
🧾 Docker Network Verification
Ensure the custom Docker network hadoop exists:

bash
Copy
Edit
docker network ls
Expected output:

sql
Copy
Edit
NETWORK ID     NAME      DRIVER    SCOPE
abcdefgh1234   hadoop    bridge    local
📌 Tips & Reminders
Use docker-compose logs -f namenode to stream NameNode logs.

You can scale DataNodes by editing the docker-compose.yml file.

Clean HDFS /output before rerunning the WordCount job.

👩‍💻 Author
Aparna J
🧠 Passionate about Big Data, Cloud, and AI
🎓 Project for learning Hadoop and Docker integration

📜 License
This project is licensed under the MIT License – feel free to use, modify, and share!
