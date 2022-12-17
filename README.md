# boston_housing_tuning_boosting
## < Nicole An > 
# Executive Summary 
## Overview
For some governments, property taxes can be a primary revenue source. For many homeowners, on the other hand, property taxes are one of the major expenses of the year. Accurately assessing property values guarantees that home owners are paying fair amount of their earnings that ultimately benefit both the community and tax payers themselves. Therefore, the Tax Authority of the City of Boston turns to machine learning to help predict the assessed values of properties of the greater Boston Area. 3 regression models are constructed based upon the dataset given, a linear regression, a random forest, and a XGBoost model. The objective is to beat the benchmark of the current model and make the most effective predictions for property values so the City of Boston could distribute more accurate property tax assessments.
## Business Problem
What variables are the most significant in predicting the housing price of residential single-family homes in the greater Boston Area? How accurate is the analytical results of the best model? Are owner-occupied homes, homes in the 1990s, and remodeling significant influencers of the home values? To construct the best models and answer the questions above, three models are built and compared, a linear regression, a random forest and a XGBoost model. The metrics of evaluation are RSQ, RMSE, and MAE. 
error=y - y ̂
###	R Squared (RSQ)
This metric quantify the strength of the relationship between two variables. In this case, RSQ shows the correlation between the home values and the predictor variables in the model. It entails the proportion of the shared variation which ranges from 0 to 1. The higher the RSQ, the better fit the model is to the data. For example, a RSQ of 0.9 means that 90% of variation in home values can be explained by the predictors. 
###	Square Root of Mean Squared Error (RMSE)
√((∑▒〖(error)〗^2 )/n)
	This metric calculates the error, the difference between the actual data from the predicted values. It has the same unit as the target variable and it is a good measure of how accurately the model predicts the target variable, the home value. The larger the correlation, the smaller the error is and the better fit the model is. However, it can be difficult to quantitively interpret the value of RMSE, but we can use it to compare between models and usually the best model has the lowest RMSE. 
###	Mean Absolute Error (MAE)
|error|/n
	The metric that evaluate the overall model in terms of mean error, the average non-negative difference between the actual data from the predicted values. It also shares the unit with the target variable, home values. For example, a MAE of 50000 means that the average difference between the predicted home value and the actual home value is $50,000. This is a pretty small error consider the average home value is $418,700. When the City of Boston conduct property assessments, it will be a relatively some error when calculating the property tax.
Explanatory Analysis on Variables
Explore the Target
 
## Target Variable	
mean	median	sd
Av_total	448,563.6	418,700	147,761.2
The target variable for the model, av_total, is the home values. It is relatively right skewed with some large outliers, meaning there are a few highly priced homes in the greater Boston Area. Therefore, the median should be a better measure of centrality because mean is artificially increased by the large outliers. The center of home values is $418,700, describing the most common value of the homes in the greater Boston area. 
## Explore the Predictors
###	Owner occupancy: 
 
The boxplot shows that the owners who received residential exemptions as owner-occupied property have generally lower mean property values of $31,570.6 less. This indicates that it may be an important influencer of home values. However, it did not show up as one of the top 10 important variables in the best performing model, so it is an insignificant predictor here.
###	Houses build in the 1990s: 
 
From the boxplot we can see that homes built in the 1990s have a smaller spread compared to the homes built in other years. It has a normal-like distribution, meaning that homes built in the 90s cluster closely to the median ~$420,000. Homes that are not built in the 90s have a lower median, but it is extremely right skewed with high outliers. This could be an indication that whether the home is built in the 90s influences the its value positively. 
###	House remodeling: 
 
The boxplots shows clear difference between houses that are recently remodeled and houses that are not remodeled. The median home value of remodeled houses is approximately $75,000 higher than the median home value of non-remodeled houses. Therefore, remodeling can be a significant factor that influence home values.
###	House age:
 
The scatterplot above shows the relationship between age of the houses and the values of the houses. The red dash line indicates the mean home age, and it divides scatters with higher home values from those with lower home values. Therefore, it is clear that younger homes tend to have higher home values. 
## Methodology 
	Data partitioning
	The holdout dataset is splitted randomly by 80/20 where 80% were training data and 20% were testing data.
	Data preprocessing
	Formula for the Models
	av_total~own_occ+land_sf+home_age+yr_remod+living_area+num_floors+r_bldg_styl+r_roof_typ+r_ext_fin+r_total_rms+r_bdrms+r_full_bth+r_half_bth+r_bth_style+r_kitch+r_kitch_style+r_heat_typ+r_ac+r_fplace+r_ext_cnd+r_ovrall_cnd+r_int_cnd+r_view+population.y+pop_density+median_income+city_state+remod_yes+built_in_90s 
	Numeric Predictor Pre-Processing 
	Replaced missing values from land square feet with median 
	Relaced missing values from remodeled year with 0
	Removed variables with low variances (almost all of the same number)
	Added new variable “home age” by subtracting remodeled year from built year
	Categorical Predictor Pre-Processing 
	Assigned a novel factor level to potential new levels in testing dataset
	Encoded categorical variables with dummy variables of 0s and 1s with one-hot encoded
	Pooled rarely occurring levels together and created a level called “other”
	Added new binary variable “is remodeled” by setting 1 = remodeled and 0 = not remodeled
	Added new binary variable “built in 90s” by setting 1 = built in 90s and 0 = built in other times
	Model specification  
	Linear Regression Model
	Hyperparameters are fixed at mixture = 1 (lasso) and penalty = 0.01
	Random Forest Model
	Trees = 300
	Min_n = 3
	Hyperparameters are tuned with grid search and k-fold CV at k = 10
	XGBoost Model
	Trees = 1923
	Tree_depth = 5
	Learning rate = 0.01099
	Hyperparameters are tuned with Bayes method and k-fold CV at k = 10
## Model Analysis  
### Tuning Analysis by Model
<Random Forest Model Hyperparameters>
  

The grid research method is adopted to tune the hyperparameters of the random forest model, the number of trees and minimum number of data points allowed in one node. It ran in total of 10-folds^2 parameters=100 tuning experiments to produce the best hyperparameter combination. We can see that the model performs best when min_n is 2 and generally worsens as min_n increases. That means that the random forest model performs better when the allowed number of data points in one node is smaller. From the graph above, we can see that the model performs better as the number of trees increase. The RMSE is lowest when number of trees is higher at 250. A model with more than 250 trees could be overfitting the training data, making predictions on home values less accurate. The table below shows the hyperparameters for the best performing model.


<XGBoost Model Hyperparameters>
  
 

The Bayesian model-based optimization method is adopted to tune the hyperparameters of the XGBoost model, learn rate, number of trees, and tree depth. The learning rate fine tunes the model to lower the overall variance, the difference between predicted home values and actual home values. From the graph above, we can see that any value lower than 0.04642, the learning rate is too slow and the model underfits while any value higher than 0.04642, the model learns too fast and becomes overfitting the data. The model performs best at the sweet spot of approximately 0.04642, where RSQ is the highest. The model performs very well with a RSQ of approximately 0.87 when tree depth is 4, and it worsens as the tree depth increases. A tree that is very complex may be overfitting the training, resulting in a suboptimal state for testing performance. The number of trees is 1338 when the model performs the best. Due to ensemble learning, the more trees trained, the more errors the model learns from the past. The table below shows the hyperparameters for the best performing model.


### Feature Importance by Model 
 
 
 
Living area and median income are the top 2 important variable for both random forest and XGBoost models. The land size that the house takes is also an important factor that influence the home value. In addition, home value depends on the conditions of the house, both external and internal. The linear regression model shows that kitchens with no remodeling, semi-modern style, and modern style all decrease the home values. 
Fireplace is an important factor for determining the home value in the XGBoost model. The graph below shows that most households have either 0 or 1 fireplace in the house, and having no fireplace will potentially decrease the home value.
 
The location where the home resides seem to be a very significant factor in predicting home values using the random forest and XGBoost models. For example, the population, population density, and city are variables with high importance. The boxplot below shows that homes in Jamaica plains, MA, a place with higher median income (~$9700 above average) and lower population density (~750/mile2 less than average), intrigue middle-to-high class residents who drive up the home values. 
 

### Model Performance and Selection
<Testing Performance>


The table above shows the metrics that are used to evaluate the best model. XGBoost has the lowest RMSE, which indicates that it has the overall lowest difference between the predicted home values and the actual home values. Therefore, this model is the best fit of the data. It also has the highest RSQ, meaning that about 87.8% of the variation in home values can be explained by the XGBoost model. This is very high in terms of prediction power. Also, it has the lowest MAE as well. That is, the average error that predictions from this model make is only $37,175.08. It is about 8.2% of the average home values, a small percentage of what we are predicting. 
## Top 10 Best and Worst Predictions from the Best Performing Model (XGBoost)
<Best Predictions>


	These are homes built in the 19th and 20th centuries and with less remodeling. 
	These are homes that are typical. The external conditions, overall conditions, internal conditions, and views for these homes are all in average category. 
	The home ages generally revolve around the mean home age, 64.06.
<Worst Over Predictions>


	80% of the homes have very large living areas, significantly higher than the mean living area (mean = 1659 sqft).
	Most homes (90%) have colonial building style (mean = 60%).
<Worst Under Predictions>

	80% of the under estimations are occupied by owners, which is lower than the average percentage of owner-occupied rate (88.2%).
	All of these homes are from Jamaica Plain, MA, a city with high median income and a relatively low population density. 
	Most homes tend to have 3 or more bedrooms.
## Key Insights
*	Living area is the most significant influencer of home values in the best performing XGBoost model. From the scatterplot below, we can see a strong positive correlation between living area and home values, where homes with higher living area often receive higher assessed home values. House prices in the real estate market are often calculated in price per sqft of living area. Therefore, the assessed home values would be highly dependent on the size of the house. 
 
*	Home that are recently remodeled tend to have higher assessed values. This explains why the conditions of home are important variables of the XGBoost model. Since homes with better conditions require less maintenance and repairs, they naturally have higher home values. 
*	Fireplace is an interesting factor that is very important to predicting home values with the XGBoost model. The number of fireplaces is positively related to home values, indicating that homes with no fireplaces tend to have lower home values. Although people do not use fireplaces to heat the houses anymore, they seem to be essential features of homes, removing it tend to decrease the home value. 
## Recommendations 
*	The City of Boston could put more human capitals into investigation on living areas of each home to better predict the property values because it is the most important variable of the XGBoost prediction model. For instance, a detailed analysis on the usable spaces within the living area could improve the current model. One of the common feature of the worst under-estimated predictions is that those homes tend to have higher # of bedrooms. If there is data regarding the usable space of each home, excluding areas such as garages, attics, it could help in predicting how much the home actually worth. 
*	The City of Boston can gather information on community features to improve the model performance. The current XGBoost model indicates that location is very important to predicting the home values. Therefore, features within the community, such as rankings of local schools, # of employment opportunities, or # of shopping malls could be important factors in predicting home values. Also, the property tax of previous years could also be important because the tax revenue often goes to building a better community for next year. 
*	The City of Boston could investigate on the outliers of certain features to find out the reason of over/under estimation of the model. There are typically strong common features of homes that are over/under estimated. The City of Boston can further analyze the homes with colonial buildings to improve the predictions on those homes. In addition, the worst under-estimated homes all reside in the Jamaica Plains, so the City of Boston should review the accuracy of those data points from Jamaica Plains to understand the reason behind it. 
## Data Dictionary
Variables are selected based on explanatory analysis of all predictor variables and business requirements from stakeholders.
Name	Keep	Key
PID		
ZIPCODE		
OWN_OCC		
AV_TOTAL	Target	
LAND_SF		
YR_BUILT	Ignore	
YR_REMOD		
LIVING_AREA		
NUM_FLOORS		
STRUCTURE_CLASS	Ignore	
R_BLDG_STYL		
R_ROOF_TYP		
R_EXT_FIN		
R_TOTAL_RMS		
R_BDRMS		
R_FULL_BTH		
R_HALF_BTH		
R_BTH_STYLE		
R_KITCH		
R_KITCH_STYLE		
R_HEAT_TYP		
R_AC		
R_FPLACE		
R_EXT_CND		
R_OVRALL_CND		
R_INT_CND		
R_INT_FIN	Ignore	
R_VIEW		
HOME_AGE		Age of the house since built
REMOD_YES		0=not remodeled; 1=remodeled
BUILT_IN_90S		0=not built in90s; 1=built in 90s

