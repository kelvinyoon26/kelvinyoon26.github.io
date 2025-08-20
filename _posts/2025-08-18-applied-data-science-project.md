---
layout: post
author: Yoon Wei Shyang
title: "Applied Data Science Project Documentation"
categories: ITD214
---
## Project Background
Provide an overview of your team's project business goals and objectives and state the objective that you are working on. 

The entertainment industry relies heavily on accurate revenue forecasting to guide production decisions, marketing strategies, and investment planning. With budgets reaching hundreds of millions of dollars, studios need data-driven tools to predict box office performance before release.
Our team’s overall business goal is to predict box office revenue and increase it by 5% through strategic insights derived from historical movie data.
Individual Objective
As the lead analyst responsible for revenue prediction, my objective was to build a machine learning model that estimates a movie’s box office earnings using only pre-release features — ensuring no data leakage and enabling real-world application.


## Work Accomplished
Document your work done to accomplish the outcome.

I successfully:
* 	Conducted comprehensive exploratory data analysis (EDA) on a dataset of over 1 million movies
* 	Identified and removed post-release metrics (e.g., vote_average, IMDB_Rating) to prevent data leakage
* 	Engineered highly predictive features from complex fields like genres_list, Cast_list, Director, and Star1
* 	Built and evaluated four regression models: Linear Regression, Random Forest, Gradient Boosting, and XGBoost
* 	Performed what-if simulations to quantify the impact of key variables on revenue
* 	Delivered a robust, interpretable model with actionable recommendations

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
<img width="1631" height="1102" alt="image" src="https://github.com/user-attachments/assets/e9af1649-5300-43f5-8149-5af97260908e" />

*	Genres: One-hot encoded top 10 genres (e.g., Action, Drama)
*	Production: Created is_us, num_languages, is_english
*	Cast: Engineered cast_size, star_power, and avg_cast_revenue (target-encoded)
*	Director & Star1: Used target encoding after replacing 'Unknown' with placeholders
*	Budget Efficiency: Added budget_per_minute = log(budget) - log(runtime)
*	Release Timing: Extracted release_month, release_dayofweek
4.	Removed Leaky Features:
*	Excluded: vote_average, IMDB_Rating, popularity, tagline, homepage, etc.

### Modelling
Time-Based Train-Test Split
To simulate real-world forecasting:  
<img width="1183" height="565" alt="image" src="https://github.com/user-attachments/assets/8811af5c-3588-496f-ab5e-944e3819d140" />
* Ensures no future data leaks into training
* Models learn from past to predict future just like in production  
  
Models Trained
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

  
### To achieve the 5% revenue increase goal:

* Increase budget by just 1.5–2% → likely to yield >5% revenue gain
* Combine with Action genre or strong director for amplified effect
This provides a clear, data-backed strategy for studios.

## AI Ethics
Discuss the potential data science ethics issues (privacy, fairness, accuracy, accountability, transparency) in your project. 

While the model provides valuable insights, we must consider ethical implications:
Bias and Fairness
*	The model favors Hollywood-centric features (US production, English language), which may undervalue international or indie films
*	Actors and directors from underrepresented regions may be penalized due to lower historical revenue (a systemic bias)
Transparency
*	Random Forest is less interpretable than Linear Regression — but we used feature importance to explain predictions
*	Avoided black-box models without explanation
Data Leakage Prevention
*	Strictly excluded post-release metrics like popularity, IMDB_Rating, and vote_average
*	Ensured all features are available before release
Responsible Use
*	The model should support, not replace, human judgment
*	Predictions should not be used to discriminate against filmmakers from non-English or non-US backgrounds
*	Regular audits should be conducted to detect drift or bias over time

## Conclusion
We built a high-performance model to predict box office revenue using pre-release features only. Random Forest achieved strong accuracy and generalization.
Recommendation: Deploy Random Forest in a decision-support dashboard for studios. This enables:
* Revenue forecasting for new projects
* Scenario testing (budget, cast, genre)
* Data-driven investment planning

Future work:
* Explore feature interactions (budget + genre)
* Use deep learning for script sentiment analysis
* Add real-time updates for market trends

## Source Codes and Datasets
Upload your model files and dataset into a GitHub repo and add the link here. 
