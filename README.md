# 🩺 AI Health Chatbot

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-Web%20App-black?logo=flask&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-RandomForest-orange?logo=scikitlearn&logoColor=white)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

A conversational healthcare chatbot that extracts symptoms from natural language input and predicts the most likely disease using a machine learning classifier, guiding users through a multi-turn dialogue before delivering a diagnosis with confidence scoring and precautionary advice.

---

## 📌 Overview

Instead of a static symptom-checklist form, this chatbot holds a conversation: it asks for the user's basic details, lets them describe symptoms in free text, makes an initial prediction, then asks targeted disease-specific follow-up questions to refine its answer — closer to how a real triage conversation flows.

## 🧠 How It Works

1. **Intake** — collects name, age, and gender to personalize the session
2. **Free-text symptom extraction** — parses a natural-language symptom description using:
   - A synonym dictionary (e.g. "belly pain" → `stomach_pain`, "high temperature" → `fever`)
   - Direct substring matching against known symptoms
   - Fuzzy string matching (`difflib.get_close_matches`, 0.8 cutoff) to catch typos and phrasing variations
3. **Initial prediction** — symptoms are vectorized and passed to a Random Forest Classifier for a first-pass disease prediction
4. **Guided follow-up** — asks up to 8 targeted yes/no questions specific to the predicted disease's known symptom profile, refining the symptom set
5. **Final diagnosis** — re-predicts using the refined symptom set, returning:
   - Predicted disease with confidence score
   - Description of the condition
   - Suggested precautions
6. **Session-based state machine** — the whole conversation flow is managed server-side via Flask sessions, so each user's dialogue progresses independently

## 📊 Dataset

- **4,920** training records
- **41** distinct diseases (prognosis labels)
- **131** symptom features (binary presence/absence)
- Separate `Testing.csv` provided for evaluation

## ⚙️ Model

- **Algorithm:** Random Forest Classifier (300 estimators)
- **Input:** binary symptom vector (131 features)
- **Output:** predicted disease + class probability (used as confidence score)

> **Note on accuracy:** this dataset's diseases map to largely non-overlapping, clean symptom sets, so classification accuracy on both the held-out split and the provided test set comes out extremely high. This reflects the dataset's strong class separability rather than real-world clinical noise — worth keeping in mind when comparing this project to messier, real-world medical data.

## 🛠️ Tech Stack

| Category | Tools |
|---|---|
| Language | Python |
| Web Framework | Flask, Flask-Session |
| Modeling | Scikit-learn (Random Forest) |
| Data Handling | Pandas, NumPy |
| NLP / Matching | Regex, difflib (fuzzy matching) |

## 📁 Project Structure

```
├── app.py                              # Flask app: model training + conversational state machine
├── Data/
│   ├── Training.csv                    # Symptom-disease training data
│   └── Testing.csv                     # Held-out evaluation data
├── MasterData/
│   ├── symptom_Description.csv         # Disease descriptions
│   ├── symptom_severity.csv            # Symptom severity weights
│   └── symptom_precaution.csv          # Precautionary advice per disease
├── templates/
│   └── index.html                      # Chat UI
└── static/                             # CSS/JS assets
```

## 🚀 Getting Started

```bash
# Clone the repo
git clone https://github.com/souravvrc/ai-health-chatbot.git
cd ai-health-chatbot

# Install dependencies
pip install flask flask-session pandas numpy scikit-learn

# Run the app
python app.py
```
Then open `http://127.0.0.1:5000` in your browser.

## 💡 Key Takeaway

This project goes beyond a single-shot classifier by wrapping prediction in a stateful, multi-turn conversation — using synonym mapping and fuzzy matching to handle imprecise natural-language symptom descriptions, and disease-specific follow-up questions to refine the diagnosis before presenting a final result.

---

*Built as part of a personal data science portfolio.*
