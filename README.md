# Safety-First Two-Stage Mental Health Support Chatbot

An English mental-health support chatbot whose first stage **routes** every message (crisis / support / small talk / out-of-scope) and whose second stage **generates** a reply with a locally hosted LLM, grounded in real counselor responses.

> 📄 Paper submitted to **ICEIR 2026** — Special Track: AI for Healthcare.
> 📦 Data: [Zenodo DOI 10.5281/zenodo.23247788](https://doi.org/10.5281/zenodo.23247788)

<!-- Add an architecture diagram here, e.g. ![architecture](docs/architecture.png) -->

## Architecture
```
user message
   │
   ▼
[Stage 1] Router (fine-tuned RoBERTa-base) ──► crisis? ──► safety response + resources
   │                     ▲
   │        two-level safety layer
   │        (per-sentence risk score + first-person keyword rule)
   ▼
[Stage 2] Similarity-gated retrieval over 10,967 counselor responses
   │
   ▼
Qwen2.5-7B (Ollama) with grounding rules ──► reply
```

## Results
| What | Result |
|---|---|
| Router macro-F1 on held-out + external messages | **0.281 → 0.849** (95% CI 0.794–0.889) after adding figurative-language and natural out-of-scope data |
| Crisis recall on unseen C-SSRS users | **0.914** at a **0.3%** false-alarm rate |
| Replies attributing emotions the user never mentioned | **62.0% → 8.0%** (p < 0.001) |

A ten-configuration ablation showed that a router scoring 0.833 macro-F1 on its own test split can still fail badly on messages from new sources — the main lesson of this project.

## Data
Trained only on published, independently annotated datasets: MentalChat16K, CLINC150, C-SSRS Reddit, GoEmotions, HWU64, MultiWOZ 2.2.

## Project history
- **v1–v6:** DistilBERT, then BGE-M3 embeddings + K-NN intent classifier over 109 intents with Llama 3.2 (Ollama); dataset cleaning raised accuracy from 84% to ~90%.
- **Paper version:** redesigned as a 4-route safety router + retrieval-grounded generation.

## Team
FPT University Can Tho — AI-DS Project (DAP). Team lead: Ho Vinh Hung; members: Nguyen Phuoc Thinh, Nguyen Phuoc Xiem. Supervisor: Cao Hong Nga.

## Disclaimer
Research prototype, **not** a replacement for professional help. If you are in crisis, contact local emergency services.
