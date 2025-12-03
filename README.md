# llm-agents

A workspace for experimenting with GenAI tooling (LangChain, OpenAI, vector search) using the course content stored in the `Module 1`, `Module 2`, `Module 4`, and `Module 5` folders. The `agents` folder holds the ingestion script and notebooks that turn the course material into text for downstream workflows.

## Repository layout
- `agents/ingestion.py` — helper that walks the module folders and extracts text from `.pdf`, `.docx`, and `.pptx` files using `pymupdf4llm`, `docx2txt`, and `python-pptx`.
- `agents/langchain_agents.ipynb` — notebook for interactive agent experiments.
- `Module 1`, `Module 2`, `Module 4`, `Module 5` — source course materials (presentations, docs, images) used by the ingestion step.
- `requirements.txt`, `pyproject.toml`, `uv.lock` — Python dependencies managed with `uv` but also compatible with `pip`.
- `.env` — API keys and service endpoints (not committed; create your own).

## Prerequisites
- Python 3.12+
- Optional: [`uv`](https://docs.astral.sh/uv/) for fast installs (or use `pip`)
- System deps if you plan to run the full PDF/OCR stack:
  - Poppler (`pdf2image` backend; e.g., `brew install poppler` on macOS)
  - Tesseract OCR (`pytesseract`; e.g., `brew install tesseract`)

## Setup
```bash
# create and activate a virtualenv (uv shown; pip/venv also works)
uv venv
source .venv/bin/activate

# install Python dependencies
uv pip install -r requirements.txt

# ingestion extras not pinned in requirements.txt
uv pip install docx2txt pymupdf4llm
```

Configure your secrets in `.env` (place in repo root). Expected keys:
```
OPENAI_API_KEY=
GOOGLE_API_KEY=
TAVILY_API_KEY=
VOYAGE_API_KEY=
ZILLIZ_API_KEY=
ZILLIZ_CLOUD_URI=
ZILLIZ_COLLECTION_NAME=
ZILLIZ_TOKEN=
LLAMA_API_KEY=
```

## Using the ingestion helper
```bash
python - <<'PY'
from agents.ingestion import load_documents
docs = load_documents(".")
print(f"Loaded {len(docs)} documents")
print(docs[0]["file"])
PY
```
This extracts text from the module folders into a list of `{file, text}` entries you can feed into embedding or vector-store pipelines.

## Working in the notebook
- Launch Jupyter: `jupyter lab` (or `uv run jupyter lab`).
- Open `agents/langchain_agents.ipynb` and ensure your `.env` is loaded so the notebook can reach external LLM/vector services.

## Notes
- The module folders contain the raw course assets; the repo does not include generated embeddings or vector indexes.
- If you add new modules or file types, update `agents/ingestion.py` to handle them before re-running ingestion.
