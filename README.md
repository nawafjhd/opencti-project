# 🛡️ OpenCTI Threat Intelligence Ecosystem

<div align="center">

![OpenCTI Version](https://img.shields.io/badge/Platform-OpenCTI%206.x-orange?style=for-the-badge&logo=opencti)
![Docker](https://img.shields.io/badge/Docker-Enabled-blue?style=for-the-badge&logo=docker)
![Architecture](https://img.shields.io/badge/Architecture-Distributed-green?style=for-the-badge)
![License](https://img.shields.io/badge/License-Apache%202.0-red?style=for-the-badge)

**A production-ready deployment framework for OpenCTI, featuring pre-configured connectors and optimized microservices orchestration.**

[Explore Docs](https://github.com/[USER_NAME]/[REPO_NAME]/wiki) • [Report Bug](https://github.com/[USER_NAME]/[REPO_NAME]/issues) • [Request Feature](https://github.com/[USER_NAME]/[REPO_NAME]/pulls)

</div>

---

## 📖 Overview
This repository provides a streamlined, Docker-based environment for deploying the **OpenCTI** platform. It integrates essential threat intelligence connectors to automate the ingestion of indicators (IoCs), tactics (TTPs), and observables from global providers.

---

## 🏗️ System Architecture
The environment is orchestrated into three logical layers to ensure scalability and high availability:

### 1. Core Infrastructure (The Backbone)
* **Redis:** High-performance caching and state management.
* **Elasticsearch / OpenSearch:** Distributed search and analytics engine for massive datasets.
* **MinIO:** S3-compatible object storage for reports and artifacts.
* **RabbitMQ:** Message broker for asynchronous task distribution between the platform and workers.

### 2. Processing Layer (The Brain)
* **OpenCTI Platform:** Central API and management console.
* **Workers:** Python-based consumers that process incoming data from RabbitMQ (Configured with 3 scalable instances).

### 3. Data Ingestion Layer (Connectors)
| Category | Connector | Data Source | Purpose |
| :--- | :--- | :--- | :--- |
| **External** | `AlienVault OTX` | AlienVault | Global IoC ingestion (IPs, Domains) |
| **External** | `MalwareBazaar` | Abuse.ch | Malware samples & hash metadata |
| **External** | `URLhaus` | Abuse.ch | Malicious URL tracking |
| **Enrichment**| `CrowdSec` | CrowdSec | Real-time IP reputation & MITRE mapping |
| **Library** | `MITRE ATT&CK` | MITRE | Enterprise/Mobile/ICS Tactics & Techniques |
| **Vulnerability**| `CVE (NVD)` | NIST | Real-time vulnerability synchronization |

---

## 📂 Project Structure
```bash
.
├── 🐳 docker-compose.yml           # Primary production stack
├── 🛠️ docker-compose.dev.yml       # Development & debugging environment
├── 🔎 docker-compose.opensearch.yml # OpenSearch alternative configuration
├── ⚙️ rabbitmq.conf                # Optimized message broker policy
├── 🤖 renovate.json                # Automated dependency management
└── 📂 assets/                      # Technical diagrams and visual resources
