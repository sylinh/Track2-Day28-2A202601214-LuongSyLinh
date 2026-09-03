# Day 28 Track 2 - Submission Answers

## 1. Repository and ownership

- Repository URL: https://github.com/sylinh/Track2-Day28-2A202601214-LuongSyLinh
- Submission branch URL: https://github.com/sylinh/Track2-Day28-2A202601214-LuongSyLinh/tree/ca-nhan-luongsy
- Branch: `ca-nhan-luongsy`
- Mode: `individual`
- Student: `Luong Sy Linh`
- Primary responsibility: all roles and all IP01-IP10.

I worked individually and completed all roles: Ingestion & Orchestration,
Data & ML, Serving & Retrieval, Platform & Observability, and Presenter / Incident Commander.

## 2. Implementation

Completed student-owned boundaries in `src/lab28_platform/integration_tasks.py`:

- `event_headers`: byte-valued idempotency and optional trace headers;
- `dedupe_latest`: deterministic replay-safe newest-event selection;
- `feast_online_request`: registry-backed Feast request;
- `readiness_status`: `ready`, `degraded`, and `not_ready` semantics.

## 3. Verification results

Environment checkpoint: `local-standard`, Python `3.11.16`, Docker daemon available,
20 CPUs, and approximately `443.7 GiB` disk free. Both Compose configuration checks
completed successfully.

| Check | Result / artifact |
|---|---|
| `uv run pytest starter-tests tests -q` | `87 passed, 1 warning` |
| `uv run ruff check .` | `All checks passed` |
| `uv run python scripts/verify_matrix.py` | `245 checks passed` |
| `uv run python scripts/check_portability.py` | `supported workflow is host-path and shell independent` |
| `uv run python scripts/validate_manifests.py` | `Kubernetes and GitOps manifest contracts passed` |
| `uv run pytest integration-tests -m "not gpu and not langsmith" -q` | `56 passed, 16 deselected` |

Critical journeys: J1 `12 passed, 3 skipped`; J2 `9 passed`; J3 `6 passed, 3 skipped`; J4 `9 passed, 4 skipped`; J5 `9 passed, 1 skipped`.
Full non-GPU/LangSmith suite: `56 passed, 16 deselected`.

## 4. Evidence

Attach or link the complete `evidence/` bundle, including `integration-report.json`,
IP01-IP10 evidence, and both IP09 Prometheus/Grafana files.

Screenshots:

- `evidence/screenshots/01-airflow-dag-success.png`
- `evidence/screenshots/02-grafana-dashboard.png`
- `evidence/screenshots/03-prometheus-targets-1.png`
- `evidence/screenshots/03-prometheus-targets-2.png`
- `evidence/screenshots/04-jaeger-trace.png`
- `evidence/screenshots/05-mlflow-champion.png`
- `evidence/screenshots/06-qdrant-collection.png`

Happy path identifiers:

- Run ID: `<RUN_ID>`
- Trace ID: `<TRACE_ID>`
- Delta version: latest integration report `feedback=v12 (17 rows); documents=v6 (18 rows)`
- MLflow release/version: `version=1; champion; run_id=25dd6a4893994827bf6d092a514f8813`
- vLLM model ID: `UNVERIFIED (endpoint unreachable; GPU gate skipped)`

Integration report summary: `5/6 verified points passing`, `4 unverified`, and `IP07 not_ready`
because no verifiable real vLLM endpoint is configured. IP08-IP10 require their live evidence
files for the final demonstration report.

## 5. Incident and recovery

- Injected failure: `<SERVICE_AND_COMMAND>`
- Time and initial state: `<TIMESTAMP / STATE>`
- Observed signals: `<LOGS / METRICS / READINESS / TRACE>`
- Root cause: `<CAUSE>`
- Recovery action: `<ACTION>`
- No-data-loss proof: `<DELTA / KAFKA / QDRANT / FEAST EVIDENCE>`

## 6. Performance and GitOps

- Load profile, 8 workers: `200 requests; 18 HTTP 200; 182 status 0; P50=6.27 ms; P95=373.25 ms; P99=437.69 ms`.
- Load profile, 16 workers: `200 requests; 12 HTTP 200; 188 status 0; P50=7.88 ms; P95=664.62 ms; P99=840.54 ms`.
- Hardware, model, corpus, concurrency and warm-up: `<DETAILS>`
- Bottleneck analysis: 16 workers increased P95/P99 substantially while reducing successful responses. Status 0 represents an exception/HTTP error in the standard-library probe and must be investigated; the missing vLLM endpoint is a known readiness blocker.
- Kubernetes/GitOps validation: `<RESULT>`
- Drift/self-heal and rollback evidence: `<LINK_OR_RESULT>`

## 7. Reflection

- Điều khó nhất: giữ tính nhất quán giữa Kafka replay, Delta MERGE, Feast, Qdrant và trace xuyên suốt nhiều service.
- Trade-off đã chọn: dùng các contract và constant dùng chung để giảm duplicate logic; chấp nhận chạy thêm validation/integration test trước khi kết luận hệ thống sẵn sàng.
- Điều sẽ cải tiến: tự động hóa thu evidence, kiểm thử tải dài hơn và triển khai vLLM/trace LangSmith trong môi trường GPU thật.

## 8. Submission safety check

- [ ] No token, password, `.env`, temporary URL, Docker data, database/cache, model weights, or `.lab28/`.
- [ ] Every demo command has an explainable output or screenshot.
