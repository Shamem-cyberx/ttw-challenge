# Outputs (Challenge 2 deliverables)

These files support **evaluation**, **figures**, and **loading trained LoRA weights**. The whole `outputs/` tree (including `checkpoints/` from training) is intended to be **committed** so nothing is missing from the submission. Large files may require [Git LFS](https://git-lfs.github.com/) if you hit GitHub’s size limits.

## What to expect after a full run

| Path | Purpose |
|------|---------|
| `loss_curve.png` | Train vs validation loss (from notebook 01) |
| `trainer_log_history.json` | Raw loss log for plotting / reports |
| `training_summary.json` | Final `TrainingArguments` snapshot + runtime |
| `lora_adapters/` | **LoRA weights** + adapter config (+ tokenizer files saved by the trainer) |
| `eval_metrics.json` | Corpus BLEU / ROUGE-L / format compliance — base vs fine-tuned |
| `eval_per_example.json` | Per-example ROUGE-L deltas (for histograms) |
| `eval_metrics_bar.png`, `eval_rouge_delta_hist.png` | Evaluation plots (from notebook 02) |
| `qualitative_examples.md` | Side-by-side generations |
| `challenge2_output_manifest.json` | File-presence audit (from notebook 02) |
| `demo_query_snapshot.*` | Optional demo outputs |

## `checkpoints/`

Hugging Face `Trainer` writes step checkpoints under `outputs/checkpoints/` (includes optimizer state; can be large). Include them in the repo for a **complete** artifact bundle, or omit and re-run `notebooks/01_dataset_and_train.ipynb` if you prefer a smaller clone. The **best** adapter weights are also copied to `outputs/lora_adapters/`.
