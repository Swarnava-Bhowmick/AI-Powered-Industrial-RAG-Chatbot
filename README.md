# 🤖 AI-Powered Industrial Chatbot
### RAG Pipeline · LLaMA 3.2 · FAISS · Streamlit

> Upload any PDF → Ask questions → Get grounded answers. No hallucinations. No API costs.

---

## 📌 Overview

Industrial manuals and technical documents are large and hard to search manually. This project solves that with a **Retrieval-Augmented Generation (RAG)** pipeline that extracts, indexes, and queries PDF content using a fully local LLM stack.

**Built for:** ROBO AI Industrial Training Program

---

## ⚡ Quick Start

```bash
# 1. Clone & enter project
git clone https://github.com/your-username/AI-Industrial-Chatbot.git
cd AI-Industrial-Chatbot

# 2. Create virtual environment
python -m venv .venv
source .venv/bin/activate          # Linux/Mac
# .venv\Scripts\activate           # Windows

# 3. Install dependencies
pip install streamlit PyPDF2 langchain langchain-huggingface
pip install langchain-community faiss-cpu sentence-transformers ollama

# 4. Pull LLaMA 3.2 (requires Ollama installed → https://ollama.com)
ollama pull llama3.2

# 5. Launch
streamlit run app.py
# Opens at → http://localhost:8501
```

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

## 🧠 Core Code

**Text Extraction**
```python
def extract_text_from_pdf(pdf_path):
    reader = PdfReader(pdf_path)
    return "".join(page.extract_text() for page in reader.pages)
```

**Chunking**
```python
splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=200)
chunks = splitter.split_text(text)
```

**Embedding & Indexing**
```python
embeddings = HuggingFaceEmbeddings(model_name="sentence-transformers/all-MiniLM-L6-v2")
vector_store = FAISS.from_texts(chunks, embedding=embeddings)
vector_store.save_local("faiss_index")
```

**QA Chain**
```python
retriever = vector_store.as_retriever()          # top-4 chunks
llm       = Ollama(model="llama3.2")
qa_chain  = RetrievalQA(retriever=retriever,
                        combine_documents_chain=load_qa_chain(llm, "stuff"))
```

**Prompt Template (LangChain default)**
```
Use the following context to answer the question.
If you don't know the answer, say you don't know.

Context: {context}
Question: {question}
Answer:
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

## 🔮 Roadmap

| Status | Feature |
|---|---|
| ✅ | PDF upload & semantic Q&A |
| ✅ | Fully offline, zero API cost |
| 🔲 | DOCX / XLSX / HTML / OCR support |
| 🔲 | Multi-PDF simultaneous querying |
| 🔲 | Chat history & session memory |
| 🔲 | Pinecone / Weaviate cloud scaling |
| 🔲 | Fine-tuning on domain corpus |
| 🔲 | User auth & role-based access |
| 🔲 | Citation-based answers with source highlighting |
| 🔲 | Cloud deployment |

---

## 🤝 Contributing

1. Fork the repository
2. Create a branch: `git checkout -b feature/your-feature`
3. Commit changes: `git commit -m "Add your feature"`
4. Push: `git push origin feature/your-feature`
5. Open a Pull Request

---

## 📜 License

MIT License — free to use, modify, and distribute.

---

## 👨‍💻 Author

**Swarnav** · B.Sc. Physics, Mathematics & Electronics

Interests: AI · Robotics · Physics · Mathematics · Scientific Computing · Open Source

---

*Generated for ROBO AI Industrial Training Program*
