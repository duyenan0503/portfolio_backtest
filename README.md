# portfolio_backtest

**Key Figures**
* Five Portfolio Strategies: Including 1/N, Quintile, Tangency (Max Sharpe), Minimum Variance (GMVP), and Hierarchical Risk Parity (HRP).
* Multi-Dimensional Visualization: Dynamic pie charts, Pandas summary tables, and rolling Sharpe Ratio distribution plots (Boxplot).
* Risk Evolution: Annualized rolling 21-day volatility curves to track volatility clusters over time.

**Tech Stack**
* yfinance: Reliable market data acquisition from Yahoo Finance.
* PyPortfolioOpt: Advanced optimization for Modern Portfolio Theory and HRP.
* Seaborn/Matplotlib: Statistical data visualization.

**Usage Workflow**

**1. The Data Engine (__init__)**
* yf.download: Fetches the price history.
* Returns Calculation: Converts prices into daily percentage changes. This is crucial because optimization math (like HRP) is based on the correlation of returns, not the raw price.
* Mean Historical Returns & Covariance: These are the two inputs for Modern Portfolio Theory. Mean tells the class which stocks grow fastest, and Covarariance tells it which stocks move together (risk).

**2. The Strategy Generator (get_strategies)**
* Benchmark Logic **(1/N and Quintile)**: These are "non-optimized." They use simple rules (equal split or picking top performers) to give us a baseline to beat.

* Optimization Logic **(Tangency and Minimum Variance)**: These use the EfficientFrontier solver.
    - Tangency looks for the highest Sharpe Ratio (Return/Risk).
    - Minimum Variance looks for the lowest possible volatility.
    - Diversification Guardrail: By setting weight_bounds=(0, 0.5), you are telling the solver: "You cannot put more than 50% in one stock." This forces the math to find at least two or three assets to complete the 100% allocation.

* Clustering Logic (HRP): Unlike the others, Hierarchical Risk Parity uses machine learning techniques to "cluster" similar stocks and distribute risk across those clusters, making it more robust against market shocks.

**3. The Helper methods (get_performance_data & rolling_sharpe_data)**
Just for the math
* get_performance_data: Calculate the returns and cumulative returns
* rolling_sharpe_data: 
    - The Problem with Static Sharpe: A portfolio might have a great Sharpe ratio overall because it had one amazing month, even if it was terrible for the other 11 months. A static number hides that instability.
    - The "Sliding Window" Logic: This method slices your 4 years of data into hundreds of small 63-day (roughly 3 month) "windows."

**4. The Visualizers**
1. show_pies: To inspect the Asset Allocation.

2. show_summary_table: To compare Comparative Benchmarks.

3. show_sharpe_distribution: To evaluate Performance Stability.
This is the "Stress Test." While the table shows one final score, this chart shows you how often the portfolio worked in short windows of time. 

4. show_performance_curves: To track Historical Momentum & Risk.

**Table for Final Performance**
<img width="1249" height="149" alt="image" src="https://github.com/user-attachments/assets/1b0c7439-08ea-4b0b-9103-e837394db9fd" />

**Pie for Asset Allocation**
<img width="2176" height="458" alt="image" src="https://github.com/user-attachments/assets/594611a5-06f0-4a28-833c-557a09d1f6a5" />

**Line Chart for Return**
<img width="1189" height="990" alt="image" src="https://github.com/user-attachments/assets/fea17804-a2c9-4404-a158-b2081b9a84fe" />

**Box Plot for Sharpe Ratio**
<img width="1189" height="690" alt="image" src="https://github.com/user-attachments/assets/3e4b1723-25b4-43f9-aafa-c38cb212bb9d" />
