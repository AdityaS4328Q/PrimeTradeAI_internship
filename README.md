# Trader Behavior vs. Market Sentiment Analysis

## 📌 Project Objective
This project analyzes how market sentiment (specifically the Fear/Greed Index) influences retail trader behavior and profitability on the Hyperliquid decentralized exchange. The goal is to uncover behavioral archetypes and propose data-driven trading strategies.

## 📊 Executive Summary
Our analysis of Hyperliquid trader performance against market sentiment required navigating real-world data irregularities, notably bypassing corrupted epoch batch-timestamps in favor of granular string parsing, and utilizing median-based segmentation to mitigate the skew from high-frequency algorithmic bots. 

The data reveals a stark divergence between profitable and unprofitable archetypes. Inconsistent traders heavily exhibit counter-trend, high-risk behavior during market stress—aggressively over-trading and increasing position sizes to "buy the dip" during Extreme Fear, yielding a win rate of just ~44%. Conversely, consistent winners maintain strict risk management through small, stable position sizes regardless of market panic, achieving their highest win rates (~53%) by patiently aligning with the trend during Greed conditions.

## 🔬 Methodology
1. **Data Cleaning & Alignment:** Addressed timestamp corruption by parsing execution-level string timestamps (`Timestamp IST`) rather than batch epoch times. Data was aggregated to a daily, account-level format to cleanly merge with the daily Fear/Greed sentiment classifications.
2. **Feature Engineering:** Created Boolean flags at the trade level to efficiently calculate daily `win_rate` and `long_ratio` (directional bias) per trader. 
3. **Behavioral Segmentation:** Traders were segmented into archetypes using median thresholds to prevent extreme outliers (e.g., algorithmic bots executing 1,000+ trades/day) from skewing the distributions. Segments included:
   * **Frequency:** Frequent vs. Infrequent Traders
   * **Risk/Volume:** High Volume vs. Low Volume Traders
   * **Consistency:** Consistent Winners (Lifetime Win Rate > 50%) vs. Inconsistent Traders

## 💡 Key Insights
1. **The "Falling Knife" Phenomenon:** During "Extreme Fear", the long ratio peaks (~76%), indicating traders are actively trying to buy the dip. However, this leads to the lowest win rate across all sentiments (~44%), suggesting unsuccessful counter-trend trading.
2. **Trend Alignment:** Traders perform best when aligning with macro momentum. During "Extreme Greed", traders continue taking long positions with the trend, significantly improving their win rate to ~53%.
3. **The Penalty of Over-Trading:** Over-trading negatively impacts win rates across all conditions, but the penalty is most severe during market panics. "Infrequent" traders consistently outperform "Frequent" traders during Fear phases.
4. **Risk Management Defines Winners:** Consistent winners maintain very small, stable average trade sizes regardless of sentiment. Inconsistent traders take on significantly larger positions during "Fear", indicating oversized risk-taking and revenge trading during drawdowns.

## 🚀 Strategy Recommendations
Based on the behavioral analysis, we propose the following rules for discretionary or algorithmic execution:

* **Rule 1: Suspend Counter-Trend Buying During "Extreme Fear"**
  Do not aggressively “buy the dip” during strong downtrends. Strategies should automatically reduce maximum position size and throttle trade execution frequency during Extreme Fear, as high activity and large bets during this phase are the primary drivers of unprofitability.

* **Rule 2: Maximize Trend-Following During "Extreme Greed" with Fixed Sizing**
  Align positions with strong upward trends (prefer Longs), but adopt the risk-management profile of "Consistent Winners" by strictly maintaining small, fixed position sizes. Avoid the urge to over-leverage; discipline and trend alignment alone maximize the win rate.

## ⚙️ Setup & Reproducibility
To run this analysis locally:
1. Clone this repository.
2. Ensure you have the required libraries installed: `pip install pandas numpy matplotlib seaborn`
3. Download the datasets from the original assignment links:
   * [Bitcoin Market Sentiment](https://drive.google.com/file/d/1PgQC0tO8XN-wqkNyghWc_-mnrYv_nhSf/view?usp=sharing) [cite: 14, 16]
   * [Historical Trader Data](https://drive.google.com/file/d/1IAfLZwu6rJzyWKqBToqwSmmVYU6VbjVs/view?usp=sharing) [cite: 17, 19]
4. Place the downloaded files (`historical_data.csv` and `fear_greed_index.csv`) into the root directory of the cloned repo.
5. Run the `notebook.ipynb` sequentially from top to bottom.
