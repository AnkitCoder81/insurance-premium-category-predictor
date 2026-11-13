# Insurance Premium Category Predictor

A machine learning web app that predicts a person's **insurance premium category** from basic personal details. The model is served through a **FastAPI** backend and used through a simple **Streamlit** frontend.

## Features

- Predicts the premium category from age, weight, height, income, smoking status, city and occupation
- Input validation with Pydantic (rejects invalid or out-of-range values)
- Automatic feature engineering: BMI, lifestyle risk, age group and city tier
- Interactive Swagger docs at `/docs`
- Simple web UI built with Streamlit

## Tech Stack

- **Python 3.12**
- **FastAPI** and **Uvicorn** for the REST API
- **Streamlit** for the frontend
- **scikit-learn** and **pandas** for the model and data handling
- **Pydantic** for request validation
- **Docker** for containerization

## Project Structure

```
.
├── main.py            # FastAPI backend (loads the model, exposes /predict)
├── frontend.py        # Streamlit user interface
├── model.pkl          # Trained ML model
├── patients.json      # Sample data
├── requirements.txt   # Python dependencies
├── Dockerfile         # Container setup
└── .gitignore
```

## How It Works

1. The user fills in the form in the Streamlit app.
2. Streamlit sends the data as JSON to the FastAPI `/predict` endpoint.
3. FastAPI validates the input and computes extra features:

| Feature | Description |
| --- | --- |
| `bmi` | weight / height² |
| `lifestyle_risk` | `high`, `medium` or `low`, based on smoking status and BMI |
| `age_group` | `young`, `adult`, `middle_aged` or `senior` |
| `city_tier` | 1, 2 or 3, based on the city |

4. The trained model predicts the premium category, and the result is shown in the UI.

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/AnkitCoder81/insurance-premium-category-predictor.git
cd insurance-premium-category-predictor
```

### 2. Create a virtual environment and install dependencies

```bash
python -m venv myenv

# Windows (PowerShell)
myenv\Scripts\Activate.ps1

# macOS / Linux
source myenv/bin/activate

pip install -r requirements.txt
```

### 3. Start the FastAPI backend

```bash
uvicorn main:app --reload
```

The API runs at `http://127.0.0.1:8000`. Open `http://127.0.0.1:8000/docs` for the interactive API docs.

### 4. Start the Streamlit frontend

Open a second terminal, activate the virtual environment, and run:

```bash
streamlit run frontend.py
```

Then open `http://localhost:8501` in your browser.

## API Usage

**Endpoint:** `POST /predict`

**Request body:**

```json
{
  "age": 30,
  "weight": 65.0,
  "height": 1.7,
  "income_lpa": 10.0,
  "smoker": false,
  "city": "Mumbai",
  "occupation": "private_job"
}
```

**Example with curl:**

```bash
curl -X POST http://127.0.0.1:8000/predict \
  -H "Content-Type: application/json" \
  -d '{"age":30,"weight":65,"height":1.7,"income_lpa":10,"smoker":false,"city":"Mumbai","occupation":"private_job"}'
```

**Response:**

```json
{
  "insurance_premium_category": "Medium"
}
```

**Valid occupations:** `retired`, `freelancer`, `student`, `government_job`, `business_owner`, `unemployed`, `private_job`

## Docker

```bash
docker build -t insurance-premium-predictor .
docker run -p 8000:8000 insurance-premium-predictor
```

## Future Improvements

- Add model evaluation metrics and the training notebook to the repo
- Deploy the API and frontend online
- Add unit tests for the API
- Support more input features

## Author

**Ankit Mishra**
GitHub: [@AnkitCoder81](https://github.com/AnkitCoder81)
