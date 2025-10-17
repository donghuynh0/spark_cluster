# Spark Cluster using Docker

**Prerequisite:**

Docker must be installed on your machine.

---

config static ip

```bash
sudo nmcli connection modify "LabSoe" ipv4.addresses "192.168.80.52/24"
sudo nmcli connection modify "LabSoe" ipv4.gateway "192.168.80.254"
sudo nmcli connection modify "LabSoe" ipv4.dns "10.200.200.1,8.8.8.8"
sudo nmcli connection modify "LabSoe" ipv4.method manual
sudo nmcli connection down "LabSoe" && sudo nmcli connection up "LabSoe"
```

## ⚙️ Start Spark Master

```bash
docker run -d \
  --name spark-master \
  --network host \
  -e SPARK_MASTER_HOST=<MASTER_IP> \
   donghuynh0/spark-python311-amd64:3.5.7  \
  /opt/spark/bin/spark-class org.apache.spark.deploy.master.Master \
  --host <MASTER_IP> \
  --port 7077 \
  --webui-port 8080
```

- Replace `<MASTER_IP>` with your spark master ip
- Communicate via 7077
- 🌐 Web: ip:8080

---

## ⚙️ Start Spark Worker

```bash
docker run -d \
  --name spark-worker \
  --network host \
  -e SPARK_LOCAL_IP=<WORKER_IP> \
   donghuynh0/spark-python311-amd64:3.5.7  \
  /opt/spark/bin/spark-class org.apache.spark.deploy.worker.Worker \
  spark://<MASTER_IP>:7077
```

- Replace `<WORKER_IP>` with your spark worker ip
- Replace `<MASTER_IP>` with your spark master ip
- Customize the container name by modifying `--name spark-worker`
