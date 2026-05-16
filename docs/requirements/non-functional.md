# Non-Functional Requirements

## NFR-01 — Security

**Description:** The system should be secure.

**Scenario (SCN-NFR-01):** When an unauthorized user tries to access a feature of the system, the system denies the request.

**Traces:** USR-CRN-03, USR-CRN-04, USR-CRN-05

**Implementation:** Authentication, Authorization and Accounting (AAA) service using Node.js + Passport.js. All service interfaces secured with common cryptographic methods.

---

## NFR-02 — Usability

**Description:** The application should have a simple user interface by abstracting complex configurations into high-level controls.

**Scenario (SCN-NFR-02):** When a user accesses the user interface, core functionality should be accessible with a maximum of two clicks/taps.

**Traces:** USR-CRN-02, AP-4

---

## NFR-03 — Device Interoperability

**Description:** The system can interface with smart devices using WPAN (Zigbee, Bluetooth), WLAN (Wi-Fi 802.11), and WWAN (LTE, 5G, NB-IoT) technologies.

**Scenario (SCN-NFR-03):** When a device connects to the same WLAN that the system is connected to, the system acknowledges the new device and offers to the user to start managing it.

**Traces:** MAN-ND-01, USR-CRN-02, EXT-05, AP-6, AP-8

**Priority:** High Importance / Medium Risk

---

## NFR-04 — Fault Recovery

**Description:** The system must recover automatically in case of failure, in a reasonable time.

**Scenario (SCN-NFR-04):** When the system loses and regains power, the system automatically resumes what it was last doing within a couple of minutes.

**Traces:** USR-ND-03, USR-CRN-04

**Implementation:** All service states and parameters persisted in respective databases. Automatic restart on power restore.

---

## NFR-05 — Data Anonymization (GDPR)

**Description:** System data must not be traceable to the system owner.

**Scenario (SCN-NFR-05):** When data leaves the system, either authorized by the system owner or not, the data should not be traceable to the system owner.

**Traces:** USR-CRN-03, USR-CRN-04, DAT-ND-01, DAT-ND-02, EXT-01, EXT-02

**Priority:** High Importance / Medium Risk

---

## NFR-06 — Standards Compliance

**Description:** The system must comply with IEC 61850 and IEC 62351.

**Traces:** MAN-ND-01, EXT-04

---

## NFR-07 — Recommendation Effectiveness

**Description:** Consumption and production recommendations should provide cost savings.

**Scenario (SCN-NFR-07):** When the usage patterns recommended by the system are implemented, the user should end up having saved money at the end of the week compared to random usage.

**Traces:** USR-ND-01

**Priority:** High Importance / Medium Risk
