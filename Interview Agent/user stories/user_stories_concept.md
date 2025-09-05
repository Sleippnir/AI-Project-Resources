# User Stories derived from Data Lineage CSV

## Manager & Setup Flow
### US-001 — Job Role Definition – Manager
**As a** Manager, **I want** Job Role Definition, **so that** Data object appears in the HR compliance queue..
**Acceptance Criteria**
- [1] Given manager enters job title & description into dashboard form., when the action is performed, then the system processes it as: Backend validates input and sets status to 'PENDING_REVIEW'..
- [2] And it is stored as: PostgreSQL: Stores definition text and status..
- [3] And the outcome is: Data object appears in the HR compliance queue..

### US-002 — Suggested Questions – Manager
**As a** Manager, **I want** Suggested Questions, **so that** Frontend: Displayed to Manager as a starting point..
**Acceptance Criteria**
- [1] Given manager selects an approved jd to build an interview., when the action is performed, then the system processes it as: Backend takes JD embedding and queries pgvector for top N similar approved questions..
- [2] And it is stored as: Ephemeral: List of question objects held in memory..
- [3] And the outcome is: Frontend: Displayed to Manager as a starting point..

### US-003 — Custom Interview Question – Manager
**As a** Manager, **I want** Custom Interview Question, **so that** Appears in HR queue for approval..
**Acceptance Criteria**
- [1] Given manager writes a new question for a specific role., when the action is performed, then the system processes it as: Backend validates and adds to the question bank with status 'PENDING_REVIEW'..
- [2] And it is stored as: PostgreSQL: Stores question text..
- [3] And the outcome is: Appears in HR queue for approval..

### US-004 — RAG-Generated Questions – Manager
**As a** Manager, **I want** RAG-Generated Questions, **so that** Frontend: Displayed to Manager for approval/rejection. Approved questions go to HR queue..
**Acceptance Criteria**
- [1] Given manager requests ai-generated questions for an approved role., when the action is performed, then the system processes it as: Backend triggers RAG System, which uses role definition and existing questions to generate new ones..
- [2] And it is stored as: Ephemeral: Held in memory for manager review..
- [3] And the outcome is: Frontend: Displayed to Manager for approval/rejection. Approved questions go to HR queue..

## HR & Compliance Flow
### US-005 — JD Approval Decision – HR
**As a** HR/Compliance Officer, **I want** JD Approval Decision, **so that** Backend Job: Triggers embedding generation for the approved role..
**Acceptance Criteria**
- [1] Given hr reviews a pending job role definition and clicks 'approve' or 'reject'., when the action is performed, then the system processes it as: Backend updates the role's status and logs the action for audit trail..
- [2] And it is stored as: PostgreSQL: Role status is updated to 'APPROVED'..
- [3] And the outcome is: Backend Job: Triggers embedding generation for the approved role..

### US-006 — Question Approval Decision – HR
**As a** HR/Compliance Officer, **I want** Question Approval Decision, **so that** Backend Job: Triggers embedding generation for the approved question..
**Acceptance Criteria**
- [1] Given hr reviews a pending question and clicks 'approve' or 'reject'., when the action is performed, then the system processes it as: Backend updates the question's status and logs the action..
- [2] And it is stored as: PostgreSQL: Question status is updated to 'APPROVED'..
- [3] And the outcome is: Backend Job: Triggers embedding generation for the approved question..

### US-007 — HR-Sourced Question – HR
**As a** HR/Compliance Officer, **I want** HR-Sourced Question, **so that** Backend Job: Triggers embedding generation..
**Acceptance Criteria**
- [1] Given hr enters a new, pre-approved question directly into the system., when the action is performed, then the system processes it as: Backend validates and adds to question bank with status 'APPROVED'..
- [2] And it is stored as: PostgreSQL: Stores question text..
- [3] And the outcome is: Backend Job: Triggers embedding generation..

### US-008 — Candidate CV – HR
**As a** HR/Compliance Officer, **I want** Candidate CV, **so that** Manager/HR view the original file. Backend can query the JSON for filtering/matching..
**Acceptance Criteria**
- [1] Given hr attaches a candidate's cv (pdf/docx) to a finalized jd., when the action is performed, then the system processes it as: 1. Backend uploads the raw file. 2. A Supabase Edge Function triggers on upload, parses the file, and extracts searchable text/JSON..
- [2] And it is stored as: Supabase Storage: Stores the original file. Supabase PostgreSQL: Stores the file path and the extracted, searchable JSON content..
- [3] And the outcome is: Manager/HR view the original file. Backend can query the JSON for filtering/matching..

### US-009 — Invite Token – HR
**As a** HR/Compliance Officer, **I want** Invite Token, **so that** Candidate: Receives email with unique interview link..
**Acceptance Criteria**
- [1] Given hr finalizes the candidate-jd link and clicks "send invite"., when the action is performed, then the system processes it as: Backend generates a unique token and triggers an email service..
- [2] And it is stored as: PostgreSQL: Stores token, its status ('PENDING'), and associated interview ID..
- [3] And the outcome is: Candidate: Receives email with unique interview link..

## System & AI Flow
### US-010 — Initial Interview Context – System
**As a** Interview Orchestrator (system), **I want** Initial Interview Context, **so that** Used by the Backend to initiate the first question round..
**Acceptance Criteria**
- [1] Given system action, triggered when a candidate starts an interview., when the action is performed, then the system processes it as: Backend retrieves all related data (JD, CV JSON, ordered questions, etc.) from PostgreSQL..
- [2] And it is stored as: Redis: Full context is loaded into the cache for the session..
- [3] And the outcome is: Used by the Backend to initiate the first question round..

### US-011 — Live Round Context Prompt – System
**As a** Interview Orchestrator (system), **I want** Live Round Context Prompt, **so that** Interviewer LLM API: Sent to generate the next conversational turn..
**Acceptance Criteria**
- [1] Given system action, for every turn within a question round., when the action is performed, then the system processes it as: Backend assembles prompt: Persona + JD/CV sections + current question + current round's chat history..
- [2] And it is stored as: Read from Redis Cache..
- [3] And the outcome is: Interviewer LLM API: Sent to generate the next conversational turn..

### US-012 — Completed Question Chunk – System
**As a** Interview Orchestrator (system), **I want** Completed Question Chunk, **so that** Used by the Backend to initiate the next question round..
**Acceptance Criteria**
- [1] Given interviewer llm returns a response with { "round": "complete" }., when the action is performed, then the system processes it as: Backend takes the conversation history for that round from Redis and appends it to the main transcript. Then clears the conversational context..
- [2] And it is stored as: PostgreSQL: Appended to the growing interview transcript record..
- [3] And the outcome is: Used by the Backend to initiate the next question round..

### US-013 — Finalized Transcript & Token Invalidation – System
**As a** Interview Orchestrator (system), **I want** Finalized Transcript & Token Invalidation, **so that** Triggers the asynchronous Proctor Evaluation step..
**Acceptance Criteria**
- [1] Given interviewer llm returns { "round": "final" } after the candidate ends the interaction., when the action is performed, then the system processes it as: Backend appends the final chunk, retrieves the full transcript from PostgreSQL, and updates the invite token's status to 'USED'..
- [2] And it is stored as: PostgreSQL: The interview record is marked complete and the token is invalidated..
- [3] And the outcome is: Triggers the asynchronous Proctor Evaluation step..

### US-014 — Proctor Evaluation Prompt – System
**As a** Interview Orchestrator (system), **I want** Proctor Evaluation Prompt, **so that** 3× Proctor LLM APIs: Sent for parallel, independent evaluation..
**Acceptance Criteria**
- [1] Given system action, triggered after transcript finalization., when the action is performed, then the system processes it as: Backend creates a prompt: Proctor Persona + JD + CV + Full Transcript + Evaluation Rubric..
- [2] And it is stored as: Read from PostgreSQL..
- [3] And the outcome is: 3× Proctor LLM APIs: Sent for parallel, independent evaluation..

### US-015 — Structured Proctor Response – System
**As a** Interview Orchestrator (system), **I want** Structured Proctor Response, **so that** Input for the Backend Aggregation Service..
**Acceptance Criteria**
- [1] Given output from each individual proctor llm api., when the action is performed, then the system processes it as: Backend receives three independent JSON objects with scores and reasoning..
- [2] And it is stored as: Ephemeral/In‑Memory while awaiting aggregation..
- [3] And the outcome is: Input for the Backend Aggregation Service..

## Candidate Flow
### US-016 — Candidate Preliminary Info – Candidate Flow
**As a** Candidate, **I want** Candidate Preliminary Info, **so that** Used to label the final report and for communication..
**Acceptance Criteria**
- [1] Given candidate enters name/email after following invite link., when the action is performed, then the system processes it as: Backend validates input and associates with the interview record..
- [2] And it is stored as: PostgreSQL: Stored in candidate/interview record..
- [3] And the outcome is: Used to label the final report and for communication..

### US-017 — Session JWT – Candidate Flow
**As a** Candidate, **I want** Session JWT, **so that** Backend API Gateway: Used to authorize subsequent requests for this session..
**Acceptance Criteria**
- [1] Given candidate submits preliminary info., when the action is performed, then the system processes it as: Backend validates the invite token and issues a short‑lived JSON Web Token..
- [2] And it is stored as: Ephemeral: Stored in browser memory..
- [3] And the outcome is: Backend API Gateway: Used to authorize subsequent requests for this session..

### US-018 — LLM Audio Output – Candidate Flow
**As a** Candidate, **I want** LLM Audio Output, **so that** Frontend: Played through the candidate's speakers..
**Acceptance Criteria**
- [1] Given interviewer llm generates welcome message, questions, or thank you message., when the action is performed, then the system processes it as: Backend sends text to a TTS API, which returns an audio stream..
- [2] And it is stored as: In‑Memory/Ephemeral: Streamed directly to the client..
- [3] And the outcome is: Frontend: Played through the candidate's speakers..

### US-019 — Candidate Audio Stream – Candidate Flow
**As a** Candidate, **I want** Candidate Audio Stream, **so that** Primary output is the Live Transcript Text..
**Acceptance Criteria**
- [1] Given browser captures microphone audio via webrtc api when candidate speaks., when the action is performed, then the system processes it as: Streamed to Backend, which chunks it and forwards to STT API..
- [2] And it is stored as: In‑Memory/Ephemeral during transit..
- [3] And the outcome is: Primary output is the Live Transcript Text..

### US-020 — Live Transcript Text – Candidate Flow
**As a** Candidate, **I want** Live Transcript Text, **so that** Frontend UI (displayed in real‑time) and Interviewer LLM (as context)..
**Acceptance Criteria**
- [1] Given output of the stt api (can be an answer or a question from the candidate)., when the action is performed, then the system processes it as: Backend appends the text to the conversation history..
- [2] And it is stored as: Redis: Appended to the "Chunk Store" for the active session..
- [3] And the outcome is: Frontend UI (displayed in real‑time) and Interviewer LLM (as context)..

### US-021 — Final Interview Transcript – Candidate Flow
**As a** Candidate, **I want** Final Interview Transcript, **so that** Proctor LLMs: Sent for final evaluation. Also available for HR to view directly..
**Acceptance Criteria**
- [1] Given backend assembles all conversation chunks from redis when interview ends., when the action is performed, then the system processes it as: Cleaned, formatted with speaker labels, and prepared for evaluation..
- [2] And it is stored as: PostgreSQL.
- [3] And the outcome is: Proctor LLMs: Sent for final evaluation. Also available for HR to view directly..

### US-022 — Structured Report – Candidate Flow
**As a** Candidate, **I want** Structured Report, **so that** Frontend Dashboard for Manager/HR..
**Acceptance Criteria**
- [1] Given created by the backend aggregation service., when the action is performed, then the system processes it as: Backend aggregates the three Structured Proctor Responses, averages scores, combines notes, and stores the final report..
- [2] And it is stored as: PostgreSQL.
- [3] And the outcome is: Frontend Dashboard for Manager/HR..
