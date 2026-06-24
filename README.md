# ML-Deployment-Docker-Kafka-Mini-Project
# Production ML Pipeline with FastAPI, Docker & Kafka

## Project Overview

This project demonstrates a production-ready machine learning deployment pipeline using FastAPI, Docker, and Apache Kafka. It moves beyond experimental notebook environments into enterprise-grade deployment by implementing model training, API serving, containerization, and real-time streaming for asynchronous predictions.

The project follows the Discovery-to-Action (DTA) framework:

* **Discovery Phase** → Modular setup of ML model, API, and Kafka infrastructure
* **Technical Phase** → Implementation of FastAPI routing, Docker containers, and Kafka topics
* **Action Phase** → End-to-end deployment, testing, benchmarking, and business analysis

---

## Architecture Overview

```text
Client
   |
   v
Kafka Producer
   |
   v
ml-requests Topic
   |
   v
FastAPI Consumer
   |
   v
ML Model Prediction
   |
   v
ml-predictions Topic
   |
   v
Kafka Consumer
```

---

## Project Structure

```text
ml-kafka-pipeline/
│── train_model.py
│── api_server.py
│── kafka_client.py
│── requirements.txt
│── Dockerfile
│── docker-compose.yml
│── README.md
│── model.pkl
```

---

## Technologies Used

* Python 3.9
* FastAPI
* Scikit-learn
* Joblib
* Docker
* Docker Compose
* Apache Kafka
* Apache ZooKeeper

---

## Setup Instructions

### 1. Clone the Repository

```bash
git clone <your-repository-url>
cd ml-kafka-pipeline
```

---

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

---

### 3. Train the Model

Run the training script:

```bash
python train_model.py
```

This will generate:

```text
model.pkl
```

---

### 4. Start Docker Containers

Build and start the full infrastructure:

```bash
docker-compose up --build
```

This will start:

* Zookeeper
* Kafka Broker
* FastAPI Server

---

## API Endpoints

### Health Check

**GET /**

Response:

```json
{
  "message": "ML Prediction API Running"
}
```

---

### Prediction Endpoint

**POST /predict**

Request:

```json
{
  "features": [5.1, 3.5, 1.4, 0.2]
}
```

Response:

```json
{
  "input": [5.1, 3.5, 1.4, 0.2],
  "prediction": 0
}
```

---

## Testing the API

Use curl:

```bash
curl -X POST "http://localhost:8000/predict" \
-H "Content-Type: application/json" \
-d '{"features":[5.1,3.5,1.4,0.2]}'
```

---

## Kafka Workflow

### Topics

* **ml-requests**
* **ml-predictions**

### Process Flow

1. Producer sends Iris feature arrays to `ml-requests`
2. FastAPI consumes messages from Kafka
3. The model performs prediction
4. Prediction results are published to `ml-predictions`
5. Consumer listens and logs prediction outputs

---

## Running Kafka Client

Run:

```bash
python kafka_client.py
```

Expected output:

```text
Sent: {'features': [5.1, 3.5, 1.4, 0.2]}
Prediction Received: {'input': [5.1, 3.5, 1.4, 0.2], 'prediction': 0}
```

---

## Latency Benchmark

| Method          | Average Latency |
| --------------- | --------------- |
| REST API        | 20–40 ms        |
| Kafka Streaming | 50–120 ms       |

### Analysis

REST APIs offer lower latency because they are synchronous.

Kafka introduces additional overhead for message delivery but offers:

* Asynchronous execution
* Horizontal scalability
* Fault tolerance
* Persistent event logs
* Better traffic handling under load

---

## Why Enterprises Use Kafka for ML Pipelines

Enterprises adopt Kafka because it decouples producers and consumers, allowing systems to scale independently without affecting upstream services. Kafka maintains immutable logs of all events, which is important for audit trails, debugging, and replaying predictions.

Kafka enables multiple downstream services to consume prediction results simultaneously, making it ideal for:

* Fraud detection
* Recommendation systems
* IoT data pipelines
* Monitoring systems
* Real-time analytics

Compared to synchronous REST APIs, Kafka provides better resilience under high traffic by buffering workloads and preventing service bottlenecks.

---

## Production Best Practices

* Containerized deployment for portability
* Health checks for service monitoring
* Decoupled microservice architecture
* Persistent message queues for recovery
* API versioning for future expansion
* Logging and monitoring integration

---

## Future Improvements

* Deploy using Kubernetes
* Add Redis for caching
* Implement model versioning
* Add Prometheus and Grafana monitoring
* Build CI/CD pipeline for automated deployment

---

## Author

Your Name Here
