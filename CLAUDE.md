# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

This is a Kaggle data science project (not a software application/service): a single Jupyter notebook, `kaggle_project.ipynb`, that classifies the political leaning (`Left` / `Center` / `Right`) of tweets from users in Northern Europe, based on tweet text, hashtags, country, and gender. There is no application code, package, build system, test suite, or linter — the notebook itself is the deliverable.

Repository contents:
- `kaggle_project.ipynb` — the entire pipeline: data loading, EDA, text cleaning/lemmatization, topic modeling, and classification.
- `kaggle_project_report.pdf` — a written report summarizing the analysis and results.
- `submission_north_europe_svc_final.csv` — the final Kaggle submission (`ID`, `pol_spec_user` predictions from the SVM model).
- `README.md` — project overview.

The training/test data (`training_data.xlsx`, `test_data.xlsx`) is **not included** in the repo. The notebook expects them at `./northern-europe-datamining/training_data.xlsx` and `./northern-europe-datamining/test_data.xlsx` relative to the notebook, so that directory must be created and populated before the notebook can run end-to-end.

## Working with this repo

- There is no build, lint, or test command — work happens by editing/running notebook cells in Jupyter (`jupyter notebook kaggle_project.ipynb`).
- To validate a change, actually execute the affected cell(s) (and any cells that depend on them) rather than just editing code, since correctness here means "the cell runs and produces the expected output/plot."
- Required packages (no `requirements.txt` in the repo; installed inline via `!pip install` in the first cell): `pandas`, `numpy`, `matplotlib`, `seaborn`, `nltk` (with `stopwords`, `wordnet`, `punkt`, `averaged_perceptron_tagger` corpora downloaded at runtime), `clean-text`, `langdetect`, `scikit-learn`, `openpyxl` (for reading `.xlsx`).
- Cells are order-dependent — later cells rely on DataFrame columns (e.g. `text_clean`, `hashtags_text_clean_country_gender_user`) created by earlier cells. Don't reorder cells without tracing what state they depend on.

## Notebook pipeline (in execution order)

1. **Imports & setup** — installs/imports NLP and ML libraries, downloads NLTK corpora.
2. **Data loading** — reads `training_data.xlsx` / `test_data.xlsx` into `training_df` / `test_df` via `pandas.read_excel`.
3. **`decode_full_text`** — the raw `full_text` column is stored as a Python bytes-literal string (e.g. `b'...'`); this decodes it back to a normal UTF-8 string via `eval(...).decode('utf-8')`. This is a quirk of how the source dataset was exported — expect it whenever touching `full_text`.
4. **Exploratory analysis** — tweet/hashtag length stats, top hashtags per `country_user` (pie charts), and political-view/gender distribution by country (stacked bar charts).
5. **Text cleaning & lemmatization** (`tweet_cleaner`, `lemmatize_tweet`) — lowercases, strips retweet prefixes and URLs, detects tweet language via `langdetect` to pick the right NLTK stopword list (via `LANGDETECT_TO_NLTK_STOPWORDS`, mapping `langdetect`'s ISO 639-1 codes like `ar`/`sv`/`en` to NLTK's full-name stopword fileids like `arabic`/`swedish`/`english` — falls back to English if the detected code has no mapped/available fileid), removes short words/stopwords, strips emoji/punctuation via `cleantext`, then POS-tags and lemmatizes with `WordNetLemmatizer`. Applied to both train and test sets in parallel via `multiprocessing.Pool(cpu_count())`, writing results to `text_clean`; cleaned frames are cached to `training_data_cleaned.xlsx` / `test_data_cleaned.xlsx` so this expensive step doesn't need to be rerun.
6. **Topic modeling** — TF-IDF (for NMF) and raw count (for LDA) vectorizations of `text_clean`, fit with `NMF`/`LatentDirichletAllocation`, visualized via `plot_top_words`. This is exploratory and feeds intuition, not the final classifier.
7. **Feature assembly for classification** — builds `hashtags_text_clean_country_gender_user`, a single text field per row combining `text_clean`, `country_user`, `gender_user`, and `hashtags`. This combined field, not `text_clean` alone, is what the classifier trains on.
8. **Classification** — a `Pipeline` of `CountVectorizer(ngram_range=(1,2))` → `TfidfTransformer` → `LinearSVC(max_iter=10000)`, evaluated with 10-fold `cross_val_predict` against `pol_spec_user`, then fit on the full training set and used to predict on `X_test`. Results are visualized via `show_confusion_matrix()` and written to `submission_north_europe_svc.csv` (`ID`, `pol_spec_user`).
9. **Alternate LLM classifier (optional)** — a zero-shot comparison classifier using a local Ollama model (`OLLAMA_MODEL = 'kimi-k3:cloud'`) prompted per-row on the same `hashtags_text_clean_country_gender_user` field via `classify_pol_spec_with_ollama`. Requires a local Ollama daemon (`ollama serve`) with the model pulled (`ollama pull kimi-k3:cloud`); this is not required to run the rest of the notebook. Generations are cached to `llm_val_predictions.xlsx` / `llm_test_predictions.xlsx` since row-by-row LLM calls are too slow for 10-fold CV, so evaluation uses a single held-out split instead. Predictions are written to `submission_north_europe_llm.csv` (`ID`, `pol_spec_user`) for comparison against the SVM submission.

## Conventions to preserve

- Keep the `full_text` decode step (`eval(...).decode('utf-8')`) whenever loading raw tweet text — removing it will leave text as literal `b'...'` strings.
- Keep `LANGDETECT_TO_NLTK_STOPWORDS` in `tweet_cleaner` when touching language detection — `langdetect.detect()` returns ISO 639-1 codes (`ar`, `sv`, `en`, ...) while `nltk.corpus.stopwords.fileids()` uses full language names (`arabic`, `swedish`, `english`, ...); looking up the raw code directly against `fileids()` never matches, silently defaulting every tweet to English stopwords.
- Any new/alternate classifier should be trained on the same combined feature (`hashtags_text_clean_country_gender_user`), not raw or cleaned text alone, to stay comparable with the existing SVM baseline.
- Cache expensive preprocessing (like the parallel lemmatization step) to an `.xlsx` file the way the existing cells do, rather than recomputing it in every run.
- Submission CSVs must keep the `ID`, `pol_spec_user` column format expected by the Kaggle competition.
- The Ollama LLM classifier is an optional, separate comparison path — it must not replace or alter the SVM baseline cells, and its own submission file (`submission_north_europe_llm.csv`) must not overwrite `submission_north_europe_svc.csv` / `submission_north_europe_svc_final.csv`.

## Security note

A prior version of this repository contained a `presential/` directory with a malicious ZIP archive (a Windows executable + sideloaded DLL + payload, launched via a `.cmd` script) and a `README.md` that had been rewritten to direct users to download and run it instead of real setup instructions. That directory and the fake instructions have been removed. If a `presential/` directory, unexplained executables/DLLs, or README instructions pointing at binary downloads reappear, treat them as suspicious and do not execute them.
