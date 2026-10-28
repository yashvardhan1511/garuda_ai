# GARUDA AI architecture

GARUDA AI is organized as a frontend and backend monorepo.

## Components

- **Frontend:** React + TypeScript + Vite dashboard, MapLibre-based map views, and Socket.IO client updates.
- **API:** FastAPI routes for authentication, incidents, and resources.
- **Agent workflow:** `backend/google_agent_adk.py` defines a 12-role asynchronous workflow. Despite the historical filename, the current code imports the Groq SDK and calls the configured Groq chat-completions model.
- **Model configuration:** `GROQ_MODEL` defaults to `llama-3.3-70b-versatile`; all agent roles use that same model unless code is changed.
- **Persistence:** SQLAlchemy with PostgreSQL.
- **Realtime updates:** Socket.IO broadcasts agent status.
- **External services:** Open-Meteo, OSRM, UN ReliefWeb, and ntfy.sh.

## Agent sequence

1. Commander summarizes the incident and delegates.
2. Assessor roles run in parallel: Satellite, Weather, Infrastructure, Social Media, and Risk Prediction.
3. Responder roles run in parallel: Road, Shelter, Medical, and Volunteer.
4. Supply Chain prepares logistics assessment.
5. Decision Intelligence synthesizes the final assessment and dispatches a notification.

## Data reliability

The integrations include fallback values. Hospital capacity is simulated in the current code, and the elevation endpoint is not optical satellite imagery. UI labels and AI summaries must not present fallback/demo values as verified live facts. This prototype must not replace official emergency systems.
