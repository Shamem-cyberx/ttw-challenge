# Data Card – PowerShell Support Instructions (Synthetic)

## Summary

- **Dataset name**: PowerShell Support Instructions (Synthetic)
- **Task**: Domain-specific instruction following for Windows PowerShell troubleshooting + scripting
- **Format**: Instruction → Response pairs (SFT)
- **Size**: 260 examples total (split into train/valid/test)

## Motivation

This dataset is designed to teach a small instruction model to respond like a helpful technical support engineer:

- Provide minimal-risk diagnostic steps first
- Use Windows-native tools when possible
- Prefer safe read-only commands before destructive actions
- Output copy/paste-ready PowerShell

## Source

- **Synthetic**: Programmatically generated via templates and controlled variations.
- **No private data**: Contains no user logs, credentials, or proprietary content.

## Schema

Each JSONL line contains:

- **id**: string
- **instruction**: string
- **response**: string
- **category**: string (e.g., `network`, `disk`, `services`, `permissions`, `processes`, `security`)
- **expects_code**: boolean (whether the answer should include PowerShell code blocks)
- **difficulty**: integer (1–3)

## Preprocessing

- Deterministic generation with a fixed RNG seed for reproducibility.
- Light normalization:
  - Trim extra whitespace
  - Enforce fenced code blocks for command-heavy answers
  - Ensure responses include warnings for risky operations where relevant
- Generation entrypoints:
  - `notebooks/01_dataset_and_train.ipynb` (recommended, Colab)
  - `scripts/generate_dataset.py` (local Python)

## Splits

- **train**: 220
- **valid**: 20
- **test**: 20

## Quality controls

- Balanced coverage across categories and difficulty.
- Negative examples included (common mistakes / missing admin rights / wrong command), with corrected responses.
- Safety guardrails: avoid instructions for malware, credential theft, or covert persistence.

## Limitations

- Synthetic templates may under-represent real-world messy inputs (logs, incomplete context).
- Not a substitute for official Microsoft documentation; the model can still hallucinate.
