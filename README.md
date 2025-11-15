# rag_wikipedia-lab

This repository contains a small Retrieval-Augmented Generation (RAG) demo that builds a Wikipedia-based summarizer using LangChain, ChromaDB, and SentenceTransformers!!

The notebook performs these steps:

- Downloads text from selected Wikipedia pages (Federated learning, Differential privacy)
- Chunks the text into ~300-word segments and saves them to `/data/wiki_corpus.csv`
- Computes sentence embeddings with `all-MiniLM-L6-v2` and upserts them into a local ChromaDB collection
- Builds a LangChain retrieval pipeline with an optional local LLM via Ollama, with a Hugging Face summarizer fallback
- Produces a 400–500 word evidence-grounded summary written to `/outputs/rag_summary.md`

Deliverables
- `/notebooks/rag_wikipedia.ipynb` — the main notebook with the complete pipeline
- `/data/wiki_corpus.csv` — chunked Wikipedia corpus
- `/outputs/rag_summary.md` — final 400–500 word summary
- `/outputs/retrieval_examples.json` — example queries and retrieved snippets
- `/outputs/reflection.md` — conceptual comparison of multi-agent vs RAG

Requirements & quick setup
1. Clone this repo and create a Python environment with Python 3.10+.

Recommended: conda

```bash
conda create -n rag python=3.10 -y
conda activate rag
pip install --upgrade pip
pip install -r requirements.txt  # or pip install wikipedia-api sentence-transformers chromadb langchain transformers torch pandas numpy
```

Notes:
- LangChain is pinned to a specific release in the notebook to ensure `vectorstores.Chroma` and `RetrievalQA` APIs are available. If you see import errors, try: `pip install langchain==0.0.287`.
- For a local LLM with the LangChain Ollama wrapper, install Ollama and add a suitable model (e.g., `mistral`). To test the local model, run: `ollama list` and `ollama run mistral "Hello"`.

How to run the notebook
1. Open `notebooks/rag_wikipedia.ipynb` in VSCode or Google Colab.
2. Execute cells sequentially. The notebook is resilient: if the LangChain+Ollama path is unavailable, it falls back to a Hugging Face summarizer.

Important usage tips
- If you want the OLLAMA path to be used from Python (LangChain wrapper), you may need to set the `base_url` and `verbose=True` inside the notebook when creating the Ollama LLM, e.g.: `Ollama(model='mistral', base_url='http://127.0.0.1:11434', verbose=True)`.
- Optional: If LangChain's Ollama wrapper doesn't work for your OS/installed LangChain, use the `ollama` CLI (shell) as a fallback inside the notebook by adding a small wrapper that invokes `ollama run <model> "<prompt>"` via subprocess.

How to reproduce the deliverables
- Run the entire notebook in order; the notebook saves `data/wiki_corpus.csv`, `outputs/rag_summary.md`, and `outputs/retrieval_examples.json` to the project root.

Licenses
- This demo uses open-source tools — check each package for licensing details and the Wikipedia content is under CC BY-SA.

Contact / troubleshooting
- If you hit errors with Chroma or LangChain, ensure versions are compatible or use the notebook fallback retriever. If you have local Ollama issues, use `ollama list` and `ollama run` to verify the daemon and models are available.

Enjoy — please let me know if you want a short script to automate the full run or small tests to validate the outputs.
