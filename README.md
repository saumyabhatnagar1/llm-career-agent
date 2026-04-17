---
title: career-conversation
app_file: agent.py
sdk: gradio
sdk_version: 6.12.0
---

# llm-career-agent

A small **Gradio** chat app that answers career questions in character using **OpenAI** (`gpt-4o-mini`). Context comes from local files under `me/` (summary text, LinkedIn export, resume). Optional **Pushover** notifications fire when users leave contact details or when the model records an unknown question via tool calls.

## Requirements

- Python 3.10+ (tested with 3.10)
- Dependencies: see [`requirements.txt`](requirements.txt)

## Setup

```bash
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

Create a `.env` file in the project root (see [`.gitignore`](.gitignore)—do not commit secrets):

| Variable | Purpose |
|----------|---------|
| `OPENAI_API_KEY` | OpenAI API key for chat completions |
| `PUSHOVER_USER` | Pushover user key (optional; omit both Pushover vars to skip notifications) |
| `PUSHOVER_TOKEN` | Pushover application token |

## Local data (`me/`)

Place these files next to the app (paths are fixed in code):

- `me/summary.txt` — short bio / background text
- `me/linkedin.pdf` — LinkedIn profile PDF export
- `me/resume.pdf` — resume PDF

## Run

```bash
python agent.py
```

Gradio serves the chat UI (default URL shown in the terminal, usually `http://127.0.0.1:7860`).

## How it works

- [`agent.py`](agent.py) builds a system prompt from `me/` files and runs a chat loop with function tools defined in [`constant.py`](constant.py): `record_user_details` and `record_unknown_question`.
- Tool results are sent back to the model until it returns a normal assistant message.

## Hugging Face Spaces

The YAML header at the top of this file is for [Gradio Spaces](https://huggingface.co/docs/hub/spaces-sdks-gradio). Configure secrets in the Space for `OPENAI_API_KEY` and Pushover if used; upload or bind the `me/` assets as your deployment requires.
