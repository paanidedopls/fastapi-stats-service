# FastAPI Descriptive Statistics API

Implements `GET /stats?values=1,2,3` with dynamically computed descriptive statistics, strict CORS for `https://dash-de9fkt.example.com`, and `X-Request-ID` / `X-Process-Time` headers on all HTTP responses, including preflight responses.

The response email is `24f2001785@ds.study.iitm.ac.in`.

## Run locally

```bash
python -m venv .venv
source .venv/bin/activate  # Windows Git Bash: source .venv/Scripts/activate
pip install -r requirements.txt
uvicorn main:app --reload
```

Test the GET endpoint:

```bash
curl -i 'http://127.0.0.1:8000/stats?values=1,2,3'
```

Expected statistic fields: count 3, sum 6, min 1, max 3, mean 2.0.

Allowed-origin preflight:

```bash
curl -i -X OPTIONS 'http://127.0.0.1:8000/stats' \
  -H 'Origin: https://dash-de9fkt.example.com' \
  -H 'Access-Control-Request-Method: GET'
```

Rejected-origin preflight (must have no `Access-Control-Allow-Origin` header):

```bash
curl -i -X OPTIONS 'http://127.0.0.1:8000/stats' \
  -H 'Origin: https://evil.example' \
  -H 'Access-Control-Request-Method: GET'
```

## Deploy on Render

Push these files to a GitHub repository, then create a new **Blueprint** in Render and select that repository. Render will read `render.yaml`. After deployment, copy the service's HTTPS base URL (not `/stats`) into the assignment.
