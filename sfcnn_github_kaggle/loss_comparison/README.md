# Twenty-epoch loss comparison

This experiment keeps the original SFCNN system, codebook, training data, validation data, initialization seed, and batch order fixed while changing only the loss function.

Run from the repository root:

```bash
python loss_comparison/22_run_loss_comparison.py
```

The default comparison trains these four losses for 20 epochs:

- `residual_mse`: original notebook objective;
- `channel_nmse`: reconstructed-channel NMSE;
- `hybrid`: channel NMSE plus 0.10 times residual MSE;
- `channel_nmse_cvar`: channel NMSE plus 0.10 times the worst-20% sample loss.

All four models use one shared generated training dataset and validation dataset. Each model is initialized with the same seed and receives the same shuffled batch order. Evaluation resets NumPy to the same test seed before every model, so channel, pilot, and noise draws are paired across losses.

Outputs are written to `loss_comparison_results/`:

- one checkpoint per loss;
- `loss_ranking.csv` and `loss_comparison_report.json`;
- raw NMSE arrays in `loss_comparison_arrays.npz`;
- training-history and NMSE comparison plots.

The default evaluation is computationally expensive because it retains 50 test channel realizations for every combination of four path counts and seven SNR values. Use the smoke test before starting the full run:

```bash
python loss_comparison/22_run_loss_comparison.py --quick
```

To compare only selected losses:

```bash
python loss_comparison/22_run_loss_comparison.py --losses residual_mse channel_nmse hybrid
```

The morph-invariance loss is not included in this comparison yet. It requires paired observations of the same propagation scene at different morphing amplitudes plus the saved normalization scale for every pair.
