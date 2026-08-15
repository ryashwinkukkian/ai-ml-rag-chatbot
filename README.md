
# Domain-Specific RAG Chatbot

A chatbot that answers questions from a custom document corpus using retrieval-augmented
generation, with source citations and honest refusal when a question falls outside the corpus.

## Corpus

`docs/sample_corpus.txt` — Jonathan Swift's *A Modest Proposal* and surrounding essays
(public domain, sourced from Project Gutenberg). Chosen as a free, no-auth stand-in corpus;
swap in your own PDFs/text files in `docs/` to point this at any domain.

## How it works

1. Documents in `docs/` are chunked into ~500-word pieces with 50-word overlap
2. Chunks are embedded locally with `sentence-transformers` (all-MiniLM-L6-v2) — no API key needed for this step
3. Embeddings are stored in a persistent Chroma vector database
4. On each question, the top-5 most similar chunks are retrieved
5. If the best match is still too dissimilar (distance > 0.6), the bot refuses rather than guessing
6. Otherwise, the chunks are stuffed into a prompt sent to Llama 3.1 8B via Groq's free API, and the answer is returned with cited sources

## Setup

```bash
git clone https://github.com/Samay8/domain-rag-chatbot.git
cd domain-rag-chatbot
python -m pip install -r requirements.txt
```
