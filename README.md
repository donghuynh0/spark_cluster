# 🚀 Apache Spark Cluster using Docker

- Docker installed on your machine

## ⚙️ Start Spark Master

```bash
  docker run -d \
  --name spark-master \
  --network host \
  -e SPARK_MASTER_HOST=<MASTER_IP> \
  apache/spark:3.5.0 \
  /opt/spark/bin/spark-class org.apache.spark.deploy.master.Master \
  --host <MASTER_IP> \
  --port 7077 \
  --webui-port 8080

Replace <MASTER_IP> with the IP address of the machine hosting the Spark Master (e.g., 192.168.80.143).
The Master node will be accessible via port 7077 for cluster communication and port 8080 for the Spark Web UI.
---

## ⚙️ Start Spark Master

```bash
docker run -d \
  --name spark-worker-1 \
  --network host \
  -e SPARK_LOCAL_IP=<WORKER_IP> \
  apache/spark:3.5.0 \
  /opt/spark/bin/spark-class org.apache.spark.deploy.worker.Worker \
  spark://<MASTER_IP>:7077

Replace <WORKER_IP> with the IP address of the machine hosting the Worker (e.g., 192.168.80.124).
Replace <MASTER_IP> with the IP address of the Spark Master node.
Customize the container name by modifying --name spark-worker-1 to a unique name (e.g., spark-worker-2 for additional worker



