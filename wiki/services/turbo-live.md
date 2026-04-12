# Turbo Live — WebSocket Streaming

> Last updated: 2026-04-12 | Sources: 2 | Related: [overview.md](overview.md), [services/nef.md](services/nef.md)

## What It Does
Real-time WebSocket streaming for live video/audio sessions. Used for video KYC, telehealth, live broadcasting.

## Connection Protocol
```
WebSocket: wss://turbo.internal/ws/v1/stream
Auth: Bearer token + NEF session ID

Client → Server: {"type": "ping"}           ← Every 25 seconds!
Server → Client: {"type": "pong"}
Server → Client: {"type": "quality", "bitrate": 4500, "latency": 120}
Server → Client: {"type": "degradation", "reason": "network_congestion"}
```

## Critical: Keepalive
```
⚠️ Send ping every 25 seconds. Load balancer kills idle connections at 30s.

If you forget the ping:
  30s → LB drops connection
  Client reconnects → new session → potential brief stream interruption
  Repeated reconnects → carrier rate limit hit → cascade
```

## Dependencies
- Requires active NEF session (X-Session-ID header)
- Quality depends on carrier QoS (if QOD session active)
- Degrades gracefully: high→medium→audio-only→disconnect

## Open Questions
- [ ] Max concurrent streams per session?
- [ ] Reconnection strategy on carrier failover?
- [ ] Recording/archival capability?
