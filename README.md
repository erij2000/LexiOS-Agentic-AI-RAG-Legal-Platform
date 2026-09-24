# LexiOS — Agentic AI & RAG Legal Platform

> **Multilingual Agentic AI platform for legal research, document analysis, and grounded legal reasoning over Tunisian law.**

**LexiOS** is an advanced legal-AI platform designed to assist lawyers and legal professionals in searching, analyzing, and reasoning over Tunisian legal information.

The platform combines **LLM Agents, Hybrid RAG, LightRAG, article-aware retrieval, BM25, dense vector search, Tool Calling, OCR, multilingual processing, and automated RAG evaluation** into an end-to-end legal intelligence system.

Unlike a generic chatbot, LexiOS is designed around **grounded retrieval and legal-source traceability**, allowing generated answers to be linked back to the underlying legal provisions and retrieved sources.

---

## 🚀 Project Overview

Legal information in Tunisia remains highly fragmented across physical legal books, scanned documents, legislation, jurisprudence, contracts, and articles.

LexiOS addresses this challenge by building a searchable and reasoning-oriented legal knowledge layer capable of processing **Arabic and French legal content**, retrieving relevant legal provisions, and generating context-aware responses.

The platform supports:

* 🔎 Legal research and information retrieval
* 📚 Retrieval over Tunisian legal sources
* 🧠 Agentic multi-step reasoning
* 🔗 Hybrid semantic + lexical retrieval
* 🕸️ Graph-based contextual retrieval
* 📄 Legal document analysis
* 🌍 Arabic / French / English interaction
* 🔤 OCR-based document ingestion
* 🛡️ Grounded generation and hallucination control
* 📑 Source-aware legal citations
* 📊 Automated RAG evaluation

---

# 🧠 System Architecture

LexiOS follows a layered architecture designed to separate **ingestion, retrieval, reasoning, generation, evaluation, and application services**.

```text
                         ┌─────────────────────────┐
                         │       User / Lawyer     │
                         │     Arabic / French     │
                         │         / English       │
                         └────────────┬────────────┘
                                      │
                                      ▼
                         ┌─────────────────────────┐
                         │      Angular Frontend   │
                         │   Chat / Documents / UI │
                         └────────────┬────────────┘
                                      │
                                      ▼
                         ┌─────────────────────────┐
                         │      FastAPI Backend    │
                         │     REST API Layer      │
                         └────────────┬────────────┘
                                      │
                    ┌─────────────────┼─────────────────┐
                    │                 │                 │
                    ▼                 ▼                 ▼
             ┌─────────────┐   ┌─────────────┐   ┌─────────────┐
             │ LLM Agents  │   │ RAG Engine  │   │ Tool Calling│
             │             │   │             │   │             │
             └──────┬──────┘   └──────┬──────┘   └─────────────┘
                    │                 │
                    │        ┌────────┴────────┐
                    │        │                 │
                    │        ▼                 ▼
                    │   ┌───────────┐   ┌────────────┐
                    │   │  BM25     │   │ Dense      │
                    │   │ Retrieval │   │ Retrieval  │
                    │   └─────┬─────┘   └─────┬──────┘
                    │         │               │
                    │         └───────┬───────┘
                    │                 ▼
                    │       ┌──────────────────┐
                    │       │  Hybrid RAG      │
                    │       │ Re-ranking /     │
                    │       │ Context Fusion   │
                    │       └────────┬─────────┘
                    │                │
                    │                ▼
                    │       ┌──────────────────┐
                    │       │ Article-Aware    │
                    │       │ Retrieval        │
                    │       └────────┬─────────┘
                    │                │
                    │                ▼
                    │       ┌──────────────────┐
                    │       │   LightRAG       │
                    │       │ Graph Context    │
                    │       └────────┬─────────┘
                    │                │
                    └──────────┬─────┘
                               ▼
                    ┌─────────────────────┐
                    │ Context Construction│
                    │ & Grounding Layer   │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │      LLM Layer      │
                    │   Qwen / Groq       │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Hallucination Guard │
                    │ + Source Citations │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Grounded Legal      │
                    │ Response            │
                    └─────────────────────┘
```

---

# 🔬 Core AI Architecture

## 1. Hybrid RAG

LexiOS combines **lexical and semantic retrieval** to improve the retrieval of legal information.

### Lexical Retrieval

**BM25** is used to identify exact or highly relevant lexical matches.

This is particularly valuable for legal information because queries may contain:

* Article numbers
* Legal terminology
* Names of laws
* Exact expressions
* Formal legal formulations

### Dense Retrieval

Semantic retrieval uses multilingual embeddings to identify conceptually related legal passages even when the wording differs from the user's query.

The project uses:

**BAAI/bge-m3**

for multilingual dense representation.

### Hybrid Retrieval

The two retrieval strategies are combined to exploit both:

```text
Exact lexical relevance
        +
Semantic relevance
        ↓
Hybrid Retrieval
        ↓
Higher-quality legal context
```

This architecture is especially useful for legal search, where both **exact terminology** and **semantic meaning** matter.

---

# 🧩 Article-Aware Retrieval

Legal documents are not ordinary text collections.

A legal answer often depends on the relationship between:

* Law
* Chapter
* Section
* Article
* Paragraph
* Legal concept

LexiOS therefore incorporates **article-aware chunking and retrieval**, preserving legal structure and metadata instead of treating the corpus as a flat collection of unrelated text fragments.

This allows retrieved context to remain associated with its legal source and article-level information.

Example:

```text
Legal Code
   │
   ├── Chapter
   │      ├── Article 1
   │      ├── Article 2
   │      └── Article 3
   │
   └── Metadata
          ├── Source
          ├── Article
          ├── Language
          └── Document
```

---

# 🕸️ LightRAG & Knowledge Context

LexiOS also integrates **LightRAG** as a graph-oriented contextual layer.

The graph representation allows relationships between legal concepts and entities to be explored beyond isolated chunks.

The graph layer is used as a **contextual reasoning/retrieval component**, while the primary retrieval pipeline remains grounded in the dedicated legal retrieval architecture.

This separation helps preserve deterministic retrieval behavior while still exploiting graph-based relationships.

---

# 🤖 Agentic AI

LexiOS goes beyond a single prompt → single response architecture.

The system incorporates **LLM Agents** capable of coordinating multi-step reasoning and tool interactions.

The agentic layer can orchestrate:

```text
User Query
    │
    ▼
Intent / Query Understanding
    │
    ▼
Retrieval Strategy
    │
    ├── Lexical Search
    ├── Dense Search
    ├── Article Retrieval
    └── Graph Context
    │
    ▼
Context Construction
    │
    ▼
LLM Reasoning
    │
    ▼
Grounding / Validation
    │
    ▼
Legal Response + Citations
```

This architecture enables **parallel reasoning and multi-stage retrieval workflows** rather than relying exclusively on a single retrieval call.

---

# 🛠️ Tool Calling

LexiOS integrates **Tool Calling** into its agentic architecture.

Tools allow the language model to interact with dedicated system capabilities rather than relying exclusively on generated text.

This creates a separation between:

```text
LLM Reasoning
      │
      ▼
Tool Selection
      │
      ▼
Deterministic System Operation
      │
      ▼
Retrieved / Processed Data
      │
      ▼
LLM Response Generation
```

This approach improves controllability and makes complex legal workflows easier to extend.

---

# 📚 Legal Knowledge Base

The legal knowledge layer was built from a combination of legal materials, including:

* Tunisian legal codes
* Legal articles
* Jurisprudence
* Contracts and legal documents
* Scanned legal books
* Other structured and unstructured legal sources

The data collection process involved **manual collection and digitization of legal materials**, including photographs/scans of physical legal books and documents.

The project was developed in collaboration with **Cabinet Hila Ben Arbia**, providing access to domain-specific legal materials and practical legal requirements.

> **Important:** The legal corpus itself is not necessarily distributed with this repository because source documents may be subject to copyright, confidentiality, or usage restrictions.

---

# 👁️ OCR & Document Ingestion

Legal information is often available as scanned documents rather than machine-readable text.

LexiOS therefore incorporates an OCR-oriented ingestion pipeline.

```text
Physical / Scanned Document
          │
          ▼
       OCR Layer
          │
          ▼
Text Extraction
          │
          ▼
Cleaning & Normalization
          │
          ▼
Legal Structure Detection
          │
          ▼
Article-Aware Chunking
          │
          ▼
Metadata Enrichment
          │
          ▼
Embedding + Indexing
```

The project uses OCR tooling including **Surya** and **Marker** for document digitization and extraction workflows.

---

# 🗄️ Retrieval & Storage Layer

### Vector Database

**ChromaDB** is used for vector-based retrieval and embedding storage.

### Embeddings

**BAAI/bge-m3** provides multilingual dense embeddings suitable for Arabic and multilingual legal content.

### Lexical Search

**BM25** provides lexical retrieval for exact legal terminology and article-level expressions.

### Relational Storage

**PostgreSQL** is used for structured application data and backend persistence.

---

# 📊 RAG Evaluation

LexiOS incorporates **RAGAS** to evaluate retrieval-augmented generation quality.

The evaluation layer is used to analyze aspects such as:

* Retrieval quality
* Context relevance
* Answer relevance
* Faithfulness / groundedness

This provides a measurable framework for improving the RAG pipeline instead of relying exclusively on subjective inspection.

---

# 🛡️ Grounding & Hallucination Control

Legal AI requires a stronger grounding strategy than a conventional conversational assistant.

LexiOS therefore incorporates mechanisms designed to:

* Ground answers in retrieved legal context
* Preserve article/source metadata
* Generate source-aware citations
* Reduce unsupported legal claims
* Detect insufficient retrieval context
* Prevent the model from freely inventing legal provisions

Example citation format:

```text
[CSP Art. 30]
```

The objective is to make the generated response traceable to the underlying legal knowledge base.

---

# 🌍 Multilingual Architecture

LexiOS is designed for multilingual legal interaction across:

**Arabic · French · English**

This is particularly important for Tunisian legal information, where Arabic and French may coexist across legal documents, terminology, user queries, and supporting resources.

The multilingual retrieval architecture is supported by multilingual embeddings and language-aware processing.

---

# 🧠 LLM Layer

The platform is designed to work with modern instruction-following LLMs.

The current architecture uses **Qwen models through Groq**, with the model layer abstracted from the retrieval and application components.

This separation allows the underlying LLM to be replaced or upgraded without redesigning the complete RAG architecture.

Earlier development stages also experimented with **Ollama and local models**, but those belong to the earlier Lexibot architecture rather than the current LexiOS stack.

---

# 🏗️ Global Project Structure

The repository is organized around the major layers of the platform:

```text
LexiOS/
│
├── backend/
│   ├── api/
│   ├── agents/
│   ├── rag/
│   ├── retrieval/
│   ├── ingestion/
│   ├── tools/
│   ├── evaluation/
│   └── services/
│
├── frontend/
│   └── angular/
│       ├── components/
│       ├── services/
│       ├── pages/
│       └── ...
│
├── data/
│   ├── documents/
│   ├── processed/
│   └── metadata/
│
├── evaluation/
│   ├── datasets/
│   ├── experiments/
│   └── results/
│
├── docker/
│
├── scripts/
│
├── tests/
│
├── docker-compose.yml
├── requirements.txt
└── README.md
```

> The exact folder names may vary depending on the current repository organization; the architecture above represents the logical separation of the LexiOS platform.

---

# ⚙️ Technology Stack

## Artificial Intelligence

* LLMs
* Agentic AI
* RAG
* Hybrid RAG
* LightRAG
* Tool Calling
* NLP
* RAG Evaluation
* Prompt Engineering

## Retrieval & NLP

* **BM25**
* **BAAI/bge-m3**
* Dense Vector Search
* Semantic Retrieval
* Article-Aware Retrieval
* Hybrid Retrieval
* Graph-Based Retrieval

## Document Intelligence

* OCR
* **Surya**
* **Marker**
* Document preprocessing
* Text extraction
* Metadata enrichment

## Backend

* **Python**
* **FastAPI**
* REST APIs
* PostgreSQL

## AI / ML Frameworks

* **Transformers**
* PyTorch ecosystem
* Hugging Face tooling

## Vector & Data Infrastructure

* **ChromaDB**
* PostgreSQL
* Structured metadata
* Vector embeddings

## Frontend

* **Angular**
* TypeScript
* REST API integration
* Multilingual UI

## Infrastructure

* **Docker**
* Docker Compose
* Linux
* Git

## Evaluation

* **RAGAS**
* Retrieval evaluation
* Generation evaluation
* Grounding / faithfulness analysis

---

# 🔐 Application Architecture

LexiOS was designed with multiple user roles and controlled access to legal-AI capabilities.

```text
                    ┌──────────────────┐
                    │    Super Admin   │
                    └────────┬─────────┘
                             │
              ┌──────────────┼──────────────┐
              ▼              ▼              ▼
          Lawyer        Premium Client   Normal Client
              │              │              │
              ▼              ▼              ▼
          Legal AI       Legal AI        Limited AI
          Workspace      Workspace       Access
```

The application supports role-based behavior and controlled usage limits according to the user profile.

The platform architecture also considers:

* Authentication
* Authorization
* Role-based access
* Tenant/user isolation
* Usage limits
* Secure API communication

---

# 🐳 Deployment

The backend is containerized using **Docker** and orchestrated through **Docker Compose**.

A typical development environment consists of:

```text
Angular Frontend
       │
       ▼
FastAPI Backend
       │
       ├── PostgreSQL
       ├── ChromaDB
       ├── RAG / Agent Layer
       └── LLM Provider
```

The backend can be launched in development using:

```bash
uvicorn api.main:app --port 8080 --reload
```

Docker Compose is used to manage the application services and database infrastructure.

---

# 📈 Engineering Highlights

LexiOS brings together several advanced AI engineering concepts within a single production-oriented architecture:

| Area                | Implementation                     |
| ------------------- | ---------------------------------- |
| LLM Architecture    | Agentic AI + LLM Agents            |
| Retrieval           | Hybrid BM25 + Dense Retrieval      |
| Semantic Search     | BGE-M3 Embeddings                  |
| Vector Store        | ChromaDB                           |
| Graph Context       | LightRAG                           |
| Legal Retrieval     | Article-Aware Retrieval            |
| AI Interaction      | Tool Calling                       |
| Document Processing | OCR + PDF/Image Processing         |
| OCR                 | Surya + Marker                     |
| Grounding           | Source-aware Retrieval + Citations |
| Evaluation          | RAGAS                              |
| Backend             | FastAPI                            |
| Database            | PostgreSQL                         |
| Frontend            | Angular                            |
| Deployment          | Docker / Docker Compose            |
| Languages           | Arabic / French / English          |

---

# 📊 Project Scale

The legal knowledge base contains **1,100+ indexed legal chunks**, with more than **1,000 structured legal entries** represented in the retrieval layer.

The indexing pipeline preserves legal metadata to enable article-level retrieval and source-aware generation.

---

# 🎯 Engineering Objectives

LexiOS was designed around five major objectives:

### 1. Grounded Legal AI

Generate responses from retrieved legal sources rather than relying exclusively on the model's parametric knowledge.

### 2. High-Quality Retrieval

Combine lexical and semantic retrieval to handle both exact legal terminology and conceptual queries.

### 3. Agentic Reasoning

Enable multi-step reasoning and tool-based workflows for more complex legal scenarios.

### 4. Multilingual Legal Intelligence

Support legal interactions across Arabic, French, and English.

### 5. Evaluatable RAG

Use systematic evaluation with RAGAS to measure and improve retrieval and generation quality.

---

# 👩‍💻 My Contribution

As a core developer of LexiOS, I worked across the **AI, RAG, backend, and application architecture** of the platform.

My work included:

* Designing and implementing the **Hybrid RAG architecture**
* Developing legal-specific **article-aware retrieval**
* Integrating **BM25 and dense retrieval**
* Working with **BGE-M3 multilingual embeddings**
* Integrating **ChromaDB** for vector retrieval
* Implementing **LLM Agent workflows**
* Integrating **Tool Calling**
* Incorporating **LightRAG** for graph-based contextual retrieval
* Developing document ingestion and **OCR workflows**
* Implementing legal-source metadata and citation mechanisms
* Integrating **RAGAS** for RAG evaluation
* Developing backend services with **FastAPI**
* Integrating PostgreSQL persistence
* Containerizing services with **Docker**
* Contributing to the multilingual **Angular frontend**
* Designing mechanisms for grounded generation and hallucination reduction

---

# 🔬 From Lexibot to LexiOS

LexiOS evolved from an earlier prototype called **Lexibot**.

### Lexibot — Initial Prototype

The initial system explored:

```text
User Query
    ↓
Rasa NLP
    ↓
Intent / Query Processing
    ↓
Ollama LLM
    ↓
Legal Response
```

This prototype established the foundation for conversational legal assistance.

### LexiOS — Advanced Architecture

The system was subsequently redesigned around a much more advanced AI architecture:

```text
User Query
    ↓
Agentic Layer
    ↓
Hybrid Retrieval
 ┌──┴───────────────┐
 │                  │
BM25            Dense Search
 │                  │
 └───────┬──────────┘
         ▼
Article-Aware Retrieval
         │
         ▼
LightRAG Context
         │
         ▼
Tool Calling / Agents
         │
         ▼
LLM Generation
         │
         ▼
Grounding + Citations
         │
         ▼
Legal Response
```

The project therefore evolved from a conversational chatbot prototype into a **multilingual legal intelligence platform built around retrieval, agents, grounding, and evaluation**.

---

# 🔮 Future Directions

Potential extensions include:

* Advanced legal knowledge graphs
* More sophisticated agent planning
* Multi-hop legal reasoning
* Improved legal citation verification
* Automated legal document comparison
* Contract analysis pipelines
* Case-law semantic search
* Long-document reasoning
* Fine-tuned legal language models
* Advanced multilingual legal embeddings
* Continuous RAG evaluation
* Retrieval observability and tracing

---

# ⚠️ Disclaimer

LexiOS is an **AI-assisted legal information and research system**.

It is not a substitute for professional legal advice, legal representation, or independent verification of applicable law.

AI-generated responses should be reviewed against the underlying legal sources by a qualified legal professional before being relied upon for legal decisions.

---

# 📜 License & Intellectual Property

**Copyright © 2026 Erij Kacem. All rights reserved.**

This project was developed for academic and educational purposes as part of an engineering project.

The source code is shared for educational, research, and portfolio demonstration purposes.

The legal documents, datasets, and other third-party materials used during development may be subject to their respective copyrights and licenses and are not necessarily included or redistributable with this repository.

Unless explicitly authorized:

* The source code may not be redistributed or commercially reused.
* The architecture and implementation may not be reproduced as a commercial product.
* The legal corpus and source documents are not licensed for redistribution through this repository.
* Third-party libraries, models, frameworks, and datasets remain subject to their respective licenses.
* Legal documents and materials originating from external sources remain subject to their original copyright and usage restrictions.

For permission regarding reuse, modification, redistribution, or commercial deployment, contact the project authors.

---

# 👥 Project

**LexiOS — Agentic AI & RAG Legal Platform**

Developed by:

* **Erij Kacem**


Legal domain collaboration:

**Cabinet Hila Ben Arbia**

---

## ⭐ Technical Keywords

```text
Agentic AI · LLM Agents · RAG · Hybrid RAG · LightRAG
BM25 · Dense Retrieval · BGE-M3 · ChromaDB
Tool Calling · NLP · OCR · Surya · Marker
RAGAS · Hallucination Guard · Legal AI
FastAPI · Python · PostgreSQL · Angular · TypeScript
Docker · REST APIs · Multilingual AI
Arabic · French · English · Tunisian Law
```
