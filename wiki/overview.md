# CAMARA API Platform — Overview

> Last updated: 2026-04-12 | Sources: 2 | Related: [services/nef.md](services/nef.md), [services/qod.md](services/qod.md), [services/aduna.md](services/aduna.md), [services/turbo-live.md](services/turbo-live.md)

## What Is CAMARA?

CAMARA (Common API Framework for Telco APIs) is a GSMA Open Gateway initiative. It exposes telecom network capabilities (QoS, session management, carrier routing) as standardized APIs that developers can call.

**The pitch:** Instead of each carrier having their own API, CAMARA defines a common spec. Every carrier implements the same endpoints.

**The reality:** Every carrier implements the spec *differently*. Same endpoints, different behaviors, different rate limits, different quirks.

## Our Platform

```
                        ┌─────────────┐
                        │  Front Door  │  (Azure Front Door — WAF + routing)
                        └──────┬──────┘
                               │
              ┌────────────────┼────────────────┐
              │                │                │
        ┌─────▼─────┐  ┌──────▼──────┐  ┌──────▼──────┐
        │    NEF     │  │    QOD      │  │ Turbo Live  │
        │  Session   │  │  Quality    │  │  Streaming  │
        │  API       │  │  on Demand  │  │  WebSocket  │
        └─────┬──────┘  └──────┬──────┘  └──────┬──────┘
              │                │                │
              └────────┬───────┘                │
                       │                        │
                 ┌─────▼──────┐                 │
                 │   Aduna    │◄────────────────┘
                 │  Carrier   │
                 │  Aggregator│
                 └─────┬──────┘
                       │
          ┌────────────┼────────────┐
          │            │            │
    ┌─────▼─────┐ ┌───▼────┐ ┌────▼────┐
    │  Airtel   │ │  Jio   │ │   Vi    │
    │  100 RPS  │ │ 200RPS │ │  50 RPS │
    └───────────┘ └────────┘ └─────────┘
```

## Infrastructure

| Component | Detail |
|-----------|--------|
| **AKS Clusters** | PROD, DR, DEV (separate) |
| **Runtime** | Kubernetes, Terraform-managed |
| **Cache** | Azure Cache for Redis P2 (2.5GB) |
| **Database** | Azure Database for PostgreSQL |
| **CI/CD** | Azure DevOps YAML pipelines |
| **Monitoring** | Azure Monitor + KQL + custom alerts |
| **Secrets** | Azure Key Vault (no hardcoded secrets) |

## Key Facts
- NEF is the **gateway** — all APIs go through it. Single point of failure.
- Redis caches sessions, phone→carrier mappings, rate limit counters, and tokens
- Each carrier has different rate limits, latencies, phone formats, and undocumented behaviors
- Aduna abstracts carrier differences but doesn't eliminate them
- Turbo Live uses WebSockets with 25-second keepalive (LB kills idle at 30s)

## Open Questions
- [ ] What's the actual token refresh mechanism in production?
- [ ] What are the exact HPA thresholds per service?
- [ ] What's the DR failover procedure?
- [ ] What's the per-carrier cost structure?
