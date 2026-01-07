# Session Progress Log

## 2026-01-06 - Project Initialized
Self-learning scaffolding installed.

## 2026-01-06 - Initial Codebase Exploration
- Explored full project architecture using `/project:explore`
- Documented codebase structure in `learnings.md`:
  - Entry points: `start.py`, `autonomous_agent_demo.py`, `server/main.py`
  - Key directories: `api/`, `mcp_server/`, `server/`, `ui/`, `.claude/`
- Identified patterns & conventions:
  - Two-agent pattern (Initializer → Coding)
  - Prompt fallback chain (project → template → error)
  - Defense-in-depth security model
- Documented gotchas:
  - Windows console encoding issues
  - Database migration for `in_progress` column
  - Circular import prevention with lazy imports
  - Lock file cleanup on agent crash
- Catalogued dependencies (Python + React)
- Noted performance optimizations (YOLO mode, WebSocket per-project)
- Recorded architectural decisions in `decisions.md`

## 2026-01-06 - App Spec Generation & Port Conflict Resolution
- Created `app_spec_for_rebuild.txt` - comprehensive spec for rebuilding autocoder as a learning exercise
  - Separates "core harness" (reuse from Anthropic) vs "custom wrapper" (what to build)
  - 9 milestones, 33 features covering full-stack implementation
  - Includes architecture diagrams, data flows, gotchas
- Fixed critical port conflict issue when autocoder builds similar projects:
  - Problem: Agent-built projects default to ports 8888/5173, conflicting with autocoder
  - Solution: Changed autocoder to use high ports (9900+ backend, 9800 Vite dev)
  - Updated `start_ui.py` with `BACKEND_PORT_START = 9900` and `VITE_DEV_PORT = 9800`
- Diagnosed and fixed stuck agent process:
  - Agent subprocess became orphaned when server port changed
  - ProcessManager lost track of subprocess state
  - Manual kill of zombie process (PID 1228) required before restart
