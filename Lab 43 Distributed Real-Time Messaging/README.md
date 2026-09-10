# Lab 43 — Distributed Real-Time Messaging

This lab covers distributed real-time messaging using WebSockets, nginx load balancing, and Redis (Pub/Sub + KV).

## Architecture

![Lab 43 Architecture](https://raw.githubusercontent.com/poridhioss/Shihab_all_lab_assets/main/Lab%2043%20Distributed%20Real-Time%20Messaging/Diagrams/43%20architect.svg)

The architecture shows multiple WebSocket clients connecting through nginx (port 8080) which load-balances connections across three app instances (ports 8001, 8002, 8003). Each app instance communicates bidirectionally with Redis Pub/Sub for broadcasting messages and Redis KV for session/presence state.

## Rehome-on-Reconnect Flow

![Rehome-on-Reconnect Sequence](https://raw.githubusercontent.com/poridhioss/Shihab_all_lab_assets/main/Lab%2043%20Distributed%20Real-Time%20Messaging/Diagrams/43%20Rehome-on-reconnect.svg)

This sequence diagram illustrates how a WebSocket session is rehomed when an instance crashes:

1. Client connects to Instance A with `session_id=s1`; A stores `session:s1 → instance=A` in Redis
2. Instance A crashes
3. Client reconnects to Instance B with the same `session_id=s1`
4. Instance B reads the previous instance mapping from Redis and health-probes A
5. Instance B re-homes the session (`HSET session:s1 instance=B`) and resumes the connection

## Full Sequence Diagram

![Lab 43 Sequence Diagram](https://raw.githubusercontent.com/poridhioss/Shihab_all_lab_assets/main/Lab%2043%20Distributed%20Real-Time%20Messaging/Diagrams/43%20sequence%20diagam.svg)

## Output Screenshots

### Docker Compose Build

![Compose Build](https://raw.githubusercontent.com/poridhioss/Shihab_all_lab_assets/main/Lab%2043%20Distributed%20Real-Time%20Messaging/screenshots/compose%20Build%20.png)

Successful `docker compose build` output showing all six services brought up:

- `lab43-app:latest` — Image built
- `lab43-redis` — Healthy
- `lab43-app-1`, `lab43-app-2`, `lab43-app-3` — Healthy
- `lab43-nginx` — Started

## Asset References

| Asset | Type | Raw URL |
| --- | --- | --- |
| Architecture | Diagram (SVG) | [Link](https://raw.githubusercontent.com/poridhioss/Shihab_all_lab_assets/main/Lab%2043%20Distributed%20Real-Time%20Messaging/Diagrams/43%20architect.svg) |
| Rehome-on-Reconnect | Sequence (SVG) | [Link](https://raw.githubusercontent.com/poridhioss/Shihab_all_lab_assets/main/Lab%2043%20Distributed%20Real-Time%20Messaging/Diagrams/43%20Rehome-on-reconnect.svg) |
| Sequence Diagram | Sequence (SVG) | [Link](https://raw.githubusercontent.com/poridhioss/Shihab_all_lab_assets/main/Lab%2043%20Distributed%20Real-Time%20Messaging/Diagrams/43%20sequence%20diagam.svg) |
| Compose Build | Screenshot (PNG) | [Link](https://raw.githubusercontent.com/poridhioss/Shihab_all_lab_assets/main/Lab%2043%20Distributed%20Real-Time%20Messaging/screenshots/compose%20Build%20.png) |
