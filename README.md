# Insurellm RAG — Expert Knowledge Worker

A Retrieval-Augmented Generation (RAG) assistant for **Insurellm**, a fictional insurance technology company. Employees can ask questions about products, contracts, employees, and company information. Answers are grounded in an internal markdown knowledge base.

This project is extracted from Week 5 of the [LLM Engineering](https://github.com/ed-donner/llm_engineering) course as a standalone repository.

## What it does

- Ingests markdown documents from `knowledge-base/` into a Chroma vector store
- Retrieves relevant context for user questions
- Generates answers with an OpenAI chat model (configurable)
- Optional **pro implementation** with query rewriting, reranking, and LLM-based semantic chunking
- Evaluation harness with retrieval metrics (MRR, nDCG, keyword coverage) and LLM-as-judge answer scoring

## Project structure

```
insurellm-rag/
├── app.py                    # Gradio chat UI (basic implementation)
├── evaluator.py              # Gradio UI for running evaluations
├── day1.ipynb … day5.ipynb   # Course notebooks (step-by-step build)
├── knowledge-base/           # Source documents (company, contracts, employees, products)
├── implementation/           # Basic RAG: ingest + answer
│   ├── ingest.py
│   └── answer.py
├── pro_implementation/       # Advanced RAG: semantic chunks, rewrite, rerank
│   ├── ingest.py
│   └── answer.py
├── evaluation/               # Test suite and metrics
│   ├── eval.py
│   ├── test.py
│   └── tests.jsonl
└── EVALUATION_IMPROVEMENTS.md
```

Generated at runtime (not in git):

- `vector_db/` — Chroma store for `implementation/`
- `preprocessed_db/` — Chroma store for `pro_implementation/`

## Requirements

- Python 3.11+
- An [OpenAI API key](https://platform.openai.com/api-keys) (used for chat, embeddings, and evaluation)

## Setup

1. Create and activate a virtual environment:

   ```bash
   python -m venv .venv
   source .venv/bin/activate   # Windows: .venv\Scripts\activate
   ```

2. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

3. Configure environment variables:

   ```bash
   cp .env.example .env
   # Edit .env and set OPENAI_API_KEY=sk-...
   ```

## Usage

### 1. Ingest the knowledge base (basic)

Build the vector database used by `implementation/`:

```bash
python -m implementation.ingest
```

This creates `vector_db/` using OpenAI `text-embedding-3-large` embeddings.

### 2. Run the chat assistant

```bash
python app.py
```

Opens a Gradio UI in your browser. The basic implementation uses HuggingFace `all-MiniLM-L6-v2` embeddings for retrieval (see `implementation/answer.py`).

### 3. Advanced RAG (optional)

Ingest with semantic LLM chunking:

```bash
python -m pro_implementation.ingest
```

Then use `pro_implementation/answer.py` in your own script or adapt `app.py` to import from there. See `EVALUATION_IMPROVEMENTS.md` for how pro features improve metrics.

### 4. Evaluate quality

```bash
python evaluator.py
```

Runs retrieval and answer evaluations against `evaluation/tests.jsonl`.

## Course notebooks

Work through the project incrementally:

| Notebook | Topic |
|----------|--------|
| `day1.ipynb` | Introduction, knowledge base, simple Q&A |
| `day2.ipynb` | Vector embeddings and similarity |
| `day3.ipynb` | Chunking and ingestion |
| `day4.ipynb` | RAG pipeline and Gradio UI |
| `day5.ipynb` | Evaluation and improvements |

Notebooks may reference images under `../assets/` from the parent course repo; those are decorative and not required to run the code.

## Knowledge base

`knowledge-base/` contains fictional Insurellm data:

- **company/** — About, culture, careers, overview
- **products/** — Bizllm, Carllm, Claimllm, Healthllm, Homellm, Lifellm, Markellm, Rellm
- **contracts/** — Partner agreements
- **employees/** — Staff profiles

## License

Course materials; use in line with the upstream [LLM Engineering](https://github.com/ed-donner/llm_engineering) repository terms.
