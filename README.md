# ClimateChange_SaphiraRivera

1. One-Sentence Summary
Created a machine learning classifer that classifies a country in a particular year as a "High-Emitter" based on economic, demographic and non-CO2 energy indicators and tested whether the model is fairly applied across the three income groups.

2. The Problem
The distinction between countries' disproportionate GHG emissions is crucial to climate policy decision-making, global climate finance, and tariff policies. But a blanket ranking of emissions has the potential to unfairly punish developing countries or ignore energy inefficient practices. Striking high-emission status exclusively with structural data, such as population, income and energy use, will also help to identify the economic and energy profiles that tend naturally to be high emitting, and to identify structural disparities in the reporting of carbon footprints.

3. The Data
I used the Our World in Data (OWID) CO2 and Greenhouse Gas Emissions dataset, which includes data on country emissions of CO2 and the Greenhouse Gases (GHGs) for recent history and socioeconomic indicators.

Data Cleaning & Filtering: I excluded regional aggregate rows (e.g. "World" or "Asia" without iso_code values) from the dataset so that emissions are not counted twice. Rows containing missing core CO2 data were dropped and missing breakdown source data metrics were filled with zeros if applicable. 13,555 country-year observations were included in the cleaned dataset, with 19 features.

EDA Finding: An exploratory analysis of CO2 emissions per person showed that there was extreme right-skew, with a few fossil-fuel intensive and rich countries emitting much more CO2 per capita than the average.

4. What I Did
I defined a binary target variable high_emitter as a country-year that had co2_per_capita higher than the global median for all years.

Data Leakage Prevention: Direct components of CO2 (coal_co2, oil_co2, gas_co2 and total_ghg) were explicitly excluded to prevent data leakage. I instead chose to use independent predictors: gdp_per_capita, population, primary_energy_consumption, energy_per_capita, methane, nitrous_oxide, and year.

Modeling: I divided my data into an 80/20 train/test split (stratified by the target class), and then scaled the numeric features and created a Dummy Classifier baseline (always predicting the most common class). I then trained and tuned four main machine learning algorithms:

Logistic Regression (mathematical linear boundary)

Any similarity based voting, such as k-Nearest Neighbors (kNN) (majority voting among similar data points).

This will be a flow chart of sequential rule splits, called Decision Tree.

Random Forest (ensemble of voting decision trees)

Evaluation: Accuracy, Precision, Recall and F1 Score were used to evaluate models on unseen test data.

5. What I Found
Main Result: The best model was the Random Forest model which performed with an overall F1 Score of ~0.96 (and an accuracy of more than 96%), well above the minimum accuracy of the dummy model of 50.0%.

The Scatter Plot of GDP per Capita vs. CO2 per Capita (log scale) shows that there is a strong non-linear relationship between wealth and per-person emissions.

The energy_per_capita and gdp_per_capita variables are the greatest contributors to the classification of high emitting country-years, as indicated by the Confusion Matrix & Feature Importance Chart in the notebook.

6. Fairness Check
I used gdp_per_capita tertiles to evaluate the fairness of the models by splitting predictions with respect to the three income tiers (Low income tier, Middle income tier, High income tier).

Results: The model shows higher predictive performance for the High-Income and Low-Income countries, and a slightly higher level of false positives/false negatives in Middle-Income countries.

Explanation: Middle-income countries tend to have much more volatile energy to emissions profiles, as they often rapidly industrialize or make economic transitions as opposed to the established low or high income countries.

In October 2012, the report was finalized.The report was completed in October 2012.
In the course of this project, I gained some important insights into global carbon emissions and predictive modelling:

In addition to the per-capita CO2, there are other per-capita metrics which predict a country's emission classification very strongly: per-capita energy use and per-capita GDP. I showed that complex models, such as RF, can classify country-years of high emissions with ~96% accuracy without direct carbon emission data.

Wealth and Emissions Non-Linearity: There is a strong correlation between GDP per capita and emissions, but the relationship levels out when it gets to higher levels of development, which makes the implicit assumption that economic growth directly determines per-capita emissions at higher development stages less obvious.

Model uncertainty is found in middle income economies in transition to industry, as found in an evaluation of the fairness, Middle Income Volatility. This necessitates that predictive climate models need to be addressed with care in the context of transitional economies to avoid unfair policy or financial sanctions.

8. Limits & What's Next
Limitations: What is derived from the target is a single historical global median, so that a country-year in 1950 is equal to a country-year in 2020. Further, assuming zero emissions for missing emission sources in small countries could underestimate emissions data in the past for developing countries.

Simple linear models such as Logistic Regression were not very successful in modelling the non-linear consumption thresholds of energy without polynomial feature engineering.

What's Next: After yet another week, I would add the percentage of renewable energy as a feature and try some regional subgroup fairness outside the GDP level boundaries.

Note: This model is a statistical classification prediction for historical data, and should not be taken as a measure of unilateral trade penalties or climate liability for any nation.

9. How to Run It
To open and run the Notebook Link: Saphira_Rivera_ClimateChange (2).ipynb in Google Colab or Jupyter Notebook.

Requirements: Pandas, numpy, matplotlib, seaborn and scikit-learn are required along with Python 3.x.

Start to finish, it takes around 1 to 2 minutes. (Data is retrieved directly from the GitHub URL).

10. Team & Roles
Saphira Rivera — Lead Data Investigator & ML Developer: Lead on the Data Cleaning pipeline, Feature Engineering, Model Training/Tuning, Fairness Check Evaluation, and Notebook Compilation.
