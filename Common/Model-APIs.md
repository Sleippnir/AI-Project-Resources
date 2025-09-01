**Table 1: LLM API Provider Master Comparison**

| Provider | Flagship Model | Price/1M Tokens (Input/Output) | Max Context Window | Free Tier Summary | Key Differentiator |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **OpenAI** | GPT-5 | $1.25 / $10.00 | 128k (GPT-4o/Turbo) | Opt-in for complimentary tokens in exchange for data sharing. | Access to the latest state-of-the-art models and a mature tools ecosystem. |
| **Google** | Gemini 2.5 Pro | $1.25 / $10.00 (<=200k) | 2M (Gemini 1.5 Pro) | Persistent, rate-limited free tier for most models (e.g., 5 RPM/100 RPD). | Industry-leading context windows and native multimodality. |
| **Anthropic** | Claude 4.1 Opus | $15.00 / $75.00 | 1M (Sonnet 4 Beta) | No free API tier; requires a minimum $5 credit purchase to start. | Focus on AI safety, reliability, and best-in-class long-context reasoning. |
| **Cohere** | Command R+ | $2.50 / $10.00 | 128k | Free, rate-limited Trial Key for evaluation (1k calls/month cap). | Specialized excellence in enterprise-grade Retrieval-Augmented Generation (RAG). |

**Table 2: Free Tier & Alternatives - A Cost vs. Tradeoff Analysis**

| Provider/Platform | Free Offering Model | Key Limits (RPM/Daily Cap) | Data Privacy Policy (Used for Training?) | Performance/Reliability Caveat |
| :--- | :--- | :--- | :--- | :--- |
| **Google Gemini** | Persistent free tier | 5-15 RPM / 100-1,000 RPD (model dependent) | Yes, data from the free tier is used to improve products. | Production-grade reliability, but with lower rate limits than paid tiers. |
| **OpenRouter** | Access to `:free` models | 50 RPD (default), 1000 RPD (with $10+ credits) | Yes, data logging is the explicit business model for free models. | Variable; depends on the underlying provider. Can be less reliable than direct API access. |
| **Hugging Face** | Monthly credit | $0.10/month credit | Varies by model provider; user must check individual model cards. | Can experience "cold start" delays (503 errors) for less popular models. |
| **Groq** | Persistent free tier | 30 RPM / 1k RPD (Llama 3.3 70B) | Not explicitly stated for the free tier, but enterprise focus suggests strong privacy controls. | Extremely high performance (tokens/sec), but with a limited selection of open-source models. |
| **OpenAI** | Complimentary tokens | Up to 11M tokens/day (shared pool) | Yes, explicitly for "traffic shared with OpenAI" to improve models. | Production-grade reliability, but requires data sharing. |
