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

## Key Learning

The Key Learning section has been moved to a separate file: [key-learning.md](key-learning.md)

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
