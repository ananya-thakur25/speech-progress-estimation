# Speech Completion Prediction – Ananya's Branch

This branch documents my individual contributions to the Speech Completion Prediction project. The objective is to estimate a speaker’s progress in their speech using content-aware methods, leveraging semantic embeddings and structural analysis instead of relying on time-based heuristics.

---

## Project Overview

Estimating how far a speaker has progressed in their speech is challenging due to its nonlinear structure. Traditional metrics like time duration or chunk count often fail to reflect true progress. This project models speech progression as a regression problem using:

- Semantic embeddings from speech chunks
- Novelty detection via cosine similarity
- Structural change detection via change point algorithms
- A machine learning model to predict completion percentage

---

## My Key Contributions

### Model Development

- Designed the machine learning pipeline to predict speech completion as a regression task.
- Used SentenceTransformer (MiniLM) to convert each speech chunk into 384-dimensional semantic embeddings.
- Computed cosine similarity between consecutive chunks to quantify semantic novelty.
- Extracted novelty-based features such as mean, variance, trend, and recent novelty values.
- Applied the PELT algorithm (via `ruptures`) with an RBF kernel to detect structural change points in the embedding sequence.
- Derived structural features like number of change points, density, recency, and last transition position.
- Combined semantic and structural metrics into a 10-dimensional feature vector for each speech.
- Trained and fine-tuned a Random Forest Regressor using `GridSearchCV`, optimizing for MAE and RMSE.

### Full-Stack Dashboard

- Developed the complete React frontend (`client/`) for user interaction and result visualization.
- Built the Python backend (`backend/`) with REST API endpoints to process input, run inference, and return predictions.
- Implemented real-time speech analysis and completion feedback through the interface.

### Ensemble Strategy

- Created an ensemble module to combine predictions from all team models.
- Assigned model weights based on validation accuracy to improve final prediction robustness.
- Packaged the ensemble logic into a reusable script (`scripts/ensemble.py`) for integration with the backend.

---

## Repository Structure

- `client/` – React frontend for user interface
- `backend/` – Python backend for processing and prediction
- `model/` – Saved trained models and metadata
- `scripts/` – Inference pipeline and ensemble logic
- `notebooks/` – Development and experimentation notebooks
- `data/` – Raw, cleaned, and processed speech data
- `results/` – Visual outputs and analysis

---
