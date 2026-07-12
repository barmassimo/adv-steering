# Repository Guidelines

## Project Structure & Module Organization

This repository contains a notebook-driven activation-steering experiment for Meta Llama 3.1 8B Instruct.

- `brand_steering_notebook.ipynb` contains configuration, model loading, vector extraction, generation, evaluation, and plotting.
- `prompts.py` is the single source of truth for generative, direct, contrastive, neutral, and stimulus prompt sets. Keep prompt text here rather than duplicating it in notebook cells.
- `results/` stores reproducible experiment artifacts. Parameterized runs use names such as `L16_F012`; comparison runs use descriptive names such as `_baseline`, `_prompted`, and `_layer_scale`.

## Setup, Run, and Validation Commands

Create and activate a virtual environment, then install the notebook dependencies:

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install jupyter torch transformers accelerate bitsandbytes sentencepiece protobuf python-dotenv scikit-learn matplotlib numpy
jupyter lab brand_steering_notebook.ipynb
```

Run cells in order; later analysis cells depend on model and result objects created earlier. For a non-interactive smoke run, use:

```powershell
jupyter nbconvert --to notebook --execute brand_steering_notebook.ipynb --output executed.ipynb
```

This requires substantial model memory and Hugging Face access, so interactive execution is usually preferable.

## Coding Style & Naming Conventions

Use four-space indentation and standard PEP 8 conventions. Name functions and variables with `snake_case`, module-level constants with `UPPER_SNAKE_CASE`, and prompt placeholders as `{brand}`. Keep notebook sections focused and ordered from configuration through evaluation. Add brief comments for hook behavior, tensor shapes, or device-specific logic; avoid comments that merely restate code.

## Testing Guidelines

There is no automated test suite or coverage threshold. Before submitting changes, import-check prompt data with `python -m py_compile prompts.py`, restart the notebook kernel, and run affected cells from their first dependency. For experiment changes, compare `results.json`, summaries, and charts against the baseline, and confirm every forward hook is removed after use.

## Commit & Pull Request Guidelines

History uses short, imperative summaries such as `Add layer scale sweep output`. Keep each commit limited to one logical experiment or documentation change. Pull requests should explain the hypothesis, model/device configuration, changed layer and scale values, and observed results. Link relevant issues and include updated charts or concise before/after metrics when outputs change. Never commit `.env`, Hugging Face tokens, model weights, or local virtual environments.
