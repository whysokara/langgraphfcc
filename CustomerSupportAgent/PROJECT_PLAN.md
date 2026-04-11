# CustomerSupportAgent — Build Plan (Phased)

This file stores the phased plan for building a production-grade customer support agent step-by-step, without getting overwhelmed.

## Phase 0 — Scope + “done” definition (1–2 sessions)
- Pick channels: `email`, `web chat`, `slack`, `zendesk/freshdesk`, etc.
- Define the agent’s job: triage only vs. draft replies vs. auto-resolve.
- Define KPIs: first-response time, resolution rate, CSAT proxy, hallucination rate, escalation rate.
- Write 10–20 real user intents + expected outcomes (your first acceptance tests).
- Decide constraints: PII rules, allowed actions, tone/brand, “never do” list.

## Phase 1 — Repo scaffolding + local dev workflow (1 session)
- Convert notebook into a real app layout (keep `main.ipynb` as scratch, but don’t ship it).
- Add: `src/`, `tests/`, `configs/`, `scripts/`, `.env.example`, logging config.
- Add dependency management (Python + `pyproject.toml`) and a single “run locally” command.
- Add secret handling (env vars), and a minimal config system (dev vs prod).

## Phase 2 — Core agent MVP (no real integrations yet) (2–4 sessions)
Goal: an end-to-end loop that works with stubbed tools.
- Define conversation state schema (user msg, history, customer profile, ticket context).
- Implement a LangGraph-style flow (or equivalent):
  - classify intent → decide: answer / ask-clarifying / escalate
  - generate draft response + citations-to-internal-context (even if mocked)
- Add “guardrails” MVP: refusal policy, escalation triggers, safe completion template.
- Build a simple CLI runner to test multi-turn conversations quickly.

## Phase 3 — Knowledge + retrieval (RAG) (2–6 sessions)
Goal: answer from your docs reliably.
- Create a “knowledge ingestion” pipeline (markdown/HTML/PDF → chunks → embeddings).
- Implement retrieval + reranking + “answer with sources” behavior.
- Add “unknown / insufficient info” behavior (ask clarifying or escalate).
- Add content freshness strategy (reindex schedule, doc versioning).

## Phase 4 — Real tools + ticketing workflow (3–8 sessions)
Goal: actually help support ops.
- Integrations (start with one): ticket create/update, order lookup, refund policy checks, etc.
- Permissioning per tool (read-only first; write actions require strong confirmation).
- Add human-in-the-loop: suggested reply → reviewer edits → send.
- Add audit trail: tool calls, inputs/outputs, who approved what.

## Phase 5 — Evaluation + reliability hardening (ongoing, but start early)
Goal: production-grade quality control.
- Build an eval dataset from Phase 0 intents + real anonymized transcripts later.
- Automated checks: grounding, policy compliance, tool-call correctness, tone.
- Add regression tests for prompt/graph changes.
- Load testing for concurrency + latency budgets.

## Phase 6 — Observability + deployment (2–6 sessions)
Goal: ship safely and diagnose issues.
- Structured logs, tracing (request → graph nodes → tool calls), metrics.
- Error handling + retries + timeouts + circuit breakers for external APIs.
- Deploy target: container + one environment first (staging), then prod.
- CI/CD: lint/test, security checks, deploy gates.

## Phase 7 — Launch + iteration (forever)
- Start with “assist mode” (draft-only), then graduate to “auto actions” gradually.
- Weekly review: failure cases → add to evals → fix → regression lock.

## Next decisions (to tighten milestones)
- Intended channel (CLI/web chat/Zendesk/etc.)
- First tools/integrations (knowledge base only vs ticketing + order lookup)

