In a world where we simultaneously from different parts of the world people are 
ordering different items based n their needs from apps like Amazon, Walmart, flipkart etc.
Regardless of the consequences the products are also returned at a very high rate 
The estimated value of products returned globally each year is over $800 billion.
Specifically, in 2022, consumers returned merchandise worth approximately $816 billion, 
according to the National Retail Federation (NRF). This figure represents a significant portion
of overall retail sales, with an average return rate of around 16.9% for e-commerce. 

📝 Project Documentation: Product Return Prediction System

📌 Overview
This project builds a machine learning model to predict the probability of a product being returned based on various features such as product details, order timing, customer segment, and pricing information. The model also uses SHAP (SHapley Additive exPlanations) for interpretable feature importance analysis and dimensionality reduction.

🎯 Objective
Predict the likelihood that a product will be returned.

Understand the key factors influencing returns using SHAP values.

Evaluate return probability per brand within each sub-category (without using sub-category during training to avoid leakage).

📂 Dataset
Source: Retail-Supply-Chain-Sales-Dataset.xlsx
Target Variable: Returned (Yes or No → converted to 1 and 0)

🧹 Data Preprocessing
Converted Order Date and Ship Date to datetime and extracted:

Duration (in days)

Order_Month

Order_Weekday

Removed unused or high-cardinality columns:

Order ID, Customer ID, Row ID, Product ID, Postal Code, etc.

Label encoded all categorical variables.

Dropped columns: Product Name, Customer Name, Country, Retail Sales People.

📈 Feature Selection with SHAP
Trained a full-feature XGBoost model.

Used SHAP to rank feature importance.

Excluded high-cardinality ID-based columns despite high SHAP values (to avoid overfitting).

Retained features that logically affect returns, such as:

Duration, Sales, Discount, Segment, Ship Mode, Region, etc.

🏗️ Model Training
Algorithm: XGBoostClassifier (with eval_metric='logloss')

Train-test split with stratification (80:20)

Features used:

python
Copy
Edit
[
  'Duration', 'Profit', 'Sales', 'Discount', 'Quantity', 'Segment',
  'Sub-Category', 'Category', 'Ship Mode', 'Order_Weekday',
  'Order_Month', 'Region'
]
Accuracy and classification report evaluated on test set.

📊 SHAP Analysis
Visualized SHAP summary plot for feature importance.

Identified Duration, Sales, Profit, Region, and Sub-Category as top predictors.

Used SHAP scores to drop unimportant or redundant features.

📉 Brand-Wise Return Probability Analysis
Trained the model without using Sub-Category as a feature.

Used predictions to evaluate return probability by brand within each sub-category:

python
Copy
Edit
grouped = df.groupby(['Sub-Category', 'Brand'])['Return_Prob'].mean()
Result helps business stakeholders identify problematic brands for specific product lines.

✅ Results
Achieved high predictive accuracy (~97%).

Generated clear, interpretable SHAP feature insights.

Created actionable intelligence on return trends by brand and sub-category.

🧠 Key Takeaways
SHAP helps identify not only what the model uses, but also what should be used.

Excluding high-cardinality IDs improves generalization.

Brand-level return prediction supports real-world decision-making in supply chain optimization.

🗃️ Files
File	Description
retail_return_model.ipynb	Jupyter notebook with complete pipeline
top_shap_features.xlsx	Top features selected using SHAP
Retail-Supply-Chain-Sales-Dataset.xlsx	Original dataset
README.md	Project documentation
.gitignore	Git ignore file to exclude checkpoints and temp files

📌 Future Improvements
Hyperparameter tuning via Optuna or GridSearchCV.

Handle class imbalance using SMOTE or class weights.

Deploy model as an API using FastAPI or Flask.

Add time-series or lag-based features to improve prediction accuracy.
