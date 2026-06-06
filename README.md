# Milan Mobile Network Traffic Analysis

## Overview
Comparative time series analysis and forecasting of mobile network traffic using the Telecom Italia Milan dataset (November–December 2013). Implements SARIMA, LSTM, and GRU models for one-step-ahead traffic prediction across three geographical areas of Milan.

## Dataset
Download from Harvard Dataverse:
- Telecom data: https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/EGZHFV
- Grid data: https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/QJWLFU

Note: You must create a free Harvard Dataverse account and accept the dataset terms before downloading.

## Requirements
- Python 3.12
- Google Colab (recommended) with Google Drive mounted
- GPU runtime recommended for LSTM and GRU training (Runtime → Change runtime type → T4 GPU)

## Installation
Run this in Colab before starting:
```
pip install pandas numpy matplotlib statsmodels torch scikit-learn pyarrow psutil
```

## How to Run
1. Open `milan_traffic_analysis.ipynb` in Google Colab
2. Run Cell 1 to mount Google Drive
3. Upload all 62 dataset zip files to `/MyDrive/milan_telecom/` in Google Drive
4. Run the unzip cell to extract all txt files
5. Run the data loading cell to process and save the parquet file
6. Run all remaining cells in order
7. If parquet already exists, skip the txt loading cells and load directly with:
   `data = pd.read_parquet('/content/drive/MyDrive/milan_telecom/processed_data.parquet')`

## Repository Structure
```
milan-traffic-analysis/
├── milan_traffic_analysis.ipynb  # Main Colab notebook (all code)
└── README.md                     # This file
```

## Results Summary
| Model  | Avg Train Time | Square 5059 MAE | Square 4159 MAE | Square 4556 MAE |
|--------|---------------|-----------------|-----------------|-----------------|
| SARIMA | 614.7s        | 52.2465         | 7.2456          | 22.4255         |
| LSTM   | 7.2s          | 12.0449         | 1.6683          | 3.8946          |
| GRU    | 9.1s          | 11.1701         | 1.7004          | 3.8705          |

**Best model: GRU** — lowest MAE in 2 out of 3 areas, trained 67x faster than SARIMA.

## Hardware Used
- CPU: Intel Xeon (Google Colab free tier)
- GPU: NVIDIA T4 (Google Colab, used for LSTM and GRU)
- RAM: 12.7 GB
- Storage: Google Drive 15GB

## Key Design Decisions
- Sequence length of 144 (1 day) chosen based on ACF analysis showing strongest correlations at lag 144
- Z-score normalization applied per area before neural network training
- SARIMA trained on last 1 week of data only to manage computation time
- Country code 39 (Italy) filtering applied to remove foreign roaming traffic

## AI Usage Statement
Claude (Anthropic) was used as a coding and debugging assistant throughout this project. All analysis, interpretations, and design decisions reflect the author's own understanding.

## References
- G. Barlacchi et al., "A multi-source dataset of urban life in the city of Milan," Scientific Data, 2015
- S. Hochreiter and J. Schmidhuber, "Long short-term memory," Neural Computation, 1997
- K. Cho et al., "Learning phrase representations using RNN encoder-decoder," EMNLP, 2014
- G.E.P. Box and G.M. Jenkins, Time Series Analysis: Forecasting and Control, 1970