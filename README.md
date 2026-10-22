# Research Paper Navigator

A capstone project that progresses from a plain LLM to an agent that searches research PDFs, reads selected pages, and answers with source citations. **Checkpoint 7.1 is the final version.**

## Setup

From the repository root (tested with Python 3.13 on macOS):

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

1. Create a root `.env` using `.env.example` as a template and add your own `OPENROUTER_API_KEY`. Keep an existing `.env` if you already have one.
2. Create a root `ResearchPapers/` folder and add PDFs you have permission to use. PDFs need selectable text; OCR is not included.

OpenRouter chat and embedding requests **use paid credits** and send questions and PDF text to the service. The papers and API key are not included in this repository.

## Run

First run — create the saved embedding index:

```bash
python Checkpoint7.1/CurrentRetriever7.py --build-index
```

Later runs — reuse that index:

```bash
python Checkpoint7.1/CurrentRetriever7.py
```

Type `exit` or `quit` to finish. Each question is independent; previous chat turns are not sent to the model. Completed sessions are saved as Markdown under `.local_data/Checkpoint7.1/conversations/`.

Optional flags: `--no-cache` bypasses saved answers, `--check-pdfs` checks whether the PDFs match the saved index, and `--help` lists options.

`--build-index` creates a missing index; it does not refresh one. If you change the PDFs or embedding model, stop the app, move `.local_data/Checkpoint7.1/chroma_db/` to a backup location, then run with `--build-index` again.

## Checkpoints

Each folder contains one standalone script, named `CurrentRetrieverN.py`.

| Checkpoint | What it adds |
| --- | --- |
| 1.1 | Plain LLM baseline, without retrieval |
| 2.1 | Keyword (BM25) and semantic search |
| 3.1 | Built-in answer evaluation |
| 4.1 | One evidence-dependent follow-up search |
| 5.1 | Agent planning, search, direct page reading, and page-ID retention |
| 6.1 | The same agent version audited for performance and cost |
| 7.1 | Instruction/evidence separation, a short-answer path, and answer caching |

Run an earlier stage with, for example, `python Checkpoint4.1/CurrentRetriever4.py --build-index`. Checkpoints 2–7 keep separate indexes in `.local_data/`; each new index incurs embedding costs. Checkpoint 1 needs no PDFs or index.

Checkpoint 3 also supports `--evaluate` to run its original five-question evaluation and LLM judge. These questions require the original course corpus, and judging uses additional credits.

## GitHub files

Include the seven checkpoint folders, `README.md`, `requirements.txt`, `.env.example`, and `.gitignore`. The ignore rules exclude real credentials, PDFs, databases, conversations, backups, and unrelated lab materials. Review staged files before committing; ignore rules do not apply to manual web uploads.

This is a learning project, not a production-ready system. Retrieved evidence can be incomplete, and citations or cached answers do not guarantee correctness. Verify important claims against the cited PDF pages.
