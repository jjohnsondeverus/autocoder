# Accumulated Learnings

## Codebase Structure

- **Entry points**: `start.py` (CLI menu), `autonomous_agent_demo.py` (agent runner), `server/main.py` (FastAPI server)
- **Key directories**:
  - `api/` - Database models (SQLAlchemy)
  - `mcp_server/` - MCP server for feature management (FastMCP)
  - `server/` - FastAPI backend with routers and services
  - `ui/` - React 18 + TypeScript frontend
  - `.claude/` - Claude Code integration (commands, skills, templates, memory)
- **Config locations**: `requirements.txt` (Python), `ui/package.json` (React), `.claude/settings.json`
- **Database**: SQLite (`features.db` per project, `~/.autocoder/registry.db` for project registry)

## Patterns & Conventions

### Two-Agent Pattern
1. **Initializer Agent** (Session 1) - Reads app spec, creates 200+ test cases in `features.db`
2. **Coding Agent** (Sessions 2+) - Implements features one-by-one, marks as passing

### Prompt Loading Fallback Chain
1. Project-specific: `{project_dir}/prompts/{name}.md`
2. Base template: `.claude/templates/{name}.template.md`
3. Error if neither exists

### Security Model (Defense-in-Depth)
- OS-level sandbox for bash commands
- Filesystem restricted to project directory only (`./**`)
- Bash commands validated against `ALLOWED_COMMANDS` in `security.py`
- Pre-tool-use hook validates before execution

### Code Organization
- **Routers** - REST endpoints in `server/routers/` (projects, features, agent, filesystem, spec_creation, assistant_chat)
- **Services** - Business logic in `server/services/` (process_manager, assistant_chat_session, spec_chat_session)
- **Hooks** - React Query hooks in `ui/src/hooks/` (useProjects, useWebSocket, useSpecChat, useAssistantChat)
- **Components** - UI components in `ui/src/components/`

### Naming Conventions
- Python: snake_case for files, functions, variables
- TypeScript: camelCase for variables/functions, PascalCase for components/types
- Files: Descriptive names matching primary export

## Gotchas & Pitfalls

### Windows-Specific
- Console encoding fix needed for emoji output (`cp1252` → `utf-8`)
- Uses system Claude CLI to avoid Bun runtime crashes on Windows
- Drive letter handling in filesystem browser

### Database
- POSIX path format internally for cross-platform compatibility
- `in_progress` column added retroactively - migration function in `database.py`
- No schema versioning yet - single Feature model

### Circular Imports
- Lazy imports in routers to avoid circular dependency issues
- Example: `server/routers/projects.py` imports registry functions lazily

### Process Management
- `.agent.lock` file prevents multiple simultaneous agents per project
- Must clean up lock files if agent crashes
- Agent subprocess can become orphaned if server restarts on different port
- ProcessManager may report "stopped" while zombie process still exists - check with `ps aux | grep autonomous`
- Kill orphaned processes manually before restarting agent from UI

### Port Conflicts (Agents Building Agents)
- When autocoder builds a project with similar infrastructure, both compete for default ports (8888, 5173)
- Autocoder now uses high ports to avoid conflicts: `BACKEND_PORT_START = 9900`, `VITE_DEV_PORT = 9800`
- Agent-built projects can safely use standard ports 8888/5173
- Future improvement: Consider port ranges per project (hash project name to port range) or Docker isolation

### WebSocket
- Heartbeat/ping mechanism detects dead connections
- ConnectionManager handles per-project broadcasting

## Dependencies

### Python Backend
- `claude-agent-sdk>=0.1.0` - Official Claude SDK for agent patterns
- `sqlalchemy>=2.0.0` - ORM for SQLite
- `fastapi>=0.115.0` + `uvicorn` - Web framework
- `websockets>=13.0` - Real-time updates
- `psutil>=6.0.0` - Process lifecycle management

### React Frontend
- `react@18.3.1` + `typescript@5.6.2`
- `@tanstack/react-query@5.60.0` - Server state
- `tailwindcss@4.0.0-beta.4` - Styling (neobrutalism design system)
- `@radix-ui/*` - Accessible components
- `lucide-react` - Icons

## Performance Notes

### YOLO Mode (Rapid Prototyping)
- Skips regression testing (`feature_get_for_regression`)
- Disables Playwright MCP server
- Still runs lint + type checks
- Use for early prototyping, switch to standard mode for production

### Agent Buffer
- Max 10MB buffer for large Playwright screenshots
- Prevents memory issues with browser automation

### WebSocket Optimization
- Per-project connections reduce broadcast overhead
- Auto-reconnect with backoff on disconnect

## Open Questions

- No traditional unit test suite - relies on feature-based testing through the agent
- Schema versioning strategy if Feature model grows
- ~~Memory file usage patterns not yet established~~ (resolved - using /project commands)
- Better port management for "agents building agents" scenarios (Docker? port ranges?)
