# BlockHouse_Task
A lightweight Python toolkit for computing and validating high-frequency Order Flow Imbalance (OFI) features—best-level, multi-level, integrated, and cross-asset—directly from limit order book snapshots to power price-impact analysis and trading signal research.
# OFI Feature Pipeline
This repository contains a modular Python pipeline for constructing and evaluating Order Flow Imbalance (OFI) features from high-frequency limit order book data. It implements four key OFI measures:
1. **Best-Level OFI**: Net order flow at the top-of-book.
2. **Multi-Level OFI**: Aggregated net flow across multiple depth levels.
3. **Integrated OFI**: Time-smoothed multi-level OFI over a rolling window.
4. **Cross-Asset OFI**: Weighted sum of best-level OFIs across assets using Lasso-estimated cross-impact coefficients.
---
## Repository Structure
```
ofi-feature-pipeline/
│
├── data/
│ └── first_25000_rows.csv # Sample order book snapshot data
│
├── notebooks/
│ └── ofi_analysis.ipynb # Exploratory analysis and validation
│
├── ofi_pipeline/
│ ├── __init__.py
│ ├── data_loader.py # Data loading and preprocessing functions
│ ├── ofi_features.py # Functions to compute OFI features
│ └── cross_impact.py # Lasso-based cross-impact estimation
│
├── tests/
│ ├── test_ofi_best.py
│ ├── test_ofi_multi.py
│ └── test_cross_impact.py # Unit tests for core functions
│
├── requirements.txt # Project dependencies
├── setup.py # Installable package configuration
└── README.md # Project overview and usage instructions
```
---
## Installation
1. Clone the repository:
```bash
git clone https://github.com/username/ofi-feature-pipeline.git
cd ofi-feature-pipeline
```
2. Create a virtual environment and install dependencies:
```bash
python -m venv venv
source venv/bin/activate # Linux/macOS
venv\Scripts\activate # Windows
pip install -r requirements.txt
```
---
## Usage
### 1. Data Preprocessing
```python
from ofi_pipeline.data_loader import load_and_clean_data
df = load_and_clean_data('data/first_25000_rows.csv')
```
### 2. Compute OFI Features
```python
from ofi_pipeline.ofi_features import (
compute_ofi_best,
compute_ofi_multi,
compute_ofi_integrated
)
# Best-Level
ofi_best = compute_ofi_best(df)
# Multi-Level (e.g., L=5 uniform weights)
ofi_multi = compute_ofi_multi(df, L=5, weights=[1]*5)
# Integrated over 5-second window
ofi_int = compute_ofi_integrated(ofi_multi, window=5)
```
### 3. Cross-Asset OFI
```python
from ofi_pipeline.cross_impact import compute_cross_ofi
# ofi_matrix: DataFrame where each column is best-level OFI for an asset
cross_ofi = compute_cross_ofi(ofi_matrix, alpha=0.1)
```
### 4. Example Notebook
See `notebooks/ofi_analysis.ipynb` for a step-by-step walkthrough of feature computation, regression analysis, and results visualization.
---
## Testing
Run unit tests with:
```bash
pytest --maxfail=1 --disable-warnings -q
```
---
## License
This project is distributed under the MIT License. See `LICENSE` for details.

