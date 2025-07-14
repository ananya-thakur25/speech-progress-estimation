# Speech Progress Estimation

This is a research-based project aimed at predicting how much of a speech 
has been completed (in percentage) without relying on the full transcript.  
We estimate this based on partial input - such as semantic features, 
speech structure, and content progression, from live or recorded 
speeches.

---

## Project Objective

To predict the **completion percentage** of a speech chunk by modeling semantic and thematic evolution. The model assumes that meaningful progress isn't linear and requires understanding topic flow and content transitions within the speech.

---

## Approach Summary

We implemented three complementary approaches that contribute to the overall prediction of speech completion:

### 1. Semantic and Structural Feature Modeling – *Ananya*

- Transcripts were divided into semantic chunks.
- Sentence embeddings (using SentenceTransformer MiniLM) were used to compute semantic similarity between adjacent chunks.
- **Novelty** was measured as `1 - cosine_similarity`, capturing how much each chunk deviates from the previous.
- **Change point detection** was applied using the PELT algorithm to detect structural shifts.
- Final feature vector included:
  - Mean, variance, and trend of novelty
  - Number of change points
  - Change point density and recency
- **Model**: Random Forest Regressor with GridSearchCV for hyperparameter tuning.

### 2. Topic Modeling-Based Completion Estimation – *Sailakshmi*

- Each transcript was divided into 3-sentence chunks and treated as mini-documents.
- **BERTopic** (BERT + UMAP + HDBSCAN) was used to assign topic labels to each chunk.
- Cumulative unique topics (`Uᵢ`) were tracked across the speech.
- **Saturation point** was detected as the index where new topic introductions plateau.
- Estimated completion: `(Saturation Chunk Index / Total Chunks) × 100`.
- **Final Model**: Random Forest Regressor using saturation metrics as features.

### 3. Ensemble Learning and Feature Fusion – *Aryan*

Input Data Format:
{Speech_ID, Chunk_ID, Chunk Text, Completion Percentage}
Speech transcripts segmented into 5-sentence chunks, each annotated with a ground-truth completion percentage (0–100%).

Feature Extraction Pipeline:

    Textual Structural Features (8 total):
    Extracted from each chunk using regex and string operations:

        Word Count, Character Count

        Punctuation Marks: Number of Commas, Periods, Exclamations, Questions

        Unique Word Count and Fraction of Unique Words

    Semantic Embeddings:

        Model: all-mpnet-base-v2 from SentenceTransformers (768-dim)

        Captures deep contextual semantics per chunk

    Dimensionality Reduction:

        PCA → Retain top 5 principal components of SBERT vectors

    Topic Clustering (Unsupervised):

        UMAP (2D) + HDBSCAN to assign thematic cluster labels

        No. of clusters adapt dynamically to semantic density

    Temporal Progression Features:

        Binary flag: cluster_seen_before (0 = new topic, 1 = recurring topic)

        fraction_unique_clusters: Tracks cumulative topic saturation during speech progression

Final Feature Vector:
Combined 17-dimensional representation for each chunk:

[8 structural features] + [5 PCA features] + [cluster ID, is_noise] + [cluster_seen_before, fraction_unique_clusters]

Modeling Approach:

    Regressor: LightGBM (LGBMRegressor)

    Tuning: Early stopping (50 rounds), MAE as evaluation metric

    Validation Strategy: 80/20 random train-test split

    Optimization: Gradient-boosted trees with learning_rate = 0.01 and 1000 estimators

Performance Summary:

    MAE: ~11.04% on test set

    R² Score: ~0.73 – indicates strong correlation between actual and predicted completion percentages

Saved Artifacts:

    chunk_embeddings.npy, pca_transformer.pkl, hdbscan_model.pkl, lgbm_model.pkl, and feature_columns.pkl

---

## Author Contributions

| Name            | Contributions                                                                 |
|------------------|--------------------------------------------------------------------------------|
| **Ananya Thakur** | Semantic similarity modeling, change point detection, feature engineering, full-stack dashboard, ensemble logic |
| **Aryan**         | Feature extraction (semantic + temporal), clustering pipeline, LightGBM ensemble model |
| **Sailakshmi**    | Topic modeling using BERTopic, saturation-based completion estimation         |

---
