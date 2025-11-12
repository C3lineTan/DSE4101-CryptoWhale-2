# 🐋 DSE4101 — 1-Hour Cryptocurrency Volatility Forecasting from Whale Activity and Dune Analytics Data

This project replicates and extends previous work on cryptocurrency volatility forecasting, focusing on Bitcoin (BTC) and Ethereum (ETH) for the 1-hour ahead forecast.

---

## Data

The analysis combines two key data sources:

1. **Whale Alert API (Free)**  
   - Tracks large cryptocurrency transfers (“whale transactions”).  
   - Data extracted, cleaned, and filtered for exchange-to-wallet (ETOW) and wallet-to-exchange (WTOE) transfers.

2. **Dune Analytics (Paid Tier Required)**  
   - Provides on-chain Ethereum and Bitcoin data (transaction counts, fees, gas metrics, staking inflows, etc.).  


---

## Models Implemented

We evaluate a range of econometric, machine learning, and deep learning approaches for volatility forecasting:

| Category | Model | Description |
|-----------|--------|-------------|
| **Econometric** | **EGARCH(1,1)** | Captures asymmetric volatility responses. |
| **Machine Learning** | **XGBoost** | Gradient boosting for nonlinear dependencies. |
| **Deep Learning** | **LSTM** | Sequential model capturing temporal volatility clustering. |
| **Transformer-based** | **Vanilla, Dense Synthesizer, Random Synthesizer** | Attention-based models adapted from Herremans & Low (2025). |

All models are trained using 5-fold rolling time-series cross-validation with purge gaps to prevent look-ahead bias.

---

## Evaluation Metrics

Model performance is evaluated using:

| Metric | Purpose |
|---------|----------|
| **RMSE** | Measures average forecasting error magnitude. |
| **QLIKE** | Penalizes under-prediction of volatility (risk-sensitive). |
| **Diebold–Mariano (DM) Test** | Tests if one model significantly outperforms another. |
| **Feature Attribution (Captum)** | Explains which features drive each model’s predictions. |

---

## Backtesting Strategies 🤑🤑🤑

Predicted one-hour-ahead volatilities are used to generate trading signals under three strategies:

| Strategy | Logic | Purpose |
|-----------|--------|----------|
| **Buy-and-Hold** | Passive benchmark holding BTC/ETH throughout. | Baseline comparison. |
| **Mean-Reversion** | Buy if volatility spike predicted & prior return negative. | Exploit short-term corrections. |
| **Momentum** | Buy if volatility spike predicted & prior return positive. | Exploit trend continuation. |

Additional details:
- Dynamic rolling quantile thresholds (80–95%) to define spikes.  
- Transaction fee: 0.1% per trade (Binance).  
- Stop-loss: –1%, Take-profit: +2%.  
- Performance metrics: Sharpe, Sortino, Calmar ratios, Max Drawdown, VaR (95%).  

---

## Set Up

### macOS Setup

```bash
# 1. Clone the repository
git clone https://github.com/C3lineTan/DSE4101-CryptoWhale-2.git
cd DSE4101-CryptoWhale-2

# 2. Create and activate virtual environment
python3 -m venv venv
source venv/bin/activate

# 3. Upgrade pip
pip install --upgrade pip

# 4. Install required libraries
pip install -r requirements.txt

```

### Windows Setup
```bash
# 1. Clone the repository
git clone https://github.com/C3lineTan/DSE4101-CryptoWhale-2.git
cd DSE4101-CryptoWhale-2

# 2. Create and activate virtual environment
python -m venv venv
venv\Scripts\activate

# 3. Upgrade pip
pip install --upgrade pip

# 4. Install required libraries
pip install -r requirements.txt

```
---

## Files structure
```
DSE4101-CryptoWhale-2/
├── BTC Models/ # Contains model, DM-Test, and backtesting notebooks for Bitcoin (BTC)
│ ├── Backtesting_BTC.ipynb 
│ ├── DM Test_BTC.ipynb 
│ ├── EGARCH_BTC.ipynb
│ ├── LSTM_BTC.ipynb # Lookback set at 1
│ ├── LSTM_seq.ipynb # With temporal dependence tuning
│ ├── XGBoost_BTC.ipynb
│ └── Transformer/ # (Vanilla, Dense, Random Synthesizer)
│
├── ETH Models/ # Contains model, DM-Test, and backtesting notebooks for Ethereum (ETH)
│ ├── Backtesting_ETH.ipynb
│ ├── DM Test_ETH.ipynb
│ ├── EGARCH_ETH.ipynb
│ ├── LSTM_ETH.ipynb # Lookback set at 1
│ ├── LSTM_seq.ipynb # With temporal dependence tuning
│ ├── XGBoost_ETH.ipynb
│ └── Transformer/ # (Vanilla, Dense, Random Synthesizer)
│
├── Data/ # Stores raw and merged datasets from Dune (BTC & ETH)
│ ├── Too many to specify
│
├── Data Preprocessing/ # VAR-FEVD, Whale_alert_processing, Merging of data sets 
│ ├── dune_btc.ipynb 
│ ├── dune_eth.ipynb 
│ ├── whale_alerts_btc.ipynb 
│ ├── whale_alerts_eth.ipynb 
│ ├── final_df_var_fevd.ipynb 
│
├── EDA/ # Contains Feature distribution, correlation, volatility visualisation plots
│ ├── BTC_EDA.ipynb
│ ├── ETH_EDA.ipynb
│
├── Results/ # DM-Test, Model's Prediction CSVs
│ ├── Too many to specify
│
├── .gitignore # Git ignore configuration for virtual environment
├── requirements.txt # Python dependencies list
└── README.md # Project documentation
```

## Recommended Execution Order

Follow this sequence to fully reproduce the results from raw data to backtesting performance.

---

### **1️) Data Preprocessing**
Run the notebooks in `/Data Preprocessing/` to extract, clean, and merge datasets.

**Order:**
1. `whale_alerts_btc.ipynb` — Extract & clean Whale Alert BTC data  
2. `whale_alerts_eth.ipynb` — Extract & clean Whale Alert ETH data  
3. `dune_btc.ipynb` — Process BTC on-chain data  
4. `dune_eth.ipynb` — Process ETH on-chain data 
5. `final_df_var_fevd.ipynb` — Compute VAR–FEVD spillover features between BTC & ETH, final merge conducted here as well

**Output:**  
`/Data/final_df_btc.csv` and `/Data/final_df_eth.csv`

---

### **2) Exploratory Data Analysis (EDA)**
Run notebooks in `/EDA/` to visualize distributions and correlations.

**Files:**
- `BTC_EDA.ipynb`
- `ETH_EDA.ipynb`

---

### **3) Model Training and Evaluation**
Train volatility forecasting models under `/BTC Models/` and `/ETH Models/`.

**For each cryptocurrency, run in this order:**

**BTC:**
1. `EGARCH_BTC.ipynb`
2. `LSTM_BTC.ipynb`
3. `XGBoost_BTC.ipynb`
4. `Transformer/` notebooks 
5. `DM Test_BTC.ipynb` 
6. `Backtesting_BTC.ipynb` 

**ETH:**
1. `EGARCH_ETH.ipynb`
2. `LSTM_ETH.ipynb`
3. `XGBoost_ETH.ipynb`
4. `Transformer/` notebooks (Vanilla, Dense, Random)
5. `DM Test_ETH.ipynb`
6. `Backtesting_ETH.ipynb` 

**Outputs under `/Results/`:**
- Model metrics (`RMSE`, `QLIKE`)
- DM-test significance tables
- Feature importance visualizations
- Backtesting results




