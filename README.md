# Classification of Tweets from Northern Europe

Welcome to the **Classification of Tweets from Northern Europe** repository! This project aims to classify over 500,000 political tweets using Natural Language Processing (NLP) and machine learning techniques. Our goal is to analyze political discourse across Northern Europe and provide insights into public sentiment and trends.

## Table of Contents

1. [Project Overview](#project-overview)
2. [Dataset](#dataset)
3. [Technologies Used](#technologies-used)
4. [Installation](#installation)
5. [Usage](#usage)
6. [Results](#results)
7. [Contributing](#contributing)
8. [License](#license)

## Project Overview

In recent years, social media has become a crucial platform for political discussion. This project leverages NLP and machine learning to classify tweets, focusing on political discourse in Northern Europe. By analyzing these tweets, we aim to uncover trends, sentiments, and patterns in political conversations.

### Objectives

- Classify tweets based on political leaning (`Left`, `Center`, `Right`).
- Use machine learning algorithms to enhance classification accuracy.
- Visualize the results to present insights effectively.

## Dataset

The dataset consists of political tweets from Northern Europe, split into a training set (`training_data.xlsx`) and a test set (`test_data.xlsx`), expected under a local `northern-europe-datamining/` directory (not included in this repository). Each tweet includes metadata such as `full_text`, `hashtags`, `country_user`, `gender_user`, and `pol_spec_user` (the political-leaning label used as the classification target).

## Technologies Used

This project utilizes a variety of technologies and libraries to process and analyze the data:

- **Python**: The primary programming language used for data analysis and modeling.
- **Pandas / NumPy**: For data manipulation and numerical operations.
- **Scikit-learn**: For feature extraction (TF-IDF, count vectors), topic modeling (NMF, LDA), and classification (LinearSVC).
- **NLTK**: For stopword removal, POS tagging, and lemmatization.
- **clean-text** / **langdetect**: For tweet cleaning and language detection.
- **Matplotlib & Seaborn**: For data visualization.

## Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/SamTheOneee1/kaggle-project-classification-of-tweets-from-northern-europe.git
   cd kaggle-project-classification-of-tweets-from-northern-europe
   ```

2. Install the required packages (see `CLAUDE.md` for the full list) and launch Jupyter:

   ```bash
   pip install pandas numpy scikit-learn nltk matplotlib seaborn clean-text langdetect openpyxl jupyter
   jupyter notebook kaggle_project.ipynb
   ```

## Usage

Place `training_data.xlsx` and `test_data.xlsx` in a `northern-europe-datamining/` directory at the repository root, then run the cells in `kaggle_project.ipynb` in order. See `CLAUDE.md` for a breakdown of the notebook's pipeline stages.

## Results

Running the notebook produces:

- Descriptive statistics and visualizations of tweet/hashtag length, top hashtags by country, and political/gender distributions.
- Topic modeling output (NMF/LDA) over cleaned tweet text.
- A trained SVM classifier, its cross-validated accuracy and confusion matrix, and a submission CSV (`submission_north_europe_svc_final.csv`) with predicted political leaning per tweet ID.

A written summary of the analysis is available in `kaggle_project_report.pdf`.

## Contributing

We welcome contributions to improve this project. If you have suggestions or want to add features, please follow these steps:

1. Fork the repository.
2. Create a new branch for your feature.
3. Make your changes and commit them.
4. Push to your branch and create a pull request.

## License

This project is licensed under the BSD 3-Clause License. See the `LICENSE` file for details.
