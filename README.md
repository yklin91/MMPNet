# MMPNet

MMPNet is a focused PyTorch codebase for training, evaluating, and running inference with the ConcatNet-MSDS image segmentation network.

The repository provides a complete workflow from raw data preparation and CSV index generation to model training, validation, checkpoint management, test-set evaluation, and dataset-specific prediction.

## Features

- Convert raw data into CSV-based dataset indexes.
- Centralized dataset loading, normalization, augmentation, and DataLoader creation.
- ConcatNet-MSDS as the main segmentation architecture.
- Segmentation loss components: Focal Loss, Dice Loss, clDice Loss, Tversky Loss, and MSDS Loss.
- A single training entry point through `train.py`.
- Validation and checkpoint management through the training engine.
- Test-set evaluation through `evaluate.py`.
- Dataset-specific prediction scripts for SH2023, SH2024, SH2026, and WorldFLD.
- Logging utilities for monitoring experiments.
- A simplified codebase focused on the main MMPNet workflow.

## Repository Structure

```text
mmpnet_github/
├── prepare_data.py
├── train.py
├── evaluate.py
├── mmpnet/
│   ├── data.py
│   ├── model.py
│   ├── losses.py
│   ├── engine.py
│   ├── utils.py
│   └── __init__.py
├── scripts/
│   ├── predict-sh2023-concatnet.py
│   ├── predict-sh2024-concatnet.py
│   ├── predict-sh2026-concatnet.py
│   ├── predict-sh2026-concatnet-overlap.py
│   ├── predict-worldfld-concatnet.py
│   └── draw_concatnet_diagrams.py
├── docs/
├── README.md
└── requirements.txt
```

## Installation

Clone the repository, replacing `<YOUR_REPOSITORY_URL>` with the actual repository URL:

```bash
git clone <YOUR_REPOSITORY_URL> mmpnet_github
cd mmpnet_github
```

Create a virtual environment:

```bash
python -m venv .venv
```

On Linux or macOS, activate the virtual environment with:

```bash
source .venv/bin/activate
```

On Windows PowerShell, activate it with:

```powershell
.venv\Scripts\Activate.ps1
```

Install the required dependencies:

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

## Environment Variables

Set the data and output directories before running the pipeline.

On Linux or macOS:

```bash
export MMPNET_DATA_DIR=/path/to/data
export MMPNET_OUTPUT_DIR=/path/to/outputs
```

On Windows PowerShell:

```powershell
$env:MMPNET_DATA_DIR = "C:\path\to\data"
$env:MMPNET_OUTPUT_DIR = "C:\path\to\outputs"
```

Replace the example paths with your actual directory paths.

| Variable | Description |
| --- | --- |
| `MMPNET_DATA_DIR` | Root directory containing the input data required by `prepare_data.py`. |
| `MMPNET_OUTPUT_DIR` | Output directory for the pipeline. Refer to the relevant script for specific output files and subdirectories. |

## Data Preparation

Run the data preparation script before training:

```bash
python prepare_data.py
```

This script converts the raw dataset into CSV-based indexes used by `mmpnet/data.py`.

The expected raw-data structure and file naming conventions are dataset-specific. Check `prepare_data.py` and any relevant documentation under `docs/` before running the preparation step.

## Training

The training entry point is `train.py`.

For example, train for 50 epochs with a batch size of 8:

```bash
python train.py --epochs 50 --batch_size 8
```

To view the available command-line options:

```bash
python train.py --help
```

The training pipeline includes:

- Dataset loading and preprocessing.
- Data augmentation.
- Model construction.
- Loss computation.
- Training and validation loops.
- Logging.
- Checkpoint saving.

## Evaluation

Evaluate a trained model on the test set with:

```bash
python evaluate.py --checkpoint /path/to/best_checkpoint.pth
```

Replace `/path/to/best_checkpoint.pth` with the path to your trained checkpoint.

To view the available evaluation options:

```bash
python evaluate.py --help
```

## Prediction

The repository provides prediction scripts for several datasets and inference modes.

| Script | Purpose |
| --- | --- |
| `scripts/predict-sh2023-concatnet.py` | Prediction on the SH2023 dataset. |
| `scripts/predict-sh2024-concatnet.py` | Prediction on the SH2024 dataset. |
| `scripts/predict-sh2026-concatnet.py` | Prediction on the SH2026 dataset. |
| `scripts/predict-sh2026-concatnet-overlap.py` | Overlap-based prediction on SH2026. |
| `scripts/predict-worldfld-concatnet.py` | Prediction on the WorldFLD dataset. |

Use the help command for the corresponding script to inspect its available arguments:

```bash
python scripts/predict-sh2023-concatnet.py --help
python scripts/predict-sh2024-concatnet.py --help
python scripts/predict-sh2026-concatnet.py --help
python scripts/predict-sh2026-concatnet-overlap.py --help
python scripts/predict-worldfld-concatnet.py --help
```
