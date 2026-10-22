# \# StudySense

# 

# !\[Python](https://img.shields.io/badge/Python-3.9%2B-blue) !\[Streamlit](https://img.shields.io/badge/Streamlit-1.30%2B-FF4B4B) !\[LangChain](https://img.shields.io/badge/LangChain-RAG-green) !\[Ollama](https://img.shields.io/badge/LLM-Ollama%20Llama%203-orange) !\[Docker](https://img.shields.io/badge/Docker-ready-2496ED)

# 

# StudySense is a document question-answering application built on Retrieval-Augmented Generation (RAG). Users upload their own documents and ask questions in natural language. The system returns answers grounded in those documents, together with the source passages used, so every answer can be verified.

# 

# The language model runs locally through Ollama, so documents never leave the user's machine.

# 

# \---

# 

# \## Overview

# 

# Teams spend a lot of time searching through long PDFs, reports, and notes for specific information. StudySense turns a collection of documents into a searchable, conversational knowledge base:

# 

# \- Upload documents once, then query them in plain language.

# \- Receive answers with citations to the exact source passages.

# \- Keep all data on-premise, with no third-party API calls.

# \- Monitor answer quality through a built-in evaluation dashboard.

# 

# \---

# 

# \## Key Features

# 

# \- \*\*Multi-format ingestion:\*\* PDF, DOCX, and TXT files.

# \- \*\*Local LLM:\*\* Llama 3 served through Ollama, which keeps data private.

# \- \*\*Semantic search:\*\* ChromaDB vector store with HuggingFace embeddings (`all-MiniLM-L6-v2`).

# \- \*\*Source citations:\*\* each answer includes expandable references to the retrieved passages.

# \- \*\*Quality evaluation:\*\* RAGAS metrics (faithfulness and relevancy) with Plotly visualizations.

# \- \*\*Clean web interface:\*\* built with Streamlit, with real-time processing status.

# \- \*\*Containerized:\*\* a `Dockerfile` is included for easier deployment.

# 

# \---

# 

# \## Architecture

# 

# ```

# Documents (PDF / DOCX / TXT)

# &#x20;       |

# &#x20;       v

# &#x20; Loading and chunking

# &#x20;       |

# &#x20;       v

# &#x20; Embeddings (HuggingFace)

# &#x20;       |

# &#x20;       v

# &#x20; Vector store (ChromaDB)

# &#x20;       |

# &#x20;       v

# &#x20; Retriever  --->  Llama 3 (Ollama)  --->  Answer + sources

# ```

# 

# | Layer | Technology |

# |-------|------------|

# | Interface | Streamlit |

# | Orchestration | LangChain |

# | LLM | Ollama (Llama 3) |

# | Vector store | ChromaDB |

# | Embeddings | HuggingFace sentence-transformers |

# | Evaluation | RAGAS |

# | Visualization | Plotly |

# 

# \---

# 

# \## Getting Started

# 

# \### Prerequisites

# 

# 1\. Python 3.9 or later

# 2\. \[Ollama](https://ollama.com) installed, with the Llama 3 model pulled:

# 

# ```bash

# ollama pull llama3

# ollama serve

# ```

# 

# \### Installation

# 

# ```bash

# git clone https://github.com/Salmaabdelfattah73/StudySense.git

# cd StudySense

# ```

# 

# Create and activate a virtual environment:

# 

# ```bash

# \# Windows

# python -m venv venv

# venv\\Scripts\\activate

# 

# \# macOS / Linux

# python3 -m venv venv

# source venv/bin/activate

# ```

# 

# Install the dependencies:

# 

# ```bash

# pip install -r requirements.txt

# ```

# 

# \### Run the application

# 

# ```bash

# streamlit run app.py

# ```

# 

# \---

# 

# \## Usage

# 

# 1\. \*\*Upload:\*\* open the Knowledge Base tab and upload your documents.

# 2\. \*\*Process:\*\* start processing. The system splits the documents into chunks and embeds them.

# 3\. \*\*Ask:\*\* open the Chat tab and ask questions about your documents.

# 4\. \*\*Verify:\*\* open the sources section under an answer to see the passages it was based on.

# 

# \---

# 

# \## Project Structure

# 

# ```

# StudySense/

# ├── app.py              # Streamlit application entry point

# ├── config.py           # Configuration (paths, model names, constants)

# ├── utils.py            # Document loading, chunking, and vector store logic

# ├── evaluation.py       # RAGAS evaluation and visualization

# ├── Dockerfile          # Container build file

# ├── requirements.txt    # Python dependencies

# └── vector\_store/       # Generated at runtime (not tracked in Git)

# ```

# 

# \---

# 

