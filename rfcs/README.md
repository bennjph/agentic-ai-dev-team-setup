# RFCs (Request for Comments)

Architecture decision records for team collaboration.

---

## Format

`RFC-NNN-short-title.md`

**Example**: `RFC-001-choose-state-management.md`

---

## When to Create

**Required** (Team/Enterprise tier):
- Choosing architecture (database, framework, state management)
- API design changes
- Major refactors

**Command**: `/rfc "Proposal: Use Zustand for state management"`

**Template**: See `templates/RFC_TEMPLATE.md`

---

## Structure

Each RFC contains:
- **Status**: Draft / Accepted / Rejected / Superseded
- **Context**: Why this decision matters
- **Options**: 2-5 alternatives with pros/cons/trade-offs
- **Decision**: What we're choosing and why
- **Consequences**: What this unlocks/constrains

---

## Lifecycle

```
Draft
  ↓
Team review (48 hours minimum for Team tier)
  ↓
Decision
  ↓
Update status → Accepted / Rejected
  ↓
[If accepted] Implement (create plan)
```

---

## Status Meanings

| Status | Meaning |
|--------|---------|
| **Draft** | Needs review, not decided yet |
| **Accepted** | Approved, should be implemented |
| **Rejected** | Not approved, won't implement |
| **Superseded** | Replaced by newer RFC (link to new one) |

---

## Tier Differences

### Team Tier
- RFCs required for architectural decisions
- 48-hour review window
- Team consensus (simple majority)

### Enterprise Tier
- RFCs required + mandatory stakeholder sign-off
- Longer review period (1-2 weeks for major decisions)
- Documented approval process

---

## Searching RFCs

**By status**:
```bash
grep -l "Status: Accepted" rfcs/
```

**By keyword**:
```bash
grep -r "database" rfcs/
```

---

## Superseding an RFC

**When**: New RFC makes old one obsolete.

**How**:
1. Update old RFC status → `Superseded by RFC-NNN`
2. Link from new RFC → `Supersedes RFC-MMM`

**Example**:
```markdown
# RFC-001: Use Redux

**Status**: Superseded by RFC-042
```

```markdown
# RFC-042: Migrate to Zustand

**Status**: Accepted
**Supersedes**: RFC-001

[Rationale for change]
```

---

*RFCs are living documents during Draft. Immutable after decision.*
