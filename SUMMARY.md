What is an LLM?
A Large Language Model is a neural network trained to predict the next token in text. With enough data + compute, this simple objective yields strong capabilities in writing, reasoning, and following instructions.

Tokenization
Text is split into sub-word units called tokens (e.g., “fantastic” → fan + tastic).
Models operate on tokens; all limits and pricing are in tokens.
Tokenizers differ by model family; for OpenAI models, tiktoken is commonly used.
Rule of thumb: English tokens ≈ ¾ words on average.

Embeddings
An embedding turns text into a fixed-length numeric vector that captures meaning.
Semantically similar texts → nearby vectors.
Use cases: search, clustering, deduplication, recommendation, and RAG retrieval.

Context windows
The maximum number of tokens a model can read + write in one request.
If input + output > window, you must shorten, summarize, or chunk the text.
Larger windows allow “stuffing” more context but are slower and cost more.

Fine-tuning vs RAG
Fine-tuning adapts the model weights with labeled examples.
Pros: bakes patterns into the model; great for style or structured outputs.
Cons: requires curated data, costs training time/money; not ideal for rapidly changing knowledge.
RAG (Retrieval-Augmented Generation) fetches relevant documents at query time and supplies them as context to a prompt.
Pros: instant updates by re-indexing docs; sources are inspectable; cheaper and simpler than fine-tuning for knowledge tasks.
Cons: depends on retrieval quality; context window limits still apply.
In practice: start with RAG for knowledge problems; consider fine-tuning for formatting, tone, or narrow tasks after you’ve validated prompts.

Minimal RAG flow (what we built):
Ingest: Load files (e.g., data/sample.txt).
Split: Chunk long text (e.g., 400–800 tokens with overlap).
Embed: Convert chunks → vectors (OpenAI embeddings).
Index: Store vectors in a vector DB (FAISS).
Retrieve: For a query, find top-k similar chunks.
Generate: Give the chunks + question to the LLM to answer with citations.

Tokens & cost
Requests are billed on input and output tokens separately.
Example (as of Sep 2025): gpt-4.1-mini ≈ $0.15 /M input tokens and $0.60 /M output tokens.
Always estimate cost = (tokens / 1e6) * rate. We implemented count_tokens(...) and estimate_cost(...) in the notebook.

Prompting tips for RAG
Tell the model to use provided context and when to say “I don’t know.”
Limit output length; ask for bullet points; request citations.
Retrieve more than you need (e.g., k=4–8) but keep the final prompt under the context limit.
Quality checks

Evaluate retrieval (does the right chunk show up?).
Spot-check answers and citations.
Log queries, retrieved chunk IDs, and latency.

Reproducible environment
Python 3.11+, virtualenv .venv, requirements.txt, and pre-commit with black, ruff, isort.
Store secrets in .env and keep it out of git via .gitignore.