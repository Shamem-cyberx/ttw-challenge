## Tatweer Assignment – Challenge Option 2 (Generative AI)

This project fine-tunes a **small language model (<1B params)** using **parameter-efficient QLoRA** for a niche domain task:

- **Task**: Domain-specific instruction following for **Windows PowerShell technical support + scripting**
- **Output style**: Safe, step-by-step troubleshooting with copy/paste-ready PowerShell commands

### Folder structure (submission-ready)

```
your-name-challenge/
├── README.md
├── requirements.txt
├── notebooks/
│   ├── 01_dataset_and_train.ipynb
│   └── 02_evaluation.ipynb
├── data/
│   ├── data_card.md
│   └── processed/
│       ├── train.jsonl
│       ├── valid.jsonl
│       └── test.jsonl
├── outputs/
│   ├── loss_curve.png
│   ├── eval_metrics.json
│   └── qualitative_examples.md
└── model_card.md
```

### Quick start (Colab)

1. Open `notebooks/01_dataset_and_train.ipynb` in Colab.
2. Run all cells (it will):
   - Generate/refresh the dataset into `data/processed/`
   - Fine-tune using **Unsloth + QLoRA**
   - Save LoRA adapters + training logs
3. Open `notebooks/02_evaluation.ipynb` in Colab.
4. Run all cells (it will):
   - Evaluate **base vs fine-tuned** on the held-out test set
   - Produce **BLEU + ROUGE-L + a domain format metric**
   - Save plots and outputs into `outputs/`

### Notes

- **No full fine-tuning**: training is QLoRA (4-bit base weights + LoRA adapters).
- **Colab free tier**: the defaults are chosen to finish within typical free tier limits.
- Replace `your-name-challenge` with your actual submission folder name if needed.
