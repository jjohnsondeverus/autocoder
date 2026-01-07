# /project:learn - Capture Session Learnings

Extract and persist valuable insights from this session.

## Process

1. **Review the session** - What did we discover, build, or fix?

2. **Identify learnings** worth capturing:
   - Codebase structure insights (where things are, how they connect)
   - Patterns/conventions (how this project does X)
   - Gotchas/pitfalls (things that caused problems)
   - Performance notes (what's slow, what to avoid)
   - Dependency quirks (library-specific knowledge)
   - Architectural decisions (and their rationale)

3. **Update memory files**:
   
   **`.claude/memory/progress.md`** - Append session summary:
   ```markdown
   ## YYYY-MM-DD - Brief Title
   - What was accomplished
   - What's in progress
   - What's blocked (if anything)
   ```
   
   **`.claude/memory/learnings.md`** - Add new insights to appropriate sections:
   - Don't duplicate existing entries
   - Keep entries concise and actionable
   - Use format: `- **Thing**: Explanation`
   
   **`.claude/memory/decisions.md`** - If architectural decisions were made:
   ```markdown
   ## Decision Title
   **Context:** Why this came up
   **Decision:** What we decided  
   **Rationale:** Why this approach
   ```

4. **Quality filter** - Only add things that are:
   - Non-obvious (not something you'd figure out in 30 seconds)
   - Stable (not changing tomorrow)
   - Useful (would actually help future sessions)

## Output

After updating, summarize what was added to each file.
