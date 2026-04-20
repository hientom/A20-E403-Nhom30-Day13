# Day 13 Observability Lab Report

> **Instruction**: Fill in all sections below. This report is designed to be parsed by an automated grading assistant. Ensure all tags (e.g., `[GROUP_NAME]`) are preserved.

## 1. Team Metadata
- [GROUP_NAME]: Nhóm 30 — A20-E403
- [REPO_URL]: https://github.com/tttduong/A20-E403-Nhom30-Day13
- [MEMBERS]:
  - Member A: [Name] | Role: Logging & PII
  - Member B: [Name] | Role: Tracing & Enrichment
  - Member C: Hồ Quang Hiển | Role: SLO & Alerts
  - Member D: [Name] | Role: Load Test & Dashboard
  - Member E: [Name] | Role: Demo & Report

---

## 2. Group Performance (Auto-Verified)
- [VALIDATE_LOGS_FINAL_SCORE]: 100/100
- [TOTAL_TRACES_COUNT]: (đếm trên Langfuse sau load test; mục tiêu ≥ 10)
- [PII_LEAKS_FOUND]: 0

---

## 3. Technical Evidence (Group)

### 3.1 Logging & Tracing
- [EVIDENCE_CORRELATION_ID_SCREENSHOT]: [Path to image]
- [EVIDENCE_PII_REDACTION_SCREENSHOT]: [Path to image]
- [EVIDENCE_TRACE_WATERFALL_SCREENSHOT]: [Path to image]
- [TRACE_WATERFALL_EXPLANATION]: (Briefly explain one interesting span in your trace)

### 3.2 Dashboard & SLOs
- [DASHBOARD_6_PANELS_SCREENSHOT]: [Path to image]
- [SLO_TABLE]: (Snapshot `GET /metrics`: `traffic`=110, `tokens_in_total`=3740, `tokens_out_total`=14809; trước nộp nên chạy lại `curl` để đồng bộ)
| SLI | Target | Window | Current Value |
|---|---:|---|---:|
| Latency P95 | < 3000ms | 28d | 150 ms (`latency_p95`; nội bộ agent, khác RTT client) |
| Error Rate | < 2% | 28d | 0% (110 request, `error_breakdown` rỗng) |
| Cost Budget | < $2.5/day | 1d | $0.2334 (`total_cost_usd`; `avg_cost_usd` ≈ 0.0021/request) |
| Quality (avg heuristic) | ≥ 0.75 | 28d | 0.88 (`quality_avg`) |

### 3.3 Alerts & Runbook
- [ALERT_RULES_SCREENSHOT]: `docs/evidence/alert-rules-screenshot.png`
- [SAMPLE_RUNBOOK_LINK]: docs/alerts.md#1-high-latency-p95

---

## 4. Incident Response (Group)
- [SCENARIO_NAME]: rag_slow (draft — D chạy inject rồi cập nhật số liệu thật)
- [SYMPTOMS_OBSERVED]: P95 HTTP/client tăng; span RAG chiếm phần lớn thời gian trace (điền số sau khi bật incident)
- [ROOT_CAUSE_PROVED_BY]: Toggle `rag_slow` trong `app/incidents.py` / API `POST /incidents/rag_slow/enable` — kèm Trace ID hoặc dòng log sau khi đo
- [FIX_ACTION]: `POST /incidents/rag_slow/disable` hoặc tắt toggle tương đương
- [PREVENTIVE_MEASURE]: Alert `high_latency_p95` + runbook mục 1 trong `docs/alerts.md`

---

## 5. Individual Contributions & Evidence

### [MEMBER_A_NAME]
- [TASKS_COMPLETED]: 
- [EVIDENCE_LINK]: (Link to specific commit or PR)

### [MEMBER_B_NAME]
- [TASKS_COMPLETED]: 
- [EVIDENCE_LINK]: 

### [Hồ Quang Hiển]
- [TASKS_COMPLETED]: Hoàn thiện `config/slo.yaml` (SLI + map `/metrics`); 4 rule `config/alert_rules.yaml` + runbook `docs/alerts.md` (thêm mục quality); điền blueprint mục 3.2–3.3 + draft mục 4; thêm `docs/huong-dan-bao-cao-va-chung-minh.md`; cập nhật `docs/grading-evidence.md`.
- [EVIDENCE_LINK]: https://github.com/tttduong/A20-E403-Nhom30-Day13/commit/80f1a71a058b551aebc43cb8ee17cbb7186ccb27

### [MEMBER_D_NAME]
- [TASKS_COMPLETED]: 
- [EVIDENCE_LINK]: 

### [MEMBER_E_NAME]
- [TASKS_COMPLETED]: 
- [EVIDENCE_LINK]: 

---

## 6. Bonus Items (Optional)
- [BONUS_COST_OPTIMIZATION]: (Description + Evidence)
- [BONUS_AUDIT_LOGS]: (Description + Evidence)
- [BONUS_CUSTOM_METRIC]: (Description + Evidence)
