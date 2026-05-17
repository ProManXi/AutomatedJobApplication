# 1. System Architecture

Frontend (Next.js or jQuery or React)  
↓  
Backend API (FastAPI or WebAPI)  
↓  
Orchestration Layer  
↓  
AI Services + Browser Automation  
↓  
ATS / LinkedIn  

---

# Phase 0 — What You Actually Need To Build

---

## 1. Core System Modules

| Module                        | Purpose                                   | Priority |
|-----------------------------|-------------------------------------------|----------|
| Candidate/Profile Engine    | Stores user identity/resume/preferences   | Critical |
| Job Discovery Engine        | Finds jobs from LinkedIn/ATS              | Critical |
| Job Understanding Engine    | Parses & analyzes job descriptions        | Critical |
| Resume Tailoring Engine     | Creates ATS-optimized resumes             | Critical |
| AI Writing Engine           | Cover letters + form answers              | Critical |
| Browser Automation Engine   | Actually fills applications               | Critical |
| Form Intelligence Engine    | Understands arbitrary forms               | Critical |
| Workflow/Orchestration      | Coordinates all steps                     | Critical |
| Human Review System         | Approval/edit workflow                    | Critical |
| Tracking/Analytics Engine   | Stores application history                | Medium   |
| Anti-Detection Layer        | Prevents bans                             | Medium   |
| Observability/Logging       | Debugging + screenshots                   | Critical |

---

## 2. Product Philosophy

### AI Application Copilot (Recommended)

**Characteristics:**
- Quality-focused  
- Highly personalized  
- Semi-autonomous  
- Safer long-term  

**Examples:**
- Custom answers  
- Intelligent matching  
- Approval workflows  

---

## 3. Technology Stack Decision

You should standardize this immediately.

### Recommended Stack

---

### Frontend
- Next.js  
- TypeScript  
- Tailwind  

**Why:**
- Easy dashboard UI  
- Authentication support  
- Strong ecosystem  

---

### Backend
- FastAPI  

**Why:**
- Strong AI ecosystem in Python  
- Easy browser automation integration  
- Async support  

---

### Browser Automation
- Playwright  

---

### Database
- PostgreSQL  

**Why:**
- Strong relational structure needed  

---

### Queue / Workflows
- Temporal  

**Why:**
- Long-running workflows  
- Retry-heavy systems  
- Stateful execution  
- Failure-prone automation flows  

---

### AI Layer
- LLM APIs  
- Embeddings  
- Reranking models  

---


---

## 5. Components You Need To Build First (Very Important)

---

### Phase 1 — Candidate Intelligence System

Build this first.

**Why:** Everything depends on it.

**Features:**
- Resume parser  
- Candidate knowledge graph (skills, projects, experience)  
- Reusable answer bank  
  - “Why do you want this role?”  
  - “Describe React experience”  
  - “Visa sponsorship?”  

---

### Phase 2 — Job Discovery Engine (APIFU????)

- LinkedIn search (filters, pagination, extraction)  
- Extract job details (title, company, description)  

⚠️ **Important:**  
Do NOT automate applying yet. Only collect and parse jobs first.

---

### Phase 3 — Job Intelligence Engine

- Parse job descriptions  
- Fit scoring (how relevant a job is)  

---

### Phase 4 — Resume Tailoring Engine

**Inputs:**
- Master resume  
- Job description  

**Outputs:**
- ATS-friendly resume  
- Customized bullet points  
- Keyword optimization  

---

### Phase 5 — AI Writing Engine

Major subsystem:
- Cover letters  
- Form answers  
- Personalized responses  

---

### Phase 6 — Browser Automation Engine

- Application filling  
- Navigation automation  

---

### Phase 7 — Form Intelligence Engine

⚠️ This is the hardest and most important part.

- Dynamic form detection  
- Field mapping  
- Auto-filling logic  

---

### Phase 8 — Human Approval System

- Review before submission  
- Edit responses  
- Approval workflows  

---

### Phase 9 — Observability

- Logs  
- Screenshots  
- Debugging traces  
- System monitoring















---

## Pre-Development Analysis — Risk, Resources & Cost Assessment

Before writing any code, this section captures every external dependency, cost, legal risk, and technical blocker.


### External Tools & Resources Needed

| Resource | What It Is | Why We Need It | Free Tier? | Estimated Cost |
|----------|-----------|----------------|------------|----------------|
| OpenAI API | GPT-4o LLM | Resume parsing, cover letters, answer generation | $5 free credit | ~$8–42/month (100 apps/week) |
| Anthropic API | Claude 3.5 Sonnet | Alternate LLM provider (cheaper per token) | Limited free tier | ~$6–25/month (100 apps/week) |
| Ollama | Local LLM runtime | Zero-cost AI — runs Llama/Mistral locally | Free (open source) | $0 (needs GPU hardware) |
| MongoDB (Docker) | NoSQL document database | Flexible schema for profiles, resumes, answers | Free (local Docker) | $0 local / $57/month Atlas dedicated |
| Docker Desktop | Containerization | Local dev environment (backend + frontend + DB) | Free for personal | $0 |
| Node.js 20+ | JS runtime | Next.js frontend | Free | $0 |
| Python 3.11+ | Python runtime | FastAPI backend | Free | $0 |
| Playwright | Browser automation | Form filling in future phases (Phase 6+) | Free (MIT) | $0 |
| Residential Proxy | IP rotation service | Anti-detection for browser automation (Phase 6+) | No | $75–300/month |
| Cloud Hosting | VPS / Cloud (future) | Deploy the product for multi-user | Various free tiers | $20–100/month |
| Domain + SSL | Web domain | Product launch | Let's Encrypt = free SSL | $12/year domain |
| GitHub | Source control + CI/CD | Code hosting, Actions for CI | Free for private repos | $0 |


### Cost Analysis — Monthly Burn Rate

| Scenario | LLM Costs | Infrastructure | Proxy | Total/Month |
|----------|-----------|----------------|-------|-------------|
| Dev/Building (now) | $0 (Ollama local) | $0 (Docker local) | $0 | **$0** |
| Personal use (50 apps/week) | $4–20 (Claude) | $0 (local) | $0 (no scraping) | **$4–20** |
| Product launch (multi-user) | $50–200 | $57–100 (Atlas + hosting) | $75–300 | **$182–600** |
| Scale (1000+ users) | $500+ | $200+ | $300+ | **$1,000+** |

Key insight: During development and personal use, costs are near-zero with Ollama. Costs only scale at product launch.


### Legal Risk Analysis

| Risk Area | Severity | Details | Consequence | Mitigation |
|-----------|----------|---------|-------------|------------|
| LinkedIn scraping | 🔴 CRITICAL | LinkedIn ToS §4.1/§8.3 explicitly prohibit automated scraping, bots, and scrapers. hiQ v. LinkedIn (settled 2024) confirmed LinkedIn can block scrapers even for public data. | Account ban, cease & desist, civil lawsuit under CFAA | **Don't scrape LinkedIn.** Use official job board APIs or let users paste job URLs manually. |
| ATS platform scraping | 🟠 HIGH | Most ATS platforms (Greenhouse, Lever, Workday) have ToS prohibiting automated access. | Account termination, IP blocking | Use official ATS APIs where available (Greenhouse and Lever have public job APIs). |
| GDPR compliance | 🟠 HIGH | Storing resume PII (name, email, phone, address) requires explicit consent, encryption, right-to-erasure, 72-hour breach notification. Applies if ANY user is in the EU. | Fines up to €20M or 4% of revenue | Encrypt PII at rest (AES-256), TLS in transit, deletion endpoint, explicit user consent, auto-delete after configurable retention period. |
| CCPA compliance | 🟡 MEDIUM | California residents have right to know what data is collected, right to delete (45 days), right to opt out of data sale. | Fines up to $7,500/violation | Privacy policy, deletion API, never sell data to third parties. |
| LLM data policies | 🟡 MEDIUM | OpenAI/Anthropic may use API inputs for training unless opted out. Sending resumes with PII to LLM APIs = data sharing. | Privacy violation, user trust loss | Use API opt-out settings. Or use Ollama (fully local, zero data sharing). |
| Library licensing (PyMuPDF) | 🟡 MEDIUM | PyMuPDF uses AGPL v3 — distributing your app forces you to open-source your entire codebase. | Forced open-sourcing or license violation lawsuit | **Use pdfplumber (MIT license) instead.** |
| Automated form submission | 🟡 MEDIUM | Submitting applications via bot without disclosure could violate platform ToS. Some employers consider bot-submitted apps fraudulent. | Application rejection, employer blacklist | Position as "AI Copilot" — human reviews and approves before submission. |


### Technical Risk Analysis

| Risk Area | Severity | Details | Mitigation |
|-----------|----------|---------|------------|
| Resume parsing accuracy | 🟠 HIGH | Resumes have wildly inconsistent formats — tables, columns, graphics, custom fonts. No parser handles 100%. | Multiple strategies (LLM/traditional/hybrid) via feature flag. LLM handles edge cases best. Always store raw text for re-parsing. |
| LLM output consistency | 🟠 HIGH | LLMs return slightly different JSON structures each time. Missing fields, wrong types. | Strict Pydantic validation on LLM output. Retry on validation failure. Use structured output mode (function calling / tool use). |
| Browser automation detection | 🟠 HIGH | LinkedIn/ATS detect Playwright via WebDriver flag, TLS fingerprinting, behavior patterns, IP reputation. Accounts last ~2–4 weeks even with stealth. | Phase 6 concern. Residential proxies, human-like delays, headless=False. Accept this is fundamentally fragile. |
| Form field mapping | 🟠 HIGH | Every ATS has different form layouts (Workday, Greenhouse, Lever, iCIMS). No universal mapping. | Phase 7 concern. Per-ATS adapters + AI-based field detection as fallback. |
| Rate limiting (LLM APIs) | 🟡 MEDIUM | OpenAI: 3,500 req/min (paid). Claude: 50 req/min. Free tiers very restricted. | Exponential backoff, request queuing, batch processing. Ollama for high-volume. |
| MongoDB document size | 🟢 LOW | 16MB document limit. Resumes with embedded images could approach this. | Store files on disk, only store extracted text + parsed data in MongoDB. |


### Library Licensing Summary

| Library | Purpose | License | Safe for Commercial Use? | Action |
|---------|---------|---------|--------------------------|--------|
| ~~PyMuPDF~~ | PDF extraction | AGPL v3 | ❌ No (copyleft) | **Replace with pdfplumber (MIT)** |
| pdfplumber | PDF extraction | MIT | ✅ Yes | Use this instead |
| python-docx | DOCX extraction | MIT | ✅ Yes | Use as-is |
| FastAPI | Backend framework | MIT | ✅ Yes | Use as-is |
| Playwright | Browser automation | MIT | ✅ Yes | Use as-is |
| Motor | Async MongoDB driver | Apache 2.0 | ✅ Yes | Use as-is |
| spaCy | NLP (traditional parser) | MIT | ✅ Yes | Use as-is |
| Next.js | Frontend framework | MIT | ✅ Yes | Use as-is |
| Tailwind CSS | Styling | MIT | ✅ Yes | Use as-is |
| Pydantic | Data validation | MIT | ✅ Yes | Use as-is |


### Accounts & API Keys Needed Before Development

| Account | When Needed | Free to Start? |
|---------|-------------|----------------|
| OpenAI Platform | Phase 1b (LLM parsing) | $5 free credit |
| Anthropic Console | Phase 1b (alternate LLM) | Limited free tier |
| Ollama | Phase 1b (local LLM, recommended for dev) | Fully free |
| GitHub | Now (source control) | Yes |
| Docker Hub | Phase 1a | Yes |


### Critical Decision: LinkedIn Strategy

| Option | Pros | Cons | Recommendation |
|--------|------|------|----------------|
| A) Scrape LinkedIn directly | Most jobs are there | Illegal (ToS), accounts banned in 2–4 weeks | ❌ Avoid |
| B) Official LinkedIn API | Legal, reliable | Requires partnership approval, very limited access | ⚠️ Apply but don't depend on it |
| C) User pastes job URLs | Zero legal risk, simple | Manual effort for user | ✅ **Start with this (Phase 1–3)** |
| D) Official job board APIs | Legal, scalable (Indeed, Glassdoor, Lever, Greenhouse) | Each API has different format | ✅ **Build toward this (Phase 2)** |
| E) RSS / public job feeds | Legal, many companies publish feeds | Limited coverage | ✅ Good supplement |


### Risk Mitigation Summary

| Problem | Resolution | When to Address |
|---------|-----------|-----------------|
| PyMuPDF licensing | Use pdfplumber (MIT) instead | Phase 1b — before writing any PDF code |
| LLM costs during dev | Use Ollama locally — $0 | Phase 1a — set up Ollama in Docker Compose |
| LLM output inconsistency | Pydantic validation + retry + structured output mode | Phase 1b — build into parser from day one |
| Resume format variety | 3 parser strategies + always store raw text | Phase 1b — core architecture decision |
| LinkedIn legal risk | Don't scrape. User pastes URLs. Add official APIs later. | Phase 2 — but architecture must not assume scraping |
| GDPR / PII | Encrypt MongoDB, TLS in transit, deletion endpoint, consent | Phase 1a — bake into infrastructure |
| LLM data privacy | Ollama for sensitive parsing, API opt-out for cloud LLMs | Phase 1b — feature flag controls this |
| Browser detection (future) | Residential proxies + human-like behavior + "copilot" model | Phase 6 — not a Phase 1 concern |
| Form mapping (future) | Per-ATS adapters + AI field detection | Phase 7 — hardest part, plan for it but build later |


### Finalized Technology Stack (Post-Analysis)

| Layer | Original Choice | Updated Choice | Reason for Change |
|-------|----------------|----------------|-------------------|
| PDF extraction | PyMuPDF | **pdfplumber** | AGPL licensing risk → MIT safe |
| Database | PostgreSQL | **MongoDB** | Flexible document storage, no migrations, natural fit for JSON profiles |
| LLM (development) | OpenAI | **Ollama first** | $0 during development |
| LLM (production) | OpenAI | **Claude preferred, OpenAI fallback** | Better cost-efficiency |
| Job discovery | LinkedIn scraping | **User paste + official APIs** | Legal risk elimination |
| Philosophy | "Fully Automated" | **"AI Copilot" (human-in-the-loop)** | Legal safety + quality |


---


## Phase 1 — Candidate Intelligence System (Implementation Plan)

Build first. Everything depends on this.

Key decisions:
- No auth/login for now (single user, simplify everything)
- MongoDB for flexible document storage
- Feature flags for parsing strategy (LLM / traditional / hybrid) and LLM provider (OpenAI / Claude / Ollama)
- Resume formats: PDF + DOCX
- All code must have clear, meaningful comments
- Simple architecture: Routes → Services → MongoDB (no over-engineering)


### Phase 1a — Scaffolding & Infrastructure

Project structure:

    AutomatedJobApplication/
    ├── backend/
    │   ├── app/
    │   │   ├── api/              # Route handlers
    │   │   ├── core/             # Config, feature flags
    │   │   ├── models/           # MongoDB document schemas (Pydantic)
    │   │   ├── services/         # Business logic
    │   │   └── main.py           # FastAPI entry point
    │   ├── tests/
    │   ├── requirements.txt
    │   └── Dockerfile
    ├── frontend/
    │   ├── src/
    │   │   ├── app/              # Next.js App Router pages
    │   │   ├── components/       # Reusable UI components
    │   │   ├── lib/              # API client, utils
    │   │   └── types/            # TypeScript interfaces
    │   ├── package.json
    │   └── Dockerfile
    ├── docker-compose.yml        # FastAPI + MongoDB + Next.js
    ├── .env.example
    └── README.md

What to build:
a) FastAPI app — CORS, health check, Motor (async MongoDB driver), Pydantic settings from .env
b) Feature flags — env-var config: PARSING_STRATEGY (llm|traditional|hybrid), LLM_PROVIDER (openai|anthropic|ollama)
c) MongoDB 7 via Docker Compose — Collections: resumes, profiles, answers
d) Next.js frontend — App Router, TypeScript, Tailwind, sidebar layout, API client utility
e) Docker Compose — 3 services: backend, frontend, mongo — hot-reload for dev

Verification: docker-compose up → all healthy, GET /health → 200


### Phase 1b — Resume Parser

What to build:
a) Text extraction — pdfplumber for PDF, python-docx for DOCX. Endpoint: POST /api/resume/upload
b) 3 parsing strategies (Strategy Pattern, swapped via feature flag):
    - LLMResumeParser — sends text to LLM, gets structured JSON
    - TraditionalResumeParser — regex + keyword section detection
    - HybridResumeParser — traditional first, LLM fills gaps
c) LLM abstraction — base LLMClient with OpenAIClient, AnthropicClient, OllamaClient (swapped via feature flag)
d) CandidateProfile schema — personal info, summary, work experience, education, skills (categorized), projects, certifications
e) Storage — resumes collection (file + raw text), profiles collection (parsed data + strategy used)

Verification: Upload real PDF → structured JSON. Switch flag → different strategy parses same resume.


### Phase 1c — Profile Management

What to build:
a) GET /api/profile — get current profile
b) PUT /api/profile — update profile fields manually
c) GET /api/profile/summary — AI-generated candidate summary
d) Merge logic — new resume merges into existing profile without overwriting manual edits

Verification: Upload → profile created. Edit skill → persisted. Second resume → merged.


### Phase 1d — Answer Bank

What to build:
a) answers collection — question, category (motivation/experience/logistical/behavioral/custom), answer_text, timestamps
b) Seed ~15 common application questions
c) CRUD endpoints + POST /api/answers/generate (AI drafts answer from profile + question)

Verification: CRUD works. AI-generated answer references actual profile data.


### Phase 1e — Simple Frontend

What to build:
a) Home page (/) — drag-and-drop resume upload, parsed result display, strategy selector dropdown
b) Profile page (/profile) — all sections displayed, edit buttons, AI summary
c) Answers page (/answers) — list with category tabs, add/edit/delete, "Generate with AI" button

Verification: Upload → see profile → edit → generate AI answer → saved.


### Implementation Order

    Phase 1a (scaffolding) — FIRST
        ↓
    Phase 1b (resume parser)
        ↓
    Phase 1c (profile management)
        ↓
    Phase 1d (answer bank)
        ↓
    Phase 1e (frontend — can start in parallel once APIs exist)
