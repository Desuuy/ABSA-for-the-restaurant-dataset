# 🍽️ Vietnamese Restaurant Review Aspect-Based Sentiment Analysis

## 📌 Introduction
Reference paper & GitHub repository: https://github.com/ds4v/absa-vlsp-2018

This project implements **Aspect-Based Sentiment Analysis (ABSA)** on  **VLSP 2018 Restaurant Dataset** in Vietnamese.
**Goal:** predict the sentiment polarity (Positive, Negative, Neutral) for each aspect mentioned in a customer review.

### Key Components
- **Vietnamese text preprocessing**: noise removal, spelling correction, teencode normalization, word segmentation.
- **Word2Vec embeddings**: using `wiki.vi.model.bin` pre-trained vectors.
- **Multi-task BiLSTM model:**: simultaneously performs aspect detection & sentiment prediction.
- **Model evaluation:**: Accuracy, Precision, Recall, F1-score, Confusion Matrix.
- **Inference demo**: input a Vietnamese review and obtain aspect-based sentiment outputs.

## 📂 Directory Structure
```
├── Data_Preprocessing.ipynb # Vietnamese text preprocessing pipeline
├── Model.ipynb              # BiLSTM model definition & training
 data/
│ ├── final_nlp_processed_dev.csv
│ ├── final_nlp_processed_test.csv
│ └── final_nlp_processed_train.csv
└── README.md
```

## 📊 Data
We use the **VLSP 2018 SA – Restaurant Dataset**, including:
- `1-VLSP2018-SA-Restaurant-train.csv`
- `2-VLSP2018-SA-Restaurant-dev.csv`
- `3-VLSP2018-SA-Restaurant-test.csv`

**Data Structure:**
- **Review**: customer review text.
- **Aspect columns:**: each aspect is labeled with
  - `0` (Not Mentioned),
  - `1` (Positive),
  - `2` (Negative),
  - `3` (Neutral).
---

## 🛠️ Preprocessing Pipeline
The preprocessing module applies:
1. Remove HTML tags, emojis, URLs, emails, phone numbers, hashtags.
2. Normalize Vietnamese Unicode.
3. Normalize tone marks using VinAI rules or **Behitek algorithm**.
4. Replace teencode and domain-specific abbreviations.
5. Correct spelling using [`bmd1905/vietnamese-correction-v2`](https://huggingface.co/bmd1905/vietnamese-correction-v2).
6. Perform word segmentation with **VnCoreNLP**.
7. Export cleaned datasets to CSV, including both raw and processed text.
---

## 🧠 Model Architecture
- **Embedding Layer**: initialized with pre-trained Word2Vec vectors.
- **BiLSTM**: for contextual feature extraction.
- **Linear Layer**: for multi-aspect sentiment prediction.
- **Masked Loss Function**: to ignore non-mentioned aspects (`label=0`).

---

## 📈 Evaluation
- **Aspect Detection**: Determine whether an aspect is mentioned in the review.
- **Sentiment Classification**: Predict sentiment for mentioned aspects.

**Metric**:
- Accuracy
- Precision / Recall / F1-score (macro, weighted)
- Confusion Matrix

---

## 🚀 Usage

### Install Dependencies
```
pip install underthesea gensim torchinfo seaborn scipy
pip install vncorenlp emoji transformers
```
