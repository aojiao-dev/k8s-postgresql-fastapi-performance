# k8s-postgresql-fastapi-performance 🚀

Have you ever wondered how much traffic/load your application can handle? This repo contains a hands-on project for learning performance tuning concepts by optimizing a common web stack: FastAPI, PostgreSQL, and Kubernetes. The goal is to systematically identify bottlenecks and measure the impact of optimizations on the system's maximum Queries Per Second (QPS).

---

## 🎯 Project Goal

The primary objective is to **maximize the QPS** a single FastAPI application pod can handle for a user profile API. The project intentionally avoids horizontal scaling (adding more pods) to force optimization of the application, database, and underlying resource configuration.

We will benchmark two primary workload types:

- **Read-Heavy:** 90% read operations (fetch user profile) and 10% write operations (update user email).
- **Write-Heavy:** 10% read operations and 90% write operations.

---

## ⚙️ Architecture

The architecture consists of three main components running in a Kubernetes cluster:

1.  **FastAPI Application:** A Python application serving a simple `/users/{user_id}` API with GET (read) and PATCH (write) methods.
2.  **PostgreSQL Database:** A single PostgreSQL instance that stores the `users` table. The FastAPI application communicates with this database.
3.  **Load Generator:** A separate pod (e.g., using Locust, k6, or JMeter) that runs on a different node to simulate user traffic and measure QPS, latency, and error rates. This ensures the load generator's resource consumption does not interfere with the application's performance.

## 📈 Methodology

The project follows a systematic, iterative approach to performance tuning:

1.  **Establish a Baseline:** Deploy the "naive" version of the application with no optimizations (e.g., no connection pooling, no database indexes, default K8s resources). Run the load tests and record the baseline QPS.
2.  **Formulate a Hypothesis:** Identify a potential bottleneck (e.g., "Lack of connection pooling is causing high latency").
3.  **Apply One Optimization:** Implement a single change, such as adding a connection pool.
4.  **Test & Measure:** Re-run the exact same load test and compare the new QPS against the baseline.
5.  **Analyze & Repeat:** Document the result and move on to the next potential bottleneck.

### Results

| Optimization Applied                | Read-Heavy QPS | Write-Heavy QPS | p99 Latency (ms) | Notes |
| ----------------------------------- | :------------: | :-------------: | :--------------: | ----- |
| **Baseline (Naive Implementation)** |      N/A       |       N/A       |       N/A        |       |

---
