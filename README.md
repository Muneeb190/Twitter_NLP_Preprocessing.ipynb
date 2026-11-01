# Twitter NLP Preprocessing

Complete, reproducible NLP preprocessing workflow for a Twitter sentiment dataset.

**Notebook:** `Twitter_NLP_Preprocessing.ipynb`

---

## Overview

This notebook cleans and normalizes the Twitter training dataset so it is ready for sentiment analysis or other downstream NLP tasks. It handles missing values, duplicates, text normalization, tokenization, and stopword removal while preserving the original text and useful metadata.

**Dataset:** `twitter_training.csv`  
**Expected columns (no header in source file):** Tweet ID, Entity, Sentiment, Tweet Text

---

## Pipeline Steps

| Step | Description |
|------|-------------|
| **1. Import libraries** | `pandas`, `numpy`, `matplotlib`, `nltk` (punkt, stopwords, wordnet), and standard text utilities |
| **2. Load dataset** | Reads CSV with no header; assigns column names: `tweet_id`, `entity`, `sentiment`, `tweet_text` |
| **3. Understand the data** | Shape, missing values, sentiment distribution, descriptive stats, and a bar chart of labels |
| **4. Handle missing values** | Drops rows with missing or empty tweet text (and missing sentiment) |
| **5. Remove duplicates** | Drops fully duplicate rows |
| **6. Standardize sentiment labels** | Strips whitespace and title-cases labels (e.g. `Positive`, `Negative`) |
| **7. Text cleaning** | HTML removal, Unicode normalization, lowercasing, URL/email/mention removal, hashtag word retention, contraction expansion, number/punctuation removal, whitespace normalization → `clean_text` |
| **8. Tokenization** | NLTK `word_tokenize` (with regex fallback) → `tokens` |
| **9. Stopword removal** | English stopwords with **negation words retained** (`no`, `nor`, `not`, `never`) → `tokens_no_stopwords` |
| **10. Final checks** | Shape, missing values in processed columns, sentiment counts |
| **11. Export** | Saves a CSV with original + intermediate columns; token lists stored as space-separated strings |

---

## Input / Output

### Input

- **File:** `twitter_training.csv` (same folder as the notebook, or update `DATA_PATH`)
- **Format:** No header row
- **Columns assigned on load:**
  - `tweet_id`
  - `entity`
  - `sentiment`
  - `tweet_text`

### Output

- **File:** `twitter_training_preprocessed.csv`
- **Columns:**
  - `tweet_id`, `entity`, `sentiment`, `tweet_text` (original)
  - `clean_text`
  - `tokens` (space-separated)
  - `tokens_no_stopwords` (space-separated)

---

## Text Cleaning Details

The `clean_tweet` function applies, in order:

1. Remove HTML tags  
2. Unicode normalization (NFKC)  
3. Lowercase  
4. Remove URLs (`http(s)://…`, `www.…`)  
5. Remove email addresses  
6. Remove `@mentions`  
7. Keep hashtag **words** (strip the `#` symbol)  
8. Expand common English contractions  
9. Remove digits  
10. Remove punctuation and special characters (keep letters and spaces)  
11. Collapse repeated whitespace and strip  

The original tweet is kept in `tweet_text`; cleaned text is stored in `clean_text`.

---

## Stopword Policy

Standard English stopwords from NLTK are removed, **except** negation words:

- `no`
- `nor`
- `not`
- `never`

These are kept because they often reverse sentiment polarity. Tokens of length ≤ 1 are also dropped.

---
























StageApprox. rowsRaw load~74,682After missing/empty text removal~73,824After duplicate removal~71,484Final export~71,484Sentiment labels (after cleaning): Negative, Positive, Neutral, Irrelevant.
