# 🚀 Data Extraction & Automation Engine

<div align="center">

[![Type](https://img.shields.io/badge/Project-Architecture%20%26%20Reference%20Blueprint-blue?style=for-the-badge)](https://github.com/coutinhoicaro/data-extraction-automation-engine)
[![Python](https://img.shields.io/badge/Language-Python%203.11%2B%20%7C%20AsyncIO-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Queue](https://img.shields.io/badge/Queue-Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white)](https://redis.io/)
[![Database](https://img.shields.io/badge/Database-PostgreSQL-336791?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![License](https://img.shields.io/badge/License-MIT-black?style=for-the-badge)](LICENSE)

<br>

**Distributed Web Scraping & Multimedia Data Extraction Architecture**  
*An asynchronous pipeline designed for high-volume data collection, proxy pool rotation, retry handling, and automated media transcript ingestion.*

</div>

---

## 📌 Overview

Scraping data at scale requires handling rate limits, IP blocks, and slow responses without crashing.

This project implements an **asynchronous distributed extraction architecture**:
* Uses **residential proxy rotation** and exponential backoff to handle rate limits cleanly.
* Queues tasks in **Redis** with worker pools processing jobs concurrently.
* Integrates **FFmpeg** to extract audio from video assets and transcribe them into **PostgreSQL**.

---

## 🏗️ System Architecture

```mermaid
flowchart TD
    subgraph Layer1 ["1. Proxy & Ingestion"]
        SCRAPERS["Async Scraper Tasks"] --> PROXY["Proxy Rotation & Rate Limiter"]
        PROXY --> BROKER["Redis Task Queue"]
    end

    subgraph Layer2 ["2. Workers & Processing"]
        BROKER --> WORKERS["Async Python Workers"]
        WORKERS --> RETRY{"Request Success?"}
        RETRY -- "Retry" --> BROKER
        RETRY -- "Success" --> MEDIA["Audio Extraction (FFmpeg)"]
        MEDIA --> SPEECH["Speech-to-Text Transcription"]
    end

    subgraph Layer3 ["3. Storage"]
        SPEECH --> DB[("PostgreSQL Analytics Database")]
    end

    style SCRAPERS fill:#1e293b,stroke:#3b82f6,stroke-width:2px,color:#fff
    style BROKER fill:#1e293b,stroke:#ef4444,stroke-width:2px,color:#fff
    style WORKERS fill:#1e293b,stroke:#8b5cf6,stroke-width:2px,color:#fff
    style DB fill:#1e293b,stroke:#10b981,stroke-width:2px,color:#fff
```

---

## 🌟 Core Modules

1. **Browser & Scraping Automation (`src/browser_automation_engine.py`):** Asynchronous web extraction handling dynamic content and delays.
2. **Distributed Task Worker (`src/distributed_task_worker.py`):** Worker loop managing proxy health and retry backoffs.
3. **Media & Transcript Ingestion (`src/video_pipeline_orchestrator.py`):** Extracts audio tracks and formats text for database storage.

---

## 📂 Repository Structure

```
data-extraction-automation-engine/
├── src/
│   ├── browser_automation_engine.py        # Async Browser & Extraction Logic
│   ├── distributed_task_worker.py          # Redis Worker & Proxy Rotation
│   └── video_pipeline_orchestrator.py      # Media ETL & Transcript Ingestion
├── docker-compose.yml                      # Local Environment Setup Sample
├── requirements.txt                        # Dependencies
├── LICENSE                                 # MIT License
└── README.md                               # Architecture Overview
```

> **Note:** This repository is an **architectural reference implementation**. Production proxy credentials and target scraper configurations are managed in private environments.

---

## 📄 License
Distributed under the **MIT License**. See `LICENSE` for details.
