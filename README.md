# DiaBETic
*Predicting Diabetes Risk from Health and Demographic Factors*

This repository contains data and code used for my DATA 1030 midterm presentation and final project. Check out report/report.pdf for the report including context, methods, results, and outlook.

# Structure of the repository
├── data/ --> original curated 2015 BRFSS data ([source](kaggle.com/datasets/alexteboul/diabetes-health-indicators-dataset))\
├── figures/ --> figures seen in the written report (note: src/src.ipynb contains some additional unpublished exploratory data analysis figures)\
├── results/  --> pickled best models obtained from grid search, results for the grid search, and model predictions across various cross-validation and test sets\
├── report/ --> written report on this machine learning pipeline \
├── src/ --> all code needed to reproduce results\
└── .gitignore --> to be ignored\
└── LICENSE --> MIT license file\
└── README.md --> you're reading this :)

# Purpose
Diabetes has a profound impact on many Americans, despite our understanding of risk factors and preventability. Can we predict diabetes risk from readily accessible personal data to inform healthcare decisions?

# The dataset
*Overview*\
The dataset used for prediction is a subsample of the 2015 CDC Behavioral Risk Factor Surveillance Survey (BRFSS), consisting of 21 features (described below) and a binary indicator of an individual being nondiabetic or (pre-)diabetic for 253,680 individuals. 

*Features*\
Our feature matrix consists of 21 unique features.
* HighBP: a binary indicator of whether an individual has high blood pressure (1) or not (0)
* HighChol: a binary indicator of whether an individual has high cholesterol (1) or not (0)
* CholCheck: a binary indicator of whether an individual has had a cholesterol levels check in the past five years (1) or not (0)
* BMI: an integer representing an individual's BMI
* Smoker: a binary indicator of whether an individual has smoked over 100 cigarettes in their lifetime (1) or not (0)
* Stroke: a binary indicator of whether an individual has had a stroke in their lifetime (1) or not (0)
* HeartDiseaseorAttack: a binary indicator of whether an individual has had coronary heart disease or myocardial infarction (1) or not (0)
* PhysActivity: a binary indicator of whether an individual has participated in physical activity (excluding work) in the past 30 days (1) or not (0)
* Fruits: a binary indicator of whether an individual eats fruits daily (1) or not (0)
* Veggies: a binary indicator of whether an individual eats vegetables daily (1) or not (0)
* HvyAlcoholConsump: a binary indicator of whether an individual is a heavy drinker (14 drinks/week for men, 7 drinks/week for women, 1) or not (0)
* AnyHealthcare: a binary indicator of whether an individual has any healthcare coverage (1) or not (0)
* NoDocbcCost: a binary indicator of whether an individual was unable to visit a doctor when needed in the past year due to cost (1) or not (0)
* GenHlth: an ordinal representation of an individual's self-assessed general health on a scale from 1-5 (lower=better)
* MentHlth: the number of days an individual had poor mental health in the past 30 days
* PhysHlth: the number of days an individual had an illness or injury in the past 30 days
* DiffWalk: a binary indicator of whether an individual has difficulty walking or climbing stairs (1) or not (0)
* Sex: a binary indicator of an individual's sex (male = 1, female = 0)
* Age: an ordinal representation of an individual's age category

| Feature | Age (years) |
----------|--------------
| 1 | 18-24 |
| 2 | 25-29 |
| 3 | 30-34 |
| 4 | 35-39 |
| 5 | 40-44 |
| 6 | 45-49 |
| 7 | 50-54 |
| 8 | 55-59 |
| 9 | 60-64 |
| 10 | 65-69 |
| 11 | 70-74 |
| 12 | 75-79 |
| 13 | >80 |

* Education: an ordinal representation of an individual's education level

| Feature | Education Level |
----------|---------
| 1 | Never attended/only kindergarten |
| 2 | Grades 1-8 |
| 3 | Grades 8-11 |
| 4 | Grade 12/GED |
| 5 | College (1-3 years) |
| 6 | College (4 years) |

* Income: an ordinal representation of an individual's income

| Feature | Income (USD) |
----------|---------
| 1 | <10k |
| 2 | 10k-<15k |
| 3 | 15k-<20k |
| 4 | 20k-<25k |
| 5 | 25k-<35k |
| 6 | 35k-<50k |
| 7 | 50k-<75k |
| 8 | >75k |

# The pipeline
The machine learning pipeline developed consists of three major phases.
1. Exploratory data analysis
How are features distributed in the dataset? Are there any obvious correlations within the features/target variable?\
Relevant figures: age.png, bmi.png, feature_corr.png, health.png, stroke.png, target_corr.png, target.png\
Relevant source code: Exploratory data analysis\
2. Model training and evaluation
Can a machine learning model learn from this data? Four models (below) were trained and evaluated on F1 score. 4-fold cross-validation was used to tune select hyperparameters through a grid search for each model before evaluation on a held-out test set (20% of data) for five random stratified splits. Confusion matrices for each model were inspected and test performance was compared.\
| Algorithm | Hyperparameters Tuned | Other Notes |
|-----------|-----------------------|-------------|
| Logistic Regression | C, L1 Ratio | Elastic Net regularization |
| Random Forest | max features, max depth | N/A |
| Decision Tree | max features, max depth | N/A |
| XGBoost | max depth, L1/L2 regularization | 50 early stopping rounds,  $\frac{2}{3}$ of rows and $\frac{9}{10}$ of columns per tree, learning rate 0.03 |
Relevant figures: confusion_matrix.png, cv_test_scores.png\
Relevant source code: Model training, model evaluation\
3. Interpretation
What has our best-performing model learned? Internal global importance measures and Global SHapley Additive exPlanation (SHAP) were calculated to demonstrate how each feature is associated with the model's prediction. For four randomly selected points, local feature importances are calculated and compared.\
Relevant figures: global_shap_1percent.png, shap_{0-4}.png, xgb_feature_importances.png\
Relevant source code: Interpretation\
# Want more detail?
Check out report/diabetic_report.pdf for more methodological detail and results!
