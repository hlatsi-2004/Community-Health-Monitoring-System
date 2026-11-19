# Community Health Monitoring System

> A dependable, reliable, safe, and secure system for monitoring disease outbreaks by collecting data from clinics and mobile devices — FHIR-compliant and built for real-time clinical response.

## Overview

The Community Health Monitoring System collects health data from clinics and mobile devices to detect disease outbreaks early. It supports remote data collection, validates and enriches incoming data, and alerts clinicians about outbreaks and at-risk patients so healthcare providers can act in time to prevent spread.

The system handles sensitive, potentially life-threatening information, so it is designed around dependability: **availability, reliability, safety, security, and maintainability**.

**Primary users:** Clinicians · Healthcare providers · Patients · Healthcare organisations · System administrators

## Key Features

- Remote data collection from mobile apps, IoT sensors, and clinic gateways
- FHIR-compliant patient records
- Real-time outbreak and at-risk patient detection
- Fail-safe alerting (uncertain cases raise `UNVERIFIED` alerts for manual review)
- Multi-source verification before any outbreak alert
- End-to-end encryption and strict role-based access control
- Immutable audit logging and automated monitoring

## Architecture

The system combines four architectural styles:

| Style | Role |
|-------|------|
| **Layered** | Data acquisition, communication, processing, storage, application, analytics, security/monitoring |
| **Microservices** | Anomaly detection, data validation, audit logging, alerting |
| **Event-driven** | Fault-isolated operation via message broker |
| **Client-server** | Edge devices (mobile, IoT, clinic gateways) talk to cloud services |

**Main layers:**

1. **Data Acquisition** — mobile apps, IoT sensors, clinic gateways
2. **Data Communication** — secure gateways, multi-protocol support, encoding/encryption
3. **Data Processing** — stream processing, API gateway, message broker
4. **Storage & Data Management** — FHIR server, NoSQL patient records, replication, backups
5. **Analytics & Decision Support** — outbreak detection, trend prediction, alerting
6. **Application & Presentation** — dashboards, notifications, user apps
7. **Security & Monitoring** — audit, key management, IAM, monitoring across all layers

## Dependability Goals

| Attribute | Target / Approach |
|-----------|-------------------|
| **Availability** | 99.99% uptime — redundant cloud deployment, load-balanced gateways, persisted messages, health checks |
| **Reliability** | 99.999% data accuracy — validation, redundant pipelines, deduplication, automated testing |
| **Safety** | Fail-safe `UNVERIFIED` alerts, multi-source verification, clinician review before action |
| **Security** | TLS 1.3 + AES-256, RBAC, immutable audit logs, rate limiting |
| **Maintainability** | Modular microservices, Infrastructure as Code, automated test suite on every commit |

## Reliability Engineering

- **Redundancy:** primary + replica databases, 3-node message broker cluster, dual analytics instances behind a load balancer
- **Error detection:** 5s heartbeats, 10s health checks, checksums, 30s timeouts, circuit breakers after 5 failures
- **Fault tolerance:** retry with backoff, resource isolation, cached fallback results
- **Monitoring:** alerts when error rate >1% for 5 min or latency >2s for 10 min
- **Goals:** MTTR < 5 min, MTBF > 720 hours

## Safety Engineering

Eight hazards were identified and mitigated, including:

- False negative (missed outbreak) — **catastrophic** → redundant pipelines, cross-source verification, fail-safe alerts
- False positive (false outbreak) — **critical** → multi-source verification, clinician review, manual override
- Delayed critical alert — **critical** → prioritized queues, dedicated pathway, SMS/phone fallback
- Wrong patient association — **catastrophic** → unique IDs, double-entry verification, immutable audit trail
- Incorrect recommendation — **critical** → advisory-only, model versioning, explainable AI
- Data leak — **critical** → encryption, RBAC, audit logs, data minimization
- Healthy patient flagged at risk — **marginal** → clinician override, patient dispute, anomaly detection
- Alert fatigue — **critical** → prioritization, daily digests, tunable thresholds

**Safety mechanisms:** fail-safe defaults, independent watchdog (checks pipeline every 30s), emergency override/shutdown/fallback, hazard-injection testing, monthly incident review.

## Security Engineering

Threat model covers: data interception, data breach at rest, auth bypass, privilege escalation, denial of service, data injection, insider threat, and supply chain attacks.

**Controls:** TLS 1.3, certificate pinning, AES-256 at rest, MFA, short-lived JWTs, RBAC enforced server-side, rate limiting, DDoS protection, dependency scanning (SCA) in CI/CD, immutable audit logs.

## Resilience & Reuse

- **Resilience engineering:** designs for coping with disturbance, balancing productivity with safety, and cyber resilience (withstand and recover from cyber compromises)
- **Software reuse:** uses existing artifacts to avoid duplicating work, improve dependability through prior testing, and ensure standards compliance — balanced against maintenance cost and component flexibility

## Future Work

- Fully serverless deployment on Azure
- AI-driven outbreak trend prediction
- Broader IoT device and protocol support
- Hardened compliance for stricter regulations


## Topics / Tags

`software-engineering` `healthcare` `dependability` `reliability` `safety` `security` `fhir` `microservices` `event-driven` `system-architecture`
