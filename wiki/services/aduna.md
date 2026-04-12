# Aduna — Carrier Aggregation

> Last updated: 2026-04-12 | Sources: 2 | Related: [overview.md](overview.md), [services/nef.md](services/nef.md)

## What It Does
Aduna abstracts multiple carriers behind a single interface. Handles carrier selection, fallback routing, and carrier health tracking.

## Carrier Selection Logic
```
1. If preferredCarrier specified → try it first
2. If rejected/error → try fallbackCarriers in order
3. If all fail → return error with per-carrier failure details
```

## Carrier Status
| Carrier | Status | Notes |
|---------|--------|-------|
| Airtel | active | Maintenance Tuesdays 2-4 AM IST |
| Jio | active | Most reliable, highest rate limit |
| Vi | degraded | Rural coverage gaps, high error rate during peak |

## Routing Recommendations
```
Priority (latency):     Jio (150ms) → Airtel (200ms) → Vi (300ms)
Priority (volume):      Jio (200 RPS) → Airtel (100 RPS) → Vi (50 RPS)
Priority (reliability): Jio (0.1% errors) → Airtel (0.3%) → Vi (1.5%)
```

## Open Questions
- [ ] How does Aduna track carrier health — heartbeat or passive observation?
- [ ] Is carrier selection configurable per API or global?
- [ ] What's the failover timeout before trying next carrier?
