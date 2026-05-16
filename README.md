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

## 4. High-Level Architecture
