# GARUDA AI — Disaster Response Command Center

GARUDA AI is a disaster-response coordination prototype with a React command-center interface, a FastAPI backend, a 12-role agent workflow, and external data integrations.

> **Safety note:** This is a prototype, not an emergency service. AI output, fallback data, and route estimates can be wrong. Confirm information with official emergency services and trained responders.

## AI model and agent workflow

- **LLM provider:** Groq API
- **Configured model:** `llama-3.3-70b-versatile` (configurable with `GROQ_MODEL`)
- **Agent roles:** Commander, Satellite, Weather, Infrastructure, Social Media, Road, Shelter, Medical, Volunteer, Supply Chain, Risk Prediction, and Decision Intelligence.
- These are **12 role-specific agent calls using the same configured model**, not 12 different AI models.
- **Realtime status:** Socket.IO broadcasts agent updates to the dashboard.

The code uses the Groq SDK (`groq` package). It does not currently use Google ADK or a local Ollama model. A valid Groq API key is required for agent analysis.

## Integrations

- **Open-Meteo:** current weather and elevation lookups.
- **OSRM public routing service:** route distance and duration.
- **UN ReliefWeb API:** historical disaster lookup.
- **ntfy.sh:** push notifications; default topic is `garuda_ai_alerts`, configurable using `NTFY_TOPIC`.
- **Hospital information:** current implementation returns fixed Miami-area hospital names with randomized ICU counts, marked in code as simulated telemetry. Do not treat these counts as live capacity.
- Several API functions return fallback/demo values if a public API fails. Review the `status` and `source` fields; fallback data must not be treated as verified live information.

## Stack

- Frontend: React, TypeScript, Vite, Tailwind CSS, MapLibre/React Map GL, Socket.IO client, Three.js.
- Backend: Python 3.11, FastAPI, SQLAlchemy, PostgreSQL, Socket.IO, Groq SDK.
- Docker Compose services: PostgreSQL, Redis, FastAPI backend, and Vite frontend.

## Run locally on Windows

### 1. Requirements

Install:
- Docker Desktop (with Docker Compose)
- Git (optional)
- A Groq API key from the Groq developer console

### 2. Configure the API key

In the GARUDA AI project root, copy `.env.example` to `.env` and edit `.env`:

```powershell
Copy-Item .env.example .env
notepad .env
```

Set `GROQ_API_KEY` to your real key. Keep the key private and do not commit `.env`.

### 3. Start the application

From the project root (the folder containing `docker-compose.yml`):

```powershell
docker compose up --build
```

Then open:
- Frontend: http://localhost:5173
- Backend health: http://localhost:8000/health
- API docs: http://localhost:8000/docs

Stop the services with `Ctrl+C`, or run `docker compose down` in another terminal. Database data is stored in a Docker volume.

### 4. Run frontend separately (optional)

```powershell
cd frontend
npm install
npm run dev
```

If running the backend outside Docker, configure `DATABASE_URL` to point to a reachable PostgreSQL database and set `GROQ_API_KEY`, `GROQ_MODEL`, and `SECRET_KEY` in the backend environment. The default local database requires PostgreSQL; this project is not configured for SQLite.

## Important configuration

- `.env.example` is a template only. You must supply your own `GROQ_API_KEY`.
- `GROQ_MODEL` defaults to `llama-3.3-70b-versatile`.
- `NTFY_TOPIC` defaults to `garuda_ai_alerts`. ntfy topics are public by default; do not send personal or sensitive information through a public topic.
- For deployment, configure `DATABASE_URL`, `SECRET_KEY`, `GROQ_API_KEY`, `GROQ_MODEL`, `NTFY_TOPIC`, and `CORS_ORIGINS` with secure values and your actual frontend domain.
- The demo includes placeholder deployment URLs in metadata. Update them before publishing.

## Attribution and license

This project retains the upstream MIT license and its required copyright notice. The starting codebase is derived from **ResQNet Contributors' ResQNet project** (MIT License, copyright notice retained in `LICENSE`). GARUDA AI is a rebranded adaptation; see `UPSTREAM_ATTRIBUTION.md`. Preserve these notices in redistributed copies.

