# System Design — CAMARA/AKS Architecture Knowledge

> Last updated: 2026-04-12 | Use for system design interview rounds

## My Architecture (I Can Draw This From Memory)

```
┌──────────────────────────────────────────────────────────────┐
│                        AZURE FRONT DOOR                      │
│                   (WAF + SSL + Global Routing)               │
└──────────────────────────┬───────────────────────────────────┘
                           │
              ┌────────────┼────────────────┐
              │            │                │
        ┌─────▼─────┐ ┌───▼──────┐  ┌──────▼─────┐
        │    NEF     │ │   QOD    │  │ Turbo Live │
        │  (3 pods)  │ │ (3 pods) │  │  (3 pods)  │
        └─────┬──────┘ └────┬─────┘  └──────┬─────┘
              │              │               │
              └──────┬───────┘               │
                     │                       │
              ┌──────▼──────┐                │
              │    Aduna    │◄───────────────┘
              │  (2 pods)   │
              └──────┬──────┘
                     │
        ┌────────────┼────────────┐
        │            │            │
  ┌─────▼─────┐┌────▼─────┐┌────▼─────┐
  │  Airtel   ││   Jio    ││    Vi    │
  │  100 RPS  ││  200 RPS ││   50 RPS │
  └───────────┘└──────────┘└──────────┘

Infrastructure:
  AKS-PROD: 6 nodes (D4s_v5, 4 vCPU, 16GB each)
  AKS-DR:   3 nodes (standby)
  AKS-DEV:  2 nodes

Data Layer:
  Redis P2 (2.5GB, zone-redundant, HA)
  PostgreSQL (Azure Database, 20 connection pool)

CI/CD:
  Azure DevOps YAML pipelines
  Terraform for infra, Helm for services
  ACR for container images
  
Secrets:
  Azure Key Vault (all secrets, certificates)
  Service Principals (ACR auth, carrier API auth)
```

## Design Decisions I Can Explain

### Why AKS, Not AKS-Essentials or Container Apps?
```
- Need full Kubernetes control (network policies, custom CNI)
- Multiple services with complex inter-dependencies
- Need separate namespaces per CAMARA service
- DR cluster requires identical config (Terraform managed)
```

### Why Front Door, Not Just AKS Ingress?
```
- WAF protection (carrier APIs are externally exposed)
- Global routing (if we expand beyond India)
- SSL termination at edge
- DDoS protection
```

### Why Redis, Not In-Memory Cache?
```
- Session data must survive pod restarts
- Shared across multiple NEF pods
- Sub-millisecond latency requirement (<10ms for cache hit)
- HA via zone redundancy
```

### Why Terraform, Not ARM Templates or Bicep?
```
- Team expertise
- Better state management
- Modular and reusable
- Multi-resource type support
```

## How I'd Design It Differently Today

```
Things I'd improve (shows growth mindset):

1. Migrate Service Principals → Managed Identity
   "We still use SPs for ACR auth. MI is more secure,
    no secret rotation needed. Migration planned for Q3."

2. Add Pod Disruption Budgets
   "We have no PDBs. Node drains could evict all pods.
    I've documented this risk and proposed adding PDBs."

3. Implement Network Policies
   "Namespaces can talk to each other freely. 
    Should restrict to explicit allow-lists."

4. Add Observability Gap
   "Missing custom metrics for carrier latency.
    Would add Prometheus + Grafana for deep visibility."

5. Consider Event-Driven Architecture
   "Carrier API calls are synchronous. Could use 
    async with message queue for better resilience."
```

## System Design Interview Templates

### If Asked to Design: API Gateway
```
Draw from NEF experience:
  - Authentication (JWT validation, token refresh)
  - Rate limiting (per carrier, sliding window)
  - Caching (Redis, TTL strategy, eviction)
  - Routing (carrier selection, fallback)
  - Monitoring (latency, error rate, cache hit rate)
```

### If Asked to Design: Notification System
```
Draw from Turbo Live experience:
  - WebSocket management (keepalive, reconnection)
  - Fan-out (multiple carriers)
  - Delivery guarantees (at-least-once)
  - Degradation strategy (high→medium→audio→disconnect)
```

### If Asked to Design: Distributed Cache
```
Draw from Redis experience:
  - Eviction policies (LRU, LFU, TTL)
  - HA (zone redundancy, failover)
  - Monitoring (memory, hit rate, fragmentation)
  - Scaling (vertical P1→P2→P3, horizontal sharding)
```

### If Asked to Design: Microservices Platform
```
Draw from full CAMARA platform:
  - Service mesh patterns (service discovery, circuit breakers)
  - Observability (distributed tracing, metrics, logging)
  - Deployment strategy (rolling update, canary)
  - Infrastructure as code (Terraform + Helm)
  - CI/CD (Azure DevOps YAML, multi-stage pipelines)
```
