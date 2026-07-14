# PrepGenie

**AI-powered study scheduler** that turns PDF syllabi into optimized, personalized study plans using constraint optimization, RAG, and spaced repetition.

<video src="https://github.com/user-attachments/assets/65dff5ff-de13-4054-b0b2-eca03fa48dae" controls width="600"></video>



## What it does

Students often have a syllabus, a set of deadlines, and no realistic plan for covering everything in time. PrepGenie:

1. Parses a PDF syllabus to extract topics, weightage, and exam dates
2. Lets you **chat with your syllabus** to ask questions, grounded in the actual document (RAG)
3. Generates an optimized weekly study schedule using constraint programming, respecting your deadlines, priorities, and available time
4. Applies spaced repetition intervals so topics are revisited for better retention

## Architecture

```
PDF Syllabus
    │
    ▼
Parsing (pypdf + LLM structured extraction) ──► Topics, weightage, exam dates (JSON)
    │
    ├──► Embedding + Vector Store (FAISS/Chroma) ──► RAG Chat ("Ask about my syllabus")
    │
    └──► CP-SAT Optimizer (Google OR-Tools) ──► Weekly schedule
                │
                └──► Spaced Repetition Layer ──► Review intervals
```

<!-- Consider exporting this as an actual diagram image via draw.io or excalidraw -->

## ✅ Implemented

- PDF syllabus parsing into structured topics/deadlines
- RAG-based "chat with my syllabus" interface (LangChain)
- **CP-SAT constraint optimization engine** (Google OR-Tools) for schedule generation — fully working, not just the greedy fallback
- Spaced repetition scheduling logic
- FastAPI backend + Next.js frontend dashboard

## 🚧 In Progress

- Google Calendar sync (OAuth flow and event creation partially implemented)
- Adaptive re-optimization based on completed sessions
- Hosted deployment (currently local-only — see Setup)

## Tech Stack

- **Backend**: FastAPI
- **Frontend**: Next.js, TypeScript
- **RAG**: LangChain + Groq (Llama 3.1) / OpenAI-compatible endpoint
- **Optimization**: Google OR-Tools (CP-SAT)
- **PDF Parsing**: pypdf
- **Vector Store**: FAISS / Chroma
- **Calendar**: Google Calendar API (in progress)

## Setup

```bash
# Clone
git clone https://github.com/MLops34/PrepGenie.git
cd PrepGenie

# Backend
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env
# Add your GROQ_API_KEY to .env

uvicorn backend.api:app --reload --port 8000
```

```bash
# Frontend (in a separate terminal)
cd frontend
cp .env.local.example .env.local
npm install
npm run dev
```

Visit `http://localhost:3000`.

### Environment variables

```
GROQ_API_KEY=your_groq_key
OPENAI_BASE_URL=https://api.groq.com/openai/v1
OPENAI_SYLLABUS_MODEL=llama-3.1-70b-versatile
OPENAI_CHAT_MODEL=llama-3.1-8b-instant
OPENAI_PLANNER_CHAT_MODEL=llama-3.1-70b-versatile
```

## Roadmap

- [ ] Complete Google Calendar sync (OAuth + event push)
- [ ] Adaptive re-optimization when sessions are marked complete/missed
- [ ] Deploy hosted demo (Render/Vercel)
- [ ] Add evaluation metrics for parsing accuracy and schedule quality

## License

<!-- Add a LICENSE file (MIT is a safe default) and reference it here -->
