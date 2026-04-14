# Universal Game Generator (UGG)

## Project Specification

---

## 1. Vision

Universal Game Generator (UGG) is an AI-powered SaaS platform that allows teachers to generate **interactive classroom games as PowerPoint presentations (.pptx)** from natural language descriptions.

The system acts as an **AI co-creator**, transforming vague ideas into structured educational experiences.

The product is **fully subject-agnostic** and supports:

* English (default)
* Mathematics
* Physics
* Science
* Any other subject

This project demonstrates how orchestrated AI agents can:

* interpret intent
* build structured schemas
* generate previews
* produce usable artifacts (PowerPoint)

---

## 2. Core Product Principle

> AI proposes → Teacher validates → System generates

This principle is **strictly enforced across the system**.

---

## 3. User Experience

### First Launch

The user runs a single command (Docker or start script).

They open:

```
http://localhost:8000
```

They immediately see:

* a simple input field: “Describe your game”
* no onboarding friction
* no technical UI

---

### What the User Can Do

* Describe a game in natural language
* See AI-generated **preview**
* Understand game mechanics visually and textually
* Edit or regenerate logic and visuals
* Approve the game
* Generate a PowerPoint file
* Download the `.pptx`

---

### Mandatory Flow (CRITICAL)

1. User input
2. AI parsing
3. Preview generation
4. User validation/editing
5. Approval gate
6. PPTX generation

---

### Approval Rule (STRICT)

```
IF preview.approved != true
→ DO NOT GENERATE PPTX
```

---

### Visual Design

* Clean, minimal UI
* No technical language
* Focus on clarity
* Teacher-first UX
* Desktop-first, responsive

---

## 4. Architecture Overview

### Single Container, Single Port

```
┌────────────────────────────────────────────┐
│ Docker Container (port 8000)              │
│                                            │
│ FastAPI Backend                            │
│ ├── /api/*        REST endpoints           │
│ ├── /api/preview  preview logic            │
│ └── /*            static frontend          │
│                                            │
│ Next.js Frontend (static export)           │
│                                            │
│ SQLite Database                            │
│                                            │
└────────────────────────────────────────────┘
```

---

### Stack

| Layer           | Technology           |
| --------------- | -------------------- |
| Frontend        | Next.js (TypeScript) |
| Backend         | FastAPI (Python)     |
| AI              | LiteLLM → OpenRouter |
| File Generation | python-pptx          |
| Database        | SQLite               |
| Deployment      | Docker               |

---

### Key Decisions

| Decision             | Rationale                     |
| -------------------- | ----------------------------- |
| Preview-first UX     | Prevents incorrect generation |
| Schema-driven system | Enables reliability           |
| SQLite               | Simple, no infra              |
| Single container     | Easy to run                   |
| Structured outputs   | Safe AI execution             |

---

## 5. Directory Structure

```
ugg/
├── frontend/
├── backend/
│   ├── services/
│   ├── models/
│   ├── schemas/
│   ├── api/
│   └── core/
├── planning/
│   ├── PLAN.md
│   └── ...
├── scripts/
├── db/
├── test/
├── Dockerfile
├── .env
└── .gitignore
```

---

## 6. AI Pipeline

### Flow

1. Parse input
2. Detect subject
3. Normalize schema
4. Generate preview
5. User refinement
6. Approval
7. Slide planning
8. PPTX generation

---

### Model

* LiteLLM via OpenRouter
* Model: `openrouter/openai/gpt-oss-120b`
* Structured output required

---

## 7. Game Schema (Core)

```json
{
  "title": "string",
  "subject": "english | math | physics | science | other",
  "topic": "string",
  "age_group": "string",
  "learning_goal": "string",
  "content": {
    "type": "vocabulary | quiz | problem_solving | conceptual",
    "items": []
  },
  "objects": [],
  "scenes": [],
  "interactions": [],
  "game_loop": {},
  "reward_system": {},
  "preview": {}
}
```

---

## 8. Subject Adaptation

### Default Behavior

* If subject not specified → **English**

---

### Logic by Subject

**English**

* vocabulary games
* word guessing

**Math**

* problem solving
* numeric tasks

**Physics / Science**

* conceptual questions
* cause-effect scenarios

---

## 9. Core Services

### `game_parser_service`

* extracts structure from prompt
* detects subject

---

### `game_validation_service`

* validates schema
* applies defaults

---

### `preview_service`

* generates:

  * description
  * interaction summary
  * visual prompt
* supports regeneration:

  * logic
  * visuals

---

### `slide_planner_service`

* converts schema → slides

---

### `pptx_generation_service`

* generates PowerPoint

---

### `storage_service`

* handles files

---

### `auth_service`

* JWT authentication

---

## 10. Preview System (CORE)

### Responsibilities

* explain game clearly
* visualize mechanics
* enable editing

---

### Preview Model

```
previews
- id
- game_id
- description
- visual_prompt
- image_url
- interaction_summary
- approved
- version
```

---

## 11. Slide Planning

```json
{
  "slides": [
    {
      "type": "intro",
      "elements": []
    }
  ]
}
```

---

### Elements

* text
* image
* button
* clickable object
* background

---

### Actions

* go_to_slide
* reveal
* show_answer
* reward_feedback

---

## 12. PowerPoint Generation

### Requirements

* opens without errors
* interactive navigation works
* editable
* classroom-ready

---

### Priority

1. navigation
2. clarity
3. usability
4. visuals

---

## 13. API Endpoints

### Auth

* POST `/api/auth/signup`
* POST `/api/auth/signin`
* GET `/api/auth/me`

---

### Game

* POST `/api/game/parse`
* POST `/api/game/preview`
* POST `/api/game/preview/regenerate`
* POST `/api/game/preview/approve`
* POST `/api/game/generate`

---

### Export

* GET `/api/export/{id}`

---

### System

* GET `/api/health`

---

## 14. Database

### users

* id
* email
* password_hash

### games

* id
* user_id
* title
* subject
* original_prompt

### game_specs

* schema_json

### previews

* description
* image_url
* approved

### files

* file_path

---

## 15. Environment Variables

```bash
OPENROUTER_API_KEY=
LLM_MOCK=false
```

---

## 16. Testing Strategy

### Unit Tests

* schema validation
* subject detection
* slide planning

---

### Integration

* prompt → preview
* preview → ppt

---

### Required Prompts

* “guess words from box”
* “solve math problems behind doors”
* “physics quiz about motion”

---

## 17. Failure Handling

System must guide user:

> “Should students answer by choosing or solving?”

---

## 18. Engineering Principles

* subject-agnostic
* preview-first
* schema-driven
* validated AI output
* separation of logic and visuals

---

## 19. Non-Goals

* real-time gameplay
* LMS integrations
* advanced simulations

---

## 20. Success Criteria

The system succeeds if a teacher can:

1. describe a game
2. see correct preview
3. edit if needed
4. approve
5. download PPT

---

## 21. Key Insight

This is not an English tool.

👉 This is a **universal educational game generation system**

---

## 22. Critical Constraints

* NO PPT generation before approval
* ALWAYS structured AI outputs
* ALWAYS preview before generation
* UX must be non-technical