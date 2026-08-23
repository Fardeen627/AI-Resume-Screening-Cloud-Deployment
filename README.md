---
title: AI Resume Screening
emoji: 📄
colorFrom: blue
colorTo: indigo
sdk: streamlit
sdk_version: "1.34.0"
app_file: app.py
pinned: false
---

# AI Resume Screening & RAG Pipeline (Deployment Version)

This is a deployment-ready copy of the original [ai-resume-screening](https://github.com/Fardeen627/ai-resume-screening)
project, adapted to run on Hugging Face Spaces (or any host without a local Ollama server).

**The only functional change from the original project is the LLM backend.** The original app called a local
Ollama server (`llama3.2:3b`). This version calls a cloud LLM through an OpenAI-compatible API instead
(defaults to Groq's free tier). Everything else — retrieval logic, RAG Fusion, FAISS vector search, the
Streamlit UI, resume uploading, and streaming responses — is unchanged.

## Setup

1. Copy `data/main-data/` and `vectorstore-synthetic/` from the original repository into this folder
   (see the `data/main-data/README_PLACEHOLDER.txt` and `vectorstore-synthetic/README_PLACEHOLDER.txt`
   notes for what's expected).
2. Copy `.env.example` to `.env` and fill in `LLM_API_KEY` (or set it as a Space secret, see below).
3. `pip install -r requirements.txt`
4. `streamlit run app.py`

## Hugging Face Spaces deployment

1. Create a new Space, SDK = Streamlit.
2. Push this folder's contents to the Space repo root (`app.py` must be at the root, matching `app_file` above).
3. In the Space's **Settings → Variables and secrets**, add:
   - `LLM_API_KEY` (secret) — your Groq API key
   - `LLM_MODEL` (variable, optional) — defaults to `llama-3.1-8b-instant`
   - `LLM_BASE_URL` (variable, optional) — defaults to `https://api.groq.com/openai/v1`
   - `DATA_PATH` (variable) — `./data/main-data/synthetic-resumes.csv`
   - `FAISS_PATH` (variable) — `./vectorstore-synthetic`
   - `EMBEDDING_MODEL` (variable) — `sentence-transformers/all-MiniLM-L6-v2`
4. Make sure `data/main-data/synthetic-resumes.csv` and the `vectorstore-synthetic/` FAISS index files are
   committed to the Space repo (Space repos support Git LFS for larger files if needed).
5. The Space will build and launch `app.py` automatically.
