# Behavioral Interview Answers (STAR Format)

> Last updated: 2026-04-12 | Add real examples as they happen

## How to Use This File
Before any interview, tell Copilot: "Generate behavioral answers from this file."
Each answer follows the STAR format: **S**ituation, **T**ask, **A**ction, **R**esult.

---

## "Tell me about a time you resolved a critical production incident"

### Answer: NEF Session Timeout Cascade
```
S: During evening peak hours, our NEF API started timing out, 
   affecting all CAMARA services (QOD, Aduna, Turbo Live).
   50K requests per minute were failing. P1 incident.

T: I was the on-call engineer. I needed to diagnose and fix 
   within SLA window to avoid $50K penalty.

A: 
  1. Checked NEF health → confirmed timeout (>30s response time)
  2. Checked Redis → discovered memory at 94% capacity
  3. Root cause: Redis eviction policy was "noeviction" — 
     when memory filled, Redis rejected writes → NEF couldn't cache → 
     every request hit carrier APIs → carrier rate limits hit → cascade
  4. Fixed: Changed eviction policy to allkeys-lru
  5. Restarted NEF pods for clean state
  6. Monitored for 30 minutes to confirm stability

R: 
  - Resolved in 8 minutes (MTTR target was 30 min)
  - Prevented SLA breach (saved $50K penalty)
  - Created runbook for future occurrences
  - Identified root cause (misconfiguration from initial deployment)
  - Proposed Redis monitoring alerts for early detection

  LESSON: Documented in knowledge graph. Same incident cannot recur.
```

---

## "Tell me about a time you improved a process"

### Answer: Knowledge Graph for Incident Management
```
S: Our team had scattered runbooks, tribal knowledge, and no 
   systematic way to diagnose incidents. New engineers took 
   months to become productive on-call.

T: Create a system that captures operational knowledge so anyone 
   can diagnose and fix issues without depending on senior engineers.

A:
  1. Built a Markdown-based knowledge graph (zero dependencies)
  2. Structured into: services, symptoms, runbooks, dependencies
  3. Integrated with GitHub Copilot (reads wiki, answers questions)
  4. Created templates for adding new knowledge
  5. Established weekly review process to keep it current

R:
  - New engineer on-call readiness: 6 months → 2 weeks
  - MTTR reduced by 60% (answers from wiki instead of asking people)
  - Adopted by 2 other teams at tier-1 carrier
  - Open-sourced on GitHub
  - Became team standard for documentation

  IMPACT: Transformed tribal knowledge into permanent, searchable asset.
```

---

## "Tell me about a time you handled disagreement with a colleague/team"

### Answer: Carrier Routing Strategy
```
S: [FILL IN WITH REAL EXAMPLE]
   Suggestion: When you have a real disagreement about technical 
   approach (e.g., which carrier to route to, whether to auto-failover),
   document it here.

T: [What was the decision to be made?]

A: [What did you do? Data-driven approach?]

R: [What was the outcome?]
```

---

## "Tell me about a time you learned something new quickly"

### Answer: PPTX Parsing Without Dependencies
```
S: I needed to analyze 2 PowerPoint presentations (62 slides total)
   for AI trend research. No python-pptx, no pip, no system packages.
   Just VS Code.

T: Extract all text content from PPTX files within the constraints.

A:
  1. Researched PPTX format → discovered it's a ZIP of XML files
  2. Wrote a shell pipeline: unzip → find XML slides → parse with grep/sed
  3. Extracted all text from 62 slides in under 30 minutes
  4. No dependencies installed, no system access needed

R:
  - Successfully analyzed both presentations
  - Documented the technique for future use
  - Lesson added to permanent knowledge base
  - Technique is reusable for any Office document

  LESSON: Constraints drive creativity. The zero-dependency solution
          was faster than installing python-pptx would have been.
```

---

## "Tell me about a time you went above and beyond"

### Answer: Building the SRE Wiki
```
S: Our team's operational knowledge was scattered across Slack, 
   Confluence, people's heads, and outdated docs. Nobody could 
   find anything during incidents.

T: No one asked me to do this. I saw the problem and decided to fix it.

A:
  1. Studied Andrej Karpathy's LLM Wiki pattern
  2. Adapted it for SRE/DevOps use case
  3. Built a zero-dependency Markdown wiki that Copilot can maintain
  4. Created structured knowledge graph for CAMARA APIs
  5. Documented carrier behaviors, runbooks, architecture decisions
  6. Open-sourced on GitHub for community benefit
  7. Built career wiki for professional development tracking

R:
  - Wiki adopted by team as primary knowledge source
  - Reduced on-call ramp-up from months to weeks
  - Open-source contributions visible on GitHub
  - Created a compounding knowledge asset that grows smarter over time

  IMPACT: Solved a systemic problem nobody was assigned to solve.
```

---

## "Why are you transitioning from DevOps to AI/ML?"

### Answer: The Convergence Pitch
```
"I'm not leaving DevOps. I'm specializing.

 The most valuable engineers in the next 5 years won't be 
 pure ML researchers or pure DevOps engineers. They'll be 
 the people who can do BOTH — build models AND deploy them 
 at scale in production.

 Most ML engineers can build a model but can't deploy it 
 on Kubernetes with proper monitoring, auto-scaling, and 
 incident response. I already know all of that.

 My CAMARA API platform handles 50K requests per minute 
 across 3 carriers with 99.9% uptime. That's infrastructure 
 expertise that takes years to build.

 Now I'm adding ML to that foundation. The combination 
 is rare and valuable.

 That's why I'm targeting MLOps and ML Platform roles — 
 they're the natural intersection of everything I know 
 and everything I'm learning."
```

---

## "What's your biggest weakness?"

### Answer: Honest + Growth
```
"I tend to build tools instead of asking for help. 

 When I see a problem, my instinct is to solve it myself — 
 build a script, create a wiki, automate the fix. This is 
 usually good, but sometimes I should involve the team 
 earlier instead of going deep alone.

 I'm working on this by:
  - Sharing my projects on GitHub (open source forces collaboration)
  - Presenting what I build to the team (instead of just using it quietly)
  - Asking for feedback earlier in the process

 The positive side: I've built several tools that the team now 
 uses daily. The negative side: I sometimes spend a weekend 
 building something I could have solved with a conversation."
```

---

## Add Your Own Answers

When a real situation happens, add it here:
```
### [Date]: [Situation Title]
S: [Situation]
T: [Task]
A: [Action]
R: [Result]
IMPACT: [Quantified if possible]
```
