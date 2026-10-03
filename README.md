# 🐳 Real-Time Iris Inference Pipeline (Docker, FastAPI & Apache Kafka)

A production-grade **Machine Learning Operations (MLOps)** repository demonstrating how to deploy, containerize, and orchestrate a real-time, event-driven streaming pipeline for model predictions.

Instead of deploying a standard, synchronous API that bottlenecks under high traffic, this architecture utilizes a distributed message broker to process inference requests asynchronously via a multithreaded consumer runtime worker.

## 🏗️ Architecture Design & Component Layout
* **Model Training Layer:** Trains a `LogisticRegression` classifier on the classic **Iris dataset** (predicting species classification), serializing the model weights securely via `joblib`.
* **FastAPI Service Layer:** Wraps the trained model in a high-performance web interface providing a fallback REST endpoint (`/predict`) validated via `Pydantic` data structures.
* **Asynchronous Kafka Workers:** A multithreaded background process using `threading.Thread` that actively streams payloads:
  * Listens continuously to incoming transactions on the `ml-requests` topic channel.
  * Processes features instantly through the loaded model matrix weights.
  * Publishes completed integer classification results immediately out to the `ml-predictions` output topic.

## 🛠️ The MLOps Tech Stack
* **Distributed Stream Broker:** Apache Kafka / Apache Zookeeper
* **Web & Validation API:** FastAPI / Pydantic / Uvicorn
* **Container Layer:** Docker & Docker Compose
* **Machine Learning Library:** Scikit-Learn (Logistic Regression Engine)
* **Serialization Broker:** Joblib / NumPy

## 📦 Ingestion Workflow
1. **Simulation:** A streaming simulation engine publishes structural vector data arrays (e.g., `[5.1, 3.5, 1.4, 0.2]`) directly to the broker.
2. **Buffering & Queueing:** The Kafka server receives raw coordinates on the `ml-requests` track topic, protecting microservices from dropping requests during surge spikes.
3. **Multithreaded Processing:** A daemon thread worker extracts payloads from the partition log, transforms data structures using NumPy arrays, runs model evaluations, and passes results forward.
4. **Result Routing:** Classifications are pushed down to `ml-predictions` for downstream consumer ingestion channels to act upon.
