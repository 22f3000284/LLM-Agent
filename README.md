# 🤖 LLM Agent POC — Browser-Based Multi-Tool Reasoning

Deployed App link : https://llm-agent-navy.vercel.app/

This is a **proof-of-concept (POC)** for a browser-based **LLM agent** that can reason, call external tools, and loop until tasks are complete — **all in pure JavaScript with no backend required**.

Modern LLMs can do more than just text: they can **call functions (tools)** like search APIs, pipelines, or even code execution.  
This demo shows how to wire that up in the simplest possible way for hacking and learning.

---

## ✨ Features

- **OpenAI-style reasoning loop**  
  The agent calls the LLM, checks for `tool_calls`, executes them, and feeds the results back until the model decides it’s done.

- **Supported Tools**
  - 🔍 **Google Search**: Fetches snippets from Google Custom Search JSON API.  
  - 🔗 **AI Pipe Proxy**: Call AI Pipe’s OpenRouter-compatible endpoints.  
  - ⚡ **JavaScript Execution**: Run JS safely in a sandboxed Web Worker.

- **Browser-Only**
  - No backend required — everything runs client-side in your browser.
  - **Bootstrap UI**: conversation window, model picker, and error alerts.
  - Minimal code: ~300 lines, designed to be hackable and extendable.

---

## 📸 Example Conversation

User: Interview me to create a blog post.

Agent: Sure! What’s the post about?

User: About IBM

Agent: Let me search for IBM...
[Google Search tool runs]

Agent: IBM is a major technology company founded in 1911...


---

## 🚀 Getting Started

1. Clone or download this repo.
2. Open `index.html` in your browser.

That’s it! No build step, no server.

---

## 🔑 Required API Keys

You’ll need to supply your own API keys in the UI:

- **LLM API**  
  - [OpenAI key](https://platform.openai.com/) (`sk-...`)  
  - or [AI Pipe token](https://aipipe.org/) (works with OpenRouter-compatible models)  

- **Google Custom Search API** (for the `google_search` tool)  
  1. Get an API key from [Google Cloud Console](https://console.cloud.google.com/).  
  2. Create a [Custom Search Engine](https://programmablesearchengine.google.com/) and note the `cx` ID.  

⚠️ Keys are stored in memory only (never persisted). For production, proxy requests server-side to protect secrets and handle CORS.

---

## 🛠 Tool Definitions

The agent exposes three OpenAI-style functions:

```jsonc
[
  {
    "type": "function",
    "function": {
      "name": "google_search",
      "description": "Search the web and return snippet results",
      "parameters": { "query": "string", "num": "integer" }
    }
  },
  {
    "type": "function",
    "function": {
      "name": "aipipe_request",
      "description": "Call an AI Pipe OpenRouter-compatible endpoint",
      "parameters": { "path": "string", "body": "object" }
    }
  },
  {
    "type": "function",
    "function": {
      "name": "exec_js",
      "description": "Run JavaScript code in a sandboxed worker",
      "parameters": { "code": "string" }
    }
  }
]
