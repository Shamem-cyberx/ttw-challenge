## Tatweer technical assessment — **Challenge 2 only** (Generative AI)

Fine-tune a **&lt;1B** instruction model with **QLoRA** (parameter-efficient) on a domain-specific task, with dataset splits, training stability, evaluation vs base model, and documentation.

### Task

- **Domain**: Windows **PowerShell** technical support + scripting (instruction → response).
- **Data**: 260 instruction–response pairs (220 / 20 / 20 train / validation / test), generated deterministically in `01_dataset_and_train.ipynb` (see `data/data_card.md`).

### Why these settings (Colab **T4**, **&lt; 2h GPU** target)

- **QLoRA** (4-bit + LoRA) fits free-tier VRAM.
- **`max_seq_length=1024`**: sufficient for this dataset; faster than 2048 on T4.
- **2 epochs** with **early stopping** on validation loss: completes quickly while still adapting.
- First run pays a one-time cost: `pip install`, Hugging Face model download (~minutes). **Re-running** training in the same session is much shorter.

### Repo layout

```
├── README.md
├── CHALLENGE2_CHECKLIST.md   # maps rubric → files
├── requirements.txt
├── model_card.md
├── notebooks/
│   ├── 01_dataset_and_train.ipynb
│   └── 02_evaluation.ipynb
├── data/
│   ├── data_card.md
│   └── processed/            # filled after notebook 01
├── scripts/
│   └── generate_dataset.py
└── outputs/                  # filled after runs (gitignored)
```

### Colab — recommended workflow

1. **Runtime → Change runtime type → GPU** (pick **T4** if offered).
2. **Runtime → Restart session** after switching GPU.
3. Clone and enter the repo:

```bash
%cd /content
!git clone https://github.com/Shamem-cyberx/tatweer_challenge.git
%cd /content/tatweer_challenge/notebooks
```

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
