# Decisions

Code reviews and architectural decision records.

---

## Structure

```
decisions/
├── code-review-YYYY-MM-DD-NNN.md     # Code reviews
└── ADR-NNN-short-title.md            # Architecture decisions (Solo tier)
```

---

## Code Reviews

**Format**: `code-review-YYYY-MM-DD-NNN.md`

**When to create**: After `/review` command

**Contains**:
- Verdict: SHIP / REVISE / BLOCK
- Findings (🔴 Critical / 🟡 Important / 🟢 Minor)
- Recommendations

**Lifecycle**:
- Create → Fix issues (if REVISE/BLOCK) → Re-review → SHIP

---

## Architecture Decision Records (ADRs)

**Format**: `ADR-NNN-short-title.md`

**When to use** (Solo tier only):
- Solo devs can use ADR format for decisions
- Team tier should use `/rfc` instead (creates `rfcs/RFC-NNN.md`)

**Contains**:
- Context (why this decision matters)
- Decision (what we're choosing)
- Consequences (what this unlocks/constrains)

**Template**: See `templates/ADR_TEMPLATE.md`

---

## Searching Decisions

**By date**:
```bash
ls decisions/code-review-2026-02-*
```

**By keyword**:
```bash
grep -r "security" decisions/
```

---

*Decisions are immutable. Don't edit after creation (append updates).*
