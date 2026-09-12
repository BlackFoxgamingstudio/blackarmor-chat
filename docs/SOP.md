# Standard Operating Procedure: Sovereign Blackarmor Chat

## 1. Service Health Verification
Run `sovereign-blackarmor-chat --health` to confirm the engine responds with status `HEALTHY`.

## 2. Webhook Adapter Operation
The webhook adapter listens on port `8804`:
```bash
python3 n8n/webhook_adapter.py
```
If port 8804 is occupied, check active processes:
```bash
lsof -i :8804
```

## 3. n8n Integration Testing
Send a probe POST request:
```bash
curl -X POST http://localhost:8804/api/v1/execute \
  -H "Content-Type: application/json" \
  -d '{"action": "health_ping", "payload": {"test": true}}'
```
