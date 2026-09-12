# Architecture: Sovereign BlackArmor Chat

## Overview

**Package ID:** `PKG-024`  
**Domain:** Enterprise Web Chat & Real-Time Sync  
**Microservice Port:** `8804`  
**n8n Webhook Path:** `blackarmor-chat-trigger`  
**GitHub:** [BlackFoxgamingstudio/blackarmor-chat](https://github.com/BlackFoxgamingstudio/blackarmor-chat)

End-to-end encrypted enterprise chat system with WebSocket real-time sync, message threading, presence detection, and n8n automation hooks.

---

## System Architecture

```
                     ┌──────────────────────────────────┐
                     │       Sovereign BlackArmor Chat     │
                     │       Port: 8804            │
                     ├──────────────┬───────────────────┤
   n8n Webhook ────▶ │  REST API    │   Core Engine     │
   HTTP POST         │  /api/v1/*   │   Dispatcher      │
                     └──────┬───────┴────────┬──────────┘
                            │                │
              ┌─────────────▼────────────────▼─────────┐
              │          Component Layer                 │
              │  WebSocketServer | MessageBroker   | PresenceEngi  │
              └────────────────────────┬────────────────┘
                                       │
              ┌────────────────────────▼────────────────┐
              │      n8n Central Event Bus (:5678)       │
              └─────────────────────────────────────────┘
```

## Core Components

### `WebSocketServer`
Handles all websocketserver operations. Exposes async methods callable from the core dispatcher.

### `MessageBroker`
Handles all messagebroker operations. Exposes async methods callable from the core dispatcher.

### `PresenceEngine`
Handles all presence operations. Exposes async methods callable from the core dispatcher.

### `EncryptionLayer`
Handles all encryptionlayer operations. Exposes async methods callable from the core dispatcher.

### `ThreadManager`
Handles all thread operations. Exposes async methods callable from the core dispatcher.

---

## API Contract

All interactions follow the SBB standard envelope:

```http
POST /api/v1/execute
Content-Type: application/json
X-SBB-API-Key: <api-key>

{
  "action": "<operation>",
  "payload": {},
  "trace_id": "optional-uuid"
}
```

**Success Response (HTTP 200):**
```json
{
  "status": "success",
  "data": {},
  "trace_id": "...",
  "timestamp": "2025-01-01T00:00:00Z"
}
```

**Health Check:**
```http
GET /health
→ {"status": "healthy", "service": "sovereign-blackarmor-chat", "port": 8804}
```

## Integration Matrix

| System | Protocol | Direction | Purpose |
|--------|----------|-----------|---------|
| n8n Event Bus (:5678) | HTTP POST | Outbound | Event forwarding |
| n8n Webhook | HTTP POST | Inbound | Trigger execution |
| SBB Codebase Vault (:8766) | HTTP | Outbound | Code analysis |
| SBB Patterns Bible (:8794) | HTTP | Outbound | Standards validation |
| External APIs | HTTPS | Outbound | Domain-specific data |

## Deployment Architecture

```yaml
# docker-compose excerpt
sovereign-blackarmor-chat:
  image: sovereign-blackarmor-chat:latest
  ports: ["8804:8804"]
  healthcheck:
    test: curl -f http://localhost:8804/health
    interval: 30s
```

## Security Model

| Control | Implementation |
|---------|---------------|
| Authentication | `X-SBB-API-Key` header (env: `SBB_API_KEY`) |
| Rate Limiting | 100 req/min per client IP |
| Input Validation | Pydantic models (strict mode) |
| Container Security | Non-root user (`appuser:1001`) |
| Secrets | Environment variables only (never hardcoded) |
| TLS | Terminate at reverse proxy (nginx/caddy) |

## Tags
`chat`, `websocket`, `e2e-encryption`, `realtime`
