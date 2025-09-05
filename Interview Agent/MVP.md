

# 🎯 MVP Scope

* **Core demo:** Run a single voice (or text fallback) interview → produce transcript → generate evaluation → show results to manager.
* **Excludes:** HR approval workflows, multiple proctor models, RAG question generation, avatars, advanced dashboards, Redis.

---

# 🗂️ MVP Backlog (User Stories)

---

## Epic 1: Repository & Environment Setup

**US-001 — Development Environment Setup**
**As a developer**, I want a GitHub repo with modular structure **so that** the team can collaborate and containerize easily.
**Acceptance Criteria**

* Repo contains backend, frontend, infra folders.
* Dockerfile + docker-compose run end-to-end locally.
* Environment variables documented.
  **Tasks**
* Initialize repo.
* Add Docker + compose.
* Write setup docs.

---

## Epic 2: Manager Setup

**US-010 — Define Role & JD**
**As a manager**, I want to define a role with JD text **so that** the interview context is aligned with the position.
**Acceptance Criteria**

* Role stored in DB with title + description.
* Status = draft (no approval needed).

**US-011 — Seed Question Bank**
**As a manager**, I want predefined questions per role **so that** I can use them to build interviews.
**Acceptance Criteria**

* DB contains seed sets (5–10 per role).
* Manager can select from seed bank.

**US-012 (Optional) — Kaggle Dataset Preload**
**As a system**, I want to preload Kaggle questions/answers **so that** suggested questions appear during interview creation.
**Acceptance Criteria**

* Kaggle set imported into DB.
* Questions tagged by role & difficulty.
* Manager can browse/suggested set.

---

## Epic 3: Persona Prompts & Templates

**US-020 — Interviewer Persona Prompt**
**As a system designer**, I want a baseline interviewer prompt **so that** the LLM behaves consistently across interviews.
**Acceptance Criteria**

* Prompt defines tone, style, follow-up rules.
* Role modifiers (ML Engineer, Data Scientist, etc.).
* Stored as DB seed or JSON file.
  **Tasks**
* Draft core persona text.
* Draft role-specific overlays.
* Seed DB with v0 baseline.

**US-021 — Evaluator Persona Prompt**
**As a system designer**, I want an evaluator prompt + rubric **so that** candidate responses can be scored consistently.
**Acceptance Criteria**

* Rubric with criteria + 1–5 scale.
* JSON schema for output.
* Evaluator reliably returns structured JSON.
  **Tasks**
* Draft rubric text.
* Draft schema.
* Validate via test calls.

**US-022 — Version Pinning**
**As a system**, I want to pin prompt/template versions **so that** interviews are reproducible.
**Acceptance Criteria**

* Interviews store prompt/template version (v0).
* Prompts/templates not mutable during session.

---

## Epic 4: Candidate Interview Flow

**US-030 — Candidate Authentication**
**As a candidate**, I want to join via invite token **so that** I can securely start the interview.
**Acceptance Criteria**

* Token required to start interview.
* Invalid/expired token rejected.

**US-031 — Candidate Answers by Voice**
**As a candidate**, I want to answer via microphone **so that** my spoken responses are transcribed.
**Acceptance Criteria**

* Audio → STT → text.
* Transcript stored chunk by chunk.

**US-032 — Interviewer LLM Responds**
**As a candidate**, I want the interviewer to ask questions naturally **so that** the interview feels realistic.
**Acceptance Criteria**

* LLM uses interviewer persona prompt.
* Next question generated or pulled from seed bank.

**US-033 — TTS Playback**
**As a candidate**, I want interviewer speech in audio **so that** the experience is voice-based.
**Acceptance Criteria**

* LLM output → TTS → audio stream.

**US-034 — Text Input Fallback**
**As a candidate**, I want a textbox fallback **so that** I can continue if mic fails.
**Acceptance Criteria**

* Candidate input accepted via text.
* Stored identically to STT output.

---

## Epic 5: Orchestration

**US-040 — Orchestrate Turns**
**As the system**, I want to manage turns **so that** candidate and interviewer exchange Q\&A seamlessly.
**Acceptance Criteria**

* Candidate turn stored in DB.
* LLM interviewer responds → next question.
* Stored in DB.

**US-041 — Store Interview Chunks**
**As the system**, I want to persist chunks **so that** the transcript is reconstructable.
**Acceptance Criteria**

* Each chunk has speaker, text, round index.
* Stored in Postgres.

**US-042 — Finalize Interview**
**As the system**, I want to aggregate chunks **so that** a clean transcript is available.
**Acceptance Criteria**

* Transcript concatenated and stored.
* Status = finalized.

---

## Epic 6: Evaluation

**US-050 — Run Evaluator LLM**
**As the system**, I want to send transcript to evaluator **so that** structured scoring is generated.
**Acceptance Criteria**

* LLM produces JSON evaluation.
* Report stored in DB.

**US-051 — Attach Evaluation to Interview**
**As a manager**, I want transcript + evaluation together **so that** I can review easily.
**Acceptance Criteria**

* Transcript + evaluation report accessible in one view.

---

## Epic 7: Dashboard & Review

**US-060 — List of Interviews**
**As a manager**, I want a list of past interviews **so that** I can navigate results.
**Acceptance Criteria**

* Table view of candidate name, role, date.

**US-061 — Transcript & Evaluation View**
**As a manager**, I want a detail page **so that** I can review transcript + evaluation.
**Acceptance Criteria**

* Full transcript displayed.
* Evaluation JSON summarized.

---

## Epic 8: Demo Prep

**US-070 — Presentation Deck**
**As a team**, I want slides **so that** we can present MVP clearly.
**Acceptance Criteria**

* Includes system overview, demo script.

**US-071 — Containerization**
**As a team**, I want Dockerized services **so that** the app runs on any machine.
**Acceptance Criteria**

* Backend + frontend dockerized.

**US-072 — Dry-Run Demo**
**As a team**, I want a practice run **so that** Demo Day is smooth.
**Acceptance Criteria**

* Candidate session runs start to finish.
* Manager view accessible.

---

# 🔑 Notes for MVP

* **Kaggle preload** is optional (nice if import is trivial, otherwise defer).
* **Redis, HR approvals, multi-proctor, RAG** are cut.
* One **single-session interview** is enough for demo.

