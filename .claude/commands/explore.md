# /project:explore - Bootstrap Memory from Existing Codebase

Use this when adding the self-learning scaffolding to an existing project. Claude will explore the codebase and populate initial learnings.

## Your Task

You're being added to a project that's already in progress. Explore it systematically and build your initial mental model.

### Step 1: Structural Discovery

Explore the project structure:
```bash
find . -type f -name "*.ts" -o -name "*.js" -o -name "*.py" -o -name "*.go" -o -name "*.rs" | grep -v node_modules | grep -v __pycache__ | head -50
```

Look at:
- Directory structure and organization
- Entry points (index.ts, main.py, etc.)
- Config files (package.json, pyproject.toml, etc.)
- README if it exists

### Step 2: Identify Key Files

Find the most important files to understand:
- Main entry point(s)
- Core business logic locations
- Database/model definitions
- API routes or handlers
- Configuration management

Read these files (or their first ~100 lines) to understand patterns.

### Step 3: Extract Patterns

As you explore, identify:
- **Naming conventions** (how are files/functions/classes named?)
- **Code organization** (where does what go?)
- **Common patterns** (error handling, validation, etc.)
- **Dependencies** (what key libraries are used and how?)
- **Testing approach** (where are tests, how do they run?)

### Step 4: Populate Memory

Update `.claude/memory/learnings.md` with your findings:

```markdown
# Accumulated Learnings

## Codebase Structure
- **Entry point**: [what you found]
- **Key directories**: [organization pattern]
- **Config location**: [where config lives]

## Patterns & Conventions
- [Pattern 1]
- [Pattern 2]
- ...

## Key Dependencies
- **[Library]**: Used for [purpose]
- ...

## Testing
- **Test location**: [where]
- **Test command**: [how to run]
- **Test patterns**: [how tests are structured]
```

Update `.claude/memory/progress.md`:
```markdown
## [Today's Date] - Initial Exploration
- Explored existing codebase structure
- Documented key patterns and conventions
- Identified important files
- Bootstrapped memory from existing project
```

### Step 5: Note What You Don't Know

Add a section to learnings.md:
```markdown
## Open Questions
- [Things that aren't clear yet]
- [Areas that need more exploration]
- [Decisions that aren't documented]
```

These become natural starting points for future `/project:learn` sessions.

## Output

After exploration, summarize:
1. What you learned about the project
2. What was added to memory files
3. Key open questions for the human to clarify

Be thorough but concise - future sessions will build on this foundation.
