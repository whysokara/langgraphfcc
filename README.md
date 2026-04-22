# langgraphfcc

## FastAPI service

This repo includes a small FastAPI app in `api_server/` that exposes a LangGraph sentiment workflow.

### Run

1) Install deps:

```bash
pip install -r requirements.txt
```

2) Start the server:

```bash
uvicorn api_server.app:app --reload
```

3) Try it:

```bash
curl -s -X POST http://127.0.0.1:8000/process \
  -H 'content-type: application/json' \
  -d '{"text":"This product is terrible"}'
```

Health check:

```bash
curl -s http://127.0.0.1:8000/healthz
```
