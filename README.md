* * * * *

🌱 Exploring U.S. Renewable Energy Trends: A Data-Driven Investigation (2015--2021)
==================================================================================

As the federal government aggressively pushes for a clean energy future---backed by major legislative initiatives like the Bipartisan Infrastructure Law and the Inflation Reduction Act---understanding the actual trajectory of renewable energy is critical. With 600,000 federal vehicles transitioning to electric and 300,000 federal buildings adopting clean energy, the macro-level data must support these sustainability goals.

Using publicly available datasets from the [U.S. Energy Information Administration (EIA)](https://www.eia.gov/opendata/), I set out to analyze the true growth rate of renewable energy production and consumption in the United States from 2015 to 2021.

* * * * *

🛠️ Tech Stack & Methodology
----------------------------

-   **SQL:** Extracted, filtered, and cleaned raw time-series data from the EIA database.

-   **Python (Pandas, Matplotlib, Scikit-Learn):** Performed linear regression modeling to analyze growth trajectories and forecast trends.

-   **Power BI:** Created interactive dashboards to showcase state-wise energy adoption.

* * * * *

💾 1. Data Extraction (SQL)
---------------------------

To ensure data consistency, I extracted both Production and Consumption metrics in a single optimized query, filtering for the target timeframe (2015--2021) and removing null or incomplete data points.

SQL

```
SELECT
    Description,
    YYYYMM,
    Unit,
    Value
FROM
    PROD
WHERE
    Description IN ('Total Renewable Energy Production', 'Total Renewable Energy Consumption')
    AND YYYYMM >= 201501
    AND Value IS NOT NULL
ORDER BY
    YYYYMM DESC;

```

* * * * *

📈 2. Predictive Modeling (Python)
----------------------------------

After cleaning the datasets and isolating the production (`dp`) and consumption (`dc`) data frames, I used `scikit-learn` to build a Linear Regression model. This allowed me to visualize the historical trendlines and identify the rate of growth over time.

Python

```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression

# 1. Load Cleaned Data
url_prod = 'https://raw.githubusercontent.com/IsaiahsWork/Research-Work/main/EDITP.csv'
url_cons = 'https://raw.githubusercontent.com/IsaiahsWork/Research-Work/main/EDITC.csv'

df_cons = pd.read_csv(url_cons)
df_prod = pd.read_csv(url_prod)

# 2. Extract Features (Time) and Target (Quadrillion Btu)
X_cons = df_cons.iloc[:, 1].values.reshape(-1, 1)
y_cons = df_cons.iloc[:, 2].values

X_prod = df_prod.iloc[:, 1].values.reshape(-1, 1)
y_prod = df_prod.iloc[:, 2].values

# 3. Train/Test Split (80% Training, 20% Testing)
Xc_train, Xc_test, yc_train, yc_test = train_test_split(X_cons, y_cons, test_size=0.2, random_state=0)
Xp_train, Xp_test, yp_train, yp_test = train_test_split(X_prod, y_prod, test_size=0.2, random_state=0)

# 4. Initialize and Train Linear Regression Models
cons_model = LinearRegression()
cons_model.fit(Xc_train, yc_train)

prod_model = LinearRegression()
prod_model.fit(Xp_train, yp_train)

# 5. Generate Predictions for Trendlines
yc_pred = cons_model.predict(Xc_train)
yp_pred = prod_model.predict(Xp_train)

# 6. Visualize the Trends
plt.figure(figsize=(10, 6))
plt.scatter(Xc_train, yc_train, color='lightgreen', label='Consumption Data', alpha=0.6)
plt.scatter(Xp_train, yp_train, color='lightcoral', label='Production Data', alpha=0.6)

plt.plot(Xc_train, yc_pred, color='green', linewidth=2, label='Consumption Trend')
plt.plot(Xp_train, yp_pred, color='red', linewidth=2, label='Production Trend')

plt.title('Renewable Energy: Production vs. Consumption (2015-2021)')
plt.xlabel("Time (YYYYMM)")
plt.ylabel("Quadrillion Btu")
plt.legend()
plt.grid(True, linestyle='--', alpha=0.7)
plt.show()

```

'![download](https://github.com/user-attachments/assets/6efe4ca8-c63b-46a2-b783-3fba06375fb8)'

* * * * *

📊 3. Geographic Visual Analysis (Power BI)
-------------------------------------------

'![download](https://github.com/IsaiahsWork/Renewable_Energy_Analysis_USA/blob/be1e22d814e7f50008208d754b06f7763fbb9c6f/Screenshot%20(856).png)'

By integrating this cleaned data into Power BI, I was able to map the data geographically to answer key state-level questions regarding infrastructure adoption.

* * * * *

🔍 Key Findings & Insights
--------------------------

-   📈 **Consumption outpaces uniform adoption:** Renewable energy consumption has steadily increased overall, but the data is heavily skewed by early-adopter states with high solar/wind infrastructure (e.g., California, Texas).

-   📉 **Fossil Fuel Reliance:** Several states show stagnant renewable growth, indicating a continued reliance on legacy fossil fuel grids and highlighting major geographical gaps in adoption.

-   🔄 **The Production/Consumption Gap:** While production is growing, the regression models indicate it is not always scaling at the exact same rate as consumption demand, hinting at potential future supply chain bottlenecks or grid storage limitations.

* * * * *

💡 Conclusion
-------------

This project demonstrates the entire data lifecycle---from extracting raw government data using SQL, to predictive modeling in Python, to executive-level visualization in Power BI. Understanding these trends is the first step in optimizing energy distribution for a carbon-neutral future.

👉 Connect with me on [LinkedIn](https://www.linkedin.com/in/isaiah-l-wright/) to discuss data analytics, energy grid optimization, and Python forecasting models.
