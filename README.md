# Name:Victoria Reynolds

## Dataset

This project uses the Telco Customer Churn dataset from Kaggle. Each row is one customer, with information about demographics, services, contract type, billing, charges, and whether the customer churned. The goal is to understand which customer traits are connected to churn and to compare interpretable models that can predict churn risk.

## Assumption Checks

| Model | Key Assumptions Checked | Evidence | Concern |
|---|---|---|---|
| Linear regression | Linearity, residual patterns, homoscedasticity, residual normality, multicollinearity | Residual-vs-fitted plot, Q-Q plot, Breusch-Pagan test, RESET test, and VIF table | Churn is really binary, so treating it as continuous creates patterned residuals and can produce predictions outside the 0 to 1 range. Some encoded service variables also overlap, so individual coefficients need caution. |
| Logistic regression | Binary target, enough churn events per predictor, no obvious perfect separation, linear log-odds for continuous features, multicollinearity | Target check, events-per-predictor calculation, category-level churn rates, binned log-odds plots, and VIF table | The sample size is large enough, but some continuous effects may not be perfectly straight on the log-odds scale. Some encoded service variables are also redundant. |
| GAM | Binary outcome with a logistic setup, enough data across continuous feature ranges, smooth effect patterns, limited extrapolation | Continuous feature range table, bin coverage table, and GAM smooth effect plots | GAM curves are easier to fit than straight lines, but they take more explanation and still should not be treated as causal evidence. |

## Model Comparison

| Model | Performance Evidence | Interpretability Strength | Interpretability Weakness |
|---|---|---|---|
| Linear regression | Test ROC-AUC was about 0.828, R2 was about 0.260, and RMSE was about 0.380 | Coefficients are easy to read as changes in churn score | Churn is binary, so OLS is not the best fit and the outputs are not true probabilities |
| Logistic regression | Test ROC-AUC was about 0.836, accuracy was about 0.805, and F1 was about 0.611 | Odds ratios give a clear way to explain which factors raise or lower churn odds | It assumes mostly straight-line effects for continuous predictors on the log-odds scale |
| GAM | Test ROC-AUC was about 0.841, accuracy was about 0.797, and F1 was about 0.587 | Smooth curves show nonlinear relationships for tenure and charge variables | The curves are more complex to explain, and the model may still miss interactions |

## Recommendation

Recommended model: Logistic regression

Why this model: Logistic regression fits the yes/no churn target, performs close to the GAM, and is easier to explain to business users through odds ratios. The GAM had the highest ROC-AUC, but the difference was small and the logistic regression results are more straightforward for decision-making.

What the company can responsibly conclude: The company can identify customer traits that are associated with higher or lower churn risk, such as contract type, tenure, internet service type, payment method, and monthly charges.

What the company should not conclude yet: The company should not treat these relationships as causal. For example, the model can show that month-to-month contracts are associated with churn, but it does not prove that changing a customer contract would directly prevent churn.

One next analysis we would run: I would test interaction effects, especially whether monthly charges and tenure affect churn differently across contract types.


NOTE: To run this notebook in google colab, upload the notebook and run as-is. To run the notebook locally, you may clone this repo and import the included csv file directly if preferred.