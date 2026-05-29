# 🤖 AI-Powered Industrial Chatbot
### RAG Pipeline · LLaMA 3.2 · FAISS · Streamlit

> Upload any PDF → Ask questions → Get grounded answers. No hallucinations. No API costs.

---
## 📌 Overview

Industrial environments generate massive volumes of documentation — equipment manuals,
maintenance SOPs, safety guidelines, research reports, and compliance documents.
Searching through hundreds of pages manually is slow, error-prone, and inefficient.
Traditional keyword search fails to understand context, meaning a technician searching
for "motor overheating fix" might miss a relevant section titled "thermal protection protocol."

This project solves that problem using **Retrieval-Augmented Generation (RAG)** —
a modern AI architecture that combines the precision of semantic document search
with the language fluency of a Large Language Model (LLM).

### How It Solves the Problem

Instead of relying on a general-purpose LLM that might hallucinate or produce
outdated answers, this system **grounds every response in the actual uploaded document**.
Here is what happens under the hood:

1. **You upload a PDF** — any industrial manual, report, or technical document.
2. **The system reads and understands it** — text is extracted page by page,
   then split into overlapping chunks to preserve context at boundaries.
3. **Every chunk becomes a vector** — using a semantic embedding model
   (`all-MiniLM-L6-v2`), each chunk is converted into a 384-dimensional
   numerical representation that captures its meaning, not just its keywords.
4. **Vectors are stored in FAISS** — a blazing-fast vector database by Meta AI
   that enables sub-millisecond similarity search across thousands of chunks.
5. **You ask a question in plain English** — the question is also embedded,
   and FAISS retrieves the top-4 most semantically similar document chunks.
6. **LLaMA 3.2 generates the answer** — the retrieved chunks are passed as
   context to the local LLM, which synthesizes a precise, document-grounded answer.
7. **The answer appears in the chat UI** — powered by Streamlit, no frontend
   expertise needed.

### Why This Approach Is Better

| Problem | Traditional Search | This RAG System |
|---|---|---|
| Keyword mismatch | ❌ Misses synonyms | ✅ Semantic understanding |
| LLM hallucination | ❌ Makes things up | ✅ Grounded in your document |
| API dependency | ❌ Requires internet + cost | ✅ Fully local, $0 cost |
| Large document search | ❌ Slow manual lookup | ✅ Instant vector retrieval |
| Domain-specific Q&A | ❌ Generic answers | ✅ Document-specific answers |

### Key Design Decisions

- **Fully Offline** — LLaMA 3.2 runs locally via Ollama. No data leaves your machine.
  This is critical for industries handling sensitive or proprietary documentation.
- **No API Costs** — Both the embedding model and the LLM are open-source and
  run on local hardware. Zero recurring cost at any scale of usage.
- **Modular Architecture** — Each stage (ingestion, retrieval, generation) is
  independent. You can swap FAISS for Pinecone, or LLaMA for Mistral, with
  minimal code changes.
- **Context-Preserving Chunking** — A 200-character overlap between chunks ensures
  that sentences split across boundaries are never lost, improving answer accuracy.

**Built for:** ROBO AI Industrial Training Program
---

## 🏗️ System Architecture

```
INGESTION PIPELINE
──────────────────
PDF Upload (Streamlit)
    │
    ▼
Text Extraction (PyPDF2)
    │
    ▼
Text Chunking — size: 1000 chars, overlap: 200 chars (LangChain)
    │
    ▼
Embeddings — 384-dim vectors (all-MiniLM-L6-v2)
    │
    ▼
FAISS Vector Index (saved to disk)

QUERY PIPELINE
──────────────
User Question
    │
    ▼
Question Embedding (MiniLM)
    │
    ▼
Semantic Search → Top-4 Chunks (FAISS cosine similarity)
    │
    ▼
LLaMA 3.2 via Ollama (stuff chain · grounded context)
    │
    ▼
Answer → Streamlit UI
```

---

## 🧰 Tech Stack

| Component | Tool | Reason |
|---|---|---|
| Language | Python 3.10+ | — |
| UI | Streamlit | Zero-boilerplate web interface |
| PDF Parser | PyPDF2 | Lightweight, no external deps |
| Embeddings | all-MiniLM-L6-v2 | 384-dim, fast, high semantic accuracy |
| Vector DB | FAISS (faiss-cpu) | Sub-millisecond similarity search |
| LLM Framework | LangChain RetrievalQA | Modular, easy to extend |
| Local LLM | Ollama LLaMA 3.2 | Runs locally · no API cost · 3B params |

---

## 📂 Project Structure

```
AI-Industrial-Chatbot/
├── app.py                  # Main Streamlit application
├── requirements.txt
├── README.md
├── flowchart.html
├── AI_Chatbot_Project_Report.pdf
├── uploaded/               # Generated at runtime
└── faiss_index/            # Generated at runtime
```

---

## 📊 Performance Characteristics

| Metric | Value |
|---|---|
| Embedding Dimensions | 384 |
| Chunk Size | 1000 characters |
| Chunk Overlap | 200 characters |
| Retrieval Method | Cosine Similarity (FAISS) |
| Top-k Chunks Retrieved | 4 |
| LLM | LLaMA 3.2 (3B params) |
| Deployment | Fully Local |
| API Cost | $0 |

---

## 🎯 Use Cases

- Industrial documentation search
- Technical manual assistant
- Research paper Q&A
- SOP / equipment maintenance docs
- Company internal knowledge bases
- Educational content retrieval

---

## 📜 License

MIT License — free to use, modify, and distribute.

---

## 👨‍💻 Author

**Swarnava Bhowmick**

---

*Generated for ROBO AI Industrial Training Program*
