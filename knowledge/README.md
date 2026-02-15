# Knowledge Base

Compound learning — reusable solutions and patterns.

---

## Purpose

**Problem**: Teams solve the same problem repeatedly.

**Solution**: Extract learnings from retros and explorations into reusable knowledge.

**Benefit**: Next time someone hits same problem, they search knowledge first (2 min) instead of debugging for 2 hours.

---

## Format

`[topic].md`

**Example**: `n1-query-pattern.md`, `circuit-breaker-pattern.md`

---

## When to Create

**After**:
- Solving a hard bug (document the fix)
- Learning a new pattern (document when/how to use)
- Making an architectural decision (link to RFC)

**Sources**:
- Retros (extract learnings)
- Explorations (document findings)
- RFCs (link to decisions)

---

## Structure

Each knowledge doc contains:

- **Problem**: What issue does this solve?
- **Solution**: How to solve it (step-by-step)
- **Why It Works**: Explanation
- **When to Use**: Applicability
- **When NOT to Use**: Limitations
- **Examples**: Code snippets (bad vs. good)
- **References**: Links to docs, RFCs, blog posts

**Template**: See examples below

---

## Example Knowledge Doc

```markdown
# N+1 Query Pattern (and How to Fix It)

**Problem**: Loading related data in a loop causes 1 query for parent + N queries for children.

**Solution**: Use eager loading (ORM) or JOIN queries (SQL).

**Why It Works**: Single database round-trip instead of N+1.

**When to Use**: Anytime loading related data (users → posts, orders → items).

**When NOT to Use**: If you only need parent data (no related entities).

**Examples**:

Bad (N+1):
\`\`\`javascript
const users = await User.findAll();
for (const user of users) {
  user.posts = await Post.findAll({ userId: user.id }); // N queries
}
\`\`\`

Good (Eager loading):
\`\`\`javascript
const users = await User.findAll({ include: [Post] }); // 1 query with JOIN
\`\`\`

**References**:
- [Sequelize Eager Loading](https://sequelize.org/docs/v6/advanced-association-concepts/eager-loading/)
- [Rails N+1 Queries](https://guides.rubyonrails.org/active_record_querying.html#eager-loading-associations)
```

---

## Searching Knowledge

**By keyword**:
```bash
grep -r "database" knowledge/
```

**By category** (if you organize into subdirectories):
```bash
ls knowledge/patterns/
ls knowledge/gotchas/
```

---

## Categories (Optional)

You can organize knowledge into subdirectories:

```
knowledge/
├── patterns/        # Reusable patterns (circuit breaker, retry logic)
├── gotchas/         # Common mistakes (forgot to close DB, async pitfalls)
├── decisions/       # Links to RFCs (why we chose X over Y)
└── tools/           # How to use specific tools (Docker, Git, etc.)
```

**Or keep flat** (simpler, easier to search).

---

## Compound Learning Loop

**Process**:
```
Encounter problem
  ↓
Solve it (via /explore or /retro)
  ↓
Extract to knowledge/[topic].md
  ↓
Next time: Search knowledge first
  ↓
Find solution in 2 min instead of 2 hours
```

**Why it matters**:
- Don't repeat same research
- Build institutional memory
- Faster problem-solving over time

---

## Maintenance

**Review quarterly**:
- Remove stale docs (outdated solutions)
- Update docs (new best practices)
- Merge duplicates

**Keep docs short**:
- 1-2 pages max
- More detail → link to external resources

---

## Integration with Retros

**After `/retro`**, analyst suggests knowledge extraction:

```
Retro complete: retros/RETRO-2026-02-15.md

Learning: N+1 query caused API timeout.

Would you like me to extract this to knowledge/n1-query-pattern.md?
```

**Accept** → Knowledge doc created → Indexed in `knowledge/README.md`

---

## Knowledge Index (This File)

**Update this file** when adding new knowledge docs:

| Topic | File | Category |
|-------|------|----------|
| N+1 Queries | `n1-query-pattern.md` | Gotchas |
| Circuit Breaker | `circuit-breaker-pattern.md` | Patterns |
| | | |

---

*Knowledge base is living. Update as you learn.*
