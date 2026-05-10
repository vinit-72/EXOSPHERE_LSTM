# ExoSphere - LSTM Traffic Detection

`ExoSphere` is a PyTorch project for DDoS detection using packet-level traces.

## What changed
- Replaced the original 1D U-Net CNN architecture with a 3-layer LSTM model in `model.py`.
- Updated the active pipeline in `train.py` to use `ExosphereLSTM` by default.
- Added sequence balancing in `data.py` so the training set retains more attack-containing segments and reduces benign-only noise.
- Preserved the legacy CNN code as a comparison reference.

## Novelty
- Uses an LSTM to model packet sequence behavior, capturing temporal relationships in inter-arrival times and packet sizes.
- Focuses on sequential attack patterns rather than only local convolutional features.
- Applies a mixed loss of binary cross-entropy and Dice loss for more stable packet-level anomaly scoring.
- Improves sensitivity for flooding-style attacks by training on balanced sequences with stronger attack signal representation.

## Pipeline
- `data.py`: reads packet traces, converts timestamps to inter-arrival times, normalizes lengths, and segments the sequence.
- `model.py`: defines a 3-layer LSTM with 64 hidden units and a final linear output.
- `train.py`: trains with Adam and a combined BCE + Dice loss, evaluates AUC/F1/EER, and saves plots.

## Usage
1. Install dependencies:
```bash
pip install torch matplotlib numpy scikit-learn
```
2. Download the dataset and place it in `dataset/`.
```bash
wget https://www.exosphere.fuchuanpu.xyz/dataset.zip
unzip dataset.zip -d dataset
```
3. Run training / detection with a config file:
```bash
python main.py -c ./config/config_amplification.json
```
4. For other attack types, replace the config file:
```bash
python main.py -c ./config/config_application.json
python main.py -c ./config/config_bruteforce.json
python main.py -c ./config/config_flooding.json
```

## Notes
- LSTM is the main model now.
- Legacy U-Net code remains in `model.py`.
- Datasets are plain text traces under `dataset/`.

## Summary
- Replaced the original 1D U-Net CNN with an LSTM model in `model.py`.
- The active pipeline uses `ExosphereLSTM` by default (`use_lstm = True` in `train.py`).
- `data.py` builds normalized packet sequences and balances benign-only segments.

