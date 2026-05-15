# HamOrSpam

**AI-Powered Email Classification System**

[![Python Version](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Framework](https://img.shields.io/badge/framework-Streamlit-ff4b4b.svg)](https://streamlit.io/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Accuracy](https://img.shields.io/badge/accuracy-98.2%25-brightgreen)](#performance)

HamOrSpam is a production-grade spam detection system that combines TF-IDF vectorization with logistic regression to classify email messages with greater than 98% accuracy. The application ships with a Streamlit-based analytics interface, real-time inference, and model export capabilities.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Architecture](#architecture)
- [Dataset](#dataset)
- [Performance](#performance)
- [Installation](#installation)
- [Usage](#usage)
- [Configuration](#configuration)
- [API Reference](#api-reference)
- [Deployment](#deployment)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

---

## Overview

Email spam detection remains a core challenge in messaging infrastructure. HamOrSpam addresses this with a lightweight yet high-performing classification pipeline trained on the Enron Spam Dataset. The system handles class imbalance via SMOTE oversampling and exposes both a web UI and a REST-compatible prediction endpoint.

---

## Features

- Real-time spam classification with calibrated probability scoring
- Interactive analytics dashboard displaying model performance metrics
- Word cloud visualizations highlighting key spam and ham indicators
- SMOTE-based training pipeline to handle imbalanced class distributions
- Exportable trained model artifacts for downstream integration
- Dark-mode Streamlit UI with responsive layout

---

## Architecture

```
Raw Email Text
      |
      v
TF-IDF Vectorization       (max_features configurable, default: 3000)
      |
      v
SMOTE Class Balancing      (applied during training only)
      |
      v
Logistic Regression        (sklearn, threshold configurable)
      |
      v
Spam / Ham Prediction      (label + confidence score)
```

The pipeline is intentionally lean. TF-IDF captures term importance relative to the corpus without requiring dense embeddings, and logistic regression provides well-calibrated probability outputs suitable for threshold tuning.

---

## Dataset

| Property        | Detail                                                                 |
|-----------------|------------------------------------------------------------------------|
| Source          | [Enron Spam Dataset](http://www2.aueb.gr/users/ion/data/enron-spam/)   |
| Total Samples   | 5,000+                                                                 |
| Class Balance   | 70% Ham / 30% Spam (pre-SMOTE)                                         |
| Label Format    | Binary — `spam` / `ham`                                                |

---

## Performance

Evaluated on a held-out test split after SMOTE-balanced training.

| Metric    | Score  |
|-----------|--------|
| Accuracy  | 98.2%  |
| Precision | 97.8%  |
| Recall    | 98.5%  |
| F1 Score  | 98.1%  |

---

## Installation

### Prerequisites

- Python 3.8 or higher
- pip

### Steps

```bash
# Clone the repository
git clone https://github.com/codewithshami/HamOrSpam-Classifier.git
cd HamOrSpam-Classifier

# Create and activate a virtual environment
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirement.txt
```

---

## Usage

### Launch the Application

```bash
streamlit run app.py
```

The application will be available at `http://localhost:8501` by default.

### Classifying a Message

1. Navigate to the **Prediction** tab in the sidebar.
2. Paste or type the email content into the input area.
3. Click **Analyze Message**.
4. Review the classification label, confidence score, and probability distribution.
5. Optionally inspect the keyword analysis panel for feature-level explanations.

### Analytics Dashboard

The **Analytics** tab surfaces model performance metrics, confusion matrix visualizations, word frequency distributions, and dataset statistics without requiring any additional configuration.

---

## Configuration

Create a `.env` file in the project root to override default settings:

```env
DEBUG_MODE=False
THRESHOLD=0.85
MAX_FEATURES=3000
```

| Variable       | Default | Description                                         |
|----------------|---------|-----------------------------------------------------|
| `DEBUG_MODE`   | `False` | Enables verbose logging when set to `True`          |
| `THRESHOLD`    | `0.85`  | Minimum spam probability required for a spam label  |
| `MAX_FEATURES` | `3000`  | Number of features retained by the TF-IDF vectorizer|

Raising `THRESHOLD` reduces false positives at the cost of recall. Lowering it increases sensitivity. Adjust based on the tolerance of the deployment environment.

---

## API Reference

### POST `/predict`

Classifies a single email message and returns a confidence-scored result.

**Parameters**

| Name      | Type   | Required | Description              |
|-----------|--------|----------|--------------------------|
| `message` | string | Yes      | Raw email content to classify |

**Response**

```json
{
    "prediction": "spam",
    "confidence": 0.95,
    "spam_probability": 0.92,
    "ham_probability": 0.08
}
```

**Response Fields**

| Field              | Type   | Description                                      |
|--------------------|--------|--------------------------------------------------|
| `prediction`       | string | Classification result — `spam` or `ham`          |
| `confidence`       | float  | Model confidence in the prediction (0.0 to 1.0)  |
| `spam_probability` | float  | Raw probability assigned to the spam class       |
| `ham_probability`  | float  | Raw probability assigned to the ham class        |

---

## Tech Stack

| Layer           | Technology                              |
|-----------------|-----------------------------------------|
| Backend         | Python, Streamlit                       |
| Machine Learning| scikit-learn, imbalanced-learn (SMOTE)  |
| Data Processing | pandas, numpy                           |
| Visualization   | matplotlib, seaborn, wordcloud          |
| Deployment      | Streamlit Cloud                         |

---

## Deployment

### Streamlit Cloud

Push the repository to GitHub, connect it to [Streamlit Cloud](https://streamlit.io/cloud), and set any required environment variables in the Secrets panel. The service will handle the rest.

### Self-Hosted

```bash
pip install streamlit
pip install -r requirement.txt
streamlit run app.py --server.port 8501 --server.headless true
```

For production deployments, place the application behind a reverse proxy such as Nginx and manage the process with a supervisor like `systemd` or `supervisord`.

---

## Contributing

Contributions are welcome. Please follow the workflow below:

1. Fork the repository.
2. Create a feature branch: `git checkout -b feature/your-feature-name`
3. Commit your changes with a descriptive message: `git commit -m "Add: brief description of change"`
4. Push the branch: `git push origin feature/your-feature-name`
5. Open a Pull Request against the `main` branch.

Please ensure new code includes appropriate tests and does not break existing functionality before submitting a PR.

---

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for the full text.

---

## Contact

**Mohd Shami**

- Email: [codexshami@gmail.com](mailto:codexshami@gmail.com)
- LinkedIn: [linkedin.com/in/mohd-shami-792133276](https://www.linkedin.com/in/codexshami)
- Repository: [github.com/codewithshami/HamOrSpam-Classifier](https://github.com/codewithshami/hamOrSpam-Classifier)
