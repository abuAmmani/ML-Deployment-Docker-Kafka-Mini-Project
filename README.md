# 🐳 Real-Time ML Inference Pipeline (Docker & Apache Kafka Mini-Project)

A high-performance **Machine Learning Operations (MLOps)** showcase demonstrating how to containerize, orchestrate, and deploy an event-driven, real-time machine learning inference pipeline.

Instead of traditional, synchronous REST APIs which suffer from high latency and scaling bottlenecks under peak traffic, this system utilizes a distributed event-streaming message broker. It allows the machine learning worker microservice to consume input features and publish model classifications asynchronously with ultra-low latency.

## 🏗️ Architecture Design & Component Layout
* **Event Ingestion Layer:** Uses **Apache Kafka** to manage real-time event messaging topics, safely buffering incoming inference payload streams.
* **Service Containerization:** Orchestrated using **Docker** and multi-container environment configurations (`docker-compose.yml`) to ensure rapid deployment scaling and 100% environment reproducibility.
* **Asynchronous ML Worker:** A dedicated Python microservice that listens to the input event stream, processes features, triggers serialized model weights, and emits streaming prediction outputs.

## 🛠️ Technical Stack
* **Distributed Stream Broker:** Apache Kafka / Apache Zookeeper
* **Container Layer:** Docker & Docker Compose
* **Orchestration Runtime:** Python 3
* **Machine Learning Ingestion:** Serialized ML Model Component (Scikit-Learn / TensorFlow / PyTorch)

## 📦 Core Pipeline Ingestion Workflow

1. **Feature Generation:** Data producers simulate live user events or business transactions by publishing payloads directly into a Kafka input topic channel.
2. **Event Buffering:** Kafka log partitions distribute the stream dynamically, ensuring high concurrent request handling without dropping data packets.
3. **Container Ingestion:** The containerized machine learning consumer service reads raw streaming features, applies standard structural transformations, and invokes model inference.
4. **Result Routing:** Classifications, scores, or anomalies are routed immediately to a distinct Kafka output topic for consuming client applications to act upon instantly.

## 💻 Local Workspace Startup Setup

1. **Clone this repository onto your machine:**
   ```bash
   git clone https://github.com
   cd ML-Deployment-Docker-Kafka-Mini-Project
   ```

2. **Launch the distributed container stack:**
   Ensure your local Docker engine dashboard is running, then run:
   ```bash
   docker-compose up --build
   ```
   *(This builds your custom inference worker image, pulls official Kafka/Zookeeper images from Docker Hub, initializes isolated virtual networks, and sets the entire real-time streaming pipeline live locally).*
