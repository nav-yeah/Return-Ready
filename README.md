# Return Ready 

> *Your career didn't pause. It took a different path.*

AI-powered workforce re-entry platform for women. 
```mermaid
flowchart LR
    A[Upload Resume] --> B[ AI Analysis]
    B --> C[Skill Decay Analysis]
    B --> D[Weekly Action Plan]
    B --> E[RAG-Matched Re-entry Stories]
    B --> F[Peer Matching]

    C --> G[Ready in Under 30 Seconds]
    D --> G
    E --> G
    F --> G
```
**P.S. This project won first place at Hackfinity,2026 :)**  
**[Video Demo](https://drive.google.com/file/d/1mFyYjCvtBahEjVKPVr5R0nscKxHRZ3Fg/view?usp=sharing)**
**[Sample Resume for Testing](https://drive.google.com/file/d/1l5eFjdf34IKI-sTucVblIQHD2sqoPIxJ/view?usp=sharing)**
---

## How It Works

**1. Upload your resume** — Claude API parses your PDF and extracts skills, proficiency levels, years of experience, and last-used dates. No manual entry.

**Mirror** — Each skill is scored using an exponential decay model (score = base × e^(-λ × months)). A D3.js force-directed constellation visualizes your skill landscape — node size = current strength, opacity = decay level. Hover to inspect any skill.

**Move** — Claude analyzes your decayed profile against your target role and generates 3 personalized weekly micro-actions with resource links. A re-entry story is retrieved via RAG — your skill embedding vector is matched against a curated corpus of 15 stories using pgvector cosine similarity. Week difficulty scales with your gap duration.

**Witness** — Your skill profile is stored as a 384-dimensional fastembed vector. pgvector finds the 3 most similar returners in the database by cosine similarity. You see peers anonymously — no pressure to connect, just proof you're not alone.

---

## Tech Stack

**Backend:** FastAPI, Claude API, fastembed, Supabase + pgvector, pdfplumber  
**Frontend:** React + Vite, Tailwind, D3.js

---

## The Science

Skill decay: `score = base × e^(-λ × months_unused)`

Decay rates grounded in WEF Future of Jobs 2023 (tech λ=0.03, half-life 23 months) and Becker 1964 (leadership λ=0.008, half-life 87 months). Thresholds derived from Dreyfus Skill Acquisition Model and adjusted using McKinsey Women in the Workplace 2023 (women underestimate skills by 15-20% after long gaps).

---

## Setup

```bash
# Backend
cd returnready-ml
python -m venv venv && venv\Scripts\activate
pip install fastapi uvicorn supabase python-dotenv anthropic fastembed pdfplumber
venv\Scripts\python.exe seed_stories.py
venv\Scripts\python.exe seed_users.py
venv\Scripts\python.exe -m uvicorn main:app --reload

# Frontend
cd returnready-frontend
npm install && npm run dev
```

`.env` (backend): `SUPABASE_URL`, `SUPABASE_KEY`, `ANTHROPIC_API_KEY`  
`.env` (frontend): `VITE_API_URL=http://localhost:8000`

---

## Known Limitations

- Supabase free tier pauses after 7 days of inactivity
- Decay rates are research-informed heuristics, not empirically calibrated
- Peer matching requires seeded users — sparse on a fresh database
- RAG corpus is 15 stories — sufficient for demo, not production
- Target role is free text — inconsistent phrasing affects gap analysis

---

## References

Dreyfus & Dreyfus (1980) · Becker (1964) · WEF Future of Jobs (2023) · McKinsey Women in the Workplace (2023) · Ebbinghaus (1885)
