# Functional Requirements

## FR-01 — Historical Electricity Data

**Description:** The system collects and can provide electricity data on demand, at least six months back.

**Scenario (SCN-FR-01):** When a user or data consumer requests electricity data for a month within the previous six months, the system provides the data with hourly precision.

**Traces:** USR-ND-02, USR-ND-03, USR-ND-04, AP-1

---

## FR-02 — Automated Device Optimization

**Description:** The system can automatically optimise electricity costs by controlling smart devices connected to the system.

**Scenario (SCN-FR-02):** When the system detects that electricity prices are high, the system commands connected electricity producers to dump electricity to the external grid.

**Traces:** USR-ND-01, USR-CRN-01, USR-CRN-02, MAN-ND-01, AP-2

**Priority:** High Importance / High Risk

---

## FR-03 — Weekly Recommendations

**Description:** The system can provide electricity consumption and production recommendations for the coming week.

**Scenario (SCN-FR-03):** Every Sunday, the system drafts recommendations for when to use smart devices and when to sell electricity based on previous consumption and actual/predicted future electricity prices.

**Traces:** USR-ND-01, USR-ND-04, USR-CRN-01, USR-CRN-02, AP-3

**Priority:** High Importance / High Risk

---

## FR-04 — Real-Time Anomaly Detection

**Description:** The system can detect abnormal electricity consumption/production/prices in real-time and report them to the user.

**Scenario (SCN-FR-04):** When a smart device has used/produced 10% more/less electricity in the previous minute than the average of the last five minutes, the system notifies the user of the event.

**Traces:** USR-ND-01, USR-ND-03, USR-ND-04, USR-CRN-01

**Priority:** Medium Importance / High Risk
