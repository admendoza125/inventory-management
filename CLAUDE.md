# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Factory Inventory Management System - Full-stack demo app with Vue 3 frontend, Python FastAPI backend, and in-memory mock data (no database).

## Commands

```bash
# Start both servers (macOS/Linux only)
./scripts/start.sh
./scripts/stop.sh

# Backend (from server/)
cd server && uv venv && uv sync    # first time setup
uv run python main.py              # runs on :8001, docs at /docs

# Frontend (from client/)
cd client && npm install            # first time setup
npm run dev                         # runs on :3000
npm run build                       # production build to client/dist/

# Tests (from tests/)
cd tests && uv run pytest backend/ -v          # all backend tests
cd tests && uv run pytest backend/test_dashboard.py -v  # single file
cd tests && uv run pytest backend/ -k "test_name" -v    # single test
```

## Architecture

**Stack**: Vue 3 (Composition API) + vue-router + axios | FastAPI + Pydantic | JSON files as data source

**Data flow**: Vue views use `useFilters` composable for shared filter state → `client/src/api.js` builds query params → FastAPI endpoints in `server/main.py` apply in-memory filtering via `apply_filters()`/`filter_by_month()` → Pydantic validates response → Vue computed properties derive display data

**Routing**: 6 views defined in `client/src/main.js`: Dashboard(/), Inventory, Orders, Demand, Spending, Reports

**Filter system**: 4 global filters (Time Period, Warehouse, Category, Order Status) managed as singleton refs in `useFilters.js`. Not all endpoints support all filters — inventory has no time dimension, demand/backlog have no filters.

**Composables**: `useFilters.js` (global filter state), `useAuth.js` (auth state), `useI18n.js` (i18n with en/ja locales)

**Mock data**: JSON files in `server/data/` loaded once at startup by `server/mock_data.py` into module-level variables. Changes don't persist across restarts. `server/generate_data.py` can regenerate data.

## API Endpoints
- `GET /api/inventory` — warehouse, category filters
- `GET /api/orders` — warehouse, category, status, month filters
- `GET /api/dashboard/summary` — all filters
- `GET /api/demand`, `/api/backlog` — no filters
- `GET /api/spending/*` — summary, monthly, categories, transactions

## Critical Tool Usage Rules

### Subagents
- **vue-expert**: **MANDATORY** for creating or significantly modifying any `.vue` file
- **code-reviewer**: Use after writing significant code
- **Explore**: Use for codebase exploration
- **general-purpose**: For complex multi-step tasks

### Skills
- **backend-api-test**: Use when writing/modifying tests in `tests/backend/`

### MCP Tools
- **Always use GitHub MCP tools** (`mcp__github__*`) for GitHub operations (exception: local branches use `git checkout -b`)
- **Always use Playwright MCP tools** (`mcp__playwright__*`) for browser testing against `localhost:3000` (frontend) and `localhost:8001` (API)

## Code Style
- Always document non-obvious logic changes with comments

## Common Issues
1. Use unique keys in v-for (`sku`, `month`, etc.) — never array index
2. Validate dates before `.getMonth()` calls
3. Update Pydantic models in `server/main.py` when changing JSON data structure
4. Inventory filters don't support month (no time dimension)
5. Revenue goals: $800K/month single, $9.6M YTD all months
6. Backend tests use FastAPI TestClient via `conftest.py` fixture — server dir is added to sys.path there

## Design System
- Colors: Slate/gray palette (#0f172a, #64748b, #e2e8f0)
- Status colors: green/blue/yellow/red
- Charts: Custom SVG, CSS Grid for layouts
- No emojis in UI
