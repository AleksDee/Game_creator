# Universal Game Generator (UGG)

An AI-powered SaaS platform that turns natural language descriptions into interactive classroom games as PowerPoint presentations.

## What It Does

Teachers describe a game in plain English. The system generates a structured preview, lets the teacher review and edit it, then produces a downloadable `.pptx` file ready for classroom use.

**Supported subjects:** English, Mathematics, Physics, Science, and more.

## How It Works

```
Describe game → AI parses → Preview generated → Teacher reviews → Approve → Download PPTX
```

> PPTX is never generated before the teacher approves the preview.

## Stack

| Layer           | Technology           |
|-----------------|----------------------|
| Frontend        | Next.js (TypeScript) |
| Backend         | FastAPI (Python)     |
| AI              | LiteLLM → OpenRouter |
| File Generation | python-pptx          |
| Database        | SQLite               |
| Deployment      | Docker               |

## Getting Started

### Prerequisites

- Docker
- An OpenRouter API key

### Setup

```bash
# Copy env file and add your API key
cp .env.example .env
# Edit .env and set OPENROUTER_API_KEY

# Start the app
docker compose up
```

Open [http://localhost:8000](http://localhost:8000) in your browser.

### Environment Variables

```bash
OPENROUTER_API_KEY=your_key_here
LLM_MOCK=false   # set to true for offline testing
```

## Project Structure

```
ugg/
├── frontend/          # Next.js app
├── backend/
│   ├── api/           # Route handlers
│   ├── services/      # Core business logic
│   ├── models/        # Database models
│   ├── schemas/       # Pydantic schemas
│   └── core/          # Config, DB setup
├── planning/          # Design docs
├── scripts/           # Utility scripts
├── db/                # SQLite database
├── test/              # Tests
├── Dockerfile
└── .env
```

## API Overview

| Method | Endpoint                         | Description              |
|--------|----------------------------------|--------------------------|
| POST   | `/api/game/parse`                | Parse game description   |
| POST   | `/api/game/preview`              | Generate preview         |
| POST   | `/api/game/preview/regenerate`   | Regenerate preview       |
| POST   | `/api/game/preview/approve`      | Approve preview          |
| POST   | `/api/game/generate`             | Generate PPTX            |
| GET    | `/api/export/{id}`               | Download PPTX            |
| GET    | `/api/health`                    | Health check             |

## Testing

```bash
# Unit tests
pytest test/unit

# Integration tests
pytest test/integration
```

## Planning Docs

See the [`planning/`](./planning/) directory for full specification and design decisions.
