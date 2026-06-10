<p align="center">
  <h1 align="center">Chromadb-rag-pipeline</h1>
  <p align="center">
    A complete, hands-on RAG system — PDF ingestion → ChromaDB → Answer Generation → Evaluation → Prompt Fine-Tuning
  </p>
  <p align="center">
    <img src="https://img.shields.io/badge/Python-3.10+-blue?style=flat-square&logo=python" />
    <img src="https://img.shields.io/badge/ChromaDB-Vector%20DB-8B5CF6?style=flat-square" />
    <img src="https://img.shields.io/badge/LangChain-Framework-10B981?style=flat-square" />
    <img src="https://img.shields.io/badge/HuggingFace-Transformers-F59E0B?style=flat-square&logo=huggingface" />
    <img src="https://img.shields.io/badge/Plotly-Visualisation-3B82F6?style=flat-square" />
    <img src="https://img.shields.io/badge/Colab-Ready-F97316?style=flat-square&logo=googlecolab" />
  </p>
</p>

---

## 📺 Learn Alongside This Repo

This project was built while studying the ChromaDB playlist below.  
Watch it to understand the **theory**, then run this notebook to see it in **working production code**.

> 🎬 **[ChromaDB Complete Tutorial Playlist →](https://youtube.com/playlist?list=PLEYSwx3duQ2DGcKullDTbHOPKuNsr9yvC&si=0tUez1x9_XjVVTj1)**

---

## 🗂️ Repository Structure

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

| Component | Library | Role |
|-----------|---------|------|
| PDF Loading | `langchain-community` PyPDFLoader | Extract raw text from PDF pages |
| Chunking | `langchain-text-splitters` | Split text into 500-char overlapping pieces |
| Embeddings | `sentence-transformers` BAAI/bge-small-en-v1.5 | Convert text → 384-dim vectors |
| Vector DB | `chromadb` | Store & search vectors by cosine similarity |
| LLM | `transformers` distilgpt2 | Generate answers from retrieved context |
| Visualisation | `plotly` | Interactive charts throughout |
| Evaluation | Cosine similarity | Measure generated vs expected answer quality |

> 💡 **Key insight:** ChromaDB is only the *storage layer* — it doesn't create embeddings. LangChain does the chunking, SentenceTransformer creates the vectors, ChromaDB stores and retrieves them.

---

## 🚀 Quick Start

### ▶️ Option A — Google Colab (Recommended, no setup needed)

1. Open [Google Colab](https://colab.research.google.com/)
2. Upload `ChromaDB_RAG_Complete.ipynb`
3. Upload your PDF when prompted in **Section 3**
4. `Runtime → Run All`

### 💻 Option B — Run Locally

```bash
git clone https://github.com/RajuKumar077/chromadb-rag-pipeline.git
cd chromadb-rag-pipeline

pip install chromadb sentence-transformers pypdf langchain \
            langchain-community langchain-text-splitters \
            transformers accelerate torch plotly pandas numpy

# Open the notebook and set PDF_PATH in Section 3
jupyter notebook ChromaDB_RAG_Complete.ipynb
```

---

## 📊 What the Notebook Covers (12 Sections)

| Section | Topic | Key Output |
|---------|-------|------------|
| 1–2 | Install & import all dependencies | Clean environment |
| 3 | Load PDF (Colab or local) | Parsed document pages |
| 4 | Chunk the document | 500-char overlapping pieces |
| 5 | Visualise chunk distribution | Plotly histogram + scatter by page |
| 6 | Embeddings + cosine similarity demo | Bar chart: similar vs unrelated queries |
| 7 | Store all vectors in ChromaDB | Indexed collection |
| 8 | Query ChromaDB + retrieval charts | Single & multi-query similarity plots |
| 9 | Full RAG pipeline | retrieve → prompt → generate |
| 10 | Evaluate answer quality | Similarity score chart + scatter |
| 11 | Prompt style comparison | `standard` / `concise` / `step_by_step` / `eli5` heatmap |
| 12 | Summary & next steps | Upgrade path |

---

## 🔑 Core Concepts Explained

**Why chunking?**
Embedding models have a token limit. Splitting into chunks lets us embed focused meaning rather than entire pages — better retrieval accuracy.

**What does ChromaDB actually do?**
ChromaDB is *only* the storage and search layer. You generate vectors outside it (via SentenceTransformer), then ChromaDB indexes them and returns the closest ones to a query vector.

**Why RAG over plain LLM?**
A plain LLM answers from training data only. RAG injects your document's actual content into the prompt — the LLM answers from *your data*, reducing hallucination significantly.

**What is "prompt fine-tuning" here?**
Not weight updates — we compare different instruction styles in the prompt template and measure which produces the highest semantic similarity to a reference answer. It's **prompt engineering as a measurable optimisation**.

---

## 📈 Visualisations Included

- 📏 Chunk length distribution histogram
- 📄 Chunk length scatter by document page
- 🔍 Cosine similarity bar chart (similar vs unrelated sentences)
- 📈 Retrieval similarity scores per rank
- 🔗 Multi-query retrieval quality grouped bar chart
- 📊 Evaluation scores per question (colour-coded by quality threshold)
- 🔗 Retrieval quality vs answer quality scatter
- 🎛️ Prompt style comparison bar chart
- 🌡️ Prompt style × question similarity heatmap

---

## 🔮 Upgrade Path

- [ ] Swap `distilgpt2` → `Phi-3-mini-4k-instruct` or call **OpenAI / Anthropic API**
- [ ] Use `chromadb.PersistentClient(path="./chroma_db")` for disk persistence across sessions
- [ ] Add **metadata filtering** — query by page, section, or source file
- [ ] Integrate [**Ragas**](https://github.com/explodinggradients/ragas) for faithfulness, context precision, and answer relevance metrics
- [ ] Add **hybrid search** — dense embedding retrieval + BM25 sparse retrieval
- [ ] Build a **Streamlit / Gradio** frontend on top

---

## 👤 Author

**Raju Kumar** — AI/ML Engineer  
🌐 [therajukumar.vercel.app](https://therajukumar.vercel.app) &nbsp;|&nbsp;
💼 [linkedin.com/in/raju-kumar7388](https://linkedin.com/in/raju-kumar7388) &nbsp;|&nbsp;
🐙 [github.com/RajuKumar077](https://github.com/RajuKumar077)

---

## ⭐ Found this useful?

Star the repo — it helps others find it.
And watch the [YouTube playlist](https://youtube.com/playlist?list=PLEYSwx3duQ2DGcKullDTbHOPKuNsr9yvC&si=0tUez1x9_XjVVTj1) that this project is built on top of.
