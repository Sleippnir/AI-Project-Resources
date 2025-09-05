

# 🎯 MVP Scope

* **Core demo:** Run a single voice (or text fallback) interview → produce transcript → generate evaluation → show results to manager.
* **Excludes:** HR approval workflows, multiple proctor models, RAG question generation, avatars, advanced dashboards, Redis.


---

# 🗂️ MVP Backlog (with subtasks)

---

## Epic 1: Repository & Environment Setup

**US-001 — Development Environment Setup**
**As a developer**, I want a GitHub repo with modular structure **so that** the team can collaborate and containerize easily.

**Acceptance Criteria**

* Repo contains backend, frontend, infra folders.
* Dockerfile + docker-compose run end-to-end locally.
* Environment variables documented.

   **Tasks**
* Create repo & base README.
* Add backend skeleton (FastAPI/Flask).
* Add frontend skeleton (React/Vue).
* Add Dockerfile for backend + frontend.
* Add docker-compose.yml for local run.
* Document environment variables & secrets.

---

## Epic 2: Manager Setup

**US-010 — Define Role & JD**
**As a manager**, I want to define a role with JD text **so that** the interview context is aligned with the position.

**Acceptance Criteria**

* Role stored in DB with title + description.
* Status = draft (no approval needed).

  **Tasks**
* DB: `roles` table migration.
* Backend: endpoint to create role.
* Frontend: simple form for role creation.

**US-011 — Seed Question Bank**
**As a manager**, I want predefined questions per role **so that** I can use them to build interviews.

**Acceptance Criteria**

* DB contains seed sets (5–10 per role).
* Manager can select from seed bank.

   **Tasks**
* Import JSON of seed questions.
* DB: `questions` table migration.
* Backend: API to fetch questions by role.
* Frontend: list UI to select questions.

**US-012 (Optional) — Kaggle Dataset Preload**
**As a system**, I want to preload Kaggle questions/answers **so that** suggested questions appear during interview creation.

**Acceptance Criteria**

* Kaggle set imported into DB.
* Questions tagged by role & difficulty.
* Manager can browse suggested set.

   **Tasks**
* Clean Kaggle dataset.
* DB: import pipeline.
* Backend: query suggested questions.

---

## Epic 3: Persona Prompts & Templates

**US-020 — Interviewer Persona Prompt**
**As a system designer**, I want a baseline interviewer prompt **so that** the LLM behaves consistently across interviews.

**Acceptance Criteria**

* Prompt defines tone, style, follow-up rules.
* Role modifiers available.
* Stored in DB or JSON seed.

   **Tasks**
* Draft baseline interviewer persona.
* Draft role-specific modifiers.
* Seed DB with v0.

**US-021 — Evaluator Persona Prompt**
**As a system designer**, I want an evaluator prompt + rubric **so that** candidate responses can be scored consistently.

**Acceptance Criteria**

* Rubric with 1–5 scale.
* JSON schema enforced.
* Evaluator returns structured JSON.

   **Tasks**
* Draft rubric.
* Draft JSON schema.
* Test with LLM → validate stability.

**US-022 — Version Pinning**
**As a system**, I want prompt/template versions pinned **so that** interviews are reproducible.

**Acceptance Criteria**

* Interview rows log `prompt_versions_used`.
* Versions immutable during session.

   **Tasks**
* DB migration: add `prompt_versions_used`.
* Backend: populate field on interview creation.

---

## Epic 4: Candidate Interview Flow

**US-030 — Candidate Authentication**
**As a candidate**, I want to join via invite token **so that** I can securely start the interview.

**Acceptance Criteria**

* Token required.
* Expired/invalid rejected.

  **Tasks**
* DB: `invite_tokens` table.
* Backend: generate & validate token.
* Frontend: token entry page.

**US-031 — Candidate Answers by Voice**
**As a candidate**, I want to answer via microphone **so that** my spoken responses are transcribed.

**Acceptance Criteria**

* Audio → STT → text.
* Transcript chunk stored.

   **Tasks**
* Integrate STT API.
* Backend: STT pipeline.
* DB: save transcript chunk.
* Frontend: mic input.

**US-032 — Interviewer LLM Responds**
**As a candidate**, I want natural interviewer prompts **so that** interview feels real.

**Acceptance Criteria**

* LLM uses interviewer persona.
* Pulls next question from DB.

   **Tasks**
* Backend: orchestrate LLM call.
* DB: log interviewer response.

**US-033 — TTS Playback**
**As a candidate**, I want to hear interviewer questions **so that** it’s voice-based.

**Acceptance Criteria**

* LLM → TTS → audio out.

  **Tasks**
* Integrate TTS API.
* Frontend: audio playback.

**US-034 — Text Input Fallback**
**As a candidate**, I want textbox fallback **so that** I can continue if mic fails.

**Acceptance Criteria**

* Text stored like STT.

   **Tasks**
* Frontend: textbox input.
* Backend: same API as STT.

---

## Epic 5: Orchestration

**US-040 — Orchestrate Turns**
**As the system**, I want to manage turns **so that** Q\&A flows correctly.

**Acceptance Criteria**

* Candidate turn saved.
* Interviewer turn generated.

   **Tasks**
* Backend: orchestration service.
* Logging for each turn.

**US-041 — Store Interview Chunks**
**As the system**, I want to persist chunks **so that** transcript can be built.

**Acceptance Criteria**

* Chunks have round, speaker, text.

  **Tasks**
* DB: `interview_chunks` migration.
* Backend: save per turn.

**US-042 — Finalize Interview**
**As the system**, I want transcript aggregated **so that** a clean version exists.

**Acceptance Criteria**

* Transcript stored.
* Status updated = finalized.

   **Tasks**
* Backend: finalize endpoint.
* DB: `transcripts` table.

---

## Epic 6: Evaluation

**US-050 — Run Evaluator LLM**
**As the system**, I want to send transcript to evaluator **so that** structured scoring is generated.

**Acceptance Criteria**

* JSON evaluation returned.
* Stored in DB.

  **Tasks**
* Backend: evaluator call.
* DB: `evaluation_reports` table.

**US-051 — Attach Evaluation to Interview**
**As a manager**, I want transcript + evaluation together **so that** I can review easily.

**Acceptance Criteria**

* Transcript + evaluation in one view.

   **Tasks**
* Backend: fetch combined.
* Frontend: render.

---

## Epic 7: Dashboard & Review

**US-060 — List of Interviews**
**As a manager**, I want a list of past interviews **so that** I can navigate results.

**Acceptance Criteria**

* Table view with candidate name, role, date.

   **Tasks**
* Backend: API for interview list.
* Frontend: table component.

**US-061 — Transcript & Evaluation View**
**As a manager**, I want detail page **so that** I can review transcript + evaluation.

**Acceptance Criteria**

* Full transcript displayed.
* Evaluation JSON summarized.

  **Tasks**
* Backend: detail API.
* Frontend: transcript view.
* Frontend: evaluation summary.

---

## Epic 8: Demo Prep

**US-070 — Presentation Deck**
**As a team**, I want slides **so that** we can present MVP clearly.

**Acceptance Criteria**

* Includes system overview + demo script.

  **Tasks**
* Draft slides.
* Review as team.

**US-071 — Containerization**
**As a team**, I want Dockerized services **so that** the app runs on any machine.

**Acceptance Criteria**

* Backend + frontend dockerized.

  **Tasks**
* Dockerfile backend.
* Dockerfile frontend.
* Compose integration.

**US-072 — Dry-Run Demo**
**As a team**, I want a practice run **so that** Demo Day is smooth.

**Acceptance Criteria**

* Candidate interview runs start to finish.
* Manager view accessible.

   **Tasks**
* Schedule dry run.
* Test candidate flow.
* Test manager flow.


---

# 🔑 Notes for MVP

* **Kaggle preload** is optional (nice if import is trivial, otherwise defer).
* **Redis, HR approvals, multi-proctor, RAG** are cut.
* One **single-session interview** is enough for demo.

