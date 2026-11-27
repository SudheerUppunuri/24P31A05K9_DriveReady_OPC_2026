# Agentic AI Day 4 --- Coffee Shop Assistant

A conversational coffee shop assistant built with **LangChain** and
**Groq**. It answers questions about the menu, calculates drink prices,
checks opening hours, and saves confirmed customer orders to Google
Sheets.

## Features

-   **Coffee menu and pricing:** Supports espresso, cappuccino, latte,
    and cold coffee.
-   **Price calculation:** Uses a tool to calculate the total price for
    a requested quantity.
-   **Opening hours:** Checks the shop's hours for a given day.
-   **Order collection:** Collects the customer's name, drink, quantity,
    and pickup date/time.
-   **Google Sheets order log:** Saves confirmed orders to a sheet named
    `Coffee Shop Orders`.
-   **Conversation history:** Keeps messages in memory during the
    current notebook session.
-   **Friendly responses:** Replies in English or Telugu, matching the
    customer's language, and aims to keep responses under 60 words.

## Technology Stack

-   Python
-   LangChain
-   `langchain-groq`
-   Groq chat model: `openai/gpt-oss-20b`
-   Google Colab
-   Google Sheets via `gspread` and Google authentication
-   Pydantic for order validation

## Sample Menu

  Drink           Price
  ------------- -------
  Espresso         ₹120
  Cappuccino       ₹180
  Latte            ₹200
  Cold coffee      ₹220

These are sample prices defined in the notebook and can be changed in
the `MENU` dictionary.

## Opening Hours

  Day                Hours
  ------------------ -------------------
  Monday--Saturday   9:00 AM--8:00 PM
  Sunday             10:00 AM--6:00 PM

These are sample hours defined in the notebook and can be changed in the
`HOURS` dictionary.

## Setup and Run

### 1. Open the notebook

Upload `Agentic_ai_Day4_Coffee_Shop.ipynb` to [Google
Colab](https://colab.research.google.com/) using **File → Upload
notebook**.

### 2. Configure the Groq API key

In Google Colab, open **Secrets** (the key icon in the sidebar), add a
secret named `GROQ_API_KEY`, enter your Groq API key, and allow the
notebook to access it.

### 3. Run the cells in order

Run each notebook cell from top to bottom. The first cell installs the
required packages, loads the API key, authenticates with Google, and
creates the Google Sheet.

### 4. Authorize Google access

When prompted, sign in to your Google account and grant the permissions
needed for Google Sheets access.

### 5. Chat with the assistant

The notebook includes sample conversations. You can add your own prompts
by calling:

``` python
print(chat("What coffees do you have on the menu?"))
print(chat("Are you open on Sunday?"))
print(chat("How much are 3 lattes?"))
print(chat("I'm Alex. Please order 2 lattes for 6 Dec at 10 am."))
```

Use a valid menu item and provide the customer's name, quantity, and
pickup date/time before confirming an order.

## Tools Used by the Agent

-   `get_price(item, qty)`: Looks up the menu price and calculates the
    total.
-   `is_open(day)`: Returns the opening hours for a weekday.
-   `log_order(name, item, qty, date)`: Validates and records a
    confirmed order in Google Sheets.

The system prompt instructs the assistant to use the tools for prices
and hours, ask for missing order details, and read the saved order and
total back to the customer.

## Google Sheets Order Format

The notebook creates a sheet with these columns:

  Column              Description
  ------------------- ----------------------------------------------
  `name`              Customer's first name
  `item`              Coffee drink
  `qty`               Number of drinks
  `pickup_time`       Pickup date or time supplied by the customer
  `total`             Order total in rupees

**Note:** The code's order tool stores the supplied date/time value in
the sheet's `pickup_time` column.

## Important Notes

-   The first setup cell creates a new spreadsheet. It is intended to be
    run once for a new setup. If you already created the sheet, update
    that cell to open the existing sheet instead of creating another
    one.
-   Keep your Groq API key private. Do not commit API keys or
    credentials to GitHub.
-   The menu, prices, and opening hours are example business data, not
    verified details for a real coffee shop.
-   This notebook uses in-memory conversation history; that history is
    not a permanent customer database.
-   Review the Google authorization permissions before running the
    notebook.

## Project Structure

``` text
.
├── Agentic_ai_Day4_Coffee_Shop.ipynb
└── README.md
```

## Future Improvements

-   Add more drinks, sizes, add-ons, and customizations.
-   Validate pickup dates and time slots.
-   Add order cancellation and order-status lookup.
-   Add automated tests for menu pricing, opening hours, and order
    validation.
-   Improve error handling and prevent duplicate spreadsheet setup.

------------------------------------------------------------------------

Built as a Day 4 Agentic AI learning project using LangChain, Groq, and
Google Sheets.

