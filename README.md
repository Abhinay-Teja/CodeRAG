🚀 CodeRAG: A Lightweight Retrieval-Augmented Code Intelligence System

A synthetic-data–driven RAG pipeline for code understanding and Text-to-SQL generation

CodeRAG is a compact but fully functional Retrieval-Augmented Generation (RAG) system built entirely on synthetic data created by us.
Inspired by the paper “Towards a Generalist Code Embedding Model Based on Massive Data Synthesis”, this project explores whether meaningful code embeddings and retrieval capabilities can be learned without large datasets or GPUs.

┌──────────────────────────────────────────────────────────┐
│                    🧬 SYNTHETIC DATA                     │
│      Generated using Qwen2.5-Coder-32B                   │
│  • Text→Code   • Code→Text   • Code→Code   • Hybrid      │
└───────────────┬──────────────────────────────────────────┘
                ▼
┌──────────────────────────────────────────────────────────┐
│               🎯 ENCODER FINE-TUNING                     │
│   Bi-Encoder: BGE-Code-Large-EN                          │
│   Loss: Multiple Negatives Ranking Loss                  │
│   Script: train_full_encoder.py                          │
└───────────────┬──────────────────────────────────────────┘
                ▼
┌──────────────────────────────────────────────────────────┐
│        🗂  CORPUS PROCESSING & FAISS INDEXING           │
│   Dataset: gretelai/synthetic_text_to_sql                │
│   Script: vectorbase.py                                  │
│   Output: index + corpus map                             │
└───────────────┬──────────────────────────────────────────┘
                ▼
┌──────────────────────────┐
│  🔍 QUERY EMBEDDING      │
│  (Fine-Tuned Encoder)    │
└───────────────┬──────────┘
                ▼
┌──────────────────────────┐
│ 📚 FAISS VECTOR STORE    │
│   Top-K Retrieval        │
└───────────────┬──────────┘
                ▼
┌──────────────────────────────────────────────────────────┐
│            🧩 RAG PROMPT CONSTRUCTION                    │
│  Query + Retrieved SQL + Prompt Template                 │
└───────────────┬──────────────────────────────────────────┘
                ▼
┌──────────────────────────────────────────────────────────┐
│                🤖 LLM GENERATION                         │
│     Mistral-7B-Instruct → Final SQL Output               │
│     Script: rag_query.py                                 │
└──────────────────────────────────────────────────────────┘

🧭 How It Works

User enters a natural-language query (e.g., “Get all customers with orders > 50”).

Query is embedded using the fine-tuned bi-encoder.

FAISS retrieves the most relevant SQL examples.

Retrieved context + query are passed to Mistral-7B.

Final SQL query is generated.

🙌 Acknowledgements

This project is inspired by:
Towards a Generalist Code Embedding Model Based on Massive Data Synthesis (2024)
