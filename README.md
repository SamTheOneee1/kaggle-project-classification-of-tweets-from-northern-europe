# 🐦 Kaggle Project: Classification of Tweets from Northern Europe

![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

## 🚀 Overview

This project analyzes a dataset of **509,031 tweets** from politicians across **seven Northern European countries**, aiming to classify tweets by **political spectrum (Left, Right, Center)** and **geography**. Using **Natural Language Processing (NLP)** and machine learning, we extracted key insights into political discourse and developed a classifier with strong performance.

---

## 📊 Dataset

- **Countries:** Belgium, Denmark, Iceland, Ireland, Netherlands, Norway, Sweden
- **Time span:** Dec 2008 – Jan 2023
- **Dataset size:** 509,031 tweets
    - **Training set:** 407,223 tweets
    - **Test set:** 101,808 tweets

**Attributes:**
- Tweet text
- Country
- Gender
- Political spectrum label (for supervised learning)

---

## 🔬 Methods

### Data Cleaning & Preprocessing

- Text cleaning: Lowercasing, punctuation removal, stopword removal, lemmatization.
- New feature creation: `text_clean_country_gender_user` combining multiple metadata.

### Feature Engineering

- **Vectorization:**
    - Count Vectorizer
    - TF-IDF transformation

### Modeling

- **Classifier:** Linear Support Vector Classifier (LinearSVC)
- **Validation:** 10-fold cross-validation

### Topic Modeling

- **LDA (Latent Dirichlet Allocation)**
- **NMF (Non-negative Matrix Factorization)**

---

## 📈 Results

| Metric                      | Value                      |
|-----------------------------|----------------------------|
| Model Accuracy              | 77.38%                     |
| Baseline Accuracy           | ~42.87%                    |
| Topic Modeling Insights     | Identified key political & social themes |
| Hashtag Analysis            | Mapped top hashtags per country |

✅ **Key findings:**
- Tweets span diverse topics from **domestic politics** to **international relations**.
- Different political leanings across countries: e.g., Netherlands leans Center, Iceland leans Left.
- Gender distribution varies: Sweden had a **female-majority**, others skewed male.

---

## 🛠️ Requirements

- Python 3.8+
- Key libraries:
    - pandas, numpy
    - scikit-learn
    - nltk, spaCy
    - gensim
    - matplotlib, seaborn

Install via:

```bash
pip install -r requirements.txt
```

---

## 🖥️ Usage

1️⃣ **Clone the repository:**

```bash
git clone https://github.com/AnakinHuang/kaggle-project-classification-of-tweets-from-northern-europe.git
cd kaggle-project-classification-of-tweets-from-northern-europe
```

2️⃣ **Install dependencies:**

```bash
pip install -r requirements.txt
```

3️⃣ **Run analysis:**
- Open `kaggle_project.ipynb` for full preprocessing, modeling, and evaluation workflows.

---

## 📂 Repository Structure

```
├── data/                       # Dataset (not included)
├── kaggle_project.ipynb        # Main Jupyter notebook
├── kaggle_project_report.pdf   # Final project report
├── README.md
├── requirements.txt            # Dependencies
└── LICENSE
```

---

## 👥 Contributors

- **Yuesong Huang** (yhu116@u.rochester.edu)
- **Junhua Huang** (jhuang77@u.rochester.edu)

---

## 📄 License

This project is licensed under the MIT License – see the [LICENSE](LICENSE) file for details.

---
