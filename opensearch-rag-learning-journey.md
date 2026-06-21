# OpenSearch + Vector Search + RAG Learning Journey

## Objective

Learn and implement an end-to-end AI Knowledge Assistant using:

* OpenSearch
* Vector Search
* Embeddings
* Semantic Search
* Ollama (Local LLM)
* Retrieval Augmented Generation (RAG)

The goal was not only to make it work but also to understand each component from first principles.

---

# Phase 1 - Understanding the Problem

Traditional keyword search has limitations.

Example:

Query:

How do I recover an unhealthy OpenShift node?

Document:

ROSA Worker Node Recovery Guide

Keyword search may fail because the exact words are different.

We need a system that understands meaning rather than exact words.

This is where embeddings and semantic search help.

---

# Phase 2 - Core Concepts

## Embedding

Embedding converts text into numerical coordinates.

Example:

Text:
"ROSA Worker Node Recovery"

Embedding:

[0.12, -0.44, 0.67, ...]

A machine cannot understand text directly.

It understands numbers.

Embeddings allow meaning to be represented mathematically.

---

## Vector

The embedding output is called a vector.

Example:

[0.12, -0.44, 0.67, ...]

Each document becomes a vector.

Documents with similar meanings are stored close together in vector space.

---

## Vector Database

A vector database stores embeddings.

Responsibilities:

* Store vectors
* Find similar vectors
* Return nearest matches quickly

In this project:

OpenSearch acts as the vector database.

---

## Semantic Search

Semantic Search = Meaning-based search.

Instead of matching keywords:

Search:
"How do I recover an unhealthy OpenShift node?"

Can find:

"ROSA Worker Node Recovery"

because the meaning is similar.

---

## LLM

Large Language Model

Examples:

* Llama
* Mistral
* DeepSeek
* GPT

The LLM generates answers.

However, it does not know company-specific information.

---

## RAG

Retrieval Augmented Generation

Flow:

Question
→ Retrieve Documents
→ Add Context
→ Generate Answer

This allows AI to answer using company documents.

---

# Phase 3 - Environment Setup

Machine:

* MacOS 12.7.6
* 8 GB RAM
* Intel Core i5
* Docker Installed

---

# Phase 4 - Install OpenSearch

Docker Compose used.

Services:

1. OpenSearch
2. OpenSearch Dashboards

Verification:

curl -k -u admin:<password> https://localhost:9200

Expected result:

Cluster information returned.

---

# Phase 5 - Verify KNN Plugin

Command:

curl -k -u admin:<password> 
https://localhost:9200/_plugins/_knn/stats?pretty

Purpose:

Verify vector search capability is enabled.

---

# Phase 6 - Create Python Environment

Create virtual environment:

python3 -m venv venv

Activate:

source venv/bin/activate

Install dependencies:

pip install opensearch-py
pip install sentence-transformers
pip install torch

Verification:

python -c "from sentence_transformers import SentenceTransformer; print('OK')"

Expected:

OK

---

# Phase 7 - Create Embeddings

Model:

all-MiniLM-L6-v2

Example:

text

↓

embedding model

↓

vector

Code:

embedding = model.encode(text)

Result:

384-dimensional vector

Purpose:

Convert meaning into numbers.

---

# Phase 8 - Create OpenSearch Index

Created index:

techcorp_docs

Added fields:

* content
* filename
* embedding

Purpose:

Store documents and vectors.

---

# Phase 9 - Document Ingestion

Created markdown documents:

* rosa.md
* aws.md
* filenet.md

Process:

Read document

↓

Generate embedding

↓

Store in OpenSearch

Verification:

curl -k -u admin:<password> 
https://localhost:9200/techcorp_docs/_count

Expected:

Document count displayed.

---

# Phase 10 - Semantic Search

Question:

How do I recover an unhealthy OpenShift node?

Process:

Question

↓

Embedding

↓

Vector Search

↓

Nearest Documents

Result:

ROSA Worker Node Recovery document returned.

Observation:

Semantic similarity worked.

No keyword matching required.

---

# Phase 11 - Install Ollama

Install:

brew install ollama

Run:

ollama serve

Pull model:

ollama run llama3.2:3b

Verification:

Ask:

What is OpenSearch?

Model responds successfully.

---

# Phase 12 - Build RAG Pipeline

Architecture:

Question
↓
Embedding
↓
OpenSearch Vector Search
↓
Relevant Documents
↓
Ollama
↓
Answer

This is the complete RAG workflow.

---

# Phase 13 - RAG Test

Question:

Tell me about AWS

Process:

1. Generate question embedding
2. Search OpenSearch
3. Retrieve relevant document
4. Build context
5. Send context to Ollama
6. Generate answer

Result:

Answer generated using retrieved documents.

---

# Key Learning

## Sequential Flow: From Text to Answer

The complete RAG pipeline follows this sequential process:

### Step 1: TEXT
Raw input from user or document.

### Step 2: EMBEDDING
Text is converted to numerical representation using a machine learning model.

**Action**: Pass text through `SentenceTransformer` or similar embedding model.

**Output**: 384-dimensional vector (in our case with all-MiniLM-L6-v2)

### Step 3: VECTOR
The output embedding is stored as a vector in vector space.

**Action**: Store in OpenSearch with vector field type.

**Benefit**: Similar documents are positioned close together in mathematical space.

### Step 4: OPENSEARCH
Vector database stores and indexes all embeddings.

**Action**: Perform approximate nearest neighbor (ANN) search using KNN plugin.

**Benefit**: Fast retrieval of similar vectors without scanning all documents.

### Step 5: SEMANTIC SEARCH
Query is converted to embedding and compared against stored vectors.

**Action**: Generate embedding for user query using same model.

**Result**: Find most similar documents by cosine similarity or distance metrics.

**Benefit**: Finds meaning-based matches, not keyword matches.

### Step 6: CONTEXT
Retrieved documents are combined with the original query.

**Action**: Format: "Context from documents: [retrieved_text]\n\nOriginal Question: [query]"

**Benefit**: Provides LLM with relevant information to answer accurately.

### Step 7: LLM
Language model processes the context and generates answer.

**Action**: Send prompt with context to Ollama (or other LLM).

**Models available**: Llama, Mistral, DeepSeek, etc.

**Benefit**: Generates human-like responses based on company-specific documents.

### Step 8: ANSWER
Final response generated using retrieved knowledge.

**Output**: Answer grounded in retrieved documents, not hallucination.

**Benefit**: Company information stays private; answers are traceable to sources.

---

**This is the foundation of modern enterprise GenAI systems.**

---

# Enterprise Use Cases

## ROSA Assistant

Question:

How do I recover a failed worker node?

Sources:

* Runbooks
* SOPs
* KB Articles

---

## FileNet Assistant

Question:

How do I troubleshoot CPE startup failures?

Sources:

* Support guides
* Operational runbooks

---

## OpenSearch Assistant

Question:

Why is cluster health yellow?

Sources:

* OpenSearch documentation
* Internal operational procedures

---

# Lessons Learned

1. Embeddings convert meaning into numbers.
2. Vectors represent semantic meaning.
3. OpenSearch can act as a vector database.
4. Semantic search finds meaning, not keywords.
5. LLMs generate answers.
6. RAG combines retrieval with generation.
7. OpenSearch + Ollama can create a complete local GenAI platform.

---

# Next Steps

* Add more documents
* Improve chunking strategy
* Add metadata filtering
* Add source citations
* Experiment with hybrid search
* Learn LangChain
* Learn OpenSearch ML Commons
* Build enterprise knowledge assistant

---

Author: Ankur Prakash

Date: June 2026
