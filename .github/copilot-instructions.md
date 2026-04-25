# Copilot Workspace Instructions

## Development Checklist
- [ ] Lint: `uv run ruff check .`
- [ ] Build: `uv run uvicorn app.main:app --reload --host 0.0.0.0 --port 8000`
- [ ] Test: `uv run pytest`

## Project Overview
**Soc Ops** is an interactive Social Bingo game for in-person mixers, built with **Python (FastAPI)** + **Jinja2 templates** + **HTMX**. Players find people matching bingo questions to mark squares and get 5 in a row. State retained via signed cookies.

## Build & Test
```bash
# Install & sync dependencies
uv sync

# Run dev server (hot reload enabled, http://localhost:8000)
uv run uvicorn app.main:app --reload --host 0.0.0.0 --port 8000

# Run tests
uv run pytest

# Lint & check
uv run ruff check .
```
All commands must pass before committing.

## Architecture
```
app/
├── main.py              # FastAPI app, routes, HTMX endpoints
├── models.py            # Pydantic models (GameState, BingoSquare)
├── game_logic.py        # Board generation, bingo detection, question shuffling
├── game_service.py      # GameSession (state management), cookie serialization
├── data.py              # Question bank (shuffled per game)
├── templates/           # Jinja2 HTML templates
│   ├── base.html        # Layout wrapper
│   ├── home.html        # Landing page
│   └── components/      # Partial templates for HTMX
│       ├── start_screen.html    # Game start UI
│       ├── game_screen.html     # Active board + modal
│       ├── bingo_board.html     # 5x5 grid with squares
│       └── bingo_modal.html     # Win/loss modal
└── static/
    ├── css/app.css      # Custom utility classes
    └── js/htmx.min.js   # HTMX library
tests/
├── test_api.py          # API endpoint tests (httpx + TestClient)
└── test_game_logic.py   # Game logic unit tests (board generation, win detection)
```

## Key Patterns
### State Management
- **GameSession** stores game state server-side (questions, board, marked squares, win status)
- State serialized to signed cookies (via `itsdangerous`) to maintain state across requests

### HTMX Integration
- Routes return HTML fragments (not JSON) for HTMX to swap into the page
- **POST** endpoints trigger game actions (start, toggle, reset, dismiss modal)
- No full page reloads after initial load

### Code Conventions
- Python 3.13+ type hints required
- Snake_case for variables/functions
- No unused imports or variables (enforced by ruff)
- Tests must cover logic changes and API behavior

## Styling & Design
See [CSS Utilities](instructions/css-utilities.instructions.md) for available utility classes. The project uses a custom Tailwind-like system in `app/static/css/app.css`.

See [Frontend Design Skill](instructions/frontend-design.instructions.md) for creative component guidelines. Create distinctive, cohesive aesthetics that avoid generic AI patterns.

## General Notes
- **No Simple Browser**: Always preview in a real browser (`$BROWSER`). HTMX requires full browser features.
- **Hot reload**: Dev server automatically restarts on code changes
- **Lab guide**: This project is part of an educational lab. See [workshop/](../workshop/) for guided steps.

---

**For more:** See [CONTRIBUTING.md](../../CONTRIBUTING.md) and workshop guides in [workshop/](../workshop/GUIDE.md).
