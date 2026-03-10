# ExamTopics Viewer + ISTQB Quiz

FastAPI backend for scraping ExamTopics.com and displaying MCQ questions with discussions. Includes a dedicated offline ISTQB quiz page.

## Quick Start with Docker

```bash
docker-compose up -d
```

- App: **http://localhost:8001**
- ISTQB Quiz: **http://localhost:8001/istqb**

This starts both:
- **FastAPI app** on port 8001
- **Pinchtab browser** on port 9867 (used for fetching question content)

## ISTQB Quiz (Static / Netlify)

A fully static version of the quiz is available in the `netlify/` folder — no server needed.

Deploy by dragging the `netlify/` folder to [Netlify Drop](https://app.netlify.com/drop). Contains:
- `index.html` — standalone quiz page
- `questions.json` — all 330 ISTQB questions pre-exported

### Quiz Features
- Module selector (CTFL v4.0, CTAL-TA, CT-TAE, CTFL-2018, and more)
- Answer reveal only after submitting your answer
- Score tracker (correct / wrong / remaining)
- Shuffle mode, jump to question #, search
- Community discussions shown after answering
- Keyboard shortcuts: `←` `→` to navigate, `1`–`4` to answer
- Dark mode

## Workflow: Scrape a new exam

1. Select an exam from the sidebar (or add a custom one)
2. Click **Scrape Exam** — fetches all question links
3. Click **Download Content** — pre-fetches all question content via Pinchtab and stores locally in SQLite
4. All questions now load offline from cache

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/exams` | List available exams |
| POST | `/api/exams/add` | Add a custom exam |
| GET | `/api/exams/{exam}/questions` | Get question links |
| POST | `/api/exams/{exam}/scrape` | Scrape question links |
| POST | `/api/exams/{exam}/prefetch` | Pre-fetch all question content |
| GET | `/api/questions/{id}` | Get question detail |
| GET | `/api/jobs/{job_id}` | Check job status |
| GET | `/api/jobs/{job_id}/stream` | Stream job progress (SSE) |
| GET | `/api/istqb/modules` | List ISTQB modules with counts |
| GET | `/api/istqb/questions` | Get all ISTQB questions (cleaned) |

## Docker Commands

```bash
# Start
docker-compose up -d

# Rebuild after code changes
docker-compose up -d --build app

# Restart Pinchtab (if browser crashes)
docker-compose restart pinchtab

# Logs
docker-compose logs -f app

# Stop
docker-compose down
```

## Manual Setup

```bash
pip install -r requirements.txt
# Start Pinchtab on port 9867
uvicorn app.main:app --reload --port 8001
```
