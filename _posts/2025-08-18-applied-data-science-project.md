---
layout: post
author: Yoon Wei Shyang
title: "Applied Data Science Project Documentation"
categories: ITD214
---
## Project Background

The entertainment industry relies heavily on accurate revenue forecasting to guide production decisions, marketing strategies, and investment planning. With budgets reaching hundreds of millions of dollars, studios need data-driven tools to predict box office performance before release.
Our team’s overall business goal is to predict box office revenue and increase it by 5% through strategic insights derived from historical movie data.  

Individual Objective
My responsible for revenue prediction, was to build a machine learning model that estimates a movie’s box office earnings using only pre-release features so to ensuring no data leakage and enabling real-world application.


## Work Accomplished

I successfully:
* 	Conducted comprehensive exploratory data analysis on a dataset of over 1 million movies
* 	Identified and removed post-release metrics (AverageRating, IMDB_Rating, Meta_score, vote_average, vote_count,        popularity, overview_sentiment) to prevent data leakage
* 	Engineered highly predictive features from complex fields like genres_list, Cast_list, Director, and Star1
* 	Built and evaluated four regression models: Linear Regression, Random Forest, Gradient Boosting, and XGBoost
* 	Performed what-if simulations to quantify the impact of key variables on revenue
* 	Delivered a Random Forest model with reasonable prediction and recommendations
* 	Using ipywidget to create a interative widget to allow users to adjust a few input to predict the revenue.

This work directly supports the team’s goal by providing a reliable tool for revenue estimation and strategic decision-making.


### Data Preparation
Dataset Overview
*	Size: 1,072,255 rows × 42 columns
*	Target Variable: revenue (box office earnings in USD)
*	Key Challenges:
*	High cardinality in Director, Star1, production_companies
*	Skewed distributions in revenue and budget
*	Missing values in IMDB_Rating, Meta_score, etc.
Preprocessing Steps
1.	Filtered Data:
*	Removed movies with revenue ≤ 0 or budget ≤ 0
*	Kept only movies with valid runtime, release_year, and non-null key features
2.	Log Transformation:
*	Applied np.log1p() to revenue and budget to normalize skewed distributions
3.	Feature Engineering:
*	Genres: One-hot encoded top 10 genres (e.g., Action, Drama)
*	Production: Created is_us, num_languages, is_english
*	Cast: Engineered cast_size, star_power, and avg_cast_revenue (target-encoded)
*	Director & Star1: Used target encoding after replacing 'Unknown' with placeholders
*	Budget Efficiency: Added budget_per_minute = log(budget) - log(runtime)
*	Release Timing: Extracted release_month, release_dayofweek
4.	Removed Leaky Features:
*	Excluded: vote_average, IMDB_Rating, popularity, tagline, homepage, etc.

##### Final Features for training
<img width="1631" height="1102" alt="image" src="https://github.com/user-attachments/assets/e9af1649-5300-43f5-8149-5af97260908e" />

### Modelling
Time-Based Train-Test Split  
To simulate real-world forecasting:  
<img width="1183" height="565" alt="image" src="https://github.com/user-attachments/assets/8811af5c-3588-496f-ab5e-944e3819d140" />
  
Predict revenue for a new and upcoming movie using only data from movies released before it. Hence, the model should only train on older data, and test on newer data. This is to prevent time leakage: the model learns from future data to predict the past, which is unrealistic.
  
###### Models Trained  
Four regression models were compared:
*	Linear Regression – Baseline
*	Random Forest Regressor
*	Gradient Boosting Regressor
*	XGBoost Regressor    
All models used 80% training / 20% testing split and were trained on the same preprocessed 28 features.


### Evaluation
5. Evaluation
Model Performance (Test Set)

 <img width="940" height="292" alt="image" src="https://github.com/user-attachments/assets/49445130-44af-4662-a551-f820c1009adf" />
  
Note: Metrics are on log-transformed revenue. Higher R² = better fit. 
Best Performer: Random Forest achieved the highest R² (0.918) and lowest RMSE/MAE — indicating strong generalization.
________________________________________
Actual vs Predicted Revenue (Sample)
<img width="1431" height="874" alt="image" src="https://github.com/user-attachments/assets/77c76039-21a1-4c74-97eb-f10d0348440b" />
 
The model captures trends well, especially for mid-to-high-revenue films. 
________________________________________
Feature Importance (Random Forest)
 
<img width="940" height="622" alt="image" src="https://github.com/user-attachments/assets/54186cdd-b705-460e-a20f-e25c0f9d1f83" />

Key Insights:
1.	log_budget is the most important feature (65.0% importance)
2.	director_target_enc is second (24.7%) — shows director reputation strongly impacts revenue
3.	star1_target_enc (2.9%), runtime (1.9%), and budget_per_minute (1.7%) also contribute
4.	Genre (genre_Comedy) and cast size have minor influence

This confirms that budget and director success are the strongest drivers of revenue. 

## Recommendation and Analysis
Explain the analysis and recommendations

Recommended Model
Random Forest Regressor is the best-performing model and should be used for deployment. 
Despite Linear Regression's simplicity, Random Forest outperforms across all metrics, offering:
*	Higher R² (0.918)
*	Lower RMSE and MAE
*	Better handling of non-linear relationships
________________________________________

We simulated changes to evaluate their impact:
A base case movie was created using the mean of test set features:
* Predicted Revenue: $32,296  
We simulated changes to evaluate their impact:
  <img width="940" height="466" alt="image" src="https://github.com/user-attachments/assets/a7eab7ae-ee1c-4bdd-a549-9c464921157b" />
 
Key Insight: A 10% increase in log-transformed budget had the largest positive impact on revenue among all tested changes. Which meaning a 10% increase in budget leads to a 30.3% increase in predicted revenue which is far exceeding the 5% goal.  

## Explanation of Analysis and Recommendations
Why Budget Matters Most  
The model shows that increasing budget has the strongest effect on predicted revenue.  
  
This aligns with industry knowledge:
* Higher budgets → more marketing, better VFX (visual effects), bigger casts  
* Studios can use this insight to justify strategic spending on high-potential films
* Director and Star Impact  
  Changing to a top-tier director (e.g., Christopher Nolan) increases revenue by 969% but this reflects a jump from a weak     base director. In reality, director changes have large but bounded impact but still director reputation is second only to    budget
* Genre and Release Timing  
  Adding 'Action' genre boosts revenue by 8.1% — confirms its broad appeal
  Release month has negligible impact — suggests timing is less critical than content and budget

  
#### To achieve the 5% revenue increase goal:

* Increase budget by just 1.5–2% → likely to yield >5% revenue gain
* Combine with Action genre or strong director for amplified effect  
This provides a clear, data-backed strategy for studios.

## AI Ethics
 
While the model provides valuable insights, we must consider ethical implications:  

##### Privacy
* The dataset used contains publicly available metadata about movies (e.g., director, cast, genre, budget) sourced from platforms like TMDB and IMDb.
* No personal or sensitive user data (e.g., audience demographics, viewing habits, or private financial records) was used.
* All features are aggregated at the movie level, not individual level, minimizing privacy risks.
Therefore, privacy concerns are low in this context.
##### Fairness
The model may systematically favor certain groups due to historical biases in the data:
*	The model favors Hollywood-centric features (US production, English language), which may undervalue international or indie films
*	Actors and directors from underrepresented regions may be penalized due to lower historical revenue (a systemic bias)    
* This could perpetuate a cycle where studios continue to invest only in "proven" (i.e., Western, male, English-speaking) talent, reinforcing existing disparities.  
* To mitigate this, stakeholders should use the model as a decision-support tool, not a gatekeeper, and actively consider diverse voices in greenlighting decisions.
##### Accuracy
* The Random Forest model achieves an R² of 0.918 on the test set, indicating high predictive accuracy. However, accuracy does not imply truth as the model reflects historical patterns, including market inefficiencies and biases.
* Predictions for low-budget, international, or niche films may be less accurate due to sparse data.

##### Accountability
* The team maintains full documentation of data sources, preprocessing steps, model selection, and assumptions.
* A time-based train-test split was used to simulate real-world forecasting, ensuring the model is evaluated fairly and its performance reflects realistic deployment conditions.
* If the model is used to deny funding to a film, there must be a human-in-the-loop to review and justify the decision as the model does not make final decisions.
* Regular audits and retraining should be conducted to ensure the model remains fair and accurate as market trends evolve.
##### Transparency
* The feature engineering process is fully documented, including how director_target_enc and star1_target_enc were created using cross-validated target encoding to prevent leakage.
* Feature importance analysis (top 3: log_budget, director_target_enc, star1_target_enc) provides insight into what drives predictions.
* An interactive ipywidget interface was developed to allow users to simulate changes and understand model behavior — promoting interpretability.
However, Random Forest is not fully interpretable like linear models. Therefore, we emphasize global explanations over individual predictions and avoid presenting results as infallible.


## Conclusion  
This project successfully developed a model to predict box office revenue using pre-release features. By leveraging feature engineering and avoiding data leakage, We built a tool that can guide budgeting and marketing strategies to increase revenue by 5% or more.

The final recommendation is to deploy the Random Forest model in a decision-support dashboard, enabling studios to:

Simulate outcomes for new projects
Compare scripts based on cast, genre, director and budget
Optimize investments with data-backed confidence

## Source Codes and Datasets
Upload your model files and dataset into a GitHub repo and add the link here. 
