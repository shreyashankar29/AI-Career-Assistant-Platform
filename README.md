# AI Career Assistant Platform

A collaborative AI-powered career guidance platform that helps students become industry-ready in AI, VLSI, and Software Engineering through personalized career guidance, learning paths, resume feedback, job matching, and an AI chat assistant.

> This project is currently under active collaborative development with ongoing feature improvements and integrations.

## Architecture

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   Frontend       │     │   Backend        │     │   PostgreSQL     │
│   (Next.js)      │────▶│   (FastAPI)      │────▶│   Database       │
│   Port 3000      │     │   Port 8000      │     │   Port 5432      │
└─────────────────┘     └─────────────────┘     └─────────────────┘
                              │
                              ▼
                        ┌─────────────┐
                        │  LLM Service │
                        │  (OpenAI /   │
                        │  Fallback)   │
                        └─────────────┘
```

### Tech Stack

| Layer      | Technology                              |
|------------|-----------------------------------------|
| Frontend   | Next.js 16, React 19, Tailwind CSS 4    |
| Backend    | Python, FastAPI, SQLAlchemy, Alembic    |
| Database   | PostgreSQL 16                           |
| Auth       | JWT (python-jose), bcrypt (passlib)     |
| AI/LLM     | OpenAI API (pluggable) + fallback       |
| DevOps     | Docker, Docker Compose                  |

## Project Structure

```
root/
├── app/                        # Next.js frontend
│   ├── components/             # Reusable UI components
│   │   ├── Navbar.tsx
│   │   ├── Sidebar.tsx
│   │   ├── JobCard.tsx
│   │   └── ChatMessage.tsx
│   ├── context/
│   │   └── AuthContext.tsx      # Auth state management
│   ├── lib/
│   │   └── api.ts              # API client
│   ├── login/page.tsx
│   ├── signup/page.tsx
│   ├── dashboard/page.tsx
│   ├── profile/page.tsx
│   ├── upload-resume/page.tsx
│   ├── jobs/page.tsx
│   ├── learning-path/page.tsx
│   ├── chat/page.tsx
│   ├── layout.tsx
│   └── page.tsx                # Landing page
│
├── backend/                    # FastAPI backend
│   ├── models/                 # SQLAlchemy ORM models
│   │   ├── user.py
│   │   ├── profile.py
│   │   ├── resume.py
│   │   ├── job.py
│   │   ├── learning_path.py
│   │   └── chat_history.py
│   ├── schemas/                # Pydantic request/response schemas
│   ├── routers/                # API route handlers
│   │   ├── auth.py
│   │   ├── profile.py
│   │   ├── resume.py
│   │   ├── jobs.py
│   │   ├── learning_path.py
│   │   └── chat.py
│   ├── services/               # Business logic layer
│   │   ├── auth_service.py
│   │   ├── profile_service.py
│   │   ├── resume_service.py
│   │   ├── job_service.py
│   │   ├── learning_path_service.py
│   │   ├── chat_service.py
│   │   └── llm_service.py     # AI abstraction layer
│   ├── migrations/             # Alembic migrations
│   ├── main.py                 # FastAPI app entry point
│   ├── config.py               # Environment configuration
│   ├── database.py             # DB connection
│   ├── auth.py                 # Auth dependencies
│   ├── seed.py                 # Database seed script
│   └── Dockerfile
│
├── docker-compose.yml
├── Dockerfile                  # Frontend Dockerfile
├── Makefile
├── .env.example
└── package.json
```
## Contribution Summary

This repository is a forked collaborative project developed under mentor guidance.  
My contributions focused on frontend structure, backend organization, API workflow setup, and authentication implementation.

### My Contributions
- Structured the initial frontend and backend project architecture
- Built and organized frontend pages, reusable components, and application flow
- Worked on frontend interaction workflows with FastAPI API endpoint
- Implemented JWT-based authentication workflows including login, registration, token handling, and protected route logic
- Assisted in backend route organization and API interaction setup
- Contributed to overall project workflow organization and development collaboration

### Technologies Worked With
- Next.js / React
- FastAPI
- JWT Authentication
- REST APIs
- PostgreSQL
- Docker

### Collaborative Development
Frontend-backend integration, additional refinements, and UI enhancements were later expanded collaboratively during subsequent development stages.

## Quick Start

### Prerequisites

- [Docker](https://docs.docker.com/get-docker/) and Docker Compose
- (Optional) [Make](https://www.gnu.org/software/make/)

### 1. Clone and configure

```bash
git clone <repo-url>
cd AI-Career-Assistant-Platform
cp .env.example .env
```

### 2. Start all services

```bash
# Using Make
make dev

# Or directly with Docker Compose
docker compose up --build
```

### 3. Seed the database

In a separate terminal:

```bash
make seed

# Or:
docker compose exec backend python seed.py
```

### 4. Access the application

| Service   | URL                         |
|-----------|-----------------------------|
| Frontend  | http://localhost:3000        |
| Backend   | http://localhost:8000        |
| API Docs  | http://localhost:8000/docs   |

### 5. Test the full flow

1. **Sign up** at http://localhost:3000/signup (or use seeded credentials below)
2. **Upload a resume** (PDF or DOCX)
3. **View job recommendations** matched to your skills
4. **Chat with the AI assistant** for career guidance
5. **Track learning paths** based on your skill profile

### Seeded Test Credentials

| Email              | Password      |
|--------------------|---------------|
| alice@example.com  | password123   |
| bob@example.com    | password123   |
| carol@example.com  | password123   |

## API Endpoints

| Method | Endpoint                | Auth     | Description                     |
|--------|-------------------------|----------|---------------------------------|
| POST   | `/auth/signup`          | No       | Register new user               |
| POST   | `/auth/login`           | No       | Login, returns JWT token        |
| GET    | `/profile`              | Bearer   | Get current user info           |
| GET    | `/profile/details`      | Bearer   | Get full profile details        |
| PUT    | `/profile/details`      | Bearer   | Update profile                  |
| POST   | `/resume/upload`        | Bearer   | Upload resume (PDF/DOCX)        |
| GET    | `/resume`               | Bearer   | List user's resumes             |
| GET    | `/jobs`                 | No       | List all jobs                   |
| GET    | `/jobs/matched`         | Bearer   | Get jobs matched to user skills |
| GET    | `/learning-path`        | Bearer   | Get user's learning paths       |
| POST   | `/learning-path/generate` | Bearer | Regenerate learning paths       |
| PUT    | `/learning-path/{id}`   | Bearer   | Update progress                 |
| POST   | `/chat`                 | Bearer   | Send message to AI assistant    |
| GET    | `/chat/history`         | Bearer   | Get chat history                |

## Development Commands

```bash
make dev          # Start all services with hot reload
make build        # Build containers
make start        # Start in background
make stop         # Stop all services
make seed         # Run seed script
make migrate      # Apply database migrations
make logs         # View all logs
make logs-backend # View backend logs only
make clean        # Remove containers, volumes, images
make reset        # Full reset: rebuild + re-seed
```

## Database Tables

| Table           | Description                         |
|-----------------|-------------------------------------|
| `users`         | Registered users with auth info     |
| `profiles`      | User profiles, skills, interests    |
| `resumes`       | Uploaded resume metadata & skills   |
| `jobs`          | Job listings with required skills   |
| `learning_paths`| Personalized learning paths         |
| `chat_history`  | Conversation history with AI        |

## AI / LLM Integration

The platform includes an abstraction layer at `backend/services/llm_service.py` that supports:

- **OpenAI API** — set `OPENAI_API_KEY` in `.env`
- **OpenRouter** — change the base URL in `llm_service.py`
- **Local models** — add a provider for Ollama / llama.cpp
- **RAG pipeline** — extend with document retrieval for context-aware responses

Without an API key, the app uses a built-in rule-based fallback that provides career guidance responses for resume tips, interview prep, AI/ML, VLSI, and job search topics.

## Environment Variables

| Variable             | Description                        | Default                        |
|----------------------|------------------------------------|--------------------------------|
| `DATABASE_URL`       | PostgreSQL connection string       | `postgresql://postgres:postgres@localhost:5432/career_assistant` |
| `JWT_SECRET`         | Secret key for JWT tokens          | `dev-secret-key-change-in-production` |
| `NEXT_PUBLIC_API_URL`| Backend API URL for frontend       | `http://localhost:8000`        |
| `OPENAI_API_KEY`     | OpenAI API key (optional)          | *(empty — uses fallback)*      |
