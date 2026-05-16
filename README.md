# 🏠 Smart Home Energy Optimizer

> **Architectural Analysis Project — Team Eternal Architects**  
> Aalto University · CS-E4200 Software Architecture · Spring 2025

A software system that helps households monitor, analyze, and optimize electricity consumption through smart device integration, real-time analytics, and ML-powered recommendations.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [System Views](#system-views)
  - [Context View](#context-view)
  - [Functional View](#functional-view)
  - [Deployment View](#deployment-view)
  - [Information View](#information-view)
- [Key Requirements](#key-requirements)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Team](#team)

---

## Overview

**Eternal Architects** is developing a smart home energy management platform targeting single detached homes and housing companies. The system provides:

- 📊 **Real-time visibility** of electricity consumption and production
- 🤖 **ML-powered recommendations** for optimal device usage schedules
- ⚡ **Automated control** of smart devices based on electricity spot prices
- 🔔 **Anomaly detection** with instant user notifications
- 🔒 **Privacy-first** design with GDPR compliance and data anonymization

### Target Users

| Stakeholder | Role |
|-------------|------|
| **Homeowners / Dwellers** | Primary users — monitor and control their devices |
| **Housing Companies** | Manage electricity across multiple apartments |
| **Data Consumers** | Access anonymized, aggregated usage data |
| **Smart Appliance Manufacturers** | Integrate devices via standardized APIs |

---

## Architecture

The system is built around a **microservices architecture** deployed on embedded hardware at the household level, with cloud offloading for ML model training.

```
┌─────────────────────────────────────────────────┐
│              Embedded Hardware (Home)            │
│                                                  │
│  ┌──────────┐  ┌──────────┐  ┌───────────────┐  │
│  │ User     │  │ Device   │  │ Data Analysis │  │
│  │ Interact.│  │ Control  │  │ Service       │  │
│  └──────────┘  └──────────┘  └───────────────┘  │
│  ┌──────────┐  ┌──────────┐  ┌───────────────┐  │
│  │ Price    │  │ Elec.    │  │ Data          │  │
│  │ Collect. │  │ Data Col.│  │ Transform     │  │
│  └──────────┘  └──────────┘  └───────────────┘  │
└─────────────────────────────────────────────────┘
                        │
              ┌─────────▼──────────┐
              │   AWS (Cloud)      │
              │  Model Training    │
              │  (EC2 + S3)        │
              └────────────────────┘
```

---

## System Views

### Context View

The system interacts with five external entities:

```
Data Consumers ──────────────────────────────────┐
                                                 │
Users ──────── [manage devices] ──────────────► Our System ◄──── Smart Devices
               [request data]                    │
                                                 ├──── Electricity Suppliers
Electricity Price Provider ◄─── [request prices]─┘
```

Full diagram: [`docs/diagrams/context-view.svg`](docs/diagrams/context-view.svg)

### Functional View

The system is decomposed into the following services:

| Service | Responsibility | Technology |
|---------|---------------|------------|
| **User Interaction Service** | Backend API for all user-facing operations | Node.js |
| **Device Control Service** | Manages and controls connected smart devices | Node.js |
| **Data Transform Service** | Protocol adapter for heterogeneous smart devices | Python + Rust |
| **Data Analysis Service** | ML anomaly detection & usage recommendations | Python (scikit-learn, PyTorch) |
| **Electricity Data Collection** | Aggregates consumption/production data | Python (Pandas, NumPy) |
| **Price Collection Service** | Fetches spot prices from external APIs | Node.js |
| **AAA Service** | Authentication, Authorization & Accounting | Node.js + Passport.js |

Full diagram: [`docs/diagrams/functional-view.svg`](docs/diagrams/functional-view.svg)

### Deployment View

| Component | Hosting | Rationale |
|-----------|---------|-----------|
| All core services | Embedded hardware (home) | Proximity for BT/Zigbee; data privacy |
| Data Analysis Service | AWS EC2 | Compute-intensive ML; multi-household aggregation |
| Model Storage | AWS S3 | Durable, scalable artifact storage |
| User DB | Redis (local) | In-memory speed; lightweight settings cache |
| Electricity Usage DB | TimescaleDB (local) | Time-series optimized for sensor data |
| Price Data DB | PostgreSQL (local) | Structured relational data |
| Device State DB | MongoDB (local) | Flexible schema for heterogeneous devices |

Full diagram: [`docs/diagrams/deployment-view.svg`](docs/diagrams/deployment-view.svg)

### Information View

Data flows through the system with strict access controls:

```
Smart Device(s) ──[usage data]──► Usage Collection ──► Data Analysis ──► User
                                                              │
Price Providers ──[spot prices]──► Price Collection ──────────┘
                                                              │
                                                   [anonymized] ──► Data Consumer
```

Full diagram: [`docs/diagrams/information-view.svg`](docs/diagrams/information-view.svg)

---

## Key Requirements

### Functional Requirements

| ID | Description | Priority |
|----|-------------|----------|
| FR-01 | Collect and serve electricity data on demand, up to 6 months back | High |
| FR-02 | Automatically optimize costs by controlling smart devices | **High / High Risk** |
| FR-03 | Provide weekly consumption & production recommendations | **High / High Risk** |
| FR-04 | Detect and report abnormal electricity consumption in real-time | Medium |

### Non-Functional Requirements

| ID | Description | Priority |
|----|-------------|----------|
| NFR-01 | System must be secure (auth + encrypted comms) | High |
| NFR-03 | Interoperable with Zigbee, Bluetooth, Wi-Fi, LTE, NB-IoT | High |
| NFR-04 | Automatic recovery from failure within minutes | Medium |
| NFR-05 | Data must not be traceable to system owner (GDPR) | High |
| NFR-06 | Compliant with IEC 61850 and IEC 62351 | Medium |
| NFR-07 | Recommendations must demonstrably save money | High |

---

## Tech Stack

```
Frontend        React (web) · React Native (mobile)
Backend         Node.js · Python · Flask · FastAPI
ML / Data       scikit-learn · PyTorch · Pandas · NumPy
Databases       Redis · PostgreSQL · TimescaleDB · MongoDB
Messaging       Apache Kafka · RabbitMQ
Infrastructure  Docker · AWS EC2 · AWS S3
Protocols       Zigbee · Bluetooth · Wi-Fi 802.11 · LTE · NB-IoT
Standards       IEC 61850 · IEC 62351 · GDPR
```

---

## Project Structure

```
smart-home-system/
├── README.md
├── docs/
│   ├── diagrams/               # Architecture diagrams (SVG)
│   │   ├── context-view.svg
│   │   ├── functional-view.svg
│   │   ├── deployment-view.svg
│   │   └── information-view.svg
│   ├── adr/                    # Architectural Decision Records
│   │   └── ADR-*.md
│   └── requirements/
│       ├── functional.md
│       └── non-functional.md
├── src/
│   ├── services/
│   │   ├── user-interaction/   # Node.js REST API
│   │   ├── device-control/     # Node.js device manager
│   │   ├── data-transform/     # Python + Rust protocol adapters
│   │   ├── data-analysis/      # Python ML service (Flask)
│   │   ├── price-collection/   # Node.js price fetcher
│   │   ├── electricity-data-collection/  # Python aggregator
│   │   └── aaa/                # Auth service (Node.js + Passport.js)
│   └── frontend/
│       ├── web/                # React web app
│       └── mobile/             # React Native app
├── infrastructure/
│   ├── docker-compose.yml
│   ├── k8s/                    # Kubernetes manifests
│   └── terraform/              # AWS infrastructure
└── scripts/
    └── setup.sh
```

---

## Architectural Decision Records

Key decisions are documented in [`docs/adr/`](docs/adr/). Highlights:

- **ADR-03** — `DataTransformService` as universal device adapter
- **ADR-15** — Host `DataTransformService` on-premises (not cloud) for Bluetooth/Zigbee proximity
- **ADR-17** — Single embedded hardware node for all local services
- **ADR-19** — `DataAnalysisService` hosted on AWS for compute scale
- **ADR-23/24** — Kafka for big-data streaming; RabbitMQ for fast internal messaging

Full log: [`docs/adr/`](docs/adr/)

---

## Team

**Team Eternal Architects** — Aalto University, Spring 2025

| Name |
|------|
| Al-Tuwaijari Ahmed |
| Kianiangolafshani Sepehr |
| Mareš David |
| Porthan Richard |
| Robin Asaduzzaman |
| Seek Anakin |
| Virolainen Julius |

---

*Report date: 9 April 2025*
