# /project:recall [topic] - Search Memory

Search accumulated learnings for relevant context.

## Usage
- `/project:recall auth` - Find learnings about authentication
- `/project:recall` - Overview of all memory

## Process

1. Read all memory files:
   - `.claude/memory/learnings.md`
   - `.claude/memory/decisions.md`  
   - `.claude/memory/progress.md`

2. If topic provided: Find and summarize relevant entries
3. If no topic: Give overview of what's stored

## Output

Present findings organized by relevance to the query.
