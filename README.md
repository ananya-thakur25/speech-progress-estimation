# Speech Progress Estimation

This is a research-based project aimed at predicting how much of a speech 
has been completed (in percentage) without relying on the full transcript.  
We estimate this based on partial input — such as semantic features, 
speech structure, and content progression, from live or recorded 
speeches.
# 🧠 Topic Modeling Analysis for Speech Completion Prediction

The Jupyter notebook implements the core logic of topic modeling-based speech progress estimation. It processes speech transcripts, identifies semantic topics in chunks, and computes topic saturation to estimate the percentage of speech completed.

---

## 🚀 Objectives

- Segment transcripts into 3 sentence chunks
- Assign topic labels using BERTopic (Transformer-based topic modeling)
- Track cumulative topic coverage across chunks
- Detect topic saturation using an adaptive window method
- Estimate speech completion percentage using topic dynamics


## ⚙️ Technologies Used

- Python 3.11+
- Jupyter Notebook
- `BERTopic`
- `UMAP`, `HDBSCAN`
- `NLTK` (for sentence chunking)
- `Scikit-learn` (RandomForestRegressor)
- `Matplotlib`, `Pandas`


## 📌 Key Methods

### Chunking
Transcripts are broken into chunks of 3 sentences using NLTK's sentence tokenizer.

### Topic Modeling
BERTopic is applied on each chunk:
- Embeddings: BERT-based
- Dimensionality reduction: UMAP
- Clustering: HDBSCAN
- Output: Topic labels (`Topic_Label`)

### Outlier Handling
Chunks labeled as `-1` are reassigned using a **group-wise majority vote** within each speech (grouped by `VideoTitle`).

### 📏 Completion Estimation

**Completion % = (Saturation Chunk / Total Chunks) × 100**

------

