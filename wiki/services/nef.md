# NEF — Network Exposure Function

> Last updated: 2026-04-12 | Sources: 2 | Related: [overview.md](overview.md), [services/qod.md](services/qod.md), [services/aduna.md](services/aduna.md)

## What It Does
NEF is the **front door** for all CAMARA APIs. Every request to QOD, Aduna, or Turbo Live goes through NEF first. It authenticates, validates, caches, and routes.

## Request Flow
```
1. Request arrives → NEF validates JWT token
2. NEF checks Redis for cached session
   ├── Cache HIT → Return immediately (~2ms)
   └── Cache MISS → Continue
3. NEF validates phone number (E.164 format)
4. NEF queries PostgreSQL for user profile
5. NEF selects carrier (default or preferred)
6. NEF calls carrier API (200-500ms)
7. NEF caches result in Redis (TTL: session duration)
8. NEF returns response
```

## Dependencies
| Dependency | Purpose | What Happens If Down |
|------------|---------|---------------------|
| Redis | Session cache, phone→carrier map, rate limits, tokens | Every request hits carrier → carrier rate limits → cascade failure |
| PostgreSQL | User profiles, sessions, audit log | NEF can't validate users → queues requests → timeout |
| Key Vault | Auth tokens, carrier credentials | New pods can't start, existing pods continue until token expires |
| Carrier APIs | Actual network API calls | Fallback to next carrier (if configured) |

## Scaling
- **Pods:** 3 min, 10 max (HPA at 70% CPU)
- **Known issue:** HPA scaling creates Redis connection spike (all new pods connect simultaneously)
- **Warm-up:** New pods take ~30s to pass health check. First requests may timeout during this window.

## Known Failure Modes
| Failure | Symptom | Root Cause | Fix | Related |
|---------|---------|------------|-----|---------|
| Redis full | NEF timeout (>30s) | Eviction policy wrong or sessions not expiring | Change to allkeys-lru, restart NEF | INC-001 |
| PG pool exhausted | NEF timeout | Connection leaks, long queries | Scale pods, check pool config | — |
| Carrier slow | NEF degraded | Carrier API >10s | Thread pool fills, queue builds | Carrier-specific |
| Token expired | 401 errors | Token refresh failed | Check Key Vault access, rotate | — |

## Configuration
```
DEFAULT_CARRIER=jio          ← Primary carrier
FALLBACK_CARRIERS=airtel,vi  ← Try in order
REDIS_MAX_MEMORY=2.5GB
PG_POOL_SIZE=20
TOKEN_REFRESH_AT=3000s       ← Refresh at 50 min (expires at 60 min)
```

## API Endpoints
See [services/aduna.md](services/aduna.md) for carrier routing logic.
See graph/api-map.md in camara-ops for full endpoint reference.

## Open Questions
- [ ] Exact HPA scaling parameters?
- [ ] Connection pool warm-up strategy?
- [ ] Graceful degradation when ALL carriers down?
