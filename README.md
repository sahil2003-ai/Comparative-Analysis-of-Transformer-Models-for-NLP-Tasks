# Comparative Analysis of Transformer Models for NLP Tasks

A notebook-based NLP experiment comparing transformer architectures for **binary sentiment classification of IMDb movie reviews**. The project documents dataset profiling, review-length analysis, sequence truncation, train/test splitting and a planned fine-tuning workflow for BERT, RoBERTa and GPT-2.

<p align="left">
  <img src="https://img.shields.io/badge/Python-3.10-3776AB?logo=python&logoColor=white" alt="Python 3.10" />
  <img src="https://img.shields.io/badge/NLP-Sentiment%20Analysis-6A5ACD" alt="Sentiment Analysis" />
  <img src="https://img.shields.io/badge/Transformers-BERT%20%7C%20RoBERTa%20%7C%20GPT--2-F7931E" alt="Transformer models" />
  <img src="https://img.shields.io/badge/Framework-PyTorch-EE4C2C?logo=pytorch&logoColor=white" alt="PyTorch" />
  <img src="https://img.shields.io/badge/Notebook-Jupyter-F37626?logo=jupyter&logoColor=white" alt="Jupyter" />
</p>

## Project overview

The goal is to compare transformer-based models for classifying movie-review text as **positive** or **negative**. The notebook is structured around the IMDb movie review dataset, uses a stratified train/test split, and contains a fine-tuning function for three transformer model families.

The repository currently contains the notebook and README. The dataset file referenced in the notebook is not included in the repository tree, so you must obtain it from an authorized source and place it at the expected path before running the analysis.

## Project objective

- Inspect the text dataset, schema and target-label distribution.
- Examine review-length patterns and long-review outliers.
- Create a reproducible train/test split.
- Truncate long reviews to manage sequence and compute requirements.
- Fine-tune BERT, RoBERTa and GPT-2 for binary sequence classification.
- Compare predictive quality and efficiency using a consistent evaluation setup.
- Document resource needs, model versions and reproducibility requirements.

## Dataset

The notebook loads `IMDB Dataset.csv` and expects two columns:

| Column | Meaning |
|---|---|
| `review` | Full text of an IMDb movie review |
| `sentiment` | Sentiment label: `positive` or `negative` |

### Dataset profile recorded in the notebook

| Metric | Saved notebook result |
|---|---:|
| Rows loaded | 20,559 |
| Columns in original loaded table | 2 |
| Positive reviews | 10,189 |
| Negative reviews | 10,370 |
| Missing values in the two profiled columns | 0 |
| Average tokenized word count | 279.85 |
| Median tokenized word count | 207 |
| 75th percentile word count | 341 |
| Maximum tokenized word count | 2,300 |

**Dataset note:** these statistics are from the notebook's saved output. The commonly distributed full IMDb review dataset may contain a different number of records; this project run specifically loaded 20,559 records after the CSV-reading step. The notebook uses `on_bad_lines="skip"`, so malformed rows may have been omitted. Record the exact source/version and verify the final row count when reproducing the experiment.

### Label mapping

The notebook maps labels to integers:

| Sentiment | Integer label |
|---|---:|
| Positive | 0 |
| Negative | 1 |

The class distribution in the loaded data is close to balanced, with 10,189 positive and 10,370 negative rows.

## Preprocessing and data preparation

The existing notebook's first part covers:

1. Read `IMDB Dataset.csv` with pandas.
2. Inspect shape, data types, and sentiment counts.
3. Map sentiment labels to integers.
4. Tokenize reviews with NLTK to inspect text length.
5. Create a `truncated_review` field by retaining up to 400 whitespace-separated words.
6. Create a stratified 80/20 train/test split with `random_state=42`.
7. Convert the split into Hugging Face `Dataset` objects for transformer processing.

The transformer tokenization function applies padding and truncation at **256 tokens**. Note that this differs from the earlier preprocessing step's 400 whitespace-separated-word cap: those are two separate length controls and should be documented distinctly.

## Transformer models

The fine-tuning section defines these checkpoints:

| Model | Checkpoint | Role in experiment |
|---|---|---|
| BERT | `bert-base-uncased` | Bidirectional encoder baseline |
| RoBERTa | `roberta-base` | Robustly optimized BERT-style encoder |
| GPT-2 | `gpt2` | Decoder-based transformer used for sequence classification |

The notebook defines a shared training function with the following settings:

| Parameter | Configured value |
|---|---:|
| Epochs | 1 |
| Per-device training batch size | 8 |
| Gradient accumulation steps | 2 |
| Learning rate | 2e-5 |
| Weight decay | 0.01 |
| Warmup steps | 500 |
| Maximum tokenizer length | 256 |
| Mixed precision | Enabled when CUDA is available |
| Trainer evaluation during training | Disabled in the current configuration |
| Trainer checkpoint saving | Disabled in the current configuration |

The code creates train and test datasets, but the configured `Trainer` is passed the training split only. It does not evaluate the held-out test split in the recorded run.

## Results and metrics

### Confirmed data-preparation results

| Measure | Value |
|---|---:|
| Records in loaded notebook dataset | 20,559 |
| Train split (80%) | 16,447 |
| Test split (20%) | 4,112 |
| Train/test strategy | Stratified split |
| Random seed | 42 |
| Text cap before model tokenization | 400 whitespace-separated words |
| Transformer tokenizer maximum length | 256 tokens |

The saved notebook output shows the fine-tuning process starting with BERT, but it does not contain a completed comparative evaluation table for BERT, RoBERTa and GPT-2. **No model accuracy or F1 score is claimed in this README**, since these metrics could not be verified from the committed notebook outputs.

### Metrics to add after completing the evaluation

To make a valid comparison, evaluate each model on the same untouched 4,112-record test set and report:

- Accuracy
- Precision, recall and F1-score for each class
- Macro-F1 and weighted-F1
- Confusion matrix
- Training duration and inference latency
- Peak GPU memory or other resource use, if available
- Model checkpoint/version, test-split details and random seed

Because training currently has `eval_strategy="no"`, add a separate evaluation step using the held-out test dataset after fine-tuning. Use the same class mapping and metrics for all model variants before ranking them.

## Workflow

```text
IMDb review CSV
      │
      ▼
Schema + label distribution checks
      │
      ▼
Review-length analysis
      │
      ▼
Create 400-word capped review text
      │
      ▼
Stratified 80/20 split (seed = 42)
      │
      ▼
Hugging Face Dataset + tokenizer (max_length = 256)
      │
      ├── BERT fine-tuning
      ├── RoBERTa fine-tuning
      └── GPT-2 fine-tuning
      │
      ▼
Held-out evaluation to be completed
      │
      ▼
Compare quality, runtime and resource use
```

## Visualizations

The notebook includes a **review-length distribution histogram**, which helps show the concentration of short-to-medium reviews and the presence of longer outliers.

The repository currently contains only the notebook and README; there are no separate image or screenshot files to embed. The histogram exists as a notebook output, not as a standalone PNG. To display real images on the README landing page, export the notebook chart(s) into an `images/` directory after a successful run and reference those actual files. No fabricated screenshots or unrelated images are used here.

## Technology stack

| Area | Tools |
|---|---|
| Language | Python 3.10 (as specified in the environment instructions) |
| Data handling | pandas, NumPy |
| Visualization | Matplotlib, Seaborn |
| NLP preprocessing | NLTK |
| Dataset framework | Hugging Face Datasets |
| Transformer models | Hugging Face Transformers |
| Training framework | PyTorch, Hugging Face Trainer |
| Evaluation utilities | scikit-learn |
| Environment | Conda, Jupyter / ipykernel |

The notebook's environment instructions pin versions including pandas 1.5.3, NumPy 1.24.4, Matplotlib 3.7.3, Seaborn 0.12.2, NLTK 3.8.1, PyTorch 2.1.2, Transformers 4.36.2, scikit-learn 1.3.2, Accelerate 0.25.0 and Datasets 2.16.1. These are the notebook's documented setup versions, not a guarantee that every later package version is compatible.

## Repository structure

```text
Comparative-Analysis-of-Transformer-Models-for-NLP-Tasks/
├── P15_Comparative_Analysis_of_Transformer_Models_for_NLP_Tasks.ipynb
└── README.md
```

The notebook expects `IMDB Dataset.csv` in its current working directory, but that CSV is not stored in this repository. Do not commit datasets unless their license and redistribution conditions permit it.

## Run the notebook

### 1. Clone the project

```bash
git clone https://github.com/sahil2003-ai/Comparative-Analysis-of-Transformer-Models-for-NLP-Tasks.git
cd Comparative-Analysis-of-Transformer-Models-for-NLP-Tasks
```

### 2. Create and activate the Conda environment

```bash
conda create -n nlp_env python=3.10 -y
conda activate nlp_env
python -m pip install --upgrade pip
```

### 3. Install the documented packages

```bash
pip install pandas==1.5.3 numpy==1.24.4 matplotlib==3.7.3 seaborn==0.12.2 nltk==3.8.1
pip install torch==2.1.2 transformers==4.36.2 scikit-learn==1.3.2 accelerate==0.25.0 datasets==2.16.1
pip install ipykernel jupyter
python -m ipykernel install --user --name nlp_env --display-name "Python (nlp_env)"
```

### 4. Provide the dataset

Obtain `IMDB Dataset.csv` from an authorized source, verify its terms of use, and place it at the path expected by the notebook. Check row counts and class distribution before training.

### 5. Run the notebook

```bash
jupyter notebook P15_Comparative_Analysis_of_Transformer_Models_for_NLP_Tasks.ipynb
```

Select the `Python (nlp_env)` kernel and run cells in order. Fine-tuning large transformer checkpoints can require substantial runtime, memory and (ideally) a compatible GPU. The notebook output includes model-download and training logs; public checkpoints may be downloaded from the Hugging Face Hub.

## Limitations and next steps

- **Incomplete benchmark:** complete fine-tuning and save evaluation results for every model.
- **No test evaluation in current Trainer config:** implement explicit prediction against the held-out set after training.
- **Reproducibility:** set and record seeds for Python, NumPy, PyTorch and model/data splitting.
- **Data provenance:** record the dataset URL/source, version, license, and any reason rows were skipped.
- **Preprocessing clarity:** explain the 400-word cap and 256-token tokenizer limit separately.
- **Model comparison fairness:** use identical train/test partitions and metrics across architectures.
- **Compute reporting:** record device, runtime, batch settings and memory requirements.
- **Visual documentation:** export genuine charts from the notebook and add them under `images/`.
- **Checkpoint storage:** document how to save trained model/tokenizer artifacts if results need to be reused.

## Skills demonstrated

NLP data preparation · binary sentiment classification · review-length analysis · stratified sampling · transformer tokenization · BERT · RoBERTa · GPT-2 · Hugging Face Datasets · PyTorch Trainer · experiment configuration · model evaluation design · reproducibility documentation

## Author

**Sahil Gaikwad**  
[GitHub](https://github.com/sahil2003-ai) · [LinkedIn](https://www.linkedin.com/in/sahil-gaikwad-a76b7224a/)
