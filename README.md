# Webserver & Application Health Monitoring using Prometheus and Grafana

This project demonstrates how to monitor a Java-based web application using **Prometheus** and **Grafana**. The setup involves two EC2 instances: one for the application and another for monitoring.

---

## 📋 Requirements

- Two AWS EC2 Instances (can be `t2.micro`):
  - **Application Server**
  - **Monitoring Server**
- A sample application (used here: [BoardGame App](https://github.com/jaiswaladi246/Boardgame.git))

---

## 🛠️ Setup Steps

### 1. Launch EC2 Instances

- Launch two EC2 Linux instances.
- Designate one as the **Application Server** and the other as the **Monitoring Server**.

---

## 📦 Application Server Setup

### Install Java and Maven

```bash
sudo apt update
sudo apt install openjdk-17-jre-headless -y
sudo apt install maven -y
```

### Clone and Build Application

```bash
git clone https://github.com/jaiswaladi246/Boardgame.git
cd Boardgame
mvn package
```

### Run the Application

```bash
cd target
java -jar boardgame.jar &
```

- Test the application at: `http://<Application-Server-IP>:<App-Port>`

---

### Install Node Exporter

```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.9.1/node_exporter-1.9.1.linux-amd64.tar.gz
tar -xvf node_exporter-1.9.1.linux-amd64.tar.gz
cd node_exporter-1.9.1.linux-amd64
./node_exporter &
```

- Node Exporter runs on port `9100` by default.

---

## 📈 Monitoring Server Setup

### Install Prometheus

```bash
wget https://github.com/prometheus/prometheus/releases/download/v3.5.0/prometheus-3.5.0.linux-amd64.tar.gz
tar -xvf prometheus-3.5.0.linux-amd64.tar.gz
cd prometheus-3.5.0.linux-amd64
./prometheus &
```

- Access Prometheus at: `http://<Monitoring-Server-IP>:9090`

### Install Grafana

```bash
sudo apt-get update
sudo apt-get install -y adduser libfontconfig1 musl
wget https://dl.grafana.com/enterprise/release/grafana-enterprise_12.1.0_amd64.deb
sudo dpkg -i grafana-enterprise_12.1.0_amd64.deb
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```

- Access Grafana at: `http://<Monitoring-Server-IP>:3000`

---

## ✅ Final Check

- Ensure the Java application is accessible via its public IP and port.
- Verify Prometheus is scraping Node Exporter metrics from the Application Server.
- Use Grafana to visualize metrics from Prometheus.

---

## 📎 Notes

- Ensure appropriate **security group rules** are set for:
  - App port (e.g., 8080)
  - Prometheus (9090)
  - Node Exporter (9100)
  - Grafana (3000)
