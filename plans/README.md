# Plans

Executable plans for features and fixes.

---

## Format

`PLAN-NNN-feature-name.md`

**Example**: `PLAN-001-dark-mode.md`

---

## When to Create

**Always**:
- Before implementing any feature >100 LOC
- Before fixing complex bugs

**Command**: `/plan backlog/issues/ISSUE-NNN.md`

---

## Structure

Each plan contains:
- **Problem statement** — What are we solving?
- **Success criteria** — How do we know it's done?
- **Tasks** — Step-by-step breakdown (< 200 LOC each)
- **Testing strategy** — How to verify
- **Dependencies** — External blockers
- **Risks** — What could go wrong

**Template**: See `templates/PLAN_TEMPLATE.md`

---

## Progress Tracking

Use emoji in plan to track progress:

- 🟩 Done
- 🟨 In Progress
- 🟥 Blocked
- ⬜ Pending

**Example**:
```markdown
### Tasks

1. Add theme context 🟩
2. Create toggle component 🟨
3. Update button styles ⬜
4. Add localStorage persistence ⬜
```

**Visual**: 1/4 tasks done (25%)

---

## Lifecycle

```
Created
  ↓
/build (implement tasks)
  ↓
All tasks 🟩
  ↓
/review (code review)
  ↓
Ship ✅
```

---

## Archiving

**When complete**: Move to `plans/archive/YYYY/` (optional)

**Why archive**: Keeps active plans list clean

**Search**: Use `grep` to find old plans if needed

---

*Plans are living documents. Update as you work.*
