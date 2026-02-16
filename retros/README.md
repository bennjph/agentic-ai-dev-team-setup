# Retros (Retrospectives)

Postmortem learning and continuous improvement.

---

## Format

`RETRO-YYYY-MM-DD.md` or `RETRO-YYYY-MM-DD-topic.md`

**Example**: `RETRO-2026-02-15-api-timeout-incident.md`

---

## When to Create

**Always** (all tiers):
- After incidents (production outage, security breach, data loss)

**Recommended** (Team/Enterprise tier):
- After features that took >2x estimated time
- After first implementation of new pattern
- Quarterly team retros

**Command**: `/retro "Production outage: API timeout cascade"`

**Template**: See `templates/RETRO_TEMPLATE.md`

---

## Structure

Each retro contains:
- **What happened** — Chronological narrative
- **5 Whys** — Root cause analysis
- **What went well** — Positive aspects
- **What went poorly** — Problems to fix
- **Learnings** — Insights extracted
- **Action items** — What to do next

---

## 5 Whys Method

**Purpose**: Find root cause, not just symptoms.

**How**:
1. State problem
2. Ask "Why?" → Answer
3. Ask "Why [answer]?" → Answer
4. Repeat 5 times
5. Last answer = root cause

**Example**:

**Problem**: API requests timing out

1. Why? → Server response >30s
2. Why? → Database queries slow
3. Why? → N+1 query pattern
4. Why? → No eager loading in ORM
5. Why? → Team wasn't aware of N+1 problem

**Root cause**: Knowledge gap (need education)

---

## Knowledge Extraction

**After every retro**, extract learnings to `knowledge/`:

**What to extract**:
- Reusable patterns (e.g., circuit breaker, N+1 fix)
- Common mistakes (e.g., forgot to close connection)
- Decisions (reference RFCs)

**Process**:
1. Run `/retro`
2. Identify learnings
3. Extract to `knowledge/[topic].md`
4. Update `knowledge/README.md` index

---

## Action Items

**Format**:
- [ ] Action — Owner: [Name] — Due: [YYYY-MM-DD]

**Example**:
```markdown
## Action Items

- [ ] Fix: Add eager loading to user query — Owner: Alex — Due: 2026-02-20
- [ ] Prevent: Add query monitoring — Owner: Jordan — Due: 2026-02-25
- [ ] Educate: Create N+1 query doc — Owner: AI — Due: 2026-02-18
```

**Tracking**: Review action items in next retro

---

## Lifecycle

```
Incident / Feature complete
  ↓
/retro (create retro doc)
  ↓
Extract learnings → knowledge/
  ↓
Implement action items
  ↓
Archive retro
```

---

## Archiving

**When**: After action items complete

**Where**: `retros/archive/YYYY/`

**Example**:
```bash
mv retros/RETRO-2026-02-15-api-timeout.md retros/archive/2026/
```

---

## Searching Retros

**By type**:
```bash
grep -l "Type: Incident" retros/
```

**By keyword**:
```bash
grep -r "security" retros/
```

---

## Tier Differences

### Tier 1 (Solo)
- Retros optional (but recommended after incidents)
- Informal format OK

### Tier 2 (Small Team)
- Retros required for incidents + complex features
- Formal 5 Whys + action items

### Tier 3 (Enterprise)
- Retros mandatory for all incidents
- Quarterly team retros
- Action items tracked in project management tool

---

*Retros are blameless. Focus on systems, not people.*
