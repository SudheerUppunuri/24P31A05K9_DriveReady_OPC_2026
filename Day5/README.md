# Agentic AI Day 5 --- PDF Question-Answering Assistant (RAG)

A Retrieval-Augmented Generation (RAG) PDF question-answering assistant built with **SentenceTransformers**, **FAISS**, and **Groq**. It extracts text from uploaded PDFs (including scanned pages via OCR), indexes document chunks as vector embeddings, and answers user questions strictly based on the PDF content.

## Features

-   **PDF text extraction with OCR fallback:** Extracts native text using `PyMuPDF` and automatically falls back to `Tesseract OCR` when a page contains scanned images instead of selectable text.
-   **Overlapping text chunking:** Splits extracted document text into 500-character chunks with a 100-character overlap to preserve context between sections.
-   **Semantic vector search:** Encodes text chunks into 384-dimensional normalized vectors using `all-MiniLM-L6-v2` and searches them with `FAISS` (`IndexFlatIP`).
-   **Grounded question answering:** Uses Groq to answer questions solely from retrieved PDF chunks, replying `"The answer is not available in the PDF."` when information is missing.
-   **Interactive Q&A loop:** Runs a continuous prompt in the notebook so you can ask multiple questions until typing `exit`.
-   **Embedding model comparison:** Includes an introductory cell comparing vector output dimensions between `all-MiniLM-L6-v2` and `all-mpnet-base-v2`.

## Technology Stack

-   Python
-   `PyMuPDF` (`fitz`) for PDF parsing and page rendering
-   `pytesseract`, `tesseract-ocr`, and `Pillow` for Optical Character Recognition (OCR)
-   `sentence-transformers` (`all-MiniLM-L6-v2`)
-   `faiss-cpu` and `NumPy` for vector indexing and similarity search
-   `groq` Python SDK with chat model: `openai/gpt-oss-20b`
-   Google Colab

## Pipeline Configuration

  Parameter         Value / Model             Description
  ----------------- ------------------------- ----------------------------------------------
  Embedding Model   `all-MiniLM-L6-v2`        Generates 384-dimensional normalized vectors
  Vector Index      `faiss.IndexFlatIP`       Inner-product similarity search index
  Chunk Size        `500` characters          Maximum character length per text chunk
  Chunk Overlap     `100` characters          Overlapping characters between adjacent chunks
  Top-K Retrieval   `3` chunks                Number of relevant chunks passed to the LLM
  Groq Model        `openai/gpt-oss-20b`      LLM used for context-grounded answers
  Temperature       `0`                       Deterministic output to minimize hallucination

These parameters are defined in the notebook cells and can be adjusted for larger or more complex PDFs.

## Setup and Run

### 1. Open the notebook

Upload `Agentic_ai_Day5.ipynb` to [Google Colab](https://colab.research.google.com/) using **File → Upload notebook**.

### 2. Run the installation cells

Run the top cells from top to bottom to install the required Python packages (`pymupdf`, `sentence-transformers`, `faiss-cpu`, `groq`, `pytesseract`, `pillow`) and system OCR tools (`tesseract-ocr`, `poppler-utils`).

### 3. Upload a PDF document

Run the PDF upload cell, click **Choose Files** when prompted, and select a PDF file from your computer. The notebook will extract the text and display the total character count and a preview.

### 4. Create chunks and build the FAISS index

Run the text-splitting and embedding cells to divide the PDF into chunks and store their vector representations in FAISS.

### 5. Enter your Groq API key

Run the Groq setup cell and paste your Groq API key into the hidden `getpass` input prompt.

### 6. Ask questions about the PDF

Run the final interactive loop cell and enter your questions at the prompt:

```text
Ask a question (or 'exit'): What is the title of the story?
Ask a question (or 'exit'): What special item does the watchmaker find in the story?
Ask a question (or 'exit'): exit
```

Type `exit` to end the Q&A session.

## Functions Used in the Pipeline

-   `read_pdf(pdf_path)`: Reads each page of the uploaded PDF using `PyMuPDF` and runs `Tesseract OCR` if no selectable text is found.
-   `split_text(text, chunk_size=500, overlap=100)`: Splits the extracted text into overlapping chunks.
-   `retrieve_context(question, top_k=3)`: Encodes the user's question and retrieves the top matching chunks from the FAISS index.
-   `ask_pdf(question)`: Sends the retrieved PDF context and user question to the Groq API and returns a concise, grounded answer.

## Important Notes

-   Keep your Groq API key private. Do not hardcode or commit API keys to GitHub.
-   The first cell in the notebook tests `all-MiniLM-L6-v2` and `all-mpnet-base-v2` on a sample sentence and is independent of the main PDF RAG pipeline.
-   `google.colab.files` is specific to Google Colab. If running locally, replace the upload widget with a local file path.
-   Large scanned PDFs may take longer to process because OCR renders and reads each image page sequentially.
-   The FAISS index is stored in memory and resets when the Colab runtime restarts.

## Project Structure

```text
.
├── Agentic_ai_Day5.ipynb
└── README.md
```

## Future Improvements

-   Support uploading and querying multiple PDFs in a single session.
-   Persist the FAISS index and text chunks to disk so documents do not need to be re-indexed.
-   Include page numbers and source chunk citations in the final answer.
-   Switch to sentence- or paragraph-aware chunking instead of fixed character slicing.
-   Load `GROQ_API_KEY` from Google Colab Secrets instead of manual `getpass` entry.

------------------------------------------------------------------------

Built as a Day 5 Agentic AI learning project using SentenceTransformers, FAISS, Tesseract OCR, and Groq.
