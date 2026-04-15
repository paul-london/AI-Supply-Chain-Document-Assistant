# AI Supply Chain Document Assistant

An AI-powered RAG (Retrieval-Augmented Generation) chatbot for pharmaceutical supply chain document processing. Upload PDF documents — batch records, certificates of quality, product labels — and ask natural language questions. The system extracts, indexes, and retrieves relevant content to generate grounded answers.

---

## Interface

<img width="657" height="605" alt="ui" src="https://github.com/user-attachments/assets/fd4337b2-fd68-423d-8d6d-bd407952427d"/>

---

## Features

- Dual-path PDF extraction: PyMuPDF for digital PDFs, Tesseract OCR fallback for scanned pages
- Semantic search using FAISS and sentence-transformers
- Local, fully offline inference with Flan-T5 — no API key required
- Document type inference (clinical, regulatory, label, research, general)
- Gradio web UI for interactive querying and source citation display
<img width="234.375" height="56.625" alt="image" src="https://github.com/user-attachments/assets/323a9c88-8ace-4a41-81ef-5f39595a9526" /> 

---

## Pipeline Architecture

| Component | Technology | Configuration |
|---|---|---|
| OCR Engine | Tesseract OCR (pytesseract) | Confidence threshold: 60; PIL preprocessing; PyMuPDF fallback |
| Text Chunking | Custom sliding window | 400 characters per chunk, 80 character overlap |
| Embeddings | sentence-transformers (all-MiniLM-L6-v2) | 384 dimensions, CPU-compatible |
| Vector Database | FAISS | In-memory flat L2 index |
| Retriever | Dense retrieval | Top-K = 4 |
| LLM | google/flan-t5-base | Greedy decoding, num_beams=2, fully local |
| Prompt Strategy | Context-injection RAG | Retrieved chunks injected into structured prompt |

---

## Getting Started

### Prerequisites

- Python 3.9+
- Tesseract OCR installed on your system: https://github.com/tesseract-ocr/tesseract
- Poppler (required by pdf2image): https://poppler.freedesktop.org

### Installation

```bash
git clone https://github.com/yourusername/AI-Supply-Chain-Document-Assistant.git
cd AI-Supply-Chain-Document-Assistant
pip install -r requirements.txt
```

### Running the Notebook

Open `AI Supply Chain Document Assistant.ipynb` in Jupyter or Google Colab and run all cells in order. The Gradio UI will launch automatically in the final cell. Alternatively, follow the generated link to the Gradio-hosted interface.

1. Upload a pharmaceutical PDF using the file input
2. Wait for indexing to complete
3. Type a question in the query box
4. The system returns an answer with source citations and retrieval confidence scores

---

## Evaluation

The notebook includes an evaluation block that measures performance on a 10-question manual test set:

- **Retrieval:** Hit Rate, Recall@4, Precision@4, MRR, latency
- **Generation:** Answer accuracy (keyword matching), average generation time
- **Processing:** Per-page PDF extraction time

---

## Design Decisions

**Simplicity-first, locally-deployed.** Every component runs without an API key or external service. The pipeline is intentionally transparent — no abstraction frameworks — so every step is visible and modifiable directly in the notebook.

**Speed over accuracy.** `all-MiniLM-L6-v2` and `flan-t5-base` were chosen for fast local inference. For higher accuracy, swap in a biomedical embedding model (`pritamdeka/S-PubMedBert-MS-MARCO`) and a larger LLM (`flan-t5-large` or OpenAI API).

---

## Known Limitations

- OCR quality degrades on low-resolution scans and complex table layouts
- Character-based chunking can fragment critical values across chunk boundaries
- `flan-t5-base` produces short answers and struggles with multi-part queries
- FAISS index is in-memory only — documents must be re-uploaded each session

---

## Roadmap

- Sentence-aware chunking with `nltk.sent_tokenize`
- Biomedical embedding model for improved domain-specific retrieval
- FAISS index persistence with `faiss.write_index`
- RAGAS integration for automated faithfulness evaluation
- Multi-document library support with ChromaDB or Pinecone

---

## Built With

- [sentence-transformers](https://www.sbert.net/)
- [FAISS](https://github.com/facebookresearch/faiss)
- [Flan-T5](https://huggingface.co/google/flan-t5-base)
- [Tesseract OCR](https://github.com/tesseract-ocr/tesseract)
- [Gradio](https://www.gradio.app/)
- [PyMuPDF](https://pymupdf.readthedocs.io/)

---

## Acknowledgements

Special thanks to [Extern](https://www.extern.com) and [Pfizer](https://www.pfizer.com) for creating and hosting the Pfizer Supply Chain Document Processing Externship.


<img width="480" height="219" alt="image" src="https://github.com/user-attachments/assets/c8650177-1beb-4c00-8b47-5a8b901fa1e8" /><img width="562.75" height="230" alt="image" src="https://github.com/user-attachments/assets/a5b7b73f-0934-47ea-91c6-1fa258045c44" />



---

## Contact

[LinkedIn](https://www.linkedin.com/in/palondon)
