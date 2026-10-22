# StudySense

![Python](https://img.shields.io/badge/Python-3.9%2B-blue) ![Streamlit](https://img.shields.io/badge/Streamlit-1.30%2B-FF4B4B) ![LangChain](https://img.shields.io/badge/LangChain-RAG-green) ![Ollama](https://img.shields.io/badge/LLM-Ollama%20Llama%203-orange) ![Docker](https://img.shields.io/badge/Docker-ready-2496ED)

StudySense is a document question-answering platform built on Retrieval-Augmented Generation (RAG). It allows users to upload their own documents and query them in natural language. Each answer is grounded in the uploaded content and is accompanied by the source passages used to produce it, so every response can be verified.

All language model inference runs locally through Ollama. Documents are never sent to an external service, which makes the system suitable for confidential or sensitive material.

---

## Table of Contents

1. [Overview](#1-overview)
2. [Key Features](#2-key-features)
3. [System Architecture](#3-system-architecture)
4. [How It Works](#4-how-it-works)
5. [Technology Stack](#5-technology-stack)
6. [Getting Started](#6-getting-started)
7. [Usage](#7-usage)
8. [Evaluation](#8-evaluation)
9. [Deployment with Docker](#9-deployment-with-docker)
10. [Project Structure](#10-project-structure)
11. [Author](#11-author)

---

## 1. Overview

Organizations and individuals often hold large collections of reports, manuals, and notes in which finding a specific piece of information is slow and error-prone. Keyword search is limited because it cannot interpret the meaning of a question.

StudySense addresses this by converting a document collection into a conversational knowledge base:

- Documents are uploaded once and indexed for semantic search.
- Questions are asked in plain language and answered from the indexed content.
- Every answer cites the passages it relied on, which supports verification.
- All processing happens locally, so data stays under the user's control.
- An evaluation dashboard measures the quality of the generated answers.

---

## 2. Key Features

| Feature | Description |
|---------|-------------|
| **Multi-format ingestion** | Accepts PDF, DOCX, and TXT files |
| **Local language model** | Llama 3 served through Ollama, with no external API calls |
| **Semantic retrieval** | ChromaDB vector store with HuggingFace embeddings (`all-MiniLM-L6-v2`) |
| **Source citations** | Each answer includes expandable references to the retrieved passages |
| **Quality evaluation** | RAGAS metrics displayed with Plotly charts |
| **Web interface** | Streamlit application with real-time processing status |
| **Containerization** | A `Dockerfile` is provided for consistent deployment |

---

## 3. System Architecture

```
Documents (PDF / DOCX / TXT)
        |
        v
  Document loading and chunking
        |
        v
  Embedding generation (HuggingFace)
        |
        v
  Vector store (ChromaDB)
        |
        v
  Retriever  --->  Llama 3 (Ollama)  --->  Answer + cited sources
```

| Component | Responsibility |
|-----------|----------------|
| **Interface layer** | Streamlit application for uploading documents, chatting, and viewing evaluation results |
| **Processing layer** | Document loading, text splitting, and embedding generation |
| **Storage layer** | Persistent ChromaDB vector store |
| **Generation layer** | Llama 3 running on a local Ollama server |
| **Evaluation layer** | RAGAS metrics and visualizations |

---

## 4. How It Works

1. **Ingestion:** uploaded files are read and converted to plain text.
2. **Chunking:** the text is split into overlapping passages so that relevant context can be retrieved precisely.
3. **Embedding:** each passage is converted into a numerical vector that represents its meaning.
4. **Indexing:** the vectors are stored in ChromaDB for fast similarity search.
5. **Retrieval:** when a question is asked, the most relevant passages are found by comparing vectors.
6. **Generation:** the retrieved passages are given as context to Llama 3, which writes the answer.
7. **Citation:** the passages used are returned alongside the answer for verification.

---

## 5. Technology Stack

| Layer | Technology |
|-------|------------|
| Interface | Streamlit |
| Orchestration | LangChain |
| Language model | Ollama (Llama 3) |
| Vector store | ChromaDB |
| Embeddings | HuggingFace sentence-transformers |
| Evaluation | RAGAS |
| Visualization | Plotly |
| Packaging | Docker |

---

## 6. Getting Started

### Prerequisites

- Python 3.9 or later
- [Ollama](https://ollama.com) installed, with the Llama 3 model available:

```bash
ollama pull llama3
ollama serve
```

### Installation

```bash
git clone https://github.com/Salmaabdelfattah73/StudySense.git
cd StudySense
```

Create and activate a virtual environment:

```bash
# Windows
python -m venv venv
venv\Scripts\activate

# macOS / Linux
python3 -m venv venv
source venv/bin/activate
```

Install the dependencies:

```bash
pip install -r requirements.txt
```

### Run the application

```bash
streamlit run app.py
```

The application opens in the browser, usually at `http://localhost:8501`.

### Default Settings

| Setting | Value |
|---------|-------|
| Supported formats | PDF, DOCX, TXT |
| Maximum file size | 50 MB |
| Maximum pages per document | 50 |
| Chunk size / overlap | 1000 / 200 characters |
| Passages retrieved per question | 4 |
| Language model | `llama3` (temperature 0.3) |
| Embedding model | `all-MiniLM-L6-v2` |

These values are defined in `config.py` and can be adjusted.

---

## 7. Usage

1. **Upload:** open the Knowledge Base tab and upload one or more documents.
2. **Process:** start processing. The system chunks and embeds the documents and shows the progress.
3. **Ask:** open the Chat tab and ask questions about the documents.
4. **Verify:** open the sources section under an answer to read the passages it was based on.
5. **Review quality:** open the analytics view to see the evaluation metrics.

---

## 8. Evaluation

Answer quality is measured with RAGAS, a framework for evaluating RAG pipelines. The metrics to compute are selected in `config.py`, and the results are shown as charts. Typical metrics include:

- **Faithfulness:** whether the answer is supported by the retrieved context.
- **Answer relevancy:** whether the answer addresses the question that was asked.
- **Context precision and recall:** how well the retrieval step finds the right passages.

The evaluation module also produces a summary with performance bands and recommendations, such as adjusting the chunk size or the number of retrieved passages when retrieval scores are low.

---

## 9. Deployment with Docker

The `Dockerfile` builds an image based on `python:3.10-slim`, installs the system dependencies required for document processing (Poppler and Tesseract OCR), and serves the application on port 8501.

```bash
docker build -t studysense .
docker run -p 8501:8501 studysense
```

The language model runs in Ollama, which must be reachable from the container. When Ollama runs on the host machine, the container must be configured to connect to the host's address instead of `localhost`.

---

## 10. Project Structure

```
StudySense/
├── app.py              # Streamlit application entry point
├── config.py           # Configuration (paths, model names, constants)
├── utils.py            # Document loading, chunking, and vector store logic
├── evaluation.py       # RAGAS evaluation and visualization
├── Dockerfile          # Container build file
├── requirements.txt    # Python dependencies
└── vector_store/       # Generated at runtime (not tracked in Git)
```

---
