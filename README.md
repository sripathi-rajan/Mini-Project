# NovaGap: Literature-Grounded Multi-Source Novelty Assessment & Gap-Filling Engine

**Team**: SRIPATHIRAJAN V (2403917724421037), CHANDRU M (2403917724421010)  
**Guide**: Mrs S.Sugantha  
**Institution**: Thiagarajar College of Engineering, Madurai  
**SDG**: 9 – Industry, Innovation and Infrastructure  

---

## 1. Clear Problem Statement

Students and early researchers struggle to answer a critical question before starting a project:

> **"Is my project idea actually novel, or does it already exist in papers, open-source code, or patents? If it exists, what specific gaps remain that I can fill?"**

### Current Pain Points 
- Manual search across Google Scholar, Semantic Scholar, arXiv, IEEE Xplore, GitHub, and ChatGPT is **time-consuming** (hours to days) and **fragmented**.
- General-purpose AI assistants are not designed for **structured academic novelty validation** — without systematic cross-source retrieval across papers, code, and patents, their responses cannot serve as a reproducible, auditable evidence trail for research.
- Existing tools either:
  - Check only one source (papers **or** code),
  - Are paid / enterprise-only,
  - Give a black-box “novelty score” without literature-grounded reasoning,
  - Do **not** identify concrete research gaps or suggest how to improve the idea.
- Result: many student projects unknowingly reinvent existing work, and literature surveys remain weak.

### Research Gap We Fill (based on recent papers)
Recent high-quality work (ICLR 2025, ACL 2025, JASIST 2025, SDP@ACL 2025, arXiv 2025–2026) shows:

1. LLMs can generate ideas judged **more novel** than human experts, but are **weaker on feasibility** and suffer from self-evaluation failures and low diversity (Si et al., ICLR 2025).
2. Pure LLM novelty judgments are biased toward “medium novelty” and poorly calibrated; **literature-grounded RAG + re-ranking** is required (Shahid et al., Literature-Grounded Novelty Assessment, 2025).
3. Most systems evaluate **papers only**. Almost none jointly consider **papers + GitHub repositories + patents** for student-level project ideas.
4. Gap identification and **actionable improvement suggestions** (not just “not novel”) remain under-addressed.
5. No free, student-focused, explainable, end-to-end system exists that produces a literature survey + novelty score + gap analysis + improved idea variants.

**NovaGap** directly addresses these gaps.

---

## 2. Proposed Solution – What is New & Better

**NovaGap** is an AI-powered platform that:

1. Accepts a project title + short abstract / proposal.
2. Simultaneously retrieves relevant work from:
   - Academic papers (arXiv + Semantic Scholar style retrieval)
   - Open-source implementations (GitHub)
   - (Extensible to patents)
3. Computes **literature-grounded** semantic similarity using modern embeddings (Sentence-Transformers) + optional BM25.
4. Produces an explainable **Project Novelty Score** (0–100) with evidence.
5. Automatically identifies **research gaps** (limitations of existing work, unexplored combinations, missing features).
6. Uses a structured generation step to **suggest concrete improved project variants** that fill those gaps while remaining feasible for a student/academic timeline.
7. Generates a ready-to-use literature survey summary + ranked references.

### Key Technical Differentiators
- Multi-source retrieval (papers + code) instead of single-source.
- Embedding-based similarity + facet-style comparison for explainability (inspired by recent Idea Novelty Checker work).
- Explicit gap extraction + gap-filling idea enhancement (addresses the “what is the benefit?” criticism).
- Fully local embedding model → works offline for core scoring; live APIs used when available.
- Transparent scoring formula and evidence snippets (no black-box LLM score only).

---

## 3. System Architecture (High-Level)

```
User Input (Title + Abstract)
        │
        ▼
┌───────────────────────┐
│  Preprocessing &      │
│  Keyword Extraction   │
└───────────┬───────────┘
            │
    ┌───────┴───────┐
    ▼               ▼
arXiv / Papers   GitHub Repos
Retrieval        Retrieval
    │               │
    └───────┬───────┘
            ▼
┌───────────────────────┐
│  Embedding +          │
│  Similarity Ranking   │
│  (Sentence-Transformers)│
└───────────┬───────────┘
            │
            ▼
┌───────────────────────┐
│  Novelty Score +      │
│  Gap Analysis Engine  │
└───────────┬───────────┘
            │
            ▼
┌───────────────────────┐
│  Gap-Filling Idea     │
│  Enhancement Module   │
└───────────┬───────────┘
            │
            ▼
   Interactive Report / Dashboard
```

---

## 4. How to Run the Prototype (Review-2 Demo)

### Prerequisites
- Python 3.10+
- ~2–3 GB RAM (for the embedding model)

### Installation
```bash
cd NovaGap-NoveltyEngine
python -m venv venv
source venv/bin/activate          # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### Run the API + Demo UI
```bash
uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
```

Open browser: **http://localhost:8000**

### CLI Demo (no browser needed)
```bash
python -m app.cli_demo
```

### Offline / Safe Demo Mode
If network or rate limits are an issue during presentation, the system automatically falls back to a high-quality curated sample corpus so the full pipeline still runs and produces realistic scores, gaps, and suggestions.

---

## 5. Expected Outcomes & Benefits

| Benefit | Description |
|---------|-------------|
| Time saving | Literature survey + novelty check reduced from hours/days to minutes |
| Evidence-based | Every claim is backed by retrieved papers/repos |
| Gap-focused | Does not stop at “not novel”; tells you **what is missing** and **how to improve** |
| Student-friendly | Free, open, no API keys required for core functionality |
| Research quality | Encourages original contributions instead of repeated projects |
| Explainable | Novelty score formula + top similar works + gap statements are all visible |

---

## 6. Module Overview (for Implementation)

1. **Input & Preprocessing** – title, abstract, keyword extraction
2. **Multi-Source Retrieval** – arXiv + GitHub (extensible)
3. **Similarity & Novelty Analysis** – embeddings + scoring
4. **Research Gap Identification** – limitation mining + unexplored angles
5. **Gap-Filling Recommendation Engine** – improved idea variants
6. **Report Generation** – literature survey, scores, references, suggestions

---

## 7. References (Selected Q1 / Recent)

- Si, Yang, Hashimoto. *Can LLMs Generate Novel Research Ideas? A Large-Scale Human Study with 100+ NLP Researchers*. ICLR 2025 / arXiv 2409.04109.
- Shahid et al. *Literature-Grounded Novelty Assessment of Scientific Ideas*. SDP Workshop @ ACL 2025 / arXiv 2506.22026.
- Wu et al. *Automated Novelty Evaluation of Academic Paper: A Collaborative Approach Integrating Human and LLM Knowledge*. JASIST 2025.
- Lin et al. *Evaluating and Enhancing Large Language Models for Novelty Assessment in Scholarly Publications*. 2025.
- Lewis et al. *Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks*. NeurIPS 2020.
- Reimers & Gurevych. *Sentence-BERT*. EMNLP 2019.
- arXiv API, Semantic Scholar Academic Graph API, GitHub REST API documentation.

---

## 8. Future Work
- Add patent search (Google Patents / USPTO open data).
- Multi-agent self-critique loop for feasibility checking.
- Fine-tuned novelty judge on peer-review data.
- User study with real students measuring time saved and project quality improvement.

---

**Contact / Demo**: Run the server and try the sample project ideas provided in the UI.
