# QOD — Quality on Demand

> Last updated: 2026-04-12 | Sources: 2 | Related: [overview.md](overview.md), [services/nef.md](services/nef.md)

## What It Does
QOD grants temporary bandwidth/latency guarantees for specific devices. Used by apps to request better network quality for video calls, gaming, streaming.

## QoS Profiles
| Profile | Bandwidth | Latency | Use Case |
|---------|-----------|---------|----------|
| QOS_S | 10-50 Mbps | <100ms | Video streaming |
| QOS_E | 5-20 Mbps | <50ms | Gaming |
| QOS_L | 1-5 Mbps | <20ms | Voice/RTC |
| QOS_M | 10-100 Mbps | <200ms | General purpose |
| QOS_A | Best effort | No guarantee | Background |

## Key API
```
POST /qod/v1/sessions
Body: phoneNumber, applicationServer (IP+port), qosProfile, devicePorts, duration
Response: sessionId, qosStatus (REQUESTED→AVAILABLE→UNAVAILABLE), timestamps
```

## Carrier Differences
| | Airtel | Jio | Vi |
|--|--------|-----|----|
| Max session | 4 hours (ignores request) | As requested | As requested |
| QoS delivery | ~80% of requested | ~85% of requested | ~24% of requested ⚠️ |
| Phone format | E.164 strict | Flexible | E.164 strict |
| Response code | 200 | 200 | 201 (not 200!) |

## Open Questions
- [ ] Actual measured QoS delivery per carrier per profile?
- [ ] What happens when QoS can't be delivered — notification mechanism?
- [ ] Billing per QoS profile?
