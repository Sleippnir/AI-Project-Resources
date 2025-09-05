Then skip Redis.

Here’s a lean MVP scoped to **one concurrent interview** for the demo.

# Minimal architecture (no Redis)

* **Client (web)**: candidate mic → STT → backend; audio out from TTS.
* **Backend** (single process/service):

  * Stateless endpoints calling STT/TTS/LLM.
  * Stores/reads everything from **Supabase (Postgres + Storage)**.
* **Supabase**:

  * Tables: `candidates`, `interviews`, `interview_chunks`, `interview_transcripts`, `evaluation_reports`, `questions` (seed), `job_roles` (optional).
  * Storage: CVs optional (can defer).

## Flow (text-first, voice optional)

1. Manager creates **role text** + picks **seed questions** (or paste a short list).
2. Candidate opens unique link (invite token).
3. Backend starts interview:

   * Create `interviews` row (`status='in_progress'`).
   * For each question:

     * (voice) STT → text, else text input.
     * Save candidate turn to `interview_chunks`.
     * Call LLM interviewer → next prompt/question.
     * Save interviewer turn to `interview_chunks`.
4. Finalize:

   * Concatenate chunks → `interview_transcripts`.
   * Call **single** evaluator LLM → summary scores → `evaluation_reports`.
5. Manager reviews transcript + evaluation in a barebones view.

## Demo-grade API surface

* `POST /interviews/start` → `{interview_id}`
* `POST /interviews/{id}/candidate-turn` → `{text}` (or audio URL→STT inside)
* `POST /interviews/{id}/next` → returns interviewer text (and optional SSML for TTS)
* `POST /interviews/{id}/finalize` → builds transcript, runs eval
* `GET /interviews/{id}/report` → transcript + evaluation JSON

## DB simplifications

* Keep **static prompts/templates** in code for MVP (no version tables yet).
* `interview_chunks(speaker, text, round, created_at)` is enough.
* `evaluation_reports(report_json)` holds the single LLM evaluator output.

## Diagram (Mermaid)

```
flowchart TD
  A[Candidate Web] -->|audio/text| B[Backend Service]
  B -->|STT (if audio)| STT[STT API]
  B -->|LLM prompt| LLMi[Interviewer LLM]
  B -->|TTS text| TTS[TTS API]
  B <--> DB[(Supabase Postgres)]
  B -->|finalize| EV[Evaluator LLM]
  A <-- TTS audio / next question --- B
```

## What to actually build this week

* **Happy-path UI**: single page for candidate; single page to view report.
* **Endpoints** above (no websockets needed for single user).
* **STT/TTS**: pick one vendor each; stream not required (polling ok).
* **LLM**: one interviewer model; one evaluator model.
* **Seed questions**: hardcode 8–12 per role in DB or a JSON file.

## Demo script (tight)

1. Manager: select role → “Start interview.”
2. Candidate: answer 3–5 questions (voice or text).
3. Click **Finalize** → show transcript + evaluation.
4. Optional: download JSON report.

## Risks & mitigations

* **Latency**: Avoid long chains per turn (keep prompts small, cap tokens).
* **Audio edge-cases**: Provide a text input fallback on same screen.
* **LLM variability**: Pin temperature low; keep interviewer prompts deterministic.
* **Finalization failure**: Allow re-run of `finalize` idempotently.

If you want, I can generate a **checklist** (DoD) and a **Jira-ready backlog** from this scope.
