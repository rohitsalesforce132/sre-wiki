# SRE Wiki — Personal LLM-Powered Knowledge Base

> **Inspired by [Andrej Karpathy's LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)**

A self-growing, AI-maintained knowledge base for CAMARA API operations, AKS infrastructure, and SRE on-call work.

**You don't write it. The AI writes it. You read it, explore it, and direct it.**

---

## Why This Exists

### The Problem with How We Usually Work

```
Traditional knowledge management:

  Monday:    Incident happens. You debug for 2 hours. Fix it.
  Tuesday:   You think "I should write a runbook." You don't.
  Wednesday: Someone asks "Why did NEF break?" You explain for 30 min.
  Thursday:  You explain it again to someone else.
  Friday:    You explain it AGAIN.
  Next month: Same incident happens. Different engineer. Same 2-hour debug.

  Knowledge is lost because WRITING IT DOWN is tedious.
```

### What SRE Wiki Does Differently

```
SRE Wiki approach:

  Monday:    Incident happens. You drop the postmortem into raw/.
             Copilot reads it, creates 10 wiki pages, cross-references everything.
  Tuesday:   Someone asks "Why did NEF break?"
             Copilot reads wiki → instant answer with links to details.
  Wednesday: New carrier quirk discovered. Drop notes into raw/.
             Copilot updates carrier page AND all 5 service pages that reference it.
  Next month: Same incident pattern detected. Wiki already has the answer.
              Engineer solves it in 10 minutes instead of 2 hours.

  Knowledge COMPOUNDS. Every source makes the wiki smarter.
```

### The Key Insight (from Karpathy)

> RAG is stateless. Every question → search docs → synthesize → throw away.
> The wiki is **persistent**. Cross-references are already built. Contradictions are already flagged. The synthesis already reflects everything you've read. It keeps getting richer with every source you add.

---

## How It Works

### Three Layers

```
┌─────────────────────────────────────────────────┐
│                                                  │
│   RAW SOURCES (you curate, AI never modifies)    │
│   ├── 2026-04-12-incident-001-postmortem.md      │
│   ├── 2026-04-10-aks-best-practices.md           │
│   └── carrier-docs/                              │
│                                                  │
├─────────────────────────────────────────────────┤
│                                                  │
│   WIKI (AI writes, you read & explore)           │
│   ├── index.md       ← Catalog of everything     │
│   ├── log.md         ← Timeline of changes       │
│   ├── overview.md    ← Big picture synthesis      │
│   ├── services/      ← Per-service deep dives     │
│   ├── carriers/      ← Per-carrier intelligence   │
│   ├── incidents/     ← Per-incident analysis      │
│   ├── concepts/      ← Topic deep dives           │
│   ├── patterns/      ← Recurring patterns         │
│   └── decisions/     ← Architecture decisions     │
│                                                  │
├─────────────────────────────────────────────────┤
│                                                  │
│   SCHEMA.md (rules for how AI maintains wiki)    │
│                                                  │
└─────────────────────────────────────────────────┘
```

### Three Operations

| Operation | What You Do | What AI Does |
|-----------|-------------|--------------|
| **INGEST** | Drop source into `raw/` | Read it, create 5-15 wiki pages, update index & log |
| **QUERY** | Ask a question in Copilot Chat | Search wiki, synthesize answer, save valuable answers as new pages |
| **LINT** | Say "lint the wiki" | Find orphans, contradictions, stale data, gaps, missing pages |

---

## Directory Structure

```
sre-wiki/
│
├── SCHEMA.md                        ← AI agent instructions (edit rarely)
├── README.md                        ← This file
│
├── raw/                             ← YOUR SOURCES (immutable)
│   ├── README.md                    ← How to name/organize sources
│   ├── incidents/                   ← Postmortems, incident timelines
│   ├── articles/                    ← Blog posts, tutorials, guides
│   ├── docs/                        ← Internal docs, specs, runbooks
│   └── notes/                       ← Quick notes, Slack exports, meeting notes
│
└── wiki/                            ← AI-MAINTAINED (don't edit directly)
    ├── index.md                     ← Master catalog of every page
    ├── log.md                       ← Chronological changelog
    ├── overview.md                  ← Big picture synthesis
    │
    ├── services/                    ← One page per service
    │   ├── nef.md                   ← NEF Session API
    │   ├── qod.md                   ← Quality on Demand
    │   ├── aduna.md                 ← Carrier Aggregation
    │   └── turbo-live.md            ← WebSocket Streaming
    │
    ├── carriers/                    ← One page per carrier
    │   ├── airtel.md                ← (to be created on ingest)
    │   ├── jio.md                   ← (to be created on ingest)
    │   └── vi.md                    ← (to be created on ingest)
    │
    ├── incidents/                   ← One page per incident
    │   └── inc-001-nef-redis.md     ← (to be created on ingest)
    │
    ├── concepts/                    ← Topic deep dives
    │   ├── caching-strategy.md      ← (to be created on ingest)
    │   ├── rate-limiting.md         ← (to be created on ingest)
    │   └── session-management.md    ← (to be created on ingest)
    │
    ├── patterns/                    ← Recurring patterns
    │   ├── cascade-failure.md       ← (to be created on ingest)
    │   └── thundering-herd.md       ← (to be created on ingest)
    │
    ├── decisions/                   ← Architecture Decision Records
    │   └── adr-001-redis-p2.md      ← (to be created on ingest)
    │
    └── comparisons/                 ← Side-by-side analyses
        └── carrier-comparison.md    ← (to be created on ingest)
```

---

## Setup

### Option 1: VS Code + GitHub Copilot (Office-Friendly)

```
1. Open sre-wiki/ in VS Code
2. Open Copilot Chat (Ctrl+Shift+I)
3. Add SCHEMA.md to your .github/copilot-instructions.md:
   cp SCHEMA.md .github/copilot-instructions.md
4. Start using it — see examples below
```

### Option 2: VS Code + Claude Code / Codex (Home)

```
1. Open sre-wiki/ in VS Code
2. Open Claude Code / Codex terminal
3. It reads SCHEMA.md automatically
4. Start ingesting sources
```

### Option 3: Obsidian (Best for Exploration)

```
1. Open sre-wiki/ as an Obsidian vault
2. Use graph view to see connections between pages
3. Use quick switcher (Ctrl+O) to jump between pages
4. Use backlinks panel to see what references each page
5. Keep Copilot Chat open on the side for queries

Recommended Obsidian plugins:
  - Dataview (query frontmatter)
  - Graph Analysis (find related pages)
  - Marp (generate slide decks from wiki pages)
```

---

## Usage Examples

### Example 1: Ingest an Incident Postmortem

**Step 1: Drop the source**
```
Save your postmortem to:
  raw/incidents/2026-04-08-inc-001-nef-redis-timeout.md
```

**Step 2: Tell the AI**
```
You (in Copilot Chat):
  "Ingest raw/incidents/2026-04-08-inc-001-nef-redis-timeout.md"
```

**Step 3: What the AI does**
```
AI reads the postmortem and:

1. Creates wiki/incidents/inc-001-nef-redis-timeout.md
   - Timeline, root cause, fix, lessons learned
   
2. Updates wiki/services/nef.md
   - Adds "INC-001: Redis eviction policy caused timeout cascade"
   - Updates known failure modes table
   
3. Creates wiki/concepts/redis-eviction.md (if not exists)
   - Explains noeviction vs allkeys-lru
   - Documents why we use allkeys-lru now
   
4. Creates wiki/patterns/cascade-failure.md (if not exists)
   - Redis → NEF → QOD → Aduna cascade pattern
   - How to prevent: circuit breakers, fallback routing
   
5. Creates wiki/decisions/adr-005-eviction-policy.md
   - Why we changed from noeviction to allkeys-lru
   - What alternatives were considered
   
6. Updates wiki/carriers/airtel.md (if incident involved Airtel)
   - Notes carrier retry storm as contributing factor
   
7. Updates wiki/overview.md
   - Adds incident to platform history
   - Updates risk assessment
   
8. Updates wiki/index.md
   - Adds all new pages to catalog
   
9. Appends to wiki/log.md:
   ## [2026-04-12] ingest | INC-001 NEF Redis Timeout
   - Created: incidents/inc-001-nef-redis-timeout.md
   - Created: concepts/redis-eviction.md
   - Created: patterns/cascade-failure.md
   - Created: decisions/adr-005-eviction-policy.md
   - Updated: services/nef.md, overview.md, index.md
   - Sources touched: 8 pages

Total: 1 source → 8+ pages touched
```

**One postmortem. Eight pages updated. All cross-referenced. Forever.**

---

### Example 2: Ingest a Blog Article

```
You: "I found this article about AKS best practices. 
      Save it to raw/articles/2026-04-12-aks-best-practices.md
      and ingest it."

AI reads the article and:

1. Creates wiki/concepts/aks-node-management.md
   - Summarizes node pool sizing, autoscaling, upgrade strategies
   
2. Creates wiki/concepts/pod-disruption-budgets.md
   - Why PDBs matter, recommended configurations
   
3. Updates wiki/services/nef.md
   - If article mentions something relevant to NEF deployment
   
4. Creates wiki/patterns/pdb-missing.md
   - Anti-pattern: deploying without PDBs
   
5. Updates wiki/overview.md
   - Adds new concepts to infrastructure section
   
6. Updates wiki/index.md + wiki/log.md

You didn't write any of this. The AI extracted, organized, and cross-referenced.
```

---

### Example 3: Query the Wiki

```
You: "What's the blast radius of a Redis failure?"

AI:
  1. Reads index.md → finds relevant pages
  2. Reads: services/nef.md, overview.md, patterns/cascade-failure.md
  3. Synthesizes:

  "Redis failure affects ALL services. Here's why:

   ## Blast Radius: Redis Failure

   ### Direct Impact
   - **NEF:** Session cache unavailable → every request hits carrier APIs
   - **QOD:** Can't cache QoS grants → carrier API call for every request
   - **Aduna:** Phone→carrier mapping lost → can't optimize routing
   - **Turbo Live:** Session validation fails → streams disconnect

   ### Cascade Effect (from patterns/cascade-failure.md)
   Redis down → NEF calls carrier for every request → 
   Carrier rate limits hit (Airtel: 100 RPS) → 
   NEF requests queue → NEF timeout (30s) →
   All APIs return 504 to clients

   ### Timeline (estimated)
   - T+0: Redis fails
   - T+2s: NEF cache misses start
   - T+10s: Carrier APIs see 10x traffic
   - T+30s: Airtel rate limit hit (100 RPS exceeded)
   - T+60s: All carriers rate limiting
   - T+90s: NEF pods timing out, HPA triggering
   - T+120s: Complete platform outage

   ### Prevention (from services/nef.md)
   - Redis HA configured ✅ (zone-redundant)
   - Fallback: NEF can fall back to PostgreSQL (slower but works)
   - Alert: Redis memory >85% triggers investigation

   ### Related Pages
   - [services/nef.md](services/nef.md) — NEF architecture
   - [patterns/cascade-failure.md](patterns/cascade-failure.md) — cascade pattern
   - [incidents/inc-001-nef-redis-timeout.md](incidents/inc-001-nef-redis-timeout.md)
   - [concepts/redis-eviction.md](concepts/redis-eviction.md)"

This answer gets SAVED as wiki/comparisons/redis-blast-radius.md.
Next time someone asks → instant answer from wiki.
```

---

### Example 4: Ingest a Carrier Behavior Discovery

```
You: "Just discovered that Airtel now requires X-Request-ID header.
      They started returning 400 for requests without it.
      Save to raw/notes/2026-04-12-airtel-header-change.md and ingest."

AI:
  1. Creates wiki/carriers/airtel.md (or updates existing)
     - Adds quirk: "X-Request-ID required (since Apr 12, 2026)"
     - Updates error patterns: "400 → check for missing X-Request-ID"
     
  2. Updates wiki/services/nef.md
     - Adds note about Airtel header requirement
     - Links to airtel.md
     
  3. Updates wiki/services/aduna.md
     - Notes carrier-specific header handling
     
  4. Creates wiki/patterns/carrier-api-drift.md
     - Pattern: carriers change APIs without notice
     - Detection strategy: monitor for new 400 patterns
     
  5. Appends to log.md

One discovery → 4 pages updated, 1 new pattern documented.
```

---

### Example 5: Lint the Wiki

```
You: "Lint the wiki. Find problems."

AI scans all wiki pages and reports:

  "## Wiki Health Check — 2026-04-12

   ### Contradictions Found: 1
   ⚠️ services/nef.md says 'default carrier is jio'
      but overview.md says 'default carrier is airtel'
      → Which is correct? Please clarify.

   ### Orphan Pages: 1
   📄 services/turbo-live.md has no inbound links from other pages
      → Add references from overview.md and nef.md

   ### Missing Pages: 4
   🔗 'Redis' is mentioned in 5 pages but has no wiki/concepts/redis.md
   🔗 'AKS' is mentioned in 3 pages but has no wiki/concepts/aks.md
   🔗 'Rate limiting' is mentioned in 4 pages but has no page
   🔗 'PostgreSQL' is mentioned in 2 pages but has no page

   ### Stale Pages: 0
   ✅ All pages updated within last 24 hours

   ### Suggested Sources to Find:
   1. Azure Cache for Redis P2 documentation (would fill redis.md gap)
   2. AKS best practices guide (would fill aks.md gap)
   3. Your incident postmortems (would fill incident pages)
   4. Carrier API spec documents (would fill carrier pages)

   ### Suggested New Pages:
   1. wiki/concepts/redis.md — Redis architecture and caching strategy
   2. wiki/concepts/aks.md — AKS cluster architecture
   3. wiki/concepts/rate-limiting.md — Rate limiting across carriers
   4. wiki/concepts/postgresql.md — Database architecture
   5. wiki/comparisons/carrier-comparison.md — Side-by-side carrier analysis

   Wiki health score: 6/10 (new wiki, needs more sources)"
```

---

### Example 6: Ask for a Comparison

```
You: "Compare Airtel, Jio, and Vi for QOD API reliability."

AI reads: carriers/airtel.md, carriers/jio.md, carriers/vi.md, services/qod.md

  "## Carrier QOD Reliability Comparison

   | Metric | Airtel | Jio ⭐ | Vi |
   |--------|--------|-------|-----|
   | Uptime | 99.5% | 99.9% | 97.2% |
   | Latency (avg) | 200ms | 150ms | 300ms |
   | Peak latency | 600ms (+200%) | 225ms (+50%) | 1100ms (+260%) |
   | Error rate | 0.3% | 0.1% | 1.5% |
   | Rate limit | 100 RPS | 200 RPS | 50 RPS |
   | QoS delivery | ~80% of requested | ~85% | ~24% ⚠️ |
   | Phone format | E.164 strict | Flexible | E.164 strict |
   | Maintenance | Weekly (Tue 2-4AM) | Rare | Unannounced |
   | Response code | 200 | 200 | 201 (non-standard) |
   
   **Winner: Jio** across all metrics.
   
   **Vi warning:** QoS delivery of 24% means customers requesting
   50Mbps actually get 12Mbps. Consider routing high-tier QOD
   requests away from Vi.

   Source: wiki/carriers/airtel.md, wiki/carriers/jio.md, 
          wiki/carriers/vi.md, wiki/services/qod.md"

This gets saved as wiki/comparisons/carrier-qod-reliability.md.
Now it's permanent. Compounding.
```

---

### Example 7: Generate a Report from Wiki

```
You: "Generate a monthly operations report for March 2026 
      from the wiki."

AI reads: all incident pages, log.md entries from March, 
         patterns/, overview.md

  Generates a formatted report covering:
  - Incident summary (count, severity, MTTR)
  - Carrier performance trends
  - Infrastructure changes
  - Knowledge graph growth (new pages, sources ingested)
  - Risk assessment (current top risks from wiki)
  - Recommendations (from open questions and lint results)

  Saved as wiki/reports/2026-03-operations-report.md
```

---

### Example 8: Weekly Knowledge Review

```
You: "What did I learn this week? Summarize the wiki changes."

AI reads log.md entries from the past 7 days:

  "## Week of April 7-12, 2026 — Knowledge Summary

   ### Sources Ingested: 3
   1. INC-001 NEF Redis timeout postmortem
   2. Airtel X-Request-ID header change discovery
   3. AKS best practices article

   ### New Pages Created: 8
   - incidents/inc-001-nef-redis-timeout.md
   - concepts/redis-eviction.md
   - concepts/aks-node-management.md
   - concepts/pod-disruption-budgets.md
   - patterns/cascade-failure.md
   - patterns/pdb-missing.md
   - patterns/carrier-api-drift.md
   - decisions/adr-005-eviction-policy.md

   ### Pages Updated: 6
   - services/nef.md (3 updates)
   - services/qod.md (1 update)
   - services/aduna.md (2 updates)
   - overview.md (3 updates)
   - carriers/airtel.md (1 update)

   ### Key Insights This Week:
   1. Redis eviction policy was the root cause of our worst outage
   2. Airtel silently added a required header — no announcement
   3. We have no PDBs on any namespace (risk!)
   4. Vi QoS delivery is only 24% — consider dropping from routing

   ### Wiki Health: 7/10 (up from 6/10 last week)
   - Pages: 14 (up from 6)
   - Cross-references: 23
   - Orphan pages: 1
   - Open questions: 12 (down from 15)"
```

---

## Workflow Recipes

### Daily On-Call Workflow
```
1. Morning:  "What changed in the wiki overnight?" (read log.md)
2. Incident: Drop postmortem into raw/ → "Ingest [file]"
3. Discovery: Drop notes into raw/ → "Ingest [file]"
4. Evening:  "What open questions should I investigate?"
```

### Weekly Review Workflow
```
1. "Lint the wiki" → find problems
2. "Summarize wiki changes this week" → what you learned
3. "What sources should I find?" → gaps to fill
4. Drop 2-3 new sources → "Ingest [file]"
```

### Pre-Incident Review Workflow
```
1. "What incidents happened in [time period]?"
2. "What patterns were identified?"
3. "What's the current risk profile?" (from overview.md)
4. "Generate a report for the review meeting"
```

### Learning a New Topic
```
1. Find 3-5 articles/tutorials
2. Drop them into raw/
3. "Ingest all files in raw/articles/"
4. "Give me a learning path based on the wiki"
5. "What concepts am I missing?" (lint will find gaps)
```

---

## Tips

1. **One source at a time** — Stay involved. Review what the AI creates. Guide it.
2. **Good questions become pages** — Don't let valuable answers disappear in chat.
3. **Lint weekly** — The wiki degrades without maintenance. AI makes maintenance cheap.
4. **Date everything** — Sources, pages, log entries. Time context matters.
5. **Use Obsidian graph view** — See the shape of your knowledge. Find orphans visually.
6. **Raw sources are sacred** — Never modify them. They're your ground truth.
7. **Index.md is your friend** — It's the fastest way to find anything. Keep it updated.
8. **Log.md is your timeline** — Grep it for quick history: `grep "^## \[" log.md | tail -10`

---

## Comparison with Alternatives

| Feature | SRE Wiki | Confluence | Notion | Notebooks | RAG (ChatGPT) |
|---------|----------|------------|--------|-----------|---------------|
| AI writes it | ✅ | ❌ | ❌ | ❌ | ❌ |
| Cross-references | Auto | Manual | Manual | Manual | None |
| Compounds over time | ✅ | ❌ | ❌ | ❌ | ❌ |
| Contradiction detection | Auto | ❌ | ❌ | ❌ | ❌ |
| Offline (VS Code) | ✅ | ❌ | ❌ | ❌ | ❌ |
| Git versioned | ✅ | ❌ | ❌ | ❌ | ❌ |
| Zero dependencies | ✅ | ❌ | ❌ | ❌ | ❌ |
| Works at office | ✅ | ✅ | ✅ | ✅ | ❌ (needs API) |
| Graph visualization | Obsidian | ❌ | ❌ | ❌ | ❌ |

---

## Getting Started (5 Minutes)

```
1. Open sre-wiki/ in VS Code
2. Drop your first source into raw/ 
   (an incident postmortem, a runbook, notes from today)
3. Open Copilot Chat
4. Type: "Read SCHEMA.md, then ingest raw/[your-file].md"
5. Watch the wiki grow
6. Explore: open wiki/index.md, follow the links
7. Ask questions: "What's the biggest risk in our platform?"
```

That's it. The wiki grows from there. Every source makes it smarter.

---

*"The tedious part of maintaining a knowledge base is not the reading or the thinking — it's the bookkeeping. LLMs don't get bored, don't forget to update a cross-reference, and can touch 15 files in one pass."*
— Andrej Karpathy
