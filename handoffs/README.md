# Handoffs

Context transfers between developers.

---

## Format

`HANDOFF-YYYY-MM-DD-feature-name.md`

**Example**: `HANDOFF-2026-02-15-dark-mode-to-alex.md`

---

## When to Create

**Required** (Team/Enterprise tier):
- Context switch >3 days
- Transferring work to another dev
- Going on vacation

**Command**: `/handoff "Dark mode feature → Alex"`

**Template**: See `templates/HANDOFF_TEMPLATE.md`

---

## Structure (SBAR)

Each handoff uses **SBAR format**:

- **S**ituation: What's happening now (1-2 sentences)
- **B**ackground: How we got here (what's done, what's pending)
- **A**ssessment: Current status (on track? blocked? risks?)
- **R**ecommendation: Next steps (what to do, gotchas)

---

## Why Handoffs Matter

**Without handoff**:
- ❌ Recipient doesn't know where you left off
- ❌ Repeats same mistakes (no gotchas documented)
- ❌ Wastes time figuring out context

**With handoff**:
- ✅ Recipient knows exactly what's done
- ✅ Knows what's pending
- ✅ Knows gotchas to avoid
- ✅ Starts productively in 10 minutes

---

## Lifecycle

```
Create handoff
  ↓
Share with recipient (sync or async)
  ↓
Recipient reads + asks questions
  ↓
Recipient picks up work
  ↓
[When complete] Archive handoff
```

---

## Archiving

**When complete**: Move to `handoffs/archive/YYYY/`

**Why**: Keeps active handoffs list clean

**Example**:
```bash
mv handoffs/HANDOFF-2026-02-15-dark-mode.md handoffs/archive/2026/
```

---

## Searching Handoffs

**By recipient**:
```bash
grep -l "To: Alex" handoffs/
```

**By date**:
```bash
ls handoffs/HANDOFF-2026-02-*
```

---

## Integration with Plans

**Handoff should reference plan**:
```markdown
**Plan**: plans/PLAN-001-dark-mode.md
```

Recipient reads handoff first, then plan for details.

---

*Handoffs are point-in-time snapshots. Don't edit after creation.*
