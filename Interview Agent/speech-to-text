## 🗣 Speech‑to‑Text Options for Low‑Volume AI Interview Agents

| Service | Free Tier / Cost for Low Volume | Ease of Integration | Pros | Cons |
|---------|--------------------------------|---------------------|------|------|
| **OpenAI Whisper (self‑hosted)** | Free (open‑source) — only infra cost (CPU/GPU) | Medium — Python/CLI setup, or use wrappers like FasterWhisper | Very accurate, multilingual (99+), robust to accents/noise | No real‑time streaming out‑of‑the‑box, GPU recommended for speed |
| **OpenAI Whisper API** | ~$0.006/min (~$0.36/hr) — no free tier, but very cheap for low volume | Easy — simple REST API | Same accuracy as self‑hosted, no infra to manage | Paid from first minute, internet required |
| **AssemblyAI** | 5 hrs free/month | Easy — REST API, good docs | Extra NLP features (summarization, sentiment, topic detection) | Free tier limited, paid after 5 hrs |
| **Deepgram** | 200 min free/month | Easy — streaming & batch, SDKs in multiple languages | Low latency, custom models | Free tier small, pricing after that |
| **Google Cloud Speech‑to‑Text** | $300 free credit (90 days) | Medium — needs GCP setup | Real‑time & batch, 73+ languages | Pricing after credit, GCP account overhead |
| **Amazon Transcribe** | 60 min free/month for 12 months | Medium — AWS setup | Real‑time, custom vocabulary | AWS complexity, limited free minutes |
| **WhisperAPI.com** (3rd‑party) | Free tier with generous credits | Very easy — upload or API | Whisper accuracy without setup | Dependent on their infra, not open‑source |

---

### 💡 Sweet Spot Recommendation for Experimentation

- **Zero hosting hassle:**  
  Start with **WhisperAPI.com** or **Deepgram** — both have free tiers and quick REST integration.  
  → Great for proof‑of‑concept without touching GPUs.

- **Maximum control & no per‑minute fees:**  
  Self‑host **FasterWhisper** (optimized Whisper) on a modest GPU or even CPU for low volume.  
  → Ideal if you want privacy and can run it on your own server.

- **Extra NLP features (summaries, sentiment):**  
  **AssemblyAI** — free 5 hrs/month, then pay‑as‑you‑go.  
  → Could auto‑summarize candidate answers.
