# Text Summarizer

A small web application that summarizes text using a locally stored Hugging Face T5 model. It includes a FastAPI backend and a browser-based interface.

## Features

- Paste text into a web form and request a summary.
- Provides a JSON API at `POST /summarize/`.
- Cleans whitespace and HTML tags before summarization.
- Uses Apple MPS or CUDA when available, and falls back to CPU.

## Requirements

- Python
- A locally saved Hugging Face T5 model and tokenizer in the `saved_summary_model/` directory.

The application loads the model from `./saved_summary_model` when it starts. Make sure that directory contains the model and tokenizer files before running the server. The model files need to be obtained or generated separately.

## Setup

1. Clone the repository and change into its directory:

   ```bash
   git clone https://github.com/Payel647/Text-Summarizer.git
   cd Text-Summarizer
   ```

2. (Optional) Create and activate a virtual environment:

   ```bash
   python -m venv .venv
   source .venv/bin/activate
   ```

   On Windows, activate it with:

   ```powershell
   .venv\Scripts\Activate.ps1
   ```

3. Install the dependencies:

   ```bash
   pip install -r requirements.txt
   ```

4. Add the required model and tokenizer files under `saved_summary_model/`.

5. Start the development server:

   ```bash
   uvicorn app:app --reload
   ```

6. Open [http://127.0.0.1:8000](http://127.0.0.1:8000) in your browser.

## API

### `POST /summarize/`

Request JSON:

```json
{
  "dialogue": "Paste the text you want summarized here."
}
```

Successful response:

```json
{
  "summary": "The generated summary appears here."
}
```

Example using `curl`:

```bash
curl -X POST http://127.0.0.1:8000/summarize/ \
  -H "Content-Type: application/json" \
  -d '{"dialogue":"Paste the text you want summarized here."}'
```

## Project Structure

- `app.py` — FastAPI application, model loading, text preprocessing, and summarization endpoint.
- `index.html` — Web interface for entering text and displaying the generated summary.
- `requirements.txt` — Python dependencies.

## Notes

Input is tokenized with a maximum length of 512 tokens and truncated if it is longer. Summary generation is configured with a maximum output length of 150 tokens.
