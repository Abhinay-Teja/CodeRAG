🚀 CodeRAG: A Lightweight Retrieval-Augmented Code Intelligence System

A synthetic-data–driven RAG pipeline for code understanding and Text-to-SQL generation

CodeRAG is a compact but fully functional Retrieval-Augmented Generation (RAG) system built entirely on synthetic data created by us.
Inspired by the paper “Towards a Generalist Code Embedding Model Based on Massive Data Synthesis”, this project explores whether meaningful code embeddings and retrieval capabilities can be learned without large datasets or GPUs.

Despite running entirely on personal laptops, we designed and implemented a complete pipeline:

🌟 Project Highlights
✅ 1. Synthetic Dataset Generation

We generated four types of supervised code transformation tasks using a local LLM
(Qwen2.5-Coder-32B-Instruct-Q5_K_M.gguf):

Text-to-Code

Code-to-Text

Code-to-Code

Hybrid Transformations

From these, we curated a 40-sample high-quality dataset for training our encoder.

✅ 2. Fine-Tuned Code Embedding Encoder

We fine-tuned a SentenceTransformer Bi-Encoder using contrastive learning (Multiple Negatives Ranking Loss).

Base model: BAAI/bge-code-large-en

Training script: train_full_encoder.py

Output directory: CodeRAG_encoder/

The encoder learns to map natural language queries and code snippets into a shared semantic space.

✅ 3. Retrieval Index Creation (FAISS)

Using the gretelai/synthetic_text_to_sql dataset, we built a retrieval corpus for Text-to-SQL evaluation:

Embeddings stored using the fine-tuned encoder

FAISS index: sql_rag_faiss_index.bin

Mapping file: sql_rag_corpus_map.npy

Indexing handled by: vectorbase.py.

✅ 4. RAG Pipeline for Text-to-SQL

The final pipeline retrieves relevant SQL examples and feeds them to a generation model:

Retrieval using our encoder + FAISS

Generation with Mistral-7B-Instruct-v0.2

Pipeline script: rag_query.py

RAG improves grounding, reduces hallucination, and stabilizes SQL generation.

🎯 Objective

To evaluate whether a small, synthetic dataset can train a useful code retriever that improves downstream Text-to-SQL generation in a RAG setup — even under severe hardware limitations.

📉 Limitations

Running Mistral-7B on CPU hardware made full benchmarking slow, so the complete 92-query evaluation set could not be executed within the project timeline.
However, qualitative tests show clear improvements in stability and correctness when retrieval is used.

📂 Repository Structure
├── CodeRAG_Dataset.csv          # 40 synthetic training samples
├── CodeRAG_encoder/             # Fine-tuned bi-encoder model
├── train_full_encoder.py        # Encoder training script
├── vectorbase.py                # Corpus indexing and FAISS builder
├── rag_query.py                 # End-to-end RAG pipeline
├── sql_rag_faiss_index.bin      # FAISS index
├── sql_rag_corpus_map.npy       # Mapping of index → text
├── rag_evaluation_metrics.py    # Evaluation Script 
└── README.md                    # Project documentation

🧭 How It Works

User enters a natural-language query (e.g., “Get all customers with orders > 50”).

Query is embedded using the fine-tuned bi-encoder.

FAISS retrieves the most relevant SQL examples.

Retrieved context + query are passed to Mistral-7B.

Final SQL query is generated.

🙌 Acknowledgements

This project is inspired by:
Towards a Generalist Code Embedding Model Based on Massive Data Synthesis (2024)
