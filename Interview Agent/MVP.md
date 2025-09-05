

# 🎯 MVP Scope

* **Core demo:** Run a single voice (or text fallback) interview → produce transcript → generate evaluation → show results to manager.
* **Excludes:** HR approval workflows, multiple proctor models, RAG question generation, avatars, advanced dashboards, Redis.

---

# 📂 MVP Epics and Stories

### Epic 1: Repository & Environment Setup

* **US-001**: As a developer, I want a GitHub repo with modular structure so we can collaborate effectively.

  * Task: Initialize repo with backend, frontend, infra folders.
  * Task: Add Dockerfile + docker-compose for local run.
  * Task: Document environment variables and setup.

---

### Epic 2: Candidate Interview Flow

* **US-010**: As a candidate, I want to join an interview via a unique link so I can authenticate easily.

  * Task: Backend endpoint to create invite token.
  * Task: Candidate UI page to input token/start.
* **US-011**: As a candidate, I want to speak my answers so the system transcribes them.

  * Task: Integrate STT API (Whisper or cloud).
  * Task: Store transcript chunks in Postgres.
* **US-012**: As a candidate, I want to hear the interviewer’s questions as natural voice.

  * Task: Integrate TTS API.
  * Task: UI playback.
* **US-013**: As a candidate, I want a fallback text input if audio fails.

  * Task: Add simple textbox fallback in UI.

---

### Epic 3: Manager Setup

* **US-020**: As a manager, I want to define a role with a set of questions so the AI can use them in an interview.

  * Task: Minimal role creation UI (title + JD text).
  * Task: Seed questions DB with predefined sets (5–10 per role).
  * Task: Backend: fetch questions by role.

---

### Epic 4: Orchestration & LLM

* **US-030**: As the system, I want to orchestrate Q\&A turns between candidate and interviewer LLM.

  * Task: Backend loop: candidate turn → store chunk → call interviewer LLM → return next question.
  * Task: Store interviewer turns in Postgres.

---

### Epic 5: Transcript & Evaluation

* **US-040**: As the system, I want to finalize an interview so the transcript is aggregated.

  * Task: Concatenate chunks → store in transcripts table.
* **US-041**: As a manager, I want an evaluation of the interview so I can assess the candidate.

  * Task: Call evaluation LLM → structured JSON with scores + summary.
  * Task: Store evaluation in `evaluation_reports`.

---

### Epic 6: Dashboard & Review

* **US-050**: As a manager, I want to view the transcript and evaluation so I can make hiring decisions.

  * Task: Manager UI: table of interviews.
  * Task: Detail view: transcript + evaluation JSON.

---

### Epic 7: Demo Preparation

* **US-060**: As the team, we want presentation slides to demo the MVP.

  * Task: Prepare slides (system overview, demo flow).
* **US-061**: As the team, we want the app containerized so it runs anywhere.

  * Task: Dockerize backend + frontend.
  * Task: Test deployment locally.

---

# 📋 Jira Board Structure

**Epics:**

* ENV Setup
* Candidate Interview Flow
* Manager Setup
* Orchestration & LLM
* Transcript & Evaluation
* Dashboard & Review
* Demo Prep

**Story points guideline:**

* Setup tasks: 1–2 points.
* API/LLM integrations: 3–5 points.
* UI screens: 2–4 points.
* End-to-end finalization: 5 points.

---

Do you want me to also generate a **Jira importable CSV/JSON** file (with epics/stories/tasks in the right format) so your team can drop it straight into Jira?
