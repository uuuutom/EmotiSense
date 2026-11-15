# EmotiSense

**EmotiSense** is a Deep Learning and Natural Language Processing (NLP)
application that predicts the emotion expressed in a textual sentence or
dialogue.

The application uses a **Bidirectional GRU (BiGRU)** model to classify
text into one of six emotion categories and returns the predicted
emotion, its model confidence score, and scores for all emotion classes.

## Emotion Categories

-   Sadness
-   Joy
-   Love
-   Anger
-   Fear
-   Surprise

## Features

-   Text-based emotion classification
-   Text preprocessing and tokenization
-   Sequence padding before model inference
-   BiGRU deep learning model
-   FastAPI backend with
-   Health-check endpoint
-   Web interface served from the `static/` directory
-   Deployment support for Render

## Tech Stack

  Area              Technology
  ----------------- --------------------------------------
  Language          Python
  Deep Learning     TensorFlow / Keras
  NLP               Keras Tokenizer and sequence padding
  Model             Bidirectional GRU (BiGRU)
  Backend           FastAPI
  Data validation   Pydantic
  Frontend          HTML, CSS, JavaScript
  Deployment        Render

## Project Structure

``` text
EmotiSense/
├── Artifacts/
│   ├── BiGRU_Model.keras
│   └── tokenizer.pkl
├── static/
│   ├── index.html
│   ├── style.css
│   └── script.js
├── app.py
├── requirements.txt
├── .python-version
└── README.md
```

The model and tokenizer filenames above must match the paths expected by
`app.py`, including capitalization.

## How It Works

1.  The user enters a sentence or dialogue.
2.  The backend converts the text to lowercase, removes apostrophes and
    unsupported characters, and normalizes whitespace.
3.  The saved tokenizer converts the cleaned text into integer
    sequences.
4.  The sequence is padded or truncated to a maximum length of 50.
5.  The BiGRU model predicts scores for the six emotion classes.
6.  The API returns the highest-scoring emotion and scores for all
    classes.

> The returned confidence is the model's highest output score. It should
> not automatically be interpreted as a calibrated probability that the
> prediction is correct.

## Run Locally

### 1. Clone the repository

``` bash
git clone https://github.com/uuuutom/EmotiSense.git
cd EmotiSense
```

### 2. Use a compatible Python version

The project pins TensorFlow CPU 2.17.0. Use Python 3.11, as specified in
the repository's `.python-version` file.

``` bash
python --version
```

### 3. Create and activate a virtual environment

**Windows PowerShell:**

``` powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

**Windows Command Prompt:**

``` cmd
python -m venv venv
venv\Scripts\activate
```

**macOS / Linux:**

``` bash
python3 -m venv venv
source venv/bin/activate
```

### 4. Install dependencies

``` bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 5. Start the API server

Run this command from the project root (the directory containing
`app.py`):

``` bash
uvicorn app:app --reload
```

Open:

-   Web interface: http://127.0.0.1:8000/
-   API documentation: http://127.0.0.1:8000/docs
-   Health check: http://127.0.0.1:8000/health

The model and tokenizer must be present in `Artifacts/` for the
application to start.

## API Reference

### `GET /health`

Checks whether the server is running and whether the model and tokenizer
have loaded.

Example response:

``` json
{
  "status": "Server is running",
  "model_loaded": true
}
```

### `POST /predict`

Predicts the emotion expressed in the submitted text.

Request body:

``` json
{
  "text": "I feel so happy and excited today!"
}
```

Example response shape:

``` json
{
  "text": "I feel so happy and excited today!",
  "predicted_emotion": "joy",
  "confidence": 0.95,
  "all_probabilites": {
    "sadness": 0.01,
    "joy": 0.95,
    "love": 0.02,
    "anger": 0.005,
    "fear": 0.01,
    "surprise": 0.005
  }
}
```

The numbers are illustrative only; actual scores depend on the model and
input. The current API spells the response field `all_probabilites`.

Input validation requires non-empty text with a maximum length of 2,000
characters.

## Deployment on Render

Create a Python Web Service connected to this GitHub repository.

**Build command**

``` bash
pip install -r requirements.txt
```

**Start command**

``` bash
uvicorn app:app --host 0.0.0.0 --port $PORT
```

Ensure the service uses Python 3.11, as specified in `.python-version`.
Confirm that the `Artifacts/` files are committed to the repository and
available to the deployed service.

After deployment, test:

``` text
https://YOUR-SERVICE.onrender.com/health
```

A successful response should include `"model_loaded": true`.

## Important Notes

-   Dataset source, dataset size, model metrics, and detailed training
    configuration are not stated here because they have not been
    verified.
-   Keep inference preprocessing consistent with the preprocessing used
    during training.
-   The tokenizer must be the same tokenizer used to train the saved
    model.
-   Only load trusted pickle files; deserializing pickle files can
    execute code.
-   Do not commit secrets, credentials, or private data.

## Future Improvements

-   Document the dataset source and class distribution.
-   Add verified evaluation results: accuracy, precision, recall,
    F1-score, and confusion matrix.
-   Compare BiGRU with baseline models such as GRU and LSTM.
-   Add automated tests for preprocessing and API endpoints.
-   Add batch prediction support.
-   Add model monitoring and confidence calibration.

## Author

**Uttam Vaghasiya**

GitHub: https://github.com/uuuutom

## License

No license information is specified here. Add a `LICENSE` file if you
want to define how others may use, modify, and distribute this project.
