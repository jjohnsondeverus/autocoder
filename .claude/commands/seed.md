# /project:seed - Manually Seed Memory

Use this to tell Claude things you already know about the codebase. Faster than having Claude rediscover everything.

## Usage

Just talk naturally:

```
/project:seed

The API is in src/api/, uses Express with async error handling.
Database is Postgres with Prisma, schema is in prisma/schema.prisma.
Auth uses JWT in httpOnly cookies, refresh token rotation.
Tests are in __tests__/, run with `npm test`.
The websocket stuff is fragile - always test after changes.
We use Zod for validation, schemas in src/validators/.
```

## Your Task

Parse what the user tells you and organize it into the memory files:

### For `.claude/memory/learnings.md`:

Add entries under appropriate sections:
- **Codebase Structure** - where things are
- **Patterns & Conventions** - how things are done
- **Gotchas & Pitfalls** - what to watch out for
- **Dependencies** - key libraries and their quirks
- **Performance Notes** - what's slow or sensitive

Format each as a concise, actionable bullet:
```markdown
- **API location**: `src/api/` - Express with async error handling
- **Auth pattern**: JWT in httpOnly cookies, refresh token rotation
- **Testing gotcha**: Websocket changes require manual testing - fragile
```

### For `.claude/memory/decisions.md`:

If the user mentions *why* something is a certain way, capture it:
```markdown
## [Decision Title]
**Context:** [What problem was being solved]
**Decision:** [What was chosen]
**Rationale:** [Why, as stated by user]
```

### For `.claude/memory/progress.md`:

Add an entry:
```markdown
## [Date] - Manual Knowledge Seeding
- User provided initial context about [areas covered]
- Key insights: [brief summary]
```

## Guidelines

- Don't ask clarifying questions unless something is genuinely ambiguous
- Organize what they say, don't just dump it verbatim
- If they mention something multiple times, consolidate
- If they contradict earlier memory entries, note the update

## Output

Confirm what was added:
- X entries added to learnings
- Y decisions recorded
- Any follow-up questions (optional, keep minimal)
