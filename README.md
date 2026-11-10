# DocuRAG

Chat with a PDF using retrieval-augmented generation (RAG). The app extracts text from an uploaded PDF, splits it into chunks, creates local embeddings, and uses a language model to answer questions with page references.

## Requirements

- Python 3.10 or later
- An OpenAI API key
- The Python packages listed in `requirements.txt`

## Setup

1. Create and activate a virtual environment:

   ```bash
   python -m venv .venv
   source .venv/bin/activate
   ```

   On Windows, activate it with `.venv\Scripts\activate`.

2. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

3. Create a `.env` file in the project root and add your API key:

   ```dotenv
   OPENAI_API_KEY=your_api_key_here
   ```

## Run

```bash
python pdf_chat.py
```

Open the local Gradio URL printed in the terminal, upload a PDF, and ask questions in the chat. The first run may take longer while the embedding model is downloaded.

## How it works

- `PyPDFLoader` loads the PDF page by page.
- `RecursiveCharacterTextSplitter` divides text into 800-character chunks with 100-character overlap.
- `all-MiniLM-L6-v2` creates embeddings locally; Chroma stores them in memory.
- The app retrieves the four most relevant chunks and sends them as context to `gpt-4o-mini`.
- Answers are instructed to use the document context and include source page numbers.

The index is held in memory and is rebuilt when a new PDF is uploaded. No persistent vector database is configured.

## Privacy and sharing

The PDF text used for answering is sent to OpenAI through the Chat Completions API. The app launches Gradio with `share=True`, which creates a public share link. Avoid uploading sensitive documents or sharing that link publicly.
