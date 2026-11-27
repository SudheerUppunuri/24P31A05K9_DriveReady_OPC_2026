# Agentic AI – Day 3: Coffee Shop Assistant

## Overview

This notebook builds a conversational AI assistant for **Coffee Corner**, a local coffee shop, using LangChain and a Groq-hosted language model. It demonstrates prompt templates, conversation history, CSV-based response tests, a basic LangChain agent, and a comparison of different approaches for handling tasks.

## Features

- Answers customer questions about coffee drinks and prices.
- Uses a defined menu and shop rules in a system prompt.
- Keeps recent conversation messages in a history list.
- Runs automated checks from a `tests.csv` file.
- Demonstrates a simple LangChain agent with no external tools configured.
- Changes the model configuration and runs the tests again to compare responses.
- Categorizes example tasks as plain code, one LLM call, or an agent.

## Coffee Menu

| Item | Price |
|---|---:|
| Espresso | ₹120 |
| Cappuccino | ₹150 |
| Latte | ₹170 |
| Cold coffee | ₹180 |

The menu and prices are defined in the notebook. They are not connected to a live inventory or point-of-sale system.

## Shop Rules in the Prompt

The assistant is instructed to:

- Be friendly and reply in fewer than 50 words.
- Match the customer's language, English or Telugu.
- Never invent menu items or prices.
- Say “I'll check with the owner” when information is unavailable.
- Calculate totals carefully.
- Avoid promising delivery times or places.
- Require 24 hours' notice for bulk orders.

## Technologies Used

- Python
- LangChain
- `langchain-groq`
- `langchain-openai` (using Groq's OpenAI-compatible API endpoint)
- Groq API
- Google Colab Secrets
- CSV for simple response tests

## How the Notebook Works

1. Installs LangChain and the Groq integration.
2. Reads `GROQ_API_KEY` from Google Colab Secrets and configures the model `openai/gpt-oss-20b`.
3. Creates a chat prompt with a system message, a conversation-history placeholder, and the user's question.
4. Builds a chain using the prompt, language model, and `StrOutputParser`.
5. Defines `ask()` to send a question, return the answer, and append the exchange to conversation history.
6. Defines `run_tests()` to read questions and expected text from `tests.csv`, run each question, and check whether the expected text appears in the response.
7. Creates a basic LangChain agent with no tools configured.
8. Replaces the bakery prompt with the Coffee Corner prompt and tests coffee-menu questions.
9. Uses `ChatOpenAI` with Groq's OpenAI-compatible endpoint and sets temperature to `1.0`, then runs the tests twice.
10. Prints example jobs categorized as plain code, one LLM call, or agent.

## Setup Instructions

### 1. Open the notebook

Open `Agentic_ai_Day3(1).ipynb` in Google Colab.

### 2. Configure the Groq API key

In Google Colab, open **Secrets**, add a secret named `GROQ_API_KEY`, enter your Groq API key, and allow the notebook to access it. Never publish the key in the notebook or repository.

### 3. Run the notebook cells in order

Execute cells from top to bottom. The notebook expects `tests.csv` to be present in the current working directory for the coffee-shop tests. Make sure the CSV contains these column headers:

```csv
question,must_include
```

Each test row should include a question and a phrase expected to appear in the assistant's answer. Since the assistant generates natural-language responses, tests based on exact substring matching can fail even when the answer is reasonable.

### 4. Install required packages if needed

The notebook installs `langchain` and `langchain-groq` in its first cell, and installs `langchain-openai` later. If imports fail, rerun the corresponding installation cells.

## Example Questions

- What is the price of espresso?
- What is the price of cappuccino?
- Do you have cold coffee?
- Calculate the total price of 3 cappuccinos.
- Do you have a drink that is not listed in the menu?
- How much notice is required for bulk orders?

## Testing

The `run_tests()` function reads `tests.csv`, submits each question to the chain with an empty conversation history, and checks whether `must_include` occurs in the answer, ignoring case. It prints PASS or FAIL for each row and reports the overall pass rate.

**Note:** The notebook contains an earlier call to `run_tests()` that uses `/content/tests.csv` and later calls that use `tests.csv`. Ensure the file is saved at the path used by the relevant cell. The notebook's initial `run_tests()` call occurs before the later Coffee Corner prompt is defined, so run the notebook in order and verify the intended prompt and file path when testing.

## Important Notes

- The agent is created with `tools=[]`, so no menu lookup or inventory tool is actually connected to it.
- The example task “Check coffee menu availability using a tool” is labeled as an Agent, but the notebook does not implement that tool in this cell.
- Temperature `1.0` may make responses more variable, which can affect substring-based test results.
- The notebook's conversation history is stored in memory only and is not saved permanently.
- This is a learning project, not a production ordering system. It does not take payments, submit real orders, or verify live stock.
- Keep API keys private and monitor API usage and rate limits.

## Repository Structure

```text
.
├── Agentic_ai_Day3(1).ipynb
├── tests.csv
└── README.md
```

## Learning Outcomes

After completing this notebook, you should understand how to:

- Build a simple conversational chain with LangChain.
- Write a system prompt for a coffee-shop assistant.
- Maintain conversation history.
- Run basic CSV-driven tests against generated responses.
- Create a basic LangChain agent.
- Compare task types and observe how model temperature can affect responses.

## License

No license is specified in the notebook. Add a license file if you intend to publish the project publicly.

