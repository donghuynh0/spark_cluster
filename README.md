# Spark Cluster using Docker

**Prerequisite:**

Docker must be installed on your machine.

---

config static ip

```bash
sudo nmcli connection modify "<wifi-name>" ipv4.addresses "<actual_ip>/24"
sudo nmcli connection modify "<wifi-name>" ipv4.gateway "192.168.80.254"
sudo nmcli connection modify "<wifi-name>" ipv4.dns "10.200.200.1,8.8.8.8"
sudo nmcli connection modify "<wifi-name>" ipv4.method manual
sudo nmcli connection down "<wifi-name>" && sudo nmcli connection up "<wifi-name>"
```

config /etc/hosts
```bash
sudo nano /etc/hosts
```
at master: add all worker
```bash
<worker-ip> spark-worker-ip
<worker-ip> spark-worker-ip
```

at worker: add master
```bash
<master-ip> spark-master
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
sudo mkdir -p /opt/spark/work
sudo chmod -R 777 /opt/spark/work

```

```bash
docker run -d \
  --name spark-worker-192 \
  --network host \
  -e SPARK_LOCAL_IP=192.168.80.192 \
  -v /opt/spark/work:/opt/spark/work \
  donghuynh0/spark-python311-amd64:3.5.7 \
  /opt/spark/bin/spark-class org.apache.spark.deploy.worker.Worker \
  spark://192.168.80.55:7077
```

- Replace `<WORKER_IP>` with your spark worker ip
- Replace `<MASTER_IP>` with your spark master ip
- Customize the container name by modifying `--name spark-worker`
