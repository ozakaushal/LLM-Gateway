# LLM Gateway Practice

A practical reference project demonstrating how to use **LiteLLM** as an LLM gateway for multi-provider routing, fallbacks, response caching, cost tracking, and security guardrails.

---

## Overview

This repository contains two practice notebooks exploring LLM gateway patterns:

1. **`llm_gateway.ipynb`**:
   - Single-interface API calls across multiple providers (OpenAI, Groq, Anthropic, Gemini).
   - Automatic fallback routing when a primary provider is unavailable or rate-limited.
   - Response caching via `litellm.cache` to reduce duplicate query costs.
   - Token usage and cost calculation using `completion_cost`.
   - Router configuration for load balancing and task-specific model routing.

2. **`chat_bot.ipynb`**:
   - Console-style chatbot pipeline applying gateway patterns in a conversational loop.
   - **Input Guardrails**: Validates input length, detects prompt injection attempts, and checks for sensitive data (credit cards, API keys).
   - **Output Guardrails**: Filters model replies for accidental credential exposure or system instruction leakage.
   - Live telemetry showing model used, prompt/completion tokens, and turn cost.
   - Guardrail verification cell for quick testing.

---

## Project Structure

```text
LLM-Gateway/
├── .env                  # API keys for configured providers
├── requirements.txt      # Project dependencies
├── llm_gateway.ipynb     # Gateway features and configuration examples
├── chat_bot.ipynb        # Console chatbot with input/output guardrails
└── README.md             # Project documentation
```

---

## Setup

### 1. Environment & Dependencies

Activate your virtual environment and install the required packages:

```bash
# Activate existing virtual environment (Windows PowerShell)
.venv\Scripts\Activate.ps1

# Install dependencies
pip install -r requirements.txt
```

### 2. Configure API Keys

Create a `.env` file in the project root with the API keys for the providers you plan to use:

```env
OPENAI_API_KEY="your-openai-key"
GROQ_API_KEY="your-groq-key"
ANTHROPIC_API_KEY="your-anthropic-key"
GEMINI_API_KEY="your-gemini-key"
```

---

## Model Strategy Used

| Role | Model | Provider | Purpose |
| :--- | :--- | :--- | :--- |
| **Primary** | `gpt-4o-mini` | OpenAI | Fast, cost-efficient default chat model |
| **Fallback 1** | `claude-3-5-haiku-20241022` | Anthropic | High-speed, high-reasoning fallback |
| **Fallback 2** | `gemini/gemini-2.0-flash` | Google | Low latency multi-modal fallback |
| **Fallback 3** | `groq/llama-3.3-70b-versatile` | Groq / Meta | Open-weights fallback on fast inference |