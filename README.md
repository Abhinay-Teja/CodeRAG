# 🚀 CodeRAG: A Lightweight Retrieval-Augmented Code Intelligence System

A synthetic-data driven RAG pipeline for code understanding and Text-to-SQL generation.

CodeRAG is a compact but fully functional Retrieval-Augmented Generation (RAG) system built entirely on synthetic data created by us. Inspired by the paper “Towards a Generalist Code Embedding Model Based on Massive Data Synthesis”, this project explores whether meaningful code embeddings and retrieval capabilities can be learned without large datasets or GPUs.

---

## 🎯 RAG Pipeline Components

We built a modular, seven-stage RAG pipeline, detailed below:

### 1. 🧬 Synthetic Data Generation

| Component | Details |
| :--- | :--- |
| **Generator** | Qwen2.5-Coder-32B (for hardware efficiency) |
| **Task Categories** | • Text→Code • Code→Text • Code→Code • Hybrid |
| **Purpose** | To learn rich semantic representations without large human-labeled corpora. |

### 2. 🤖 Encoder Fine-Tuning

| Component | Details |
| :--- | :--- |
| **Bi-Encoder** | BGE-Code-Large-EN |
| **Loss Function** | Multiple Negatives Ranking Loss (Contrastive Learning) |
| **Script** | `train_full_encoder.py` |

### 3. 🗂 Corpus Processing & Indexing

| Component | Details |
| :--- | :--- |
| **Corpus Source** | `gretelai/synthetic_text_to_sql` (Schema Snippets) |
| **Indexing Tool** | FAISS (IndexFlat IP for Cosine Similarity) |
| **Output** | `sql_rag_faiss_index.bin` + `sql_rag_corpus_map.npy` |
| **Script** | `vectorbase.py` |

### 4. 🔍 Retrieval Stage (Inference)

| Component | Details |
| :--- | :--- |
| **Query Embedding** | Uses the **Fine-Tuned Encoder** |
| **Vector Store** | **FAISS** (Top-K Retrieval) |
| **RAG Prompt** | **Query + Retrieved Context + Prompt Template** |

### 5. 💡 Generation Stage

| Component | Details |
| :--- | :--- |
| **Large Language Model** | Mistral-7B-Instruct-v0.2 |
| **Output** | Final SQL Query |
| **Script** | `rag_query.py` |

---

## 🧭 How It Works (Execution Flow)

1.  User enters a natural-language query (e.g., “Get all customers with orders > 50”).
2.  The query is embedded using the **fine-tuned bi-encoder**.
3.  **FAISS** retrieves the **Top-K most relevant schema snippets** (context).
4.  The retrieved context and the original query are passed to **Mistral-7B**.
5.  The **Final SQL query** is generated, grounded by the retrieved schema.

---

## ✅ Evaluation & Status

* **Metric:** Exact Match (EM) and Execution Accuracy (EX) on a complex Text-to-SQL evaluation dataset.
* **Status:** All pipeline components are **functionally validated**.

---

## 🙌 Acknowledgements

This project is inspired by the paper: *Towards a Generalist Code Embedding Model Based on Massive Data Synthesis* (2024).

---
