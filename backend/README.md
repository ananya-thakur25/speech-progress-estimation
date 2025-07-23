# Backend: Speech Completion Predictor

This is the **Node.js + Python** backend for the Speech Progress Estimation web application.

## Deployment Status

The backend **requires GPU and high-memory packages** (like `torch`, `BERTopic`, `sentence-transformers`, etc.)  
Hence, it **cannot be deployed on Render free tier** (512MB limit).  
Users can **run it locally** using the steps below.

## Local Setup Instructions

1. Navigate to the backend folder:
```bash
cd backend
```

2. Create and activate Python virtual environment:
```bash
python -m venv venv
source venv/bin/activate     # On Windows: venv\Scripts\activate
```

3. Install Python dependencies:
```bash
pip install -r requirements.txt
```

4. Install Node.js dependencies:
```bash
npm install
```

5. Run the backend server:
```bash
node server.js
```

Backend will start at:
 [http://localhost:3001](http://localhost:3001)

## API Usage

### POST /predict
**Request:**
```json
{
  "transcript": "Hello everyone, today we'll discuss..."
}
```

**Response:**
```json
{
  "prediction": 42.57
}
```

## 📎 Note

We've also provided the Jupyter Notebooks in the GitHub repo for training the ML model and saving the `.pkl` file.
