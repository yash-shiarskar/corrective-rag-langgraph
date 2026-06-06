# Corrective Retrieval-Augmented Generation (CRAG)

Implementation of **Corrective Retrieval-Augmented Generation (CRAG)** using LangGraph.

This project explores the ideas introduced in the CRAG research paper and demonstrates how retrieval quality can be evaluated and corrected before generating a final response. Unlike traditional RAG systems that blindly trust retrieved documents, CRAG introduces mechanisms to assess retrieval quality, refine knowledge, leverage external search, and handle ambiguous retrieval results.

---

# Overview

Traditional RAG follows a simple workflow:

```text
Query → Retrieve → Generate
```

A major limitation of this approach is that the LLM assumes the retrieved documents are relevant and trustworthy. If retrieval quality is poor, the generated answer is also likely to be incorrect.

CRAG addresses this problem by introducing a retrieval evaluation and correction layer:

```text
Query
  ↓
Retriever
  ↓
Retrieval Evaluator
  ↓
Correct / Ambiguous / Incorrect
  ↓
Knowledge Refinement
  ↓
Generate
```

---

# Features

## Retrieval Evaluation

Evaluates retrieved documents before they are sent to the generator.

The evaluator assigns confidence scores and classifies retrieval quality into:

- Correct Retrieval
- Ambiguous Retrieval
- Incorrect Retrieval

This prevents the LLM from blindly trusting poor retrieval results.

---

## Knowledge Refinement

Retrieved documents often contain irrelevant information due to chunking.

Knowledge Refinement:

- Decomposes documents into smaller knowledge units
- Filters noisy and irrelevant content
- Retains only query-relevant information
- Produces cleaner context for generation

---

## Query Rewriting

User queries are rewritten into search-engine-friendly queries.

Benefits:

- Improves retrieval quality
- Improves web search quality
- Produces more focused search results

---

## Web Search Integration

When internal retrieval quality is poor, the system can leverage external knowledge using Tavily Search.

Workflow:

```text
Incorrect Retrieval
        ↓
Query Rewrite
        ↓
Web Search
        ↓
Knowledge Refinement
        ↓
Generate
```

---

## Ambiguity Handling

When retrieval confidence is uncertain, both internal and external knowledge sources are combined.

Workflow:

```text
Internal Documents
        +
External Documents
        ↓
Knowledge Refinement
        ↓
Generate
```

This helps answer questions where internal knowledge alone is insufficient.

---

# Architecture

```text
User Query
     ↓
 Retriever
     ↓
 Retrieval Evaluator
     ↓
 ┌─────────────┬──────────────┬──────────────┐
 │             │              │
Correct    Ambiguous     Incorrect
 │             │              │
 ↓             ↓              ↓
Knowledge   Internal +    Query Rewrite
Refinement  External          ↓
 │         Knowledge      Web Search
 │             │              ↓
 └─────────────┴──────────────┘
               ↓
      Knowledge Refinement
               ↓
          Generator
               ↓
         Final Answer
```

---

# Project Structure

```text
corrective-rag/
│
├── graph.py
├── nodes/
│   ├── retrieve.py
│   ├── retrieval_evaluator.py
│   ├── query_rewrite.py
│   ├── web_search.py
│   ├── knowledge_refinement.py
│   └── generate.py
│
├── prompts/
│
├── notebooks/
│
├── requirements.txt
│
└── README.md
```

---

# Tech Stack

- LangGraph
- LangChain
- OpenAI
- Tavily Search
- Vector Databases
- Python

---

# Learning Objectives

This project was built to understand:

- Advanced Retrieval-Augmented Generation (RAG)
- Corrective Retrieval-Augmented Generation (CRAG)
- Retrieval Quality Assessment
- Knowledge Refinement Strategies
- Query Rewriting Techniques
- External Knowledge Augmentation
- Conditional Workflows in LangGraph
- Agentic RAG Architectures

---

# Research Paper

This implementation is inspired by:

**Corrective Retrieval Augmented Generation (CRAG)**

Authors:
- Shi-Qi Yan
- Jia-Chen Gu
- Yun Zhu
- Zhen-Hua Ling

Paper:
https://arxiv.org/abs/2401.15884

---

# Future Improvements

- Fine-Tuned Retrieval Evaluator
- Hybrid Search (BM25 + Vector Search)
- Reranking Models
- Multi-Query Retrieval
- Self-Reflective RAG
- Human-in-the-Loop Evaluation
- Production Observability
- Advanced Agentic Retrieval Pipelines

---

# Repository Goals

The goal of this repository is not only to reproduce CRAG concepts but also to provide a clear educational implementation that demonstrates how modern RAG systems can evaluate, correct, and refine retrieved knowledge before generation.

By implementing each component step-by-step, this repository serves as both a learning resource and a practical reference for building advanced retrieval systems using LangGraph.
