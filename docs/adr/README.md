# Architectural Decision Records

This document logs all architectural decisions made during the design of the Smart Home Energy Optimizer. Each ADR follows the template:

> *In the context of `<ASR>`, facing concern `<concern>`, we decided for `<option>` to achieve `<quality>`, accepting `<downside>`.*

---

## ADR-01 — Authentication Service

**Context:** NFR-01 (Security)  
**Concern:** The system should be secure against unauthorized access.  
**Decision:** Include a dedicated Authentication, Authorization and Accounting (AAA) service.  
**Quality:** System cannot be compromised by adversaries.

---

## ADR-02 — Client-Server Pattern

**Context:** NFR-02 (Usability)  
**Concern:** The system should achieve usability across multiple interface types.  
**Decision:** Employ the client-server pattern.  
**Quality:** Enables creation of multiple different interfaces (web, mobile, data platform) sharing the same backend.

---

## ADR-03 — DataTransformService as Universal Adapter

**Context:** NFR-03 (Device Interoperability)  
**Concern:** The system must be compatible with many different smart devices using different protocols.  
**Decision:** Introduce a dedicated `DataTransformService` providing a common interface for all smart devices.  
**Quality:** Compatibility with heterogeneous device ecosystem.

---

## ADR-04 — Database Replication

**Context:** NFR-04 (Fault Recovery)  
**Concern:** A database failure will cause a denial of service.  
**Decision:** All central databases should be replicated and updated alongside the originals.  
**Quality:** Enhanced reliability.  
**Downside:** Increased infrastructure requirements and network load.

---

## ADR-05 — Separate AAA Class for IEC 62351

**Context:** NFR-06 (Standards Compliance)  
**Concern:** IEC 62351 mandates immediate detection of cyber attacks and security by design.  
**Decision:** Create a separate AAA class/service for authentication and authorization.  
**Quality:** Flexibility in enforcing security; regulatory compliance with IEC 62351.

---

## ADR-06 — Single DeviceControlService

**Context:** NFR-07 (Recommendation Effectiveness)  
**Concern:** Consumption predictions should not deviate more than 10%.  
**Decision:** Create a single coherent `DeviceControlService`.  
**Quality:** Reduced deviations from recommended patterns.

---

## ADR-08 — Electricity Price Monitoring Services

**Context:** NFR-08 / FR-03  
**Concern:** The prediction model needs to accurately predict electricity prices in addition to consumption.  
**Decision:** Create dedicated services to monitor and store both current and historical electricity price data.

---

## ADR-09 — User Data Cached in Application Layer

**Context:** FR-01 (Historical Data)  
**Concern:** Multiple queries to a central database may cause lag or denial of service.  
**Decision:** Store electricity consumption data for the past 6 months per user in an application cache (Redis).  
**Quality:** Removes potential bottleneck at the central ElectricityUsageDB.  
**Downside:** Increased complexity in data management and larger attack surface.

---

## ADR-10 — Centralized ElectricalConsumptionDB

**Context:** FR-03 (Weekly Recommendations)  
**Concern:** An accurate ML model requires a vast quantity and diversity of data.  
**Decision:** Forward and store energy consumption data from all users in one central `ElectricalConsumptionDB`.

---

## ADR-11 — ML Model for Anomaly Detection

**Context:** FR-04 (Anomaly Detection)  
**Concern:** Detecting anomalies is not a computationally trivial task.  
**Decision:** Create an ML model service to establish a baseline of user consumption for anomaly detection.  
**Quality:** Accuracy in detecting potential electrical faults.  
**Downside:** Increased computational load.

---

## ADR-12 — Single DeviceControlService for Remote Control

**Context:** FR-02 (Automated Device Optimization)  
**Concern:** The application needs to remotely control smart devices reliably.  
**Decision:** Create a single `DeviceControlService` to interface with all smart devices.  
**Quality:** Modularity and extensibility.

---

## ADR-13 — API-Gated Access to ElectricalUsageDB

**Context:** NFR-05 (Data Anonymization)  
**Concern:** Data consumers might act in bad faith and attempt to steal personal data.  
**Decision:** All access to the central `ElectricalUsageDB` must go through a managed API — no direct DB access.

---

## ADR-14 — Separate DB for Data Consumers

**Context:** NFR-05 (Data Anonymization)  
**Concern:** Data consumers might act in bad faith and attempt to steal personal data.  
**Decision:** Data consumers access a separate database containing only anonymized, aggregated usage data.

---

## ADR-15 — DataTransformService on Local Hardware

**Context:** NFR-03 (Device Interoperability)  
**Concern:** Distance-limited protocols (Bluetooth, Zigbee, physical cables) require physical proximity.  
**Decision:** Host `DataTransformService` on a physical device in or near the household, **not** in the cloud.  
**Quality:** Ability to interface with short-range wireless and wired protocols.

---

## ADR-16 — Smart Devices as Source of Truth for Settings

**Context:** FR-02  
**Concern:** Information desynchronization between stored settings and actual device state.  
**Decision:** Use the smart devices themselves as the primary reference for device-specific settings.

---

## ADR-17 — Single Hardware Node for Local Services

**Context:** Technical constraints / hardware cost  
**Concern:** Physical hardware limitations and cost.  
**Decision:** Host all system components except the `DataAnalysisService` on a single device node.  
**Quality:** Minimum hardware footprint.

---

## ADR-18 — Embedded Hardware as System Node

**Context:** NFR-05 (Data Privacy)  
**Concern:** Data anonymity and local data sovereignty.  
**Decision:** Choose embedded hardware as the whole system's device node for local services.

---

## ADR-19 — DataAnalysisService on AWS

**Context:** ASR-01 / Big Data requirements  
**Concern:** Big data analysis limitations of local hardware.  
**Decision:** Host the `DataAnalysisService` on AWS.  
**Quality:** Necessary performance, reliability, and multi-household data aggregation.

---

## ADR-20 — AWS S3 + EC2 for Model Storage and Training

**Context:** Technical infrastructure  
**Concern:** Reliable execution environments for ML model storage and training.  
**Decision:** Use AWS S3 for `ModelStorage` and AWS EC2 for `ModelTraining`.  
**Quality:** Reliability through mature, widely-used infrastructure.

---

## ADR-23 — Kafka for Big Data Streaming

**Context:** Technical infrastructure  
**Concern:** Performance for big data streaming use cases.  
**Decision:** Use Apache Kafka protocol for big data streaming pipelines.  
**Quality:** High throughput, fault-tolerant streaming.

---

## ADR-24 — RabbitMQ for Internal Message Queuing

**Context:** Technical infrastructure  
**Concern:** Fast and reliable message queuing between internal services.  
**Decision:** Use RabbitMQ for internal messaging (e.g., between `DataTransformService` and `ElectricityDataCollectionService`).  
**Quality:** Low-latency, reliable internal communication.

---

## ADR-25 — Anonymization in DataAnalysisService

**Context:** NFR-05 (Data Anonymization) + NFR-06 (Standards Compliance)  
**Concern:** All data provided to Data Consumers must be properly anonymized.  
**Decision:** Perform all anonymization within the `DataAnalysisService` before data reaches consumers.

---

## ADR-26 — One-Way Interaction: System → External Devices

**Context:** FR-01, FR-02, FR-03  
**Concern:** Concurrency issues arising from bidirectional communication with devices.  
**Decision:** Make the interaction between the system and smart devices/electricity suppliers unidirectional (system → device).

---

## ADR-27 — Device State Database

**Context:** FR-02 + NFR-04  
**Concern:** Need to save relevant device settings for recovery and control.  
**Decision:** Introduce a `DeviceStateDB` in the functional view.

---

## ADR-28 — Two-Way Interaction: UserInteraction ↔ DataAnalysis

**Context:** FR-04 (Anomaly Detection)  
**Concern:** The `UserInteractionService` would otherwise need to regularly poll for anomalies.  
**Decision:** Allow two-way interaction between `UserInteractionService` and `DataAnalysisService` so anomaly alerts can be pushed.

---

## ADR-29 — EnergySupplierSystem via DataTransformService

**Context:** NFR-03 (Device Interoperability)  
**Concern:** Energy supplier systems also need to be managed via abstract interfaces.  
**Decision:** Route `EnergySupplierSystem` interaction through the `DataTransformService`, treating it like any other external device.
