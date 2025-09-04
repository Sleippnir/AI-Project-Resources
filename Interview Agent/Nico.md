# AI-Assisted Interviewer 🤖🎙️

## Overview
This project is a Proof of Concept (POC) of an **AI-based voice interview platform**.  
The system simulates a real interview experience:  
- Candidates interact with an AI interviewer.  
- Answers are recorded, transcribed, and evaluated automatically by an LLM.  
- Recruiters can access a dashboard with transcripts and evaluation scores.  

Main industries: **Human Resources, Education, AI Technology, Communication**.  

---

## Features (MVP)
- Candidate registration and login (JWT).  
- Predefined or custom role selection.  
- Text-based interview flow (first iteration).  
- Transcription stored in database.  
- Automatic evaluation of answers using an LLM (scale-based).  
- Dashboard with transcripts and scores.  

---

## Architecture Diagram
```mermaid
flowchart LR
  subgraph Client["Client / Web App"]
    UI["UI: React or Streamlit"]
  end

  subgraph Backend["Backend API - FastAPI"]
    Auth["JWT Auth"]
    Orchestrator["LLM Orchestrator - LangChain"]
    Eval["Evaluator - LLM Judge"]
    STT["STT Adapter - Whisper (optional)"]
    TTS["TTS Adapter - pyttsx3 (optional)"]
  end

  subgraph Data["Storage Layer"]
    DB["Database: SQLite or PostgreSQL"]
    OBJ["Object Storage: MinIO or S3"]
    Logs["Logs and Metrics"]
  end

  subgraph LLMs["LLM Providers or Runtime"]
    LLMlocal["Ollama Local LLM"]
    LLMcloud["OpenAI / Anthropic / Gemini"]
  end

  %% Client <-> Backend
  UI -->|HTTP JSON| Auth
  Auth -->|token| UI
  UI <--> Orchestrator

  %% Orchestrator core logic
  Orchestrator -->|fetch questions| DB
  DB -->|questions| Orchestrator

  Orchestrator -->|generate/refine questions| LLMlocal
  Orchestrator -.-> LLMcloud

  %% Answers
  UI -->|text or audio| Orchestrator
  Orchestrator -->|store answer| DB
  Orchestrator -->|store audio| OBJ

  %% Voice adapters
  Orchestrator --> STT
  STT -->|transcript| Orchestrator
  Orchestrator --> TTS
  TTS -->|voice question| UI

  %% Evaluation
  Orchestrator --> Eval
  Eval -->|score + rationale| LLMlocal
  Eval -.-> LLMcloud
  Eval -->|save score| DB
  Orchestrator -->|feedback| UI

  %% Observability
  Orchestrator --> Logs
  Eval --> Logs
