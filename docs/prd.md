# GARUDA AI — Product brief

## Goal
Provide a disaster-response command-center prototype that organizes incident reports, presents agent status updates, and produces an AI-assisted tactical summary.

## Core capabilities
- Incident intake and tracking through the FastAPI API.
- A 12-role agent workflow using the configured Groq model.
- Realtime agent status updates over Socket.IO.
- Weather, elevation, routing, and historical disaster lookups through public APIs.
- A notification hook through ntfy.sh.
- Dashboard views for operational context and resources.

## Agent roles
1. Commander
2. Satellite
3. Weather
4. Infrastructure
5. Social Media
6. Road
7. Shelter
8. Medical
9. Volunteer
10. Supply Chain
11. Risk Prediction
12. Decision Intelligence

All roles currently use the same configured Groq model, `llama-3.3-70b-versatile` by default. The role descriptions are prompts, not separate fine-tuned models.

## Limitations
- Hospital capacity values are simulated.
- Some external integrations return fallback/demo values when requests fail.
- Social media monitoring is described as an agent role, but no direct social platform ingestion is configured in the current MCP tool file.
- The prototype is not an emergency dispatch service and must not be relied on as the sole source for rescue decisions.
