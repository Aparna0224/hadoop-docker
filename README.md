🐘 Hadoop Multi-Node Cluster with Docker Compose
Set up a fully working Hadoop cluster using Docker and Docker Compose on your local machine. This setup includes:

✅ 1 NameNode

✅ 2 DataNodes

✅ Easy HDFS access and MapReduce job execution

✅ Web UIs for monitoring Hadoop services

✅ Tested on Ubuntu with Docker and Docker Compose
🎯 Perfect for students, researchers, and developers learning Big Data

📂 Repository Structure
pgsql
Copy
Edit
hadoop-docker-cluster/
│
├── docker-compose.yml         # Cluster configuration
├── README.md                  # This documentation
🔧 Prerequisites
Make sure you have:

Docker installed

Docker Compose installed

A Linux system (recommended: Ubuntu 22.04+)

🚀 Quick Start
1️⃣ Clone the Repository
bash
Copy
Edit
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
This will spin up:

🧠 NameNode at http://localhost:9870

💾 DataNode 1 at http://localhost:9864

💾 DataNode 2 at http://localhost:9865

To check:

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
Step 1: Enter the NameNode Container
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
Inside the namenode container:

bash
Copy
Edit
cd $HADOOP_HOME

# Prepare input
hdfs dfs -mkdir /input
hdfs dfs -put etc/hadoop/*.xml /input

# Run job
hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-*.jar wordcount /input /output

# Check output
hdfs dfs -cat /output/part-r-00000
🧹 Shutting Down the Cluster
From your host terminal:

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
Ensure the hadoop network exists:

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
Use docker-compose logs -f namenode to see real-time logs

You can scale DataNodes by modifying the docker-compose.yml

Clean /output directory on HDFS before re-running WordCount

👩‍💻 Author
Aparna J
🧠 Passionate about Big Data, Cloud, and AI
🎓 Project for learning Hadoop and Docker integration

📜 License
This project is licensed under the MIT License – use it, modify it, and share it.
