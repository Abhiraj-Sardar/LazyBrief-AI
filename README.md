# LazyBrief AI 🦥

**Read less. Know more.** A local-first document summarizer and question-answering assistant built for the Hacktoberfest **Slow and Steady** open-source AI challenge.

## Features
- Summarize PDF, TXT and Markdown documents or pasted notes
- Two-sentence TL;DR, key takeaways, action items and decisions
- Ask questions grounded in retrieved document excerpts, with section markers
- SQLite local history and delete controls
- Runs Qwen2.5 3B through Ollama on your machine; no cloud AI key required
- Responsive React dashboard

## Requirements
- Python 3.10+
- Node.js 18+
- [Ollama](https://ollama.com/download)
- Sufficient RAM/disk for the local Qwen2.5 3B model

## Quick start

### 1. Start local AI
```bash
ollama pull qwen2.5:3b
```
Make sure the Ollama app/service is running. Test with `ollama run qwen2.5:3b` if needed.

### 2. Backend
```bash
cd backend
python -m venv .venv
```
Activate the environment:
- Windows PowerShell: `.venv\Scripts\Activate.ps1`
- macOS/Linux: `source .venv/bin/activate`

Then:
```bash
pip install -r requirements.txt
```
Copy `.env.example` to `.env` (Windows: `copy .env.example .env`; macOS/Linux: `cp .env.example .env`).
```bash
uvicorn app.main:app --reload --port 8000
```
Health check: http://localhost:8000/health
API docs: http://localhost:8000/docs

### 3. Frontend (new terminal)
```bash
cd frontend
npm install
npm run dev
```
Open http://localhost:5173

## Usage
1. Upload a PDF/TXT/MD file or paste notes.
2. Click **Generate brief** and wait for local model inference.
3. Read the TL;DR, takeaways, decisions and action items.
4. Ask questions about the selected document.
5. Copy summaries or delete stored documents.

## Privacy and limitations
- The document text, summaries, and history are stored in `backend/lazybrief.db` on your machine. Deleting a document removes its database row; normal SQLite/storage recovery caveats apply.
- AI inference is performed via the configured Ollama host. Keep `OLLAMA_HOST` pointing to localhost for private, local inference.
- After installing dependencies and downloading the model, the core app can run without internet. The UI imports a Google font when online but has local font fallbacks.
- **Scanned/image-only PDFs are not OCR'd**. Use selectable-text PDFs.
- Answers use lightweight keyword-ranked excerpts, not vector embeddings. Section citations refer to internal text chunks, not exact PDF pages. Check important facts against originals.
- AI summaries can make mistakes. No guarantee that every action item or deadline is found.
- Single-user localhost MVP. There is no authentication. Do not expose the API to the public internet.
- Max upload: 5 MB / 100 PDF pages / 210k extracted characters / 30 processing chunks. Large files can take time on CPU.

## Architecture
React + Vite → FastAPI → PyMuPDF (PDF text extraction) → Ollama/Qwen2.5 3B → SQLite local storage.

## Demo ideas
Use a sample meeting-notes TXT file, generate a summary, ask about a deadline, and show the result in history. Compare time spent reading vs. generating a brief, with measured rather than invented timings.

## License
MIT (see LICENSE).
