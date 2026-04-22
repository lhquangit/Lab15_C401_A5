# SRE Readiness Checklist

This document outlines the Site Reliability Engineering (SRE) requirements and operational standards for the production rollout.

## 1. Monitoring & Observability

- [ ] **Infrastructure Metrics:** CPU, RAM, Disk, Network I/O for all services.
- [ ] **Application Metrics:** Request rate, Error rate (HTTP 4xx/5xx), Latency (P50, P95, P99).
- [ ] **AI Specific Metrics:** Token usage (input/output), LLM latency, Cache hit rate, LLM Provider throttles.
- [ ] **Retrieval Metrics:** Retrieval precision, Vector DB search time.
- [ ] **Dashboards:** Real-time dashboards for Engineering and Operations.

## 2. Alerting Thresholds (Baseline)

- [ ] **Latency Alert:** P95 response time > 10 seconds for 5 consecutive minutes.
- [ ] **Error Rate Alert:** > 2% failure rate over a 15-minute window.
- [ ] **Availability Alert:** Any core service health check failing for > 2 minutes.
- [ ] **Cost Alert:** Daily spend exceeds 120% of estimated baseline.

## 3. High Availability & Disaster Recovery (DR)

- [ ] **Multi-Zone Deployment:** Services deployed across at least 2 Availability Zones.
- [ ] **Auto-Scaling:** Configured based on RPS and resource utilization.
- [ ] **Backup Schedule:** Daily automated backups of Vector DB and chat logs.
- [ ] **Recovery Plan:** Documented RTO (4h) and RPO (1h) procedures.
- [ ] **Failover Drill:** Quarterly testing of the "Warm Standby" switch.

## 4. Incident Response

- [ ] **On-Call Rotation:** Defined primary and secondary engineers.
- [ ] **Severity Levels:** Clear definitions for Sev-1 (Critical), Sev-2 (Major), Sev-3 (Minor).
- [ ] **Runbooks:**
    - [ ] Traffic Spike mitigation.
    - [ ] LLM Provider outage (Switching to secondary region/provider).
    - [ ] VPN/ExpressRoute disconnection.
    - [ ] Vector Store corruption recovery.
- [ ] **Post-Mortem Policy:** Required for all Sev-1 and Sev-2 incidents.

## 5. Security & Compliance

- [ ] **Audit Trail:** Comprehensive logging of all user interactions (scrubbed).
- [ ] **Access Review:** Monthly review of privileged access.
- [ ] **Security Scanning:** Automated dependency and vulnerability scanning.
- [ ] **Penetration Test:** Required before major version releases.
