# Decision Log

<!-- Format:
## Decision Title
**Context:** Why this came up
**Decision:** What we decided
**Rationale:** Why
-->

## Use Claude Agent SDK Instead of Raw API
**Context:** Needed a way to run long-running autonomous agent sessions
**Decision:** Use official `claude-agent-sdk` package
**Rationale:** Provides proper session management, tool handling, and streaming support. Official SDK means better maintenance and compatibility.

## SQLite Over JSON for Feature Storage
**Context:** Originally used JSON files for feature storage
**Decision:** Migrated to SQLite with SQLAlchemy ORM
**Rationale:** Better concurrent access handling, proper querying capabilities, easier migrations. JSON had race condition issues.

## MCP Server for Feature Management
**Context:** Agent needs to interact with feature database
**Decision:** Use FastMCP server (`mcp_server/feature_mcp.py`) instead of REST API
**Rationale:** More efficient integration with Claude agent, reduces HTTP overhead, native tool support.

## Project Registry in User Home Directory
**Context:** Projects can be stored anywhere on the filesystem
**Decision:** SQLite registry at `~/.autocoder/registry.db` maps project names to paths
**Rationale:** Cross-platform support, POSIX path format internally, survives project moves if re-registered.

## Defense-in-Depth Security Model
**Context:** Agent executes bash commands in user's project
**Decision:** Multi-layer security: OS sandbox + filesystem restriction + command allowlist + pre-tool validation
**Rationale:** Any single layer can fail; multiple layers provide redundancy. Fail-safe defaults (block unknown commands).

## YOLO Mode for Rapid Prototyping
**Context:** Full testing cycle (Playwright, regression) is slow for early development
**Decision:** `--yolo` flag skips browser testing, keeps lint/type-check
**Rationale:** 80/20 rule - most value from code compilation checks. Browser testing adds significant overhead. Explicit opt-in makes tradeoff clear.

## Neobrutalism Design System
**Context:** UI needed distinctive visual identity
**Decision:** Custom Tailwind CSS v4 theme with neobrutalism aesthetic
**Rationale:** Memorable, professional appearance. Defined via CSS variables in `globals.css`. Custom animations for engagement.

## WebSocket Per-Project Connections
**Context:** UI needs real-time updates for agent progress and logs
**Decision:** Dedicated WebSocket connection per active project
**Rationale:** Reduces broadcast overhead, enables per-project message filtering. ConnectionManager handles lifecycle.

## Lazy Imports in Routers
**Context:** Circular import errors between modules
**Decision:** Import dependencies inside functions rather than at module top
**Rationale:** Breaks import cycles without restructuring entire codebase. Minor performance cost acceptable.

## Two-Agent Pattern
**Context:** Building apps requires planning (what features) and execution (implement features)
**Decision:** Split into Initializer Agent (creates test suite) and Coding Agent (implements features)
**Rationale:** Clear separation of concerns. Initializer runs once per project, Coding runs multiple sessions. Different prompts optimize each phase.

## High Port Range for Autocoder (9900+)
**Context:** Port conflicts when autocoder builds projects with similar infrastructure (FastAPI + Vite)
**Decision:** Changed autocoder to use ports 9900+ (backend) and 9800 (Vite dev) instead of 8888/5173
**Rationale:** Agent-built projects default to 8888/5173. Using high ports prevents autocoder from being killed when its child agent tries to start its own servers. Simple solution that doesn't require changes to agent prompts or project specs.
