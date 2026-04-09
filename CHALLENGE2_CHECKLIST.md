# Challenge 2 (Generative AI) — Compliance Checklist

This repo targets **Technical Assignment — Option 2** only: **fine-tune a &lt;1B model with LoRA/QLoRA**, dataset + evaluation + documentation.

## Core requirements

| Requirement | How this repo satisfies it |
|-------------|------------------------------|
| Model &lt; 1B params | `Qwen/Qwen2.5-0.5B-Instruct` |
| LoRA / QLoRA only | 4-bit base + LoRA adapters via **Unsloth** |
| PEFT stack | **PEFT** (via Unsloth `get_peft_model`) + **TRL** `SFTTrainer` |
| Dataset ≥ 200 examples + splits | **260** examples: 220 train / 20 valid / 20 test (`data/processed/*.jsonl` after running notebook 01) |
| Instruction–response format | `### Instruction:` / `### Response:` in SFT `text` field |
| Data card | `data/data_card.md` |
| Hyperparameters documented | Comments in `01_dataset_and_train.ipynb` + `model_card.md` |
| Training &lt; ~2h on Colab free T4 | Defaults: **2 epochs**, `max_seq_length=1024`, early stopping on `eval_loss` |
| Early stopping | `EarlyStoppingCallback` on validation loss |
| Checkpoints | `outputs/checkpoints/` + `save_total_limit` (commit in repo for a complete bundle; optional Git LFS if large) |
| Loss curves | `outputs/loss_curve.png` + `trainer_log_history.json` |
| Training summary (final metrics) | `outputs/training_summary.json` (written by notebook **01** after `train()`) |
| Eval + artifact audit | `outputs/eval_metrics.json` (`meta`, `warnings`, `base`, `fine_tuned`), `outputs/challenge2_output_manifest.json` (notebook **02**) |
| Metrics ≥ 2 task-specific | **BLEU**, **ROUGE-L**, **format compliance** (code fences + cmdlet-like tokens) |
| Base vs fine-tuned | `02_evaluation.ipynb` |
| Qualitative + error analysis | `outputs/qualitative_examples.md` |

## Deliverables (submission folder)

- **Training notebook**: `notebooks/01_dataset_and_train.ipynb`
- **Evaluation notebook**: `notebooks/02_evaluation.ipynb`
- **Model card**: `model_card.md`
- **Dataset**: generated JSONL + `data/data_card.md`; generator script `scripts/generate_dataset.py`
- **Environment**: `requirements.txt`
- **README**: `README.md`

## Bonus ideas (pick 2+ for write-up)

- **Inference optimization**: 4-bit loading + LoRA merge discussion in `model_card.md`
- **Prompt baseline**: compare base model without adapters vs fine-tuned (evaluation notebook)
- **Catastrophic forgetting**: add a few general prompts in a new cell and compare (optional extension)

## What we cannot do from Cursor

Colab **T4** selection and **Run all** must be done in your browser (`Runtime → Change runtime type → T4 GPU`). This repo is configured so a single **Run all** on notebook 01 then 02 completes the pipeline under `/content/tatweer_challenge/` when cloned in Colab.
