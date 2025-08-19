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
•	Conducted comprehensive exploratory data analysis (EDA) on a dataset of over 1 million movies
•	Identified and removed post-release metrics (e.g., vote_average, IMDB_Rating) to prevent data leakage
•	Engineered highly predictive features from complex fields like genres_list, Cast_list, Director, and Star1
•	Built and evaluated four regression models: Linear Regression, Random Forest, Gradient Boosting, and XGBoost
•	Performed what-if simulations to quantify the impact of key variables on revenue
•	Delivered a robust, interpretable model with actionable recommendations
This work directly supports the team’s goal by providing a reliable tool for revenue estimation and strategic decision-making.


### Data Preparation
3. Data Preparation
Dataset Overview
•	Size: 1,072,255 rows × 42 columns
•	Target Variable: revenue (box office earnings in USD)
•	Key Challenges:
•	High cardinality in Director, Star1, production_companies
•	Skewed distributions in revenue and budget
•	Missing values in IMDB_Rating, Meta_score, etc.
Preprocessing Steps
1.	Filtered Data:
•	Removed movies with revenue ≤ 0 or budget ≤ 0
•	Kept only movies with valid runtime, release_year, and non-null key features
2.	Log Transformation:
•	Applied np.log1p() to revenue and budget to normalize skewed distributions
3.	Feature Engineering:
•	Genres: One-hot encoded top 10 genres (e.g., Action, Drama)
•	Production: Created is_us, num_languages, is_english
•	Cast: Engineered cast_size, star_power, and avg_cast_revenue (target-encoded)
•	Director & Star1: Used target encoding after replacing 'Unknown' with placeholders
•	Budget Efficiency: Added budget_per_minute = log(budget) - log(runtime)
•	Release Timing: Extracted release_month, release_dayofweek
4.	Removed Leaky Features:
•	Excluded: vote_average, IMDB_Rating, popularity, tagline, homepage, etc.

### Modelling
Models Trained
Four regression models were compared:
•	Linear Regression – Baseline
•	Random Forest Regressor
•	Gradient Boosting Regressor
•	XGBoost Regressor
All models used 80% training / 20% testing split and were trained on the same preprocessed feature set.


### Evaluation
5. Evaluation
Model Performance (Test Set)

 <img width="940" height="292" alt="image" src="https://github.com/user-attachments/assets/49445130-44af-4662-a551-f820c1009adf" />
  
Linear Regression	2.030	1.345	0.896
Random Forest	1.800	0.959	0.918
Gradient Boosting	1.946	1.185	0.904
XGBoost	1.842	1.042	0.914
📌 Note: Metrics are on log-transformed revenue. Higher R² = better fit. 
✅ Best Performer: Random Forest achieved the highest R² (0.918) and lowest RMSE/MAE — indicating strong generalization.
________________________________________
Actual vs Predicted Revenue (Sample)

 <img width="911" height="478" alt="image" src="https://github.com/user-attachments/assets/ef33da8e-4353-43d9-a7d2-43222a790217" />
 
0	$376	$100
1	$8	$8
2	$183,632,568	$114,629,169
3	$47,396,939	$75,100,000
4	$22,630,364	$97,644,617
While some predictions are off (e.g., #4), the model captures trends well, especially for mid-to-high-revenue films. 
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
•	Higher R² (0.918)
•	Lower RMSE and MAE
•	Better handling of non-linear relationships
________________________________________
What-If Simulations (Based on Base Case)
A representative base case movie was created using the mean of test set features:
•	Predicted Revenue: $25,345
•	Features: Average budget, runtime, genre, director, star, release timing
We simulated changes to evaluate their impact:
			
+10% log-budget	$27,670	+$2,324	+9.17%
Add 'Action' genre	$27,104	+$1,759	+6.94%
Change release month to July	$25,355	+$10	+0.04%
Key Insight: A 10% increase in log-transformed budget had the largest positive impact on revenue among all tested changes. 

Explanation of Analysis and Recommendations
Why Budget Matters Most
The model shows that increasing budget has the strongest effect on predicted revenue. This aligns with industry knowledge:
•	Higher budgets → more marketing, better VFX, bigger casts
•	Studios can use this insight to justify strategic spending on high-potential films
Genre Impact
Adding the 'Action' genre increased revenue by 6.94%, confirming its broad appeal. This suggests:
•	Action films have strong global marketability
•	Marketing campaigns should emphasize action elements
Release Month Has Minimal Effect
Changing release month to July resulted in only 0.04% increase, indicating:
•	Release timing is less impactful than budget or cast
•	Other factors (e.g., competition, holidays) may dominate

## AI Ethics
Discuss the potential data science ethics issues (privacy, fairness, accuracy, accountability, transparency) in your project. 

While the model provides valuable insights, we must consider ethical implications:
Bias and Fairness
•	The model favors Hollywood-centric features (US production, English language), which may undervalue international or indie films
•	Actors and directors from underrepresented regions may be penalized due to lower historical revenue (a systemic bias)
Transparency
•	Random Forest is less interpretable than Linear Regression — but we used feature importance to explain predictions
•	Avoided black-box models without explanation
Data Leakage Prevention
•	Strictly excluded post-release metrics like popularity, IMDB_Rating, and vote_average
•	Ensured all features are available before release
Responsible Use
•	The model should support, not replace, human judgment
•	Predictions should not be used to discriminate against filmmakers from non-English or non-US backgrounds
•	Regular audits should be conducted to detect drift or bias over time

## Conclusion
We built a high-performance model to predict box office revenue using pre-release features only. Random Forest achieved strong accuracy and generalization.
Recommendation: Deploy Random Forest in a decision-support dashboard for studios. This enables:
• Revenue forecasting for new projects
• Scenario testing (budget, cast, genre)
• Data-driven investment planning

Future work:
• Explore feature interactions (budget + genre)
• Use deep learning for script sentiment analysis
• Add real-time updates for market trends

## Source Codes and Datasets
Upload your model files and dataset into a GitHub repo and add the link here. 
