# Wallet Risk Scoring From Scratch (ZERU Internship Assignment 2)

This project implements a risk scoring model for Ethereum wallet addresses using on-chain transaction data. The solution fetches raw transactional activity for 100 wallets, engineers behavioral features, and applies an unsupervised ML model to assign each wallet a normalized risk score between 0 and 1000.

## Objective

To assess the relative risk of DeFi wallets interacting with the Compound protocol, based solely on publicly available transaction behavior — without using any external scoring APIs or labels.

## File Structure

wallet-risk-scoring/
├── data/  
│   └── wallet_addresses.csv  
├── notebooks/  
│   └── AyantikaDhar_ZERU_assignment2.ipynb  
├── output/  
│   ├── wallet_features_raw.csv  
│   ├── wallet_features_scaled.csv  
│   └── wallet_risk_scores.csv  
├── plots/  
│   ├── score_distribution_hist.png  
│   ├── score_trend_plot.png  
│   └── risk_bucket_counts.png  
├── README.md  
└── analysis.md  

## Methodology

### 1. Data Collection

- On-chain transaction history was retrieved using the Covalent API.
- The Ethereum mainnet (Chain ID 1) was queried to collect up to 100 transactions per wallet.
- All data was processed within a Colab-based Python environment.

### 2. Feature Engineering

The following behavioral features were extracted for each wallet:

| Feature                  | Description |
|--------------------------|-------------|
| tx_count                 | Total number of transactions |
| tx_success_rate          | Proportion of successful transactions |
| avg_gas_spent            | Average gas used per transaction |
| avg_tx_value_eth         | Average ETH transferred per transaction |
| defi_interaction_count   | Count of transactions involving DeFi protocols like Compound, Aave, etc. |
| wallet_age_days          | Number of days between first and last transaction |
| tx_frequency_per_day     | Transactions per active day |

### 3. Preprocessing

- All features were scaled to [0, 1] using MinMaxScaler to normalize the data before clustering.
- Missing data was handled gracefully, and all computations were made robust to empty or low-activity wallets.

### 4. Risk Scoring Model

- KMeans Clustering (k=4) was applied on the normalized features to segment wallets by behavioral similarity.
- The cluster with the lowest average transaction success rate was identified as the riskiest.
- Final scores were assigned based on each wallet’s distance from the riskiest cluster centroid, then inverted and scaled to a 0–1000 range.
- Wallets closer to risky behavior received lower scores.

## Visualizations

- Histogram: Distribution of risk scores across all 100 wallets  
  plots/score_distribution_hist.png

- Line Chart: Trend of scores over wallet index  
  plots/score_trend_plot.png

- Bar Chart: Categorized risk levels (Low, Medium, High, Critical)  
  plots/risk_bucket_counts.png

## Output

Final output file:  
output/wallet_risk_scores.csv  

Contains two columns:  
- wallet_id  
- soft_score (integer between 0 and 1000)

## Author

Ayantika Dhar  
M.Tech (CSE), IIT Jodhpur  
GitHub: https://github.com/Ayantika-dhar
