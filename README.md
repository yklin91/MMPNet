# MMPNet

MMPNet is a focused PyTorch codebase for training, evaluating, and running inference with the ConcatNet-MSDS image segmentation network.

The repository provides a complete workflow from raw data preparation and CSV index generation to model training, validation, checkpoint management, test-set evaluation, and dataset-specific prediction.

## Features

- Convert raw data into CSV-based dataset indexes.
- Centralized dataset loading, normalization, augmentation, and DataLoader creation.
- ConcatNet-MSDS as the main segmentation architecture.
- Multiple segmentation loss components, including:
  - Focal Loss
  - Dice Loss
  - clDice Loss
  - Tversky Loss
  - MSDS Loss
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
Clone the repository and install the required dependencies:

git clone <YOUR_REPOSITORY_URL>
cd mmpnet_github

python -m venv .venv
source .venv/bin/activate

pip install --upgrade pip
pip install -r requirements.txt
For Windows PowerShell, activate the virtual environment with:

.venv\Scripts\Activate.ps1

## Environment Variables
Set the data and output directories before running the pipeline:

export MMPNET_DATA_DIR=/path/to/data
export MMPNET_OUTPUT_DIR=/path/to/outputs
MMPNET_DATA_DIR should contain the raw input data required by prepare_data.py.

MMPNET_OUTPUT_DIR is used for generated indexes, logs, checkpoints, evaluation results, and prediction outputs, depending on the selected entry point.

## Data Preparation
Run the data preparation script before training:

python prepare_data.py
This script converts the raw dataset into CSV-based indexes used by mmpnet/data.py.

The expected raw-data structure and file naming conventions are dataset-specific. Check prepare_data.py and the documentation under docs/ before running the preparation step.

## Training
The only supported training entry point is train.py.

Example:

python train.py --epochs 50 --batch_size 8
To view all available command-line options:

python train.py --help
The training pipeline includes:

Dataset loading and preprocessing.
Data augmentation.
Model construction.
Loss computation.
Training and validation loops.
Logging.
Checkpoint saving.

## Evaluation
Evaluate a trained model on the test set with:

python evaluate.py \
    --checkpoint /path/to/best_checkpoint.pth
To view all evaluation options:

python evaluate.py --help

## Prediction
The repository provides prediction scripts for several supported datasets and inference modes.

Use the help command for the corresponding script to inspect its available arguments:

python scripts/predict-sh2023-concatnet.py --help
python scripts/predict-sh2024-concatnet.py --help
python scripts/predict-sh2026-concatnet.py --help
python scripts/predict-sh2026-concatnet-overlap.py --help
python scripts/predict-worldfld-concatnet.py --help
The available prediction entry points are:

Script	Purpose
predict-sh2023-concatnet.py	Prediction on the SH2023 dataset
predict-sh2024-concatnet.py	Prediction on the SH2024 dataset
predict-sh2026-concatnet.py	Prediction on the SH2026 dataset
predict-sh2026-concatnet-overlap.py	Overlap-based prediction on SH2026
predict-worldfld-concatnet.py	Prediction on the WorldFLD dataset
