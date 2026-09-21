# 🚀 Day 15: Building an End-to-End RAG Pipeline

This repository contains the complete implementation of a **Retrieval-Augmented Generation (RAG)** pipeline connecting a dense vector search engine (FAISS) to an LLM interface, designed to eliminate hallucinations by grounding responses in private context.

---

## 📌 Project Overview & Objectives

- **Custom Knowledge Base:** Curated 15 proprietary facts for a fictional company (*AetherOps*) with zero public web footprint to rigorously test parametric hallucination.
- **Dense Vector Retrieval:** Embedded chunks using sentence-transformers (`all-MiniLM-L6-v2`, 384 dimensions) and indexed them into a FAISS `IndexFlatIP` vector store.
- **RAG vs Non-RAG Evaluation:** Built `generate_with_rag()` and `generate_without_rag()` functions and benchmarked 5 test queries side by side.
- **Failure Mode Diagnosis:** Traced and documented two fundamental RAG failure patterns (Retrieval Miss vs. Generation Drift).
- **System Architecture:** Visualized the full query-to-generation pipeline data flow.

---

## 🔬 Benchmark: With RAG vs Without RAG (Hallucination Test)

| # | Test Query | With RAG (Grounded Ground Truth) | Without RAG (Parametric Hallucination) |
|---|------------|----------------------------------|---------------------------------------|
| 1 | **Flagship Product & Use** | **ChronoMesh-v4**, engineered for quantum-resistant data streaming. | Hallucinated a generic Kubernetes CI/CD automation tool. |
| 2 | **Founders & Origin** | Founded in **2024 by Dr. Elena Vance & Marcus Thorne** in Reykjavik, Iceland. | Hallucinated ex-Google engineers in Austin, Texas (2021). |
| 3 | **Learning Allowance** | Exact **$6,500 annual allowance** with **14 days** of dedicated study leave. | Guessed standard industry stipend ($1,500/year). |
| 4 | **Project Borealis** | Submarine data center initiative cooled by **geothermal ocean currents**. | Fabricated an atmospheric solar radiation balloon project. |
| 5 | **Enterprise Pricing** | Exactly **$120,000 per rack node per year** with 24/7 SRE support. | Fabricated generic SaaS pricing ($49/user/month). |

---

## 🧩 Tracing Real-World RAG Failure Modes

1. **Retrieval Miss (Semantic Misalignment):**
   - *Symptom:* The vector search fails to pull the ground-truth chunk into the top-$k$ context because colloquial query phrasing lacks vector alignment with domain-specific document tokens.
   - *Solution:* Implement Hybrid Search (BM25 keyword matching + Dense FAISS) and Hypothetical Document Embeddings (HyDE).

2. **Generation Drift (Context Distraction):**
   - *Symptom:* When context contains multiple numeric values or overlapping entities, the LLM misbinds attributes (e.g., confusing a bug bounty reward with an office equipment stipend).
   - *Solution:* Re-rank candidate chunks using Cross-Encoders and enforce strict inline source citation in the prompt instructions.

---

## 📐 System Architecture Diagram
