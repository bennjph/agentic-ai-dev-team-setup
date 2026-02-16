# Working Agreements

**Team**: `[Your team name]`
**Last Updated**: `[YYYY-MM-DD]`

---

## Purpose

Working agreements are **team-specific conventions** that extend the base workflow.

**What belongs here**:
- ✅ Code style preferences
- ✅ Testing standards
- ✅ Commit message formats
- ✅ Review turnaround expectations
- ✅ When to use which command

**What doesn't**:
- ❌ Tier configuration (see `config/TIER.md`)
- ❌ Model selection (see `config/PEER_REVIEW.md`)
- ❌ Workflow basics (see `WORKFLOW.md`)

---

## Code Standards

### Style

**Language**: `[e.g., JavaScript, Python, Go]`

**Formatter**: `[e.g., Prettier, Black, gofmt]`

**Linter**: `[e.g., ESLint, Pylint, golangci-lint]`

**Run before commit**:
```bash
# Example (JavaScript)
npm run format
npm run lint
```

**Auto-format on save**: `[YES / NO]`

---

### Naming Conventions

**Variables**: `[camelCase / snake_case / PascalCase]`

**Functions**: `[camelCase / snake_case / PascalCase]`

**Constants**: `[UPPER_CASE / camelCase]`

**Files**: `[kebab-case / camelCase / PascalCase]`

**Examples**:
```javascript
// Good
const userProfile = getUserProfile(userId);

// Bad
const user_profile = get_user_profile(userId);  // Mixed styles
```

---

### File Organization

**Structure**:
```
src/
├── components/      # UI components
├── utils/           # Helper functions
├── api/             # API clients
├── types/           # TypeScript types
└── tests/           # Tests (or colocated)
```

**Conventions**:
- One component per file
- Tests colocated (`Button.js` + `Button.test.js`) OR in `tests/`
- Index files export public API only

---

## Testing Standards

### Coverage Target

**Minimum**: `[e.g., 80%]`

**How to check**:
```bash
npm test -- --coverage
```

**Enforcement**: `[Block PR if below / Warning only / Trust team]`

---

### Test Types

**Unit tests**:
- **When**: Every function, component
- **Tools**: `[e.g., Jest, Vitest, pytest]`
- **Run**: `npm test`

**Integration tests**:
- **When**: Multi-component flows (e.g., login → dashboard)
- **Tools**: `[e.g., Testing Library, Playwright]`
- **Run**: `npm run test:integration`

**E2E tests**:
- **When**: Critical user flows (checkout, signup)
- **Tools**: `[e.g., Playwright, Cypress]`
- **Run**: `npm run test:e2e`

---

### TDD Expectation

**Requirement**: `[Mandatory / Recommended / Optional]`

**Exception cases**:
- Exploratory prototypes (`/explore` phase)
- UI polish (visual tweaks)

**Verification**: `/review` checks for tests before SHIP

---

## Commit Standards

### Format

**Style**: `[Conventional Commits / Custom / Freeform]`

**Example** (Conventional Commits):
```
feat: add dark mode toggle [PLAN-001]
fix: prevent logout on refresh [PLAN-012]
test: add edge cases for theme persistence [PLAN-001]
refactor: extract applyTheme to separate function [PLAN-001]
```

**Parts**:
- `<type>`: feat, fix, test, refactor, docs
- `<description>`: What changed (imperative mood)
- `[PLAN-NNN]`: Reference to plan (if applicable)

---

### Commit Size

**Target**: < 200 LOC per commit

**Why**: Easier to review, revert, bisect

**Enforcement**: `/review` flags large commits

---

### Branch Strategy

**Model**: `[Trunk-based / Feature branches / Gitflow]`

**Convention**:
```
main                     # Production
  ├── feature/dark-mode  # Feature branch
  ├── fix/api-timeout    # Bug fix branch
```

**Branch naming**:
- `feature/[name]` — New features
- `fix/[name]` — Bug fixes
- `refactor/[name]` — Code improvements

**Delete after merge**: `[YES / NO]`

---

## Review Standards

### Turnaround Time

**Target**: `[e.g., 24 hours, 48 hours, best effort]`

**Priority**:
- 🔴 Critical (production down) → `[e.g., 1 hour]`
- 🟡 High (blocking other work) → `[e.g., 4 hours]`
- 🟢 Normal → `[e.g., 24 hours]`

---

### What to Review

**Always**:
- ✅ Security vulnerabilities
- ✅ Performance issues
- ✅ Test coverage

**Sometimes** (case-by-case):
- 🟡 Code style (if egregious)
- 🟡 Naming (if unclear)

**Never**:
- ❌ Nitpicking formatting (use auto-formatter)
- ❌ Personal preference without rationale

---

### How to Give Feedback

**Structure**:
- 🔴 Critical: Must fix before merge
- 🟡 Important: Should fix
- 🟢 Minor: Nice-to-have

**Tone**:
- Be specific: "Extract this to a function" > "This could be better"
- Be constructive: Suggest alternatives, don't just criticize
- Be empathetic: Acknowledge good work

**Example**:
```
🔴 Critical: SQL injection risk in user search

Location: src/api/users.js:42
Problem: String interpolation allows injection
Fix: Use parameterized query

Example:
  db.query('SELECT * FROM users WHERE name = ?', [userName])
```

---

## Command Usage

### When to Use `/explore`

**Always**:
- Unclear requirements
- Researching new library/framework
- Investigating incident root cause

**Never**:
- During `/build` (context switch)
- For well-defined tasks (go straight to `/plan`)

---

### When to Use `/plan`

**Always**:
- Before `/build`
- For features >100 LOC

**Skip**:
- Trivial changes (typo fix, update README)
- Emergency hotfixes (but do `/retro` after)

---

### When to Use `/rfc`

**Required** (Team/Enterprise tier):
- Choosing architecture (database, framework, etc.)
- API design changes
- Major refactors

**Optional** (Solo tier):
- Can use ADR format in `decisions/` instead

---

### When to Use `/handoff`

**Required**:
- Context switch >3 days
- Transferring work to another dev
- Going on vacation

**Optional**:
- Quick pairing (sync communication)

---

## Deployment

### Strategy

**Model**: `[Continuous deployment / Staged rollout / Manual]`

**Environments**:
- `dev` — Development
- `staging` — Pre-production
- `production` — Live

**Feature flags**: `[Required / Optional]`

---

### Release Cadence

**Frequency**: `[Daily / Weekly / Sprint-based]`

**Process**:
1. Merge to `main`
2. CI runs tests
3. Deploy to `staging`
4. Manual QA (if needed)
5. Deploy to `production`

**Rollback plan**: `[Automated / Manual / Not defined]`

---

## Team-Specific Conventions

*Add any team-specific rules here.*

**Example**:
- All API endpoints must have OpenAPI spec
- Backend changes require database migration plan
- UI changes must include screenshot in PR

---

## Revision History

| Date | Change | Reason |
|------|--------|--------|
| YYYY-MM-DD | Initial version | Team kickoff |
| | | |

---

*This is a living document. Update as the team evolves.*
