# VectorSearch
A semantic search system built with **FAISS** and **Sentence-Transformers** to search news articles by meaning rather than just keywords and enable intelligent document retrieval from CSV datasets

---
🚀 Features

Semantic Search: Uses sentence transformers to understand query meaning, not just keyword matching
Fast Retrieval: FAISS indexing for sub-second search on large datasets
Intelligent Caching: Automatic embedding and index persistence to avoid recomputation
HTML Content Cleaning: BeautifulSoup integration for processing HTML in text fields
Duplicate Filtering: Automatic removal of duplicate results based on titles
Interactive CLI: Simple command-line interface for real-time searches

---
## 📌 Overview

This project loads a dataset of financial or news articles, generates vector embeddings for each item using a transformer model, and builds a **FAISS** index for fast, real-time similarity search.

The user can then input a natural-language query, and the system returns the top 5 most semantically relevant, unique results.

---

## 🧰 Technologies Used

- **Python 3.8+**
- [`sentence-transformers`](https://www.sbert.net/) — for generating dense vector embeddings
- [`FAISS`](https://github.com/facebookresearch/faiss) — for fast vector similarity search
- `pandas`, `numpy` — data handling
- `BeautifulSoup` — for HTML tag cleaning in news text

## 🔍 How It Works

### 📥 1. Data Loading & Cleaning
- Loads the `data.csv` file into a pandas DataFrame
- Fills missing values with empty strings
- Cleans HTML from `TITLE` and `DESCRIPTION` columns using **BeautifulSoup**

---

### 🧠 2. Embedding Generation
- Uses the `all-MiniLM-L6-v2` model from **Sentence-Transformers**
- Generates a semantic vector (embedding) for each article
- Converts embeddings to `float32` format (required for FAISS)

---

### 🗂️ 3. FAISS Indexing
- Builds a **FAISS** index using `IndexFlatL2` (Euclidean distance)
- Adds all embeddings to the index
- Saves both:
  - `models/embeddings.npy` — vector matrix
  - `models/faiss.index` — FAISS search index

---

### 🔎 4. Semantic Search
- User enters a **natural language** search query (e.g., `"education penalty"`)
- The query is embedded using the same model
- The query embedding is compared to indexed vectors
- The top **5 unique** matches are returned (skipping duplicates)

---

## 🧪 Example Usage

Run the search:

```bash
cd src
python search.py

💬 Prompt Example:-

bash
Copy
Edit
Enter your search query: education penalty

✅ Expected Output:-

 Top 5 unique results for: "education penalty"
------------------------------------------------------------
 Result #1
 Title   : The Egyptian Modern Education Systems (MOED.CA) - Listing Committee Decision
 Date    : 4/17/2025 1:45:37.000000 PM |  Source: EGX-NEWS
 Summary : Company Name : The Egyptian Modern Education Systems ISIN Code : EGS729F1C012 Reuters Code : MOED.CA Content : With reference to the provisions of Article 46...
------------------------------------------------------------

 Result #2
 Title   : Private School Violates Ministry Policy on Fees
 Date    : 4/10/2025 11:00:00 AM |  Source: Cairo Gazette
 Summary : The Ministry of Education imposed sanctions on the school after failing to comply with tuition transparency guidelines...
------------------------------------------------------------

... (up to 5 results shown)


