# SaaS-Sentinel
AI-driven FinOps &amp; API Orchestration. A secure middleware layer that optimizes third-party API usage (OpenAI, Stripe, AWS) in real-time using ML. Prevents cost explosions with predictive billing, detects anomalies to stop API key abuse, and provides autonomous throttling. Secure, proactive, and intelligent API governance. 

SaaS-Sentinel offers unified observability through time-series metrics, audit logs, and alerts, enabling teams to understand consumption patterns and enforce organization-wide policies without modifying client applications. Designed as a plug-and-play gateway, it integrates seamlessly into existing stacks while maintaining low latency and enterprise-grade security.

## Architecture
<img width="854" height="585" alt="image" src="https://github.com/user-attachments/assets/a2e13503-617f-4b93-bc0b-9b9bd31c6f3b" />


## Tech Stack
### Frontend
- Next.js - dashboard interface for viewing usage, costs, and alerts
- Shadcn/UI - consistent and accessible UI components for monitoring panels

### Backend
- Python + FastAPI - high-performance async API gateway that receives client requests, executes middleware policies, and forwards traffic to target SaaS APIs

### ML Models
- Scikit-learn - statistical models for anomaly detection on request patterns
- Prophet - time-series forecasting to predict cost/usage spikes

### Database and Cache
- InfluxDB / TimescaleDB - time-series storage for metrics such as requests/min, cost/hour, and latency trends
- Redis - in-memory store for real-time rate limits, quota counters, and short-term session data

### Secrets Manager
- AWS Secrets Manager / Vault - secure storage and rotation of SaaS API keys used by the gateway and middleware

### Alerts
- Slack / Twilio - real-time alerts when anomalies, quota breaches, or cost spikes are detected

### Deployment
- Docker - containerized services for portability
- Kubernates - horizontal scaling of gateway under high traffic

## Folder Structure
