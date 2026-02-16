# Backlog

Work items and explorations.

---

## Structure

```
backlog/
├── issues/          # Backlog items (features, bugs, improvements)
└── explorations/    # Spikes and research (NO CODE)
```

---

## Issues

**Format**: `ISSUE-NNN-short-title.md`

**When to create**:
- New feature request
- Bug report
- Technical debt item
- Improvement idea

**Next step**: Convert to plan (`/plan backlog/issues/ISSUE-NNN.md`)

**Template**: See `templates/ISSUE_TEMPLATE.md`

---

## Explorations

**Format**: `EXP-NNN-topic.md`

**When to create**:
- Unclear requirements (need research)
- Evaluating new library/framework
- Investigating incident

**Command**: `/explore "How should we handle X?"`

**Next step**: Convert findings to backlog issue or RFC

**Template**: Managed by `/explore` command

---

## Workflow

```
User idea
  ↓
Create issue (backlog/issues/ISSUE-NNN.md)
  ↓
[Optional] /explore (backlog/explorations/EXP-NNN.md)
  ↓
/plan (creates plans/PLAN-NNN.md)
  ↓
/build → /review → Ship
```

---

*All items here are "not started" work.*
