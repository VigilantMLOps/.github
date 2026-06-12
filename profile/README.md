# Atlas AI

**A production-grade platform for building, deploying, and monitoring ML/AI applications.**

Atlas AI gives teams the full stack — from model training and deployment to real-time observability, drift detection, and incident management. Built around real use cases (network intrusion detection, account takeover, RAG) but designed to be model-agnostic.

🔗 Live UI: https://vigilant-ui.duckdns.org
</br>
🔗 Live API: https://vigilant-api.duckdns.org
</br>
🔗 LinkedIn post: https://www.linkedin.com/posts/bara-alsedih_mlops-machinelearning-python-activity-7458189356126863360-7yBI

Tech: Python · FastAPI · PostgreSQL · ClickHouse · Polars · scikit-learn · React · TypeScript · Vite · Docker · Caddy · Poetry

---

## Repositories

| Repo | Purpose | Stack |
|---|---|---|
| [vigilant-api](https://github.com/VigilantMLOps/vigilant-api) | Monitoring backend — evaluation reports, drift detection, incident lifecycle, telemetry | Python · FastAPI · PostgreSQL · ClickHouse |
| [vigilant-ui](https://github.com/VigilantMLOps/vigilant-ui) | Observability dashboard — overview, evaluation, drift, incidents | React 18 · TypeScript · Vite · Tailwind · TanStack Query |
| [vigilant-detect](https://github.com/VigilantMLOps/vigilant-detect) | ML service — ATO and network intrusion detection with automated weekly retraining | Python · FastAPI · scikit-learn · Polars · APScheduler |
| [vigilant-rag](https://github.com/VigilantMLOps/vigilant-rag) | RAG application — document retrieval and LLM-powered responses over local models | Python · FastAPI · Ollama |
| [vigilant-pack](https://github.com/VigilantMLOps/vigilant-pack) | CLI runtime — boots any AI application stack (services, models, app) in one command | Python · Click · Rich · Docker |

Each repo owns its own setup, env vars, tests, and tag-triggered deploy workflow.

---

## Architecture

### Vigilant-API

<img width="1376" height="768" alt="Gemini_Generated_Image_n40oden40oden40o" src="https://github.com/user-attachments/assets/35f26b65-37ab-477b-a916-d6be4cb29b2f" />

</br>
</br>

### Vigilant-UI

<img width="1512" height="829" alt="Overview dashboard" src="https://github.com/user-attachments/assets/814c3748-6a9d-4cad-8a9e-2e4e9787cc33" />

</br>
</br>

<img width="1512" height="827" alt="Evaluation dashboard" src="https://github.com/user-attachments/assets/ebb6a6e3-e527-4d21-8f61-50756f515665" />

</br>
</br>

### Data layer

- **OLTP (PostgreSQL)** — model registry, evaluation reports, incidents. Transactional, mutable, relational.
- **OLAP (ClickHouse)** — production traffic log, alerts, drift results, report metrics, LLM traces. Append-only, high-volume.

### Key API routes

| Prefix | Purpose |
|---|---|
| `/api/v1/reports` | Evaluation report history (latest, last 10) |
| `/api/v1/reporter` | Pre/post-production evaluation runners (data, model, drift) |
| `/api/v1/monitoring` | Live drift & telemetry metrics |
| `/api/v1/incidents` | Incident lifecycle (auto-triggered by alerting engine) |
| `/api/v1/telemetry` | System health & latency probes |

Full reference: [vigilant-api README](https://github.com/VigilantMLOps/vigilant-api#api-reference).

---

## The 3 Pillars

**Drift Detection** — statistical shifts between training reference and incoming production data via PSI, KS-test, and Chi² test.

**Performance Monitoring** — accuracy, F1, precision, recall, and confusion-matrix deltas over time. Alerts on decay relative to the pre-production baseline.

**System Health** — API latency and schema consistency via middleware. Alerts on slow requests (>500ms) and 5xx errors.

---

## Incident Procedures

Automated remediation is defined in [vigilant-api/core/procedures.yaml](https://github.com/VigilantMLOps/vigilant-api/blob/main/core/procedures.yaml). Low-risk incidents auto-resolve; high-risk ones create tickets for human review.

| Incident Type | Risk | Behavior |
|---|---|---|
| `system_latency` | Low | Auto-resolves (refetch DB) |
| `schema_skew` | Low | Auto-resolves (refresh schema) |
| `data_drift` | High | Creates incident ticket |
| `performance_drop` | High | Creates incident ticket |

---

## Deployment

All services run on a single Oracle Cloud Always-Free VM behind Caddy (automatic Let's Encrypt TLS). Each repo has a tag-triggered GitHub Actions workflow:

- `deploy.api.*` → ships [vigilant-api](https://github.com/VigilantMLOps/vigilant-api)
- `deploy.ui.*` → ships [vigilant-ui](https://github.com/VigilantMLOps/vigilant-ui)
- `deploy.ml.*` → ships [vigilant-detect](https://github.com/VigilantMLOps/vigilant-detect)

---

## Contact

**Bara Al-Sedih** — [github.com/baraalsedih](https://github.com/baraalsedih) · baraalsedih@gmail.com
