## Processed dataset files

This folder is populated by the dataset generation pipeline (reproducible).

### Generate in Colab (recommended)

Run all cells in:

- `notebooks/01_dataset_and_train.ipynb`

It will write:

- `train.jsonl` (220 examples)
- `valid.jsonl` (20 examples)
- `test.jsonl` (20 examples)

### Generate locally (optional)

If you have Python 3 installed:

```bash
python scripts/generate_dataset.py
```

