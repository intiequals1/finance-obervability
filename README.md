# Finance Observability MVP

A portfolio-ready MVP for testing and monitoring finance data quality with **SEC/XBRL data**, **PostgreSQL**, **Prometheus**, **Grafana**, and optional **k6** pipeline testing.

Unlike the Kaggle prototype, this version needs **no Kaggle login** and uses public SEC company facts for listed US companies.

## Architecture

```text
SEC Company Facts API
        |
        v
Python ETL + DQ rules + Prometheus metrics
        |
        v
PostgreSQL finance facts + DQ results
        |
        +--> Grafana PostgreSQL dashboards
        +--> Prometheus metrics dashboards
        +--> k6 smoke/performance tests
```

## What it tests

- Data freshness: latest filing date per ticker
- Completeness: minimum available facts
- Null values: missing financial values
- Accounting equation: `Assets - Liabilities - Equity`
- ETL latency and load health

## Quick start

```bash
cp .env.example .env
# Optional: edit SEC_USER_AGENT in .env with your email/contact

docker compose up --build
```

Open:

- Grafana: <http://localhost:3000>
- Login: `admin / admin`
- Prometheus: <http://localhost:9090>
- ETL health: <http://localhost:8000/health>
- ETL metrics: <http://localhost:8000/metrics>

## Trigger a reload manually

```bash
curl -X POST http://localhost:8000/load
```

## Run k6 smoke test

Install k6 locally, then:

```bash
k6 run k6/pipeline-smoke-test.js
```

## Import to GitHub Web UI

Do **not** upload `.env`.

Upload the project folder contents including:

- `.env.example`
- `.gitignore`
- `docker-compose.yml`
- `etl/`
- `sql/`
- `prometheus/`
- `provisioning/`
- `dashboards/`
- `k6/`

## Roadmap

- Add SAP ACDOCA simulation table
- Add RZL-to-SAP reconciliation sample
- Add anomaly detection for journal entries
- Add CI workflow with lint and Docker build check
- Add Grafana alert rules
- Add OpenTelemetry traces for ETL pipeline steps
