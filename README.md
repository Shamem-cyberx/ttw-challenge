## Tatweer technical assessment — **Challenge 2 only** (Generative AI)

The original assignment text is in `docs/assignment/Technical Assignment - AI_ML 1.txt`.

Fine-tune a **&lt;1B** instruction model with **QLoRA** (parameter-efficient) on a domain-specific task, with dataset splits, training stability, evaluation vs base model, and documentation.

### Task

- **Domain**: Windows **PowerShell** technical support + scripting (instruction → response).
- **Data**: 260 instruction–response pairs (220 / 20 / 20 train / validation / test), generated deterministically in `01_dataset_and_train.ipynb` (see `data/data_card.md`).

### Why these settings (Colab **T4**, **&lt; 2h GPU** target)

- **QLoRA** (4-bit + LoRA) fits free-tier VRAM.
- **`max_seq_length=1024`**: sufficient for this dataset; faster than 2048 on T4.
- **2 epochs** with **early stopping** on validation loss: completes quickly while still adapting.
- First run pays a one-time cost: `pip install`, Hugging Face model download (~minutes). **Re-running** training in the same session is much shorter.

### Repo layout (this repository)

Matches Tatweer **§6.1.2 File Organization** for a generative-AI submission: `README`, environment file, `notebooks/`, `data/`, `outputs/`, plus Challenge 2 extras (`model_card.md`, `scripts/`, checklist).

```
├── README.md
├── requirements.txt
├── model_card.md                 # deliverable: Model Card (Markdown)
├── CHALLENGE2_CHECKLIST.md      # rubric → files (helper)
├── docs/
│   └── assignment/
│       └── Technical Assignment - AI_ML 1.txt
├── notebooks/
│   ├── 01_dataset_and_train.ipynb   # training + data prep
│   └── 02_evaluation.ipynb        # eval + plots + qualitative
├── data/
│   ├── data_card.md
│   └── processed/               # train/valid/test JSONL (from notebook 01 or scripts/)
├── scripts/
│   └── generate_dataset.py
└── outputs/                     # metrics, plots, LoRA adapters (see outputs/README.md)
    ├── README.md
    ├── lora_adapters/
    ├── *.json, *.md, *.png
    └── checkpoints/            # HF trainer checkpoints (commit for full artifacts; large — may need Git LFS)
```

**What gets pushed to GitHub:** the full tree above, including `outputs/` (add `git add outputs/` so nothing is left out). Only **local caches** are ignored (see `.gitignore`: `notebooks/huggingface_tokenizers_cache/`, etc.). **Do not** add `outputs/` to `.gitignore`.

### Colab — recommended workflow

1. **Runtime → Change runtime type → GPU** (pick **T4** if offered).
2. **Runtime → Restart session** after switching GPU.
3. Clone and enter the repo:

```bash
%cd /content
!git clone https://github.com/Shamem-cyberx/ttw-challenge.git tatweer_challenge
%cd /content/tatweer_challenge/notebooks
```

Canonical repo: [Shamem-cyberx/ttw-challenge](https://github.com/Shamem-cyberx/ttw-challenge).

4. Open **`01_dataset_and_train.ipynb`** → **Runtime → Run all**.
5. Open **`02_evaluation.ipynb`** → **Runtime → Run all**.

Artifacts (under `/content/tatweer_challenge/`):

- `data/processed/*.jsonl`
- `outputs/lora_adapters/`
- `outputs/loss_curve.png`
- `outputs/trainer_log_history.json`, `outputs/training_summary.json`
- `outputs/eval_metrics.json` (includes `meta`, `warnings`, corpus metrics)
- `outputs/challenge2_output_manifest.json` (after artifact check cell in **02**)
- `outputs/qualitative_examples.md`

### Optional: save to Google Drive

At the **end** of notebook 01, set `SAVE_TO_DRIVE = True` in the last cell, run it, and approve the Drive prompt — or zip/download `/content/tatweer_challenge` from Colab’s file browser.

### Local (optional)

If Python is available: `python scripts/generate_dataset.py` writes `data/processed/*.jsonl`.

### Rubric mapping

See **`CHALLENGE2_CHECKLIST.md`**.

### Submission checklist (Tatweer §6.1.1)

| Item | Location |
|------|----------|
| Required notebooks / code | `notebooks/01_dataset_and_train.ipynb`, `notebooks/02_evaluation.ipynb` |
| Outputs (metrics, plots, adapters) | `outputs/` |
| Model card | `model_card.md` |
| Dataset + data card | `data/processed/*.jsonl`, `data/data_card.md`; optional regen: `scripts/generate_dataset.py` |
| Environment | `requirements.txt` |
| README + setup | this file |
| Demo video | optional for Challenge 2 (not required by §5.1.5) |

**Before you push:** run `git add -A` (or at least `git add outputs/` and `data/processed/`) and `git status` so every deliverable is staged. If GitHub rejects a file (>100 MB per file), use Git LFS or omit that single blob — see `outputs/README.md`.
