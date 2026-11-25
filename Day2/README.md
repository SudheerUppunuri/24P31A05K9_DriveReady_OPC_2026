# Agentic AI – Day 2

## Overview

This notebook demonstrates a simple conversational AI assistant for **Sweet Crumbs Home Bakery**. It uses a Groq-hosted language model to answer customer questions, includes a menu with prices, calculates item totals with a Python function, keeps a short conversation history, and handles selected API errors.

The notebook also contains a separate experiment with a Prompt Guard model.

## Features

- **Bakery FAQ assistant:** Answers questions using bakery-specific instructions.
- **Menu and pricing:** Stores prices for plum cake, chocolate cake, and cupcakes.
- **Price calculation:** Calculates the total cost for a menu item and quantity.
- **Conversation history:** Keeps recent messages so the assistant can use earlier context.
- **Logging decorator:** Prints function calls and their results for learning and debugging.
- **Error messages:** Provides fallback replies for rate-limit and connection errors.
- **Prompt Guard experiment:** Sends a sample prompt to a Prompt Guard model.

## Menu

| Item | Price (INR) |
|---|---:|
| Plum cake | ₹500 |
| Chocolate cake | ₹650 |
| Cupcake | ₹80 |

These prices are defined directly in the notebook and are examples for this project.

## Technologies Used

- Python
- LangChain Groq integration (`langchain_groq`)
- Groq Python SDK (`groq`)
- Groq-hosted language models
- Google Colab Secrets for API-key storage
- `json` and `functools` Python modules

## Bakery Assistant Rules

The system prompt instructs the assistant to:

- Explain that orders need 24 hours' notice.
- State that all cakes have eggless versions.
- Never promise a delivery time.
- Say “I'll check with the owner” when information is not available.
- Remember the customer's name and order during the chat.
- Reply in fewer than 50 words, using English or Telugu to match the customer.

These are instructions for the language model. The notebook does not connect to a real order-management or delivery system.

## How It Works

1. Installs and imports the LangChain Groq integration.
2. Loads `GROQ_API_KEY` from Google Colab Secrets.
3. Initializes a `ChatGroq` model for the first chatbot example.
4. Defines the bakery system prompt and an `ask()` function.
5. Runs a sample question about receiving an order in five hours.
6. Calls the Prompt Guard model with a sample prompt-injection-style message.
7. Defines a menu and a `get_price()` function to calculate item totals.
8. Uses a logging decorator to display function calls and results.
9. Stores messages in a `history` list and sends recent conversation messages to the model.
10. Runs an interactive chat loop until the user enters `bye`, `exit`, or `quit`.

## Setup Instructions

### 1. Open the notebook

Open `agentic_ai_Day2.ipynb` in Google Colab. The notebook uses `google.colab.userdata`, so Google Colab is the easiest environment to run it in.

### 2. Install dependencies

Run the notebook's installation cell:

```python
!pip install langchain_groq
```

The notebook also imports the Groq SDK and exception classes from the `openai` package. If they are not already available in your runtime, install the required packages:

```python
!pip install groq openai
```

### 3. Add your API key

In Google Colab, open **Secrets**, create a secret named `GROQ_API_KEY`, enter your Groq API key, and allow the notebook to access it.

Do not publish your API key or place it directly in the notebook.

### 4. Run cells in order

Run the cells from top to bottom. The model calls need a valid API key and an internet connection. When the final interactive loop starts, type a question at the `You:` prompt. Enter `bye`, `exit`, or `quit` to stop.

## Example Price Calculation

```python
print("Total for 6 cupcakes:", get_price("cupcake", 6))
```

Expected calculation:

```text
6 × ₹80 = ₹480
```

The `get_price()` function looks up the item in `MENU` and multiplies its price by the quantity.

## Conversation History

The notebook stores the system prompt and conversation messages in the `history` list. It sends the system prompt and a limited number of recent messages with each request. This helps the assistant respond with context from the ongoing conversation.

The notebook's final cell evaluates `history`, allowing you to inspect the messages recorded during the session.

## Important Notes and Improvements

- The notebook uses two model configurations: `openai/gpt-oss-120b` with `ChatGroq` in the first example and `openai/gpt-oss-20b` in the interactive chatbot.
- The interactive chatbot's `MODEL` and `MENU` are defined in the notebook. Confirm that the selected model is available to your Groq account before running it.
- The `get_price()` function raises a `KeyError` if an item is not in `MENU`; input validation would make it more robust.
- Validate that quantities are positive integers before calculating prices.
- The notebook imports `RateLimitError` and `APIConnectionError` from `openai`. Depending on the SDK and exceptions raised by the Groq client, these handlers may need to be changed to the corresponding Groq exception classes.
- The Prompt Guard example contains the placeholder `[INSERT UNSAFE ACTION HERE]`. Replace it with a safe test string when experimenting.
- `history` is stored in memory and is not saved permanently. Restarting the notebook clears it.
- The notebook is a learning example, not a production-ready ordering service. It does not verify payments, place actual orders, or retrieve live delivery information.

## Repository Structure

```text
.
├── agentic_ai_Day2.ipynb
└── README.md
```

## Learning Outcomes

After working through this notebook, you should have practice with:

- Calling a hosted language model from Python.
- Writing system prompts for a focused assistant.
- Creating Python functions and decorators.
- Calculating menu totals using structured data.
- Maintaining a short conversation history.
- Handling selected API errors and inspecting chatbot behavior.

## License

No license is specified in the notebook. Add a license if you intend to distribute the project publicly.
