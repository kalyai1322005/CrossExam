# The step-by-step build

CrossExam is built up in eight steps. Each step starts from a problem we saw, and adds only what that problem needs. The same shipment (PO-40002) is used throughout, so you can follow one paper trail from step 1 to step 8.

Load the exact demo papers first: `make demo-fixed` (see `demo-data/README.md`).

> **Two ways to follow this.** Read the table below and open the files it points to, right here on `main` -- that is what this page is for. Or check out the same eight steps **one branch at a time**, starting from [`build/map`](https://github.com/mayank953/crossexam-project/tree/build/map): each branch is a complete, runnable checkout at exactly that point, with its own `README.md` and `STEP.md`. Steps 1 and 2 as plain files (`ladder/`) and the `build/step-1-one-agent` / `build/step-2-three-fixes` branches are the same two steps; the branches additionally isolate steps 3a-3e as their own tags.

| Step | The problem we saw | What we add | Where to look | Try it |
|---|---|---|---|---|
| 1 | Nothing yet: can a model read one paper? | One LangChain agent and a form to fill | `ladder/step1_one_agent.py` | `ladder/README.md` |
| 2 | The wrong form for non-invoices, a repeat that costs the same, a stop that loses everything | Nothing new: a kind of paper, a file fingerprint, results saved as they arrive | `ladder/step2_three_small_fixes.py` | `ladder/README.md` |
| 3 | A bundle read as one invoice, one lorry written two ways, no order to compare with | Nothing new: plain code that splits, normalises, groups and matches exactly | `app/ingestion/readers.py`, `app/rules/normalize.py`, `app/services/grouping.py`, `app/rules/match_engine.py` | `make test` |
| 4 | Results sit in a file on one laptop; nobody else can see or decide | FastAPI, PostgreSQL and the React screen | `app/api/main.py` (17 routes), `app/db/models.py`, `web/` | `make up`, then http://localhost:8080 and http://localhost:8000/docs |
| 5 | Month end: one provider limit, thousands of papers, workers that die | Celery and Redis | `app/workers/celery_app.py`, `app/workers/tasks.py`, `app/workers/ratelimit.py` | upload the 51 papers; watch http://localhost:5555 |
| 6 | Finding a problem is not deciding what to pay | Agents that propose, and approval by role | `app/agents/`, the `decide` route in `app/api/main.py` | as the AP executive try to approve PO-40002 (refused with 403), then approve as the finance manager |
| 7 | Is work moving, what did it cost, why did the agent say that? | Flower, metrics, traces, LangSmith | `app/observability/`, `monitoring/` | Flower on 5555, `/metrics` on 8000; LangSmith needs your own key in `.env` |
| 8 | It runs on one laptop | Terraform, Kubernetes, cloud monitoring | `infra/` | not part of this guide: it needs a cloud project |

## What was measured, and what was not

Steps 1 and 2 were run on the nine papers of shipments 0002 and 0010, and step 5 was tested by killing a worker mid-batch. Prometheus and Grafana (`make monitoring`) and everything in step 8 were not run for this guide.

## Order of work

Follow the steps in order. At each step, first see the problem (run the previous step on the papers that break it), then add the part, then run the same papers again and compare.
