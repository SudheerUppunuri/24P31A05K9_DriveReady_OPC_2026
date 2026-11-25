# Agentic AI – Day 1

A beginner-friendly notebook demonstrating how to connect a language model to a Python application and build a simple customer-support assistant for **Sweet Crumbs Home Bakery**. The notebook also includes a small experiment with Groq's Prompt Guard model.

## Project Overview

The notebook covers:

- Installing and importing `langchain-groq`.
- Configuring a Groq-hosted language model using `ChatGroq`.
- Loading the `GROQ_API_KEY` from Google Colab Secrets.
- Defining system instructions for a bakery assistant.
- Sending a user question to the model and displaying its response.
- Trying a Prompt Guard model with an example prompt.

## Technologies Used

- **Python**
- **LangChain Groq integration** (`langchain_groq`)
- **Groq API**
- **Google Colab Secrets** for API-key storage
- **Groq Prompt Guard model** (`meta-llama/llama-prompt-guard-2-22m`)

## How It Works

1. Install the LangChain Groq integration.
2. Import `ChatGroq`.
3. Read `GROQ_API_KEY` from Google Colab Secrets and configure the model `openai/gpt-oss-120b` with `temperature=0`.
4. Create a system prompt for the Sweet Crumbs Home Bakery assistant. It states that orders need 24 hours' notice, eggless versions are available for all cakes, delivery times must not be promised, and replies should be polite and under 60 words.
5. Define an `ask(question)` function that sends the system prompt and user's question to the model and returns the response text.
6. Run a sample question about receiving an order within 24 hours.
7. Call the Prompt Guard model with an example prompt-injection-style message.

## Setup and Usage

### 1. Open the notebook

Open `agentic_ai_Day1.ipynb` in Google Colab or Jupyter Notebook. Google Colab is recommended because the notebook uses `google.colab.userdata` to read secrets.

### 2. Install the dependency

Run the notebook cell:

```python
!pip install langchain_groq
```

If your environment reports that the `groq` package is missing, install it as well:

```python
!pip install groq
```

### 3. Configure your API key

In Google Colab, open **Secrets** and add a secret named `GROQ_API_KEY` containing your Groq API key. Grant the notebook access to the secret. Do not hard-code or publish your API key.

The notebook attempts to load the secret with:

```python
from google.colab import userdata
userdata.get("GROQ_API_KEY")
```

If you run the notebook outside Google Colab, replace this secret-loading step with a secure environment-variable setup suitable for your environment.

### 4. Run the cells in order

Execute each cell from top to bottom. The model calls require a valid API key and network access.

## Example Assistant Rules

The bakery assistant is instructed to:

- Explain that orders require 24 hours' notice.
- Mention that eggless versions are available for all cakes.
- Avoid promising delivery times.
- Admit when it does not know an answer.
- Reply politely in fewer than 60 words.

These are instructions supplied to the language model; they are not a substitute for checking real order or delivery systems.

## Important Notes

- The notebook is a learning example, not a complete production-ready agent or ordering system.
- The Prompt Guard cell currently sends a sample message containing `[INSERT UNSAFE ACTION HERE]`. Replace this placeholder with a safe test input if you want to experiment, and interpret the model's output as a model response rather than a guaranteed security verdict.
- The notebook's `Groq()` client relies on the `GROQ_API_KEY` environment variable being configured.
- Keep API keys private and monitor API usage and rate limits.

## Possible Improvements

- Add a menu and verified product prices.
- Add structured order details such as item, quantity, pickup/delivery preference, and requested date.
- Validate inputs before calling the model.
- Add tests for common customer questions and prompt-injection attempts.
- Connect to an order database only with appropriate validation and security controls.
- Add error handling for missing API keys, connection failures, and rate limits.

## Repository Structure

```text
.
├── agentic_ai_Day1.ipynb
└── README.md
```

## Learning Outcome

After completing this notebook, you should understand the basics of configuring a hosted language model with LangChain, writing system instructions for a focused assistant, invoking the model from Python, and exploring a prompt-guard model.

## License

No license is specified in the notebook. Add a license file if you plan to publish or share the project publicly.
