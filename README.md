# 📝 Task 1: Managing Conversation History with Summarization
[Open Colab Notebook](https://colab.research.google.com/drive/1IZJy8ECge0_2eU1MOf1-Ef3MUnMemOkt?usp=sharing)

## 📌 Overview
This project implements a **Conversation History Manager** along with a **periodic summarization mechanism** using the **Groq API (OpenAI-compatible SDK)**.

It allows you to:
- Store conversation history in memory.
- Truncate the history based on different criteria (turns, characters, words).
- Automatically summarize older parts of the conversation periodically and replace them with a concise system summary message.

This is useful for building **chat-based applications** where the context window must be controlled to avoid exceeding token limits while still maintaining continuity.

---

## ⚙️ Features
- **History Management**
  - Add messages with metadata and timestamps.
  - Retrieve full conversation history.
- **Truncation Utilities**
  - By number of turns (last `n` messages)
  - By number of characters (max `n` chars)
  - By number of words (max `n` words)
- **Persistence**
  - Save conversation history to JSON file
  - Load conversation history from JSON file
- **Summarization**
  - Periodically summarize old messages and replace them with a compact summary using **Groq API**
  - Configurable parameters:
    - `k`: trigger summarization every `k` messages
    - `keep_last_n`: number of most recent messages to preserve
    - `max_summary_sentences`: number of sentences to include in the summary

---
        ┌──────────────────────┐
        │  Conversation Input  │
        └──────────┬───────────┘
                   │
         ┌─────────▼───────────┐
         │   HistoryManager    │
         │  - add_message()    │
         │  - get_history()    │
         │  - truncate_*()     │
         └─────────┬───────────┘
                   │
         ┌─────────▼─────────────┐
         │ periodic_summarize()  │
         │  - summarize_text()   │
         │ (calls Groq API)      │
         └─────────┬─────────────┘
                   │
         ┌─────────▼─────────────┐
         │  Updated Conversation │
         │  (with summary)       │
         └───────────────────────┘

---

## 🔐 Setup (Secrets in Colab)
Before running the notebook, store your **Groq API Key** as a secret in Colab:

- **Name**: `secretName`
- **Value**: `gsk_your_actual_key_here`

**Code to retrieve secret and set environment variables:**
```python
from google.colab import userdata
import os

os.environ["GROQ_API_KEY"] = userdata.get('secretName')
os.environ["GROQ_API_BASE"] = "https://api.groq.com/openai/v1"


## 🧠 Architecture
