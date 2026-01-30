# Conversational AI 
## Assignment 2 – Hybrid RAG System with Automated Evaluation
### Group 122
### Group Member Details:
|S.No|   Group Member Names        |      BITS Email ID                        | Contribution in %  |
|----|-----------------------------|-------------------------------------------|--------------------|
|1   | **SK SHAHRUKH SABA**        |     2024aa05401@wilp.bits-pilani.ac.in    | 100%               |
|2   | **SANKHA CHAKRABORTY**      |     2024AA05393@wilp.bits-pilani.ac.in    | 100%               |
|3   | **NEELASHA ROY**            |     2024aa05698@wilp.bits-pilani.ac.in    | 100%               |
|4   | **ARUNAVA DUTTA**           |     2024aa05374@wilp.bits-pilani.ac.in    | 100%               |
|5   | **CHINTAN DESAI**           |     2024aa05648@wilp.bits-pilani.ac.in    | 100%               |


## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Environment Setup](#environment-setup)
- [Running the Notebook](#running-the-notebook)
- [Notebook Structure](#notebook-structure)
- [Output Files](#output-files)
- [Streamlit Web Application](#-streamlit-web-application)
- [Troubleshooting](#troubleshooting)

## 🎯 Overview

This notebook implements a RAG system with the following components:

1. **Wikipedia Dataset Creator**: Scrapes content from 200 fixed + 300 random Wikipedia URLs
2. **Dense Vector Retrieval**: Uses sentence-transformers (`all-MiniLM-L6-v2`) with FAISS indexing
3. **Sparse Keyword Retrieval**: Uses BM25 algorithm for keyword-based matching
4. **Reciprocal Rank Fusion (RRF)**: Combines dense and sparse retrieval results
5. **Response Generation**: Uses TinyLlama-1.1B-Chat (4-bit GGUF quantization) for CPU-optimized inference

## ✨ Features

- **Hybrid Retrieval**: Combines semantic (dense) and keyword (sparse) search
- **500 Wikipedia Documents**: 200 curated + 300 random articles across 8 domains
- **CPU-Optimized LLM**: TinyLlama runs efficiently on CPU (~10-20 sec/query)
- **Detailed Output**: Shows query, answer, retrieved chunks, all scores, and timing

## 💻 Prerequisites

- **Python**: 3.10 or higher (tested with Python 3.14.2)
- **Operating System**: Windows 10/11 (instructions for Windows PowerShell)
- **RAM**: Minimum 8GB recommended
- **Disk Space**: ~2GB for models and virtual environment
- **Internet Connection**: Required for downloading models and Wikipedia content

## 🛠️ Environment Setup

### Step 1: Clone or Download the Project

```powershell
# Navigate to your desired directory
cd C:\temp

# Create project folder (if not exists)
mkdir ConvAI
cd ConvAI
```

### Step 2: Create Virtual Environment

```powershell
# Create a new virtual environment
python -m venv .venv

# Activate the virtual environment
.\.venv\Scripts\Activate.ps1
```

> **Note**: If you encounter an execution policy error, run:
> ```powershell
> Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
> ```

### Step 3: Install Required Packages

```powershell
# Upgrade pip
python -m pip install --upgrade pip

# Install core dependencies
pip install requests beautifulsoup4 numpy

# Install NLP and ML packages
pip install sentence-transformers faiss-cpu
pip install nltk rank-bm25

# Install LLM inference packages
pip install ctransformers huggingface_hub

# Install Jupyter support
pip install ipykernel jupyter_client

# Install SSL certificate fix
pip install certifi --upgrade
```

### Step 4: Install All Packages at Once (Alternative)

```powershell
pip install requests beautifulsoup4 numpy sentence-transformers faiss-cpu nltk rank-bm25 ctransformers huggingface_hub ipykernel jupyter_client certifi
```

### Complete Package List

The following packages are required (with tested versions):

| Package | Version | Purpose |
|---------|---------|---------|
| `requests` | 2.32.5 | HTTP requests for Wikipedia scraping |
| `beautifulsoup4` | 4.14.3 | HTML parsing |
| `numpy` | 2.4.1 | Numerical operations |
| `sentence-transformers` | 5.2.2 | Dense embeddings |
| `faiss-cpu` | 1.13.2 | Vector similarity search |
| `nltk` | 3.9.2 | Text preprocessing |
| `rank-bm25` | 0.2.2 | BM25 sparse retrieval |
| `ctransformers` | 0.2.27 | GGUF model inference |
| `huggingface_hub` | 1.3.4 | Model downloading |
| `certifi` | 2026.1.4 | SSL certificates |
| `ipykernel` | 7.1.0 | Jupyter kernel support |

## 🚀 Running the Notebook

### Step 1: Open VS Code

```powershell
# Open VS Code in the project directory
code C:\temp\ConvAI
```

### Step 2: Select Python Interpreter

1. Press `Ctrl+Shift+P` to open Command Palette
2. Type "Python: Select Interpreter"
3. Select the `.venv` interpreter: `C:\temp\ConvAI\.venv\Scripts\python.exe`

### Step 3: Open and Run the Notebook

1. Open `ConversationalAI_Assignment_2_Par1_1.1_to_1.4.ipynb`
2. Select the `.venv` kernel when prompted
3. Run cells sequentially using `Shift+Enter` or click "Run All"

### Expected Execution Times

| Section | Estimated Time |
|---------|----------------|
| Dataset Creation (200 fixed URLs) | 5-10 minutes |
| Dataset Creation (300 random URLs) | 15-20 minutes |
| Model Loading (sentence-transformers) | 30-60 seconds |
| Model Loading (TinyLlama) | 1-2 minutes (first time) |
| Embedding Generation | 1-2 minutes |
| Single RAG Query | 10-20 seconds |

> **Note**: First run downloads models (~1GB total). Subsequent runs are faster.

## 📁 Notebook Structure

The notebook is organized into the following sections:

### Part 1: Wikipedia Dataset Creator
- Import libraries
- Helper functions for Wikipedia scraping
- Generate 200 fixed URL dataset
- Generate 300 random URL dataset
- Save to JSON files

### Part 1.1: Dense Vector Retrieval
- Load sentence-transformers model
- Generate document embeddings
- Build FAISS index
- Implement semantic search

### Part 1.2: Sparse Keyword Retrieval (BM25)
- Text preprocessing (tokenization, stopwords)
- Build BM25 index
- Implement keyword-based search

### Part 1.3: Reciprocal Rank Fusion (RRF)
- Combine dense and sparse rankings
- RRF formula: `score(d) = Σ 1/(k + rank_i(d))`
- Hybrid retrieval function

### Part 1.4: Response Generation
- Load TinyLlama-1.1B-Chat (GGUF)
- Context construction from retrieved chunks
- Generate answers using ChatML format
- Display comprehensive results

## 📄 Output Files

After running the notebook, the following files are created:

| File | Description | Size |
|------|-------------|------|
| `fixed_url.json` | 200 curated Wikipedia articles | ~400KB |
| `random_url.json` | 300 random Wikipedia articles | ~600KB |

### JSON Schema

```json
{
  "id": "uuid-string",
  "url": "https://en.wikipedia.org/wiki/...",
  "domain": "people|places|events|technology|science|arts|sports|organization|other",
  "content": "Up to 200 words of extracted text..."
}
```

## 🌐 Streamlit Web Application

A Streamlit web interface is available for interactive RAG queries without running the notebook.

### Installation

```powershell
# Activate virtual environment first
.\.venv\Scripts\Activate.ps1

# Install Streamlit
pip install streamlit
```

### Running the Streamlit App

```powershell
# Make sure virtual environment is activated
.\.venv\Scripts\Activate.ps1

# Run the Streamlit app
streamlit run app.py
```

The app will open in your default browser at `http://localhost:8501`

### Features

- **Sidebar Configuration**: Adjust `top_k` for retrieval and generation parameters
- **Query Input**: Enter any question to search the Wikipedia dataset
- **Results Display**: 
  - Generated answer from TinyLlama
  - Retrieved chunks with source URLs
  - Dense, sparse, and RRF scores for each chunk
  - Response time breakdown

### Prerequisites

Before running the Streamlit app, ensure:
1. The notebook has been run at least once to create `fixed_url.json` and `random_url.json`
2. All required packages are installed (see [Environment Setup](#environment-setup))
3. Models will download automatically on first run (~1GB)

### Screenshot

```
┌─────────────────────────────────────────────────────────────────┐
│  🔍 Hybrid RAG System                                           │
│  ═══════════════════                                            │
│                                                                 │
│  ┌─────────────────┐   ┌─────────────────────────────────────┐  │
│  │ ⚙️ Settings     │   │ Enter your query:                   │  │
│  │                 │   │ [Who was Albert Einstein?        ]  │  │
│  │ Top K: [5    ]  │   │ [🔍 Search]                         │  │
│  │ Max Tokens:[512]│   │                                     │  │
│  │ Temperature:[0.7]│  │ 🤖 Generated Answer:                │  │
│  │                 │   │ Albert Einstein was a German...    │  │
│  └─────────────────┘   │                                     │  │
│                        │ 📚 Retrieved Chunks:                │  │
│                        │ [1] Albert Einstein (RRF: 0.033)    │  │
│                        └─────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

## 🔧 Troubleshooting

### SSL Certificate Errors

If you encounter SSL errors when downloading models:

```python
import certifi
import os
os.environ['SSL_CERT_FILE'] = certifi.where()
os.environ['REQUESTS_CA_BUNDLE'] = certifi.where()
```

### Memory Issues

If you run out of memory:
- Close other applications
- Reduce `top_k` parameter in retrieval functions
- Use smaller batch sizes for embedding generation

### Model Download Issues

If model download fails:
1. Check internet connection
2. Try downloading manually:
   ```powershell
   pip install huggingface_hub
   huggingface-cli download TheBloke/TinyLlama-1.1B-Chat-v1.0-GGUF tinyllama-1.1b-chat-v1.0.Q4_K_M.gguf
   ```

### NLTK Data Missing

If NLTK complains about missing data:
```python
import nltk
nltk.download('punkt')
nltk.download('stopwords')
nltk.download('punkt_tab')
```

### Virtual Environment Not Activating

```powershell
# Check if venv exists
Test-Path .\.venv\Scripts\Activate.ps1

# If not, recreate it
python -m venv .venv --clear
```

## 📊 Sample Output

```
================================================================================
RAG RESPONSE GENERATION RESULTS
================================================================================

📝 USER QUERY:
   Who was Albert Einstein?

🤖 GENERATED ANSWER:
   Albert Einstein was a German-born theoretical physicist who developed the 
   theory of relativity and made important contributions to quantum theory.

📚 TOP RETRIEVED CHUNKS (2 chunks):
   ──────────────────────────────────────────────────────────────────────────

   [1] Albert Einstein
       📖 Source: https://en.wikipedia.org/wiki/Albert_Einstein
       🏷️  Domain: people
       📊 Scores:
          • Dense (Semantic):  Rank 1, Score: 0.8542
          • Sparse (BM25):     Rank 1, Score: 12.4523
          • RRF (Hybrid):      Score: 0.032787

⏱️  RESPONSE TIME BREAKDOWN:
   ────────────────────────────────────────
   • Retrieval Time:     0.15 seconds
   • Generation Time:   12.34 seconds
   • Total Time:        12.49 seconds

================================================================================
```