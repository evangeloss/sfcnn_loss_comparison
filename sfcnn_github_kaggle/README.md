# SFCNN Python export

This repository-style folder was generated from `diplomatikh200epoches-matlab-version (4).ipynb`. It separates the original SFCNN work from every later test while preserving the notebook's code and execution order. Outputs were deliberately omitted.

## Partition

- `original_sfcnn/`: notebook code cells 0-22. This contains the propagation/channel model, deformable-array geometry, pilot generation and transmission, ridge/TE estimate, dataset builder, SFCNN architecture, LS/CNN evaluation, timing helper, and the original training/evaluation script. Its exported filename retains the notebook's “200 epochs” label, although the active code currently sets `max_epochs = 50`.
- `additional_tests/`: notebook code cells 23-46. These contain all later investigations and alternative runs, preserved separately from the original implementation.
- `loss_comparison/`: a new controlled 20-epoch experiment that compares selectable loss functions while keeping the generated data, initialization, batch order, and evaluation seed fixed.
- `manifest.json`: maps every exported file to its original notebook cell and records SHA-256 hashes for traceability.
- `run_everything.py`: reproduces notebook-style execution by running all files in one shared namespace.

The numbered filenames are intentional. The notebook relies heavily on variables and functions created by earlier cells, so files must be executed in numeric order. The code inside each exported cell is unchanged apart from a provenance docstring at the top.

## Quick start on GitHub or locally

```bash
python -m pip install -r requirements.txt
python original_sfcnn/run_original.py
```

The original run trains for 200 epochs and performs Monte Carlo evaluations, so it can take a long time. Model checkpoints, generated codebooks, arrays, and Python caches are excluded by `.gitignore`.

To reproduce the complete notebook, including all later tests:

```bash
python run_everything.py
```

To compare the proposed losses for 20 epochs without changing the preserved
notebook cells:

```bash
python loss_comparison/22_run_loss_comparison.py
```

To run the additional-test sequence with the original cells bootstrapped first:

```bash
python additional_tests/run_additional_tests.py
```

That command necessarily executes the original section first because the later notebook cells depend on its shared variables, functions, model, and generated codebook. `--without-original` is supplied only for advanced use inside an already prepared shared execution environment.

## Kaggle usage

1. Upload this folder or the accompanying ZIP as a Kaggle Dataset, or commit the folder to GitHub and import/clone it in Kaggle.
2. Enable a GPU accelerator for the training runs.
3. Change into the project directory.
4. Run `python original_sfcnn/run_original.py` for the original experiment, or `python run_everything.py` for the complete notebook sequence.

Kaggle normally provides NumPy, Matplotlib, and PyTorch. The requirements file documents the dependencies and supports other environments. Generated artifacts are written relative to the process working directory, matching the notebook's use of `saveFolder = "."`.

## Important preservation notes

- This is a faithful code-cell export, not a scientific rewrite. Existing global state, repeated imports, repeated training blocks, hard-coded experiment values, and plotting calls are retained.
- The later experiments include computationally expensive repeated training and Monte Carlo loops. Review their sample counts and epoch counts before running the complete sequence.
- Some cells compare manually entered result arrays. They remain as recorded in the source notebook.
- The source notebook hash is recorded in `manifest.json`, making it possible to verify which notebook version produced this export.
