# Comparative Analysis of Transformer Models for NLP Tasks

A notebook-based NLP project organized around sentiment classification with transformer models, including BERT, RoBERTa, and GPT-2 fine-tuning experiments.

## Repository contents

| File | Purpose |
|---|---|
| `P15_Comparative_Analysis_of_Transformer_Models_for_NLP_Tasks.ipynb` | Data inspection, preprocessing, splitting, and transformer experiments |
| `README.md` | Project documentation |

## Dataset profile recorded in the notebook

The saved notebook output reports **20,559 rows × 2 columns** after loading `IMDB Dataset.csv`. The two fields shown are `review` and `sentiment`.

**Important reproducibility gap:** `IMDB Dataset.csv` does not appear in the repository root file inventory. The notebook expects this file, so a fresh checkout will not reproduce the dataset loading step unless the file is obtained from its original authorized source and placed at the expected path. Confirm whether `on_bad_lines="skip"` removed malformed records before reporting the dataset dimensions as source-dataset size.

## Notebook workflow

1. Load the review text and sentiment labels.
2. Inspect dataset shape and label distribution.
3. Perform review-length exploration and select a sequence truncation strategy.
4. Split data for model development.
5. Tokenize text with Hugging Face Transformers.
6. Fine-tune the transformer architectures implemented in the notebook.
7. Compare performance using consistent validation/test data and appropriate classification metrics.

## Tools

Python, pandas, NLTK, scikit-learn, Hugging Face Transformers, PyTorch, and Jupyter/Colab, based on notebook imports and cells.

The notebook documentation recommends Python 3.10 in a Conda environment; dependency versions and hardware requirements should be recorded for reproducibility.

## Results and validation status

The notebook contains a section for comparing BERT, RoBERTa, and GPT-2, but the connector-visible saved outputs did not provide a complete, comparable result table for accuracy, F1, training time, or inference latency. No model winner or performance percentage is claimed here.

For a defensible comparison, publish:
- Exact dataset source and final row count after preprocessing.
- Train/validation/test split and random seed.
- Per-model accuracy, precision, recall, F1, and confusion matrix.
- Training environment, model configuration, sequence length, and epochs.
- Runtime/latency measurements only if actually recorded.
- A clear indication of which result is validation versus final test performance.

## Reproduce

1. Create a Conda environment with a compatible Python version.
2. Install the libraries imported by the notebook, including Transformers and PyTorch.
3. Obtain the dataset from an authorized source; do not commit restricted data.
4. Place it at the path expected by the notebook or update the loading cell.
5. Run the notebook from the first cell and save the result tables for all models.

A root-level `requirements.txt` was not found in the repository inventory.

## Limitations

- The required CSV is not included in the repository.
- Notebook outputs do not establish a verified, apples-to-apples model comparison by themselves.
- Transformer scores depend on model/version, training budget, split, and hardware.
- Avoid publishing review text beyond the source dataset's license.

## Skills demonstrated

NLP data preparation, text classification, tokenizer workflows, transformer fine-tuning, evaluation design, and reproducible experiment documentation.

## Author

Sahil Gaikwad · [GitHub](https://github.com/sahil2003-ai) · [LinkedIn](https://www.linkedin.com/in/sahil-gaikwad-a76b7224a/)
