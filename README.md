# ClimateChange_SaphiraRivera
Predicting High-Emission Country-Years from Global Energy and Economic Data
1. One-Sentence Summary
I built a machine learning classifier that predicts whether a country in a given year is a "High-Emitter" using economic, demographic, and non-CO2 energy metrics, and verified whether the model performs fairly across low-, middle-, and high-income nations.

2. The Problem
Climate policy decisions, global climate financing, and trade tariffs often rely on identifying countries with disproportionately high greenhouse gas output. However, evaluating emissions without context can unfairly penalize developing nations or overlook energy-inefficient practices. Predicting high-emission status strictly from structural indicators—like population, income, and overall energy consumption—helps highlight which economic and energy profiles naturally drive heavy carbon footprints, while surfacing structural disparities in global reporting.

3. The Data
I used the Our World in Data (OWID) CO2 and Greenhouse Gas Emissions dataset, covering historical country emissions and socioeconomic indicators.

Data Cleaning & Filtering: I removed regional aggregate rows (such as "World" or "Asia" which lacked iso_code values) to prevent double-counting emissions. I dropped rows with missing core CO2 figures and filled missing breakdown source metrics with zero where appropriate. The cleaned dataset contained 13,555 country-year observations across 19 features.

EDA Finding: Exploratory Data Analysis revealed an extreme right-skew in per-person CO2 emissions—a small handful of fossil-fuel-dependent and wealthy nations produced emissions per person far exceeding the global median.

4. What I Did
Target Definition: I created a binary target variable, high_emitter, where a country-year was labeled 1 if its co2_per_capita exceeded the global median across all recorded years, and 0 otherwise.

Feature Selection & Leakage Prevention: To avoid data leakage, I explicitly excluded direct CO2 components (coal_co2, oil_co2, gas_co2, total_ghg). Instead, I selected independent predictors: gdp_per_capita, population, primary_energy_consumption, energy_per_capita, methane, nitrous_oxide, and year.

Modeling: After splitting data into 80% training and 20% testing sets (stratified by target class) and standardizing numeric features, I established a Dummy Classifier baseline (always predicting the majority class). I then trained and tuned four main machine learning algorithms:

Logistic Regression (mathematical linear boundary)

k-Nearest Neighbors (majority voting among similar data points)

Decision Tree (flowchart of sequential rule splits)

Random Forest (ensemble of voting decision trees)

Evaluation: Models were evaluated on unseen test data using Accuracy, Precision, Recall, and F1 Score.

5. What I Found
Main Result: The Random Forest model achieved the highest performance, reaching an overall F1 Score of ~0.96 (and accuracy over 96%), significantly outperforming the baseline dummy model accuracy floor of 50.0%.

Key Visualizations: * The Scatter Plot of GDP per Capita vs. CO2 per Capita (log scale) illustrates the strong non-linear relationship between wealth and per-person emissions.

The Confusion Matrix & Feature Importance Chart in the notebook shows that energy_per_capita and gdp_per_capita are the strongest drivers in identifying high-emitting country-years.

6. Fairness Check
I evaluated model fairness by splitting test predictions across three income tiers (Low income tier, Middle income tier, High income tier) using gdp_per_capita tertiles.

Findings: The model demonstrated higher predictive accuracy on High-Income and Low-Income nations, but exhibited a slightly higher rate of false positives/negatives in the Middle-Income tier.

Explanation: Middle-income nations are often undergoing rapid industrialization or economic transition, making their energy-to-emissions profiles much more volatile compared to established low- or high-income economies.

7. Conclusion of All Main Findings
Through this project, I uncovered several key insights regarding global carbon emissions and predictive modeling:

Strong Predictability from Structural Metrics: Non-CO2 metrics—specifically per-capita energy consumption and per-capita GDP—are exceptionally strong predictors of a country's emission classification. I demonstrated that complex models like Random Forest can classify high-emitting country-years with ~96% accuracy without needing direct carbon emission inputs.

Wealth and Emissions Non-Linearity: While GDP per capita strongly correlates with higher emissions, the relationship flattens at upper wealth levels, challenging the simple assumption that economic growth strictly dictates per-capita emissions at higher development tiers.

Middle-Income Volatility: The fairness evaluation revealed that model uncertainty is concentrated in middle-income economies undergoing industrial transitions. As a result, predictive climate models must handle transitional economies carefully to prevent unjust policy or financial penalties.

8. Limits & What's Next
Limitations: The derived target uses a single historical global median, which treats a 1950 country-year under the same cutoff as a 2020 country-year. Additionally, filling missing emission sources with zero for small nations may undercount historical emissions in developing regions.

What Didn't Work: Simple linear models like Logistic Regression struggled to capture non-linear energy consumption thresholds without extensive polynomial feature engineering.

What's Next: With another week, I would implement dynamic yearly median thresholds, incorporate renewable energy percentage as a feature, and test regional subgroup fairness beyond GDP tiers.

Usage Warning: This model predicts historical statistical classification based on macro data; it should not be used as an absolute metric to impose unilateral trade penalties or climate liability on individual nations.

9. How to Run It
Notebook Link: Open and run Saphira_Rivera_ClimateChange (2).ipynb in Google Colab or Jupyter Notebook.

Prerequisites: Python 3.x with pandas, numpy, matplotlib, seaborn, and scikit-learn installed.

Execution Time: ~1 to 2 minutes start-to-finish (data is fetched directly via GitHub URL).

10. Team & Roles
Saphira Rivera — Lead Data Investigator & ML Developer: Responsible for the data cleaning pipeline, feature engineering, model training/tuning, fairness check evaluation, and notebook compilation.
