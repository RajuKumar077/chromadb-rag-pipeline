# 🧠 chromadb-rag-pipeline

> **A complete, hands-on RAG (Retrieval-Augmented Generation) system built with ChromaDB, SentenceTransformers, and HuggingFace — from PDF ingestion to answer evaluation and prompt fine-tuning.**

---

## 📺 Learn Alongside This Repo

This project was built while studying the ChromaDB playlist below.  
Watch it to understand the theory, then use this notebook to see it in production code.

🎬 **[ChromaDB Complete Tutorial Playlist →](https://youtube.com/playlist?list=PLEYSwx3duQ2DGcKullDTbHOPKuNsr9yvC&si=0tUez1x9_XjVVTj1)**

---

## 🗂️ What's Inside

```
chromadb-rag-pipeline/
│
├── ChromaDB_RAG_Complete.ipynb   ← Main notebook (run this)
├── README.md                     ← You are here
└── data/
    └── your_document.pdf         ← Drop your PDF here
```

---

## 🔄 Pipeline Overview

```
PDF
 │
 ▼
Chunking  ──────────────────────────────  LangChain RecursiveCharacterTextSplitter
 │                                         chunk_size=500, overlap=100
 ▼
Embedding  ─────────────────────────────  SentenceTransformer (BAAI/bge-small-en-v1.5)
 │                                         384-dim dense vectors
 ▼
ChromaDB Store  ────────────────────────  Cosine similarity index
 │
 ▼
Query → Retrieve Top-k Chunks  ─────────  Semantic search
 │
 ▼
RAG Prompt  ────────────────────────────  Context + question injected into prompt
 │
 ▼
LLM Generation  ────────────────────────  distilgpt2 (swap with Phi-3 / GPT-4 easily)
 │
 ▼
Evaluation  ────────────────────────────  Cosine similarity vs expected answer
 │
 ▼
Prompt Fine-Tuning  ────────────────────  Compare standard / concise / step_by_step / eli5
```

---

## 📦 Tech Stack

| Component | Library | Purpose |
|-----------|---------|---------|
| PDF Loading | `langchain-community` PyPDFLoader | Extract text from PDF |
| Chunking | `langchain-text-splitters` | Split text into manageable pieces |
| Embeddings | `sentence-transformers` BAAI/bge-small-en-v1.5 | Text → vectors |
| Vector DB | `chromadb` | Store & search vectors |
| Generation | `transformers` distilgpt2 | Generate answers |
| Visualisation | `plotly` | Interactive charts |
| Evaluation | Cosine similarity | Measure answer quality |

---

## 🚀 Quick Start

### Option A — Google Colab (Recommended)

1. Open [Google Colab](https://colab.research.google.com/)
2. Upload `ChromaDB_RAG_Complete.ipynb`
3. Upload your PDF when prompted in Section 3
4. `Runtime → Run All`

### Option B — Local

```bash
git clone https://github.com/YOUR_USERNAME/chromadb-rag-pipeline.git
cd chromadb-rag-pipeline

pip install chromadb sentence-transformers pypdf langchain \
            langchain-community langchain-text-splitters \
            transformers accelerate torch plotly pandas numpy

# Set your PDF path in Section 3 of the notebook
jupyter notebook ChromaDB_RAG_Complete.ipynb
```

---

## 📊 What the Notebook Covers

| Section | Topic |
|---------|-------|
| 1–2 | Install dependencies & imports |
| 3 | Load a PDF (Colab upload or local path) |
| 4 | Chunk the document |
| 5 | Visualise chunk length distribution (Plotly histogram + scatter) |
| 6 | Generate embeddings + cosine similarity demo |
| 7 | Store all vectors in ChromaDB |
| 8 | Query ChromaDB + retrieval similarity charts |
| 9 | Full RAG pipeline (retrieve → prompt → generate) |
| 10 | Evaluate answer quality with semantic similarity |
| 11 | Prompt style comparison (`standard` / `concise` / `step_by_step` / `eli5`) + heatmap |
| 12 | Summary & next steps |

---

## 🔑 Key Concepts Explained

**Why chunking?**  
Embedding models have a token limit. Splitting text into chunks lets us embed focused pieces of meaning rather than entire pages.

**Why ChromaDB?**  
ChromaDB is the *storage layer* — it doesn't create embeddings, it stores and searches them. You generate the vectors, ChromaDB indexes and retrieves them by cosine similarity.

**Why RAG over plain LLM?**  
A plain LLM answers from training data only. RAG injects your document's actual content into the prompt, so the LLM answers from *your* data — reducing hallucination.

**What is prompt fine-tuning here?**  
Not weight updates — we compare different instruction styles in the prompt and measure which produces the highest semantic similarity to a reference answer. This is **prompt engineering as optimisation**.

---

## 📈 Sample Outputs

- Chunk length distribution histogram
- Cosine similarity bar chart across queries
- Multi-query retrieval quality grouped bar chart
- Evaluation scores per question (coloured by quality threshold)
- Prompt style × question similarity heatmap

---

## 🔮 Next Steps / Upgrades

- [ ] Swap `distilgpt2` with `Phi-3-mini` or call **OpenAI / Anthropic API**
- [ ] Use `chromadb.PersistentClient(path="./chroma_db")` for disk persistence
- [ ] Add **metadata filtering** (by page, section, source)
- [ ] Integrate [**Ragas**](https://github.com/explodinggradients/ragas) for faithfulness + context precision metrics
- [ ] Add **hybrid search** (dense + BM25 sparse retrieval)
- [ ] Build a **Streamlit / Gradio** UI on top

---

## 👤 Author

**Raju Kumar** — AI/ML Engineer  
🌐 [therajukumar.vercel.app](https://therajukumar.vercel.app)  
💼 [linkedin.com/in/raju-kumar7388](https://linkedin.com/in/raju-kumar7388)  
🐙 [github.com/RajuKumar077](https://github.com/RajuKumar077)

---

## ⭐ If this helped you

Give it a star on GitHub — it helps others find it.  
And check out the [YouTube playlist](https://youtube.com/playlist?list=PLEYSwx3duQ2DGcKullDTbHOPKuNsr9yvC&si=0tUez1x9_XjVVTj1) that inspired this build.
