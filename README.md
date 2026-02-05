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
```
SAAS-Sentinel/
│
├── client/                              # Frontend Dashboard
│   ├── src/
│   │   ├── app/                        # Next.js app directory
│   │   │   ├── dashboard/
│   │   │   ├── api-usage/
│   │   │   ├── alerts/
│   │   │   └── settings/
│   │   ├── components/                 # Reusable UI components (Shadcn/UI)
│   │   │   ├── ui/
│   │   │   ├── charts/
│   │   │   ├── alerts/
│   │   │   └── tables/
│   │   ├── lib/                        # Utilities and API clients
│   │   │   ├── api-client.ts
│   │   │   └── utils.ts
│   │   └── styles/
│   ├── public/
│   ├── package.json
│   ├── tsconfig.json
│   └── next.config.js
│![alt text](image.png)
├── gateway/                             # SaaS-Sentinel API Gateway Core
│   ├── src/
│   │   ├── main.py                     # FastAPI application entry point
│   │   ├── config/
│   │   │   ├── settings.py             # Configuration management
│   │   │   └── logging.py              # Logging configuration
│   │   ├── proxy/                      # Smart Proxy Layer
│   │   │   ├── __init__.py
│   │   │   ├── handler.py              # Main proxy request handler
│   │   │   ├── interceptor.py          # Request/response interceptor
│   │   │   └── router.py               # API routing logic
│   │   ├── security/                   # Security & Key Management
│   │   │   ├── __init__.py
│   │   │   ├── key_manager.py          # API key retrieval and caching
│   │   │   ├── vault_client.py         # Secrets manager integration
│   │   │   ├── auth.py                 # mTLS and service authentication
│   │   │   └── rotation.py             # Automatic key rotation
│   │   ├── models/                     # Data models
│   │   │   ├── __init__.py
│   │   │   ├── request.py
│   │   │   ├── response.py
│   │   │   └── metrics.py
│   │   └── utils/
│   │       ├── __init__.py
│   │       └── helpers.py
│   ├── requirements.txt
│   ├── Dockerfile
│   └── tests/
│       ├── test_proxy.py
│       ├── test_security.py
│       └── test_integration.py
│
├── middleware/                          # ML & Decision Engine
│   ├── src/
│   │   ├── main.py                     # Middleware service entry point
│   │   ├── collector/                  # Data Collection Pipeline
│   │   │   ├── __init__.py
│   │   │   ├── metrics_collector.py    # Collect API usage metrics
│   │   │   ├── aggregator.py           # Aggregate and preprocess data
│   │   │   └── storage.py              # Write to time-series DB
│   │   ├── ml/                         # Machine Learning Engine
│   │   │   ├── __init__.py
│   │   │   ├── forecasting.py          # Prophet/ARIMA cost prediction
│   │   │   ├── anomaly_detection.py    # Isolation Forest anomaly detection
│   │   │   ├── model_trainer.py        # Model training pipeline
│   │   │   └── inference.py            # Real-time inference
│   │   ├── decision/                   # Control & Decision Logic
│   │   │   ├── __init__.py
│   │   │   ├── policy_engine.py        # Rate limiting, throttling rules
│   │   │   ├── cost_optimizer.py       # API fallback and downgrade logic
│   │   │   └── alert_manager.py        # Alert dispatch (Slack, Twilio)
│   │   ├── models/
│   │   │   ├── saved_models/           # Serialized ML models
│   │   │   └── __init__.py
│   │   └── utils/
│   │       ├── __init__.py
│   │       └── preprocessing.py
│   ├── requirements.txt
│   ├── Dockerfile
│   └── tests/
│       ├── test_ml.py
│       ├── test_anomaly.py
│       └── test_decision.py
│
├── mock-api/                            # Mock Target APIs for Testing
│   ├── src/
│   │   ├── main.py                     # FastAPI mock server
│   │   ├── openai_mock.py              # Mock OpenAI API
│   │   ├── stripe_mock.py              # Mock Stripe API
│   │   ├── aws_mock.py                 # Mock AWS API
│   │   └── utils/
│   │       └── response_generator.py
│   ├── requirements.txt
│   └── Dockerfile
│
├── database/                            # Database Configuration
│   ├── influxdb/
│   │   ├── init-scripts/
│   │   │   └── init.iql                # InfluxDB initialization
│   │   └── docker-compose.influx.yml
│   ├── timescaledb/
│   │   ├── init-scripts/
│   │   │   └── init.sql                # TimescaleDB schema
│   │   └── docker-compose.timescale.yml
│   ├── redis/
│   │   ├── redis.conf                  # Redis configuration
│   │   └── docker-compose.redis.yml
│   └── migrations/
│       └── versions/
│
├── shared/                              # Shared Libraries & Utilities
│   ├── models/                         # Shared data models
│   │   ├── __init__.py
│   │   ├── api_request.py
│   │   └── api_response.py
│   ├── constants/
│   │   ├── __init__.py
│   │   ├── api_providers.py
│   │   └── cost_mapping.py
│   └── utils/
│       ├── __init__.py
│       ├── logging.py
│       └── validators.py
│
├── infra/                               # Infrastructure & Deployment
│   ├── kubernetes/
│   │   ├── gateway-deployment.yaml
│   │   ├── middleware-deployment.yaml
│   │   ├── redis-deployment.yaml
│   │   ├── ingress.yaml
│   │   └── secrets.yaml
│   ├── terraform/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   └── helm/
│       └── saas-sentinel/
│           ├── Chart.yaml
│           ├── values.yaml
│           └── templates/
│
├── scripts/                             # Utility Scripts
│   ├── setup.sh                        # Project setup script
│   ├── seed_data.py                    # Seed mock usage data
│   ├── train_models.py                 # Train ML models
│   ├── rotate_keys.sh                  # Manual key rotation
│   └── deploy.sh                       # Deployment automation
│
├── docs/                                # Documentation
│   ├── architecture.md
│   ├── api-reference.md
│   ├── security-guide.md
│   ├── ml-models.md
│   └── deployment-guide.md
│
├── .github/                             # GitHub Actions CI/CD
│   └── workflows/
│       ├── ci.yml
│       ├── deploy.yml
│       └── security-scan.yml
│
├── docker-compose.yml                   # Local development setup
├── .env.example                         # Example environment variables
├── .gitignore
├── README.md
└── LICENSE
```

### **Folder Description**

| **Folder**    | **Purpose**                                                            |
| ------------- | ---------------------------------------------------------------------- |
| `client/`     | Next.js frontend dashboard for monitoring API usage, costs, and alerts |
| `gateway/`    | Core API gateway that proxies all external API requests                |
| `middleware/` | ML engine for forecasting, anomaly detection, and decision-making      |
| `mock-api/`   | Mock external APIs (OpenAI, Stripe, AWS) for testing                   |
| `database/`   | Database configurations for InfluxDB, TimescaleDB, and Redis           |
| `shared/`     | Shared libraries, models, and utilities used across services           |
| `infra/`      | Kubernetes, Terraform, and Helm charts for deployment                  |
| `scripts/`    | Automation scripts for setup, training, and deployment                 |
| `docs/`       | Project documentation including architecture and guides                |

---
