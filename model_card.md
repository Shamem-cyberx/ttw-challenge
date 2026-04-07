# Model Card – PowerShell Support Assistant (QLoRA)

## Model details

- **Base model**: `Qwen/Qwen2.5-0.5B-Instruct` (<= 1B parameters)
- **Fine-tuning method**: **QLoRA** (4-bit quantization + LoRA adapters)
- **Training framework**: Unsloth + TRL SFT

## Intended use

Assist with:

- PowerShell scripting help
- Windows troubleshooting steps (network, services, processes, disk, permissions)
- Safe diagnostic commands and structured remediation suggestions

## Out-of-scope / not recommended

- Any guidance enabling malware, unauthorized access, persistence, or credential theft
- Commands that cause destructive changes without explicit user confirmation
- Replacing official incident response procedures

## Training data

See `data/data_card.md`.

## Training configuration (documented in notebook)

Key hyperparameters are recorded in `notebooks/01_dataset_and_train.ipynb`, including:

- LoRA rank \(r\), alpha, dropout
- Learning rate, batch size, epochs / max steps
- Target modules selection (attention + MLP projections)
- Early stopping on validation loss

## Evaluation

See `notebooks/02_evaluation.ipynb` and outputs in `outputs/`:

- **Loss curves** (train/valid)
- **Task metrics**: BLEU, ROUGE-L, and a domain “format compliance” metric
- **Base vs fine-tuned** comparison
- **Qualitative examples** with error analysis

## Example usage

Load base + adapters (Colab):

```python
from unsloth import FastLanguageModel
import torch

model_name = "Qwen/Qwen2.5-0.5B-Instruct"
model, tokenizer = FastLanguageModel.from_pretrained(
    model_name=model_name,
    max_seq_length=2048,
    dtype=None,
    load_in_4bit=True,
)

model = FastLanguageModel.get_peft_model(
    model,
    r=16,
    lora_alpha=32,
    lora_dropout=0.05,
    bias="none",
    use_gradient_checkpointing="unsloth",
    random_state=3407,
)

# After training, load adapters from your saved output directory.
```

## Limitations & risks

- Can still hallucinate command flags or environment-specific behavior.
- May require admin privileges; always verify before running remediation commands.
- Synthetic training may bias responses toward templated structure.
