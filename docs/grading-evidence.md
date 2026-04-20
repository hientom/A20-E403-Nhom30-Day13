# Evidence Collection Sheet

## Required screenshots
- Langfuse trace list with >= 10 traces
- One full trace waterfall
- JSON logs showing correlation_id
- Log line with PII redaction
- Dashboard with 6 panels
- Alert rules with runbook link

## Optional screenshots
- Incident before/after fix
- Cost comparison before/after optimization
- Auto-instrumentation proof

---

## Member A — Logging & PII (Tạ Thị Thuỳ Dương)

| Việc đã làm (file) | Trạng thái |
|--------------------|------------|
| `app/middleware.py` | Implement correlation ID: clear_contextvars(), extract x-request-id, bind_contextvars(), response headers x-request-id + x-response-time-ms |
| `app/pii.py` | Thêm regex Passport VN (`[A-Z]\d{7}`), CMND/CCCD (`\d{9}|\d{12}`), địa chỉ VN (phường/quận/thành phố) |
| `app/logging_config.py` | Uncomment processor `scrub_event` trong danh sách structlog processors |

**Bằng chứng:**
- Screenshot log có `"correlation_id": "req-xxxxxxxx"` → `docs/evidence/log-pii-redacted-email-and correlation-id.png`
- Screenshot log đã redact PII → `docs/evidence/log-pii-redacted-creditcard.png`

**Git Evidence (Member A):** 
code (without screenshot): https://github.com/tttduong/A20-E403-Nhom30-Day13/commit/0228cf554620421bb7bcd58a21176242e0e60f98

screenshot evidence: https://github.com/tttduong/A20-E403-Nhom30-Day13/commit/ef1a6c30413590b69939aefdbca1a6dc010f55ae
---

## Member B — Tracing & Enrichment (Nguyễn Thị Thu Hiền)

| Việc đã làm (file) | Trạng thái |
|--------------------|------------|
| `app/main.py` | `bind_contextvars(user_id_hash, session_id, feature, model, env)` sau khi parse body endpoint /query |
| `app/agent.py` | Xác nhận decorator `@observe` trên hàm pipeline chính |
| `app/tracing.py` | Verify Langfuse client khởi tạo đúng từ `.env` |

**Bằng chứng:**
- Log line có `"user_id_hash"`, `"session_id"`, `"feature"`, `"model"` trong JSON output
- ≥ 20 traces trên Langfuse UI sau load test

**Git Evidence (Member B):** https://github.com/tttduong/A20-E403-Nhom30-Day13/commit/a3aa355fc48ea24b26e300e77f1b6c1cfe82dcdc

---

## Member C — SLO & Alerts (Hồ Quang Hiển)

| Việc đã làm (file) | Trạng thái |
|--------------------|------------|
| `config/slo.yaml` | Đã chỉnh (SLI + ghi chú map metrics) |
| `config/alert_rules.yaml` | 4 rule + runbook link |
| `docs/alerts.md` | Runbook + mapping SLI + mục 4 quality |
| `docs/blueprint-template.md` | Điền mục 1 (nhóm/repo/C), 2 (validate/PII), 3.2–3.3 (SLO + link runbook), 4 (draft incident), 5 (Member C) |
| `docs/huong-dan-bao-cao-va-chung-minh.md` | Hướng dẫn lệnh + chụp ảnh |
| `docs/evidence/validate_logs_output.txt` | Output validate 100/100 |

**Bằng chứng:**
- `docs/evidence/alert-rules-screenshot.png`
- `docs/evidence/validate_logs_output.txt` — score 100/100

**Git Evidence (Member C):** https://github.com/tttduong/A20-E403-Nhom30-Day13/commit/80f1a71a058b551aebc43cb8ee17cbb7186ccb27

---

## Member D — Load Test & Dashboard (ịnh Đức An)

| Việc đã làm (file) | Trạng thái |
|--------------------|------------|
| `scripts/load_test.py` | Chạy baseline (≥ 100 requests, concurrency 5) + stress test |
| `scripts/inject_incident.py` | Inject `rag_slow` — ghi nhận P95 tăng từ ~800ms lên >26,000ms |
| `scripts/generate_dashboard.py` | Tự động vẽ 6 dashboard panels từ Metrics API |
| `scripts/check_langfuse.py` | Script kiểm tra traces trên Langfuse |
| `app/main.py` | Hỗ trợ bind context enrichment |
| `docs/evidence/dashboard_*.png` | 6 dashboard screenshots (latency, traffic, errors, cost, tokens, quality) |

**Bằng chứng:**
- `docs/evidence/dashboard_latency.png`
- `docs/evidence/dashboard_traffic.png`
- `docs/evidence/dashboard_errors.png`
- `docs/evidence/dashboard_cost.png`
- `docs/evidence/dashboard_tokens.png`
- `docs/evidence/dashboard_quality.png`

**Git Evidence (Member D):** https://github.com/tttduong/A20-E403-Nhom30-Day13/commit/b9bea5ef03f160087abab42694837c042c061d79

---

## Member E — Demo & Report (Lương Thanh Hậu)

| Việc đã làm (file) | Trạng thái |
|--------------------|------------|
| `docs/blueprint-template.md` | Điền đầy đủ tất cả tag còn trống: §2 TOTAL_TRACES_COUNT, §3.1 screenshot paths + trace explanation, §5 tên + task tất cả 5 thành viên |
| `docs/grading-evidence.md` | Cập nhật đầy đủ tất cả các section cho 5 thành viên với link commit cụ thể |
| `docs/mock-debug-qa.md` | Tạo kịch bản Q&A cho demo live (câu hỏi + đáp án chuẩn) |
| `scripts/validate_logs.py` | Chạy lần cuối — đạt **100/100** |

**Validate logs final output:**
```
--- Lab Verification Results ---
Total log records analyzed: 99
Records with missing required fields: 0
Records with missing enrichment (context): 0
Unique correlation IDs found: 44
Potential PII leaks detected: 0

--- Grading Scorecard (Estimates) ---
+ [PASSED] Basic JSON schema
+ [PASSED] Correlation ID propagation
+ [PASSED] Log enrichment
+ [PASSED] PII scrubbing

Estimated Score: 100/100
```

**Kịch bản demo live (thứ tự):**
1. Khởi động app: `uvicorn app.main:app --reload`
2. Chạy load test: `python scripts/load_test.py --concurrency 5`
3. Xem structured logs trong `data/logs.jsonl` — chỉ ra `correlation_id` và `user_id_hash`
4. Xem traces trên Langfuse UI — chỉ ra waterfall spans (RAG, LLM, tool)
5. Xem dashboard 6 panels — chỉ ra latency P95 và cost
6. Inject incident: `python scripts/inject_incident.py --scenario rag_slow` → alert kích hoạt
7. Fix incident: `POST /incidents/rag_slow/disable` → latency trở về bình thường
