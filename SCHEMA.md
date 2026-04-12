# SRE Wiki Schema — Instructions for AI Agent

You are maintaining a **personal SRE/DevOps knowledge wiki** for Rohit (Manav), an Azure DevOps Engineer at tier-1 carrier.

## Your Job
- Read raw sources (articles, runbooks, incidents, docs)
- Extract knowledge into structured wiki pages
- Cross-reference everything (link related pages)
- Keep the wiki current, consistent, and useful
- NEVER modify files in `raw/` — they are immutable sources

## Wiki Structure
```
wiki/
├── index.md          ← CATALOG: every page listed with link + summary
├── log.md            ← CHANGELOG: append-only record of all changes
├── overview.md       ← SYNTHESIS: the big picture of everything
│
├── services/         ← Pages for each service (NEF, QOD, Aduna, etc.)
├── carriers/         ← Pages for each carrier (Airtel, Jio, Vi)
├── incidents/        ← Pages for each incident, linked to services/carriers
├── concepts/         ← Topics (rate limiting, caching, session management)
├── patterns/         ← Recurring patterns and anti-patterns
├── decisions/        ← Architecture decisions (ADR-style)
└── comparisons/      ← Side-by-side comparisons
```

## Page Format
Every wiki page MUST have:
```markdown
# [Title]
> Last updated: YYYY-MM-DD | Sources: N | Related: [linked pages]

[Content]

## References
- [source file or URL]
```

## Ingest Workflow
When given a new source:
1. Read the source completely
2. Discuss key takeaways with Rohit
3. Write/update summary page in `wiki/`
4. Update ALL related pages that this source touches
5. Update `index.md` with new/changed pages
6. Append entry to `log.md` with format: `## [YYYY-MM-DD] ingest | [Source Title]`
7. A single source should touch 5-15 wiki pages

## Query Workflow
When Rohit asks a question:
1. Read `index.md` first to find relevant pages
2. Read those pages
3. Synthesize an answer with citations (page links)
4. If the answer is valuable → save it as a new wiki page
5. Good answers compound — they become part of the knowledge base

## Lint Workflow
When asked to health-check:
1. Find contradictions between pages
2. Find orphan pages (no inbound links)
3. Find concepts mentioned but without their own page
4. Find stale pages (not updated in 30+ days)
5. Suggest new pages to create
6. Suggest new sources to find

## Conventions
- Use `[wiki/page-name.md](wiki/page-name.md)` for cross-references
- Tag pages with frontmatter: `tags: [nef, redis, incident]`
- Keep pages focused: one topic per page
- Update `index.md` on EVERY change — it's the navigation backbone
- Append to `log.md` on EVERY change — it's the timeline

## Domains
This wiki covers:
- CAMARA APIs: NEF Session, QOD, Aduna, Turbo Live
- Infrastructure: AKS (PROD/DR/DEV), Azure, Terraform
- Data: Redis, PostgreSQL
- CI/CD: Azure DevOps YAML pipelines
- Monitoring: Azure Monitor, KQL, alerts
- Carriers: Airtel, Jio, Vi
- On-call: incidents, runbooks, post-mortems
