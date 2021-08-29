# sales_predictions

Finalize your README with an overview of the project, an explanation of the data (including visualizations), and a summary of the results and recommendations. Think of this as an executive summary or an overview of your project.

Overview

The goal of this project is to help the retailer understand the properties of products and outlets that play crucial roles in predicting sales. Data used in this project is from the sales prediction dataset.

The data dictionary identified the following:

Variable Name	Description
Item_Identifier	Unique product ID
Item_Weight	Weight of product
Item_Fat_Content	Whether the product is low fat or regular
Item_Visibility	The percentage of total display area of all products in a store allocated to the particular product
Item_Type	The category to which the product belongs
Item_MRP	Maximum Retail Price (list price) of the product
Outlet_Identifier	Unique store ID
Outlet_Establishment_Year	The year in which store was established
Outlet_Size	The size of the store in terms of ground area covered
Outlet_Location_Type	The type of area in which the store is located
Outlet_Type	Whether the outlet is a grocery store or some sort of supermarket
Item_Outlet_Sales	Sales of the product in the particular store. This is the target variable to be predicted.

The data was cleaned and explored to find any possible relationships, trends, etc.

Data cleaning:
Item_Weight and Outlet_Size had null values.
Item_Weight null values were replaced with the mean weight; Outlet_Size, since it was categorical, was dummy-encoded. 
All categorical variables were dummy-encoded later on for use with machine learning model (e.g., Item Fat Content, Item Identifier, Item Type, Outlet Identifier, Outlet Size, Outlet Location Type, Outlet Type).

Summary
Item Weight and Outlet Size had null values. Item Weight null values were replaced with the mean weight; Outlet Size, since it was categorical, was dummy-encoded. 

The following descriptive statistics were discovered:
There seems to be a weight outlier (min of Item_Weight = 4.555). Note Item_Weight data may be skewed due to presence of NaN values for this column.
Low Fat items seemed to be the most bought (Item_Fat_Content frequency = 5089)
Average price of purchased items was about 140.99 (see Item_MRP). Possible outliers for min value and max value.
For Outlet_Size, frequency of Medium was highest (2793); however, this may be skewed due to presence of NaN values for this column.
For Outlet_Location_Type, frequency of Tier 3 was highest (3350).
For Outlet_Type, frequency of Supermarket Type 1 was highest (5577).
For Item_Outlet_Sales, there seems to be outliers with min at 33.29 and max at 13086.96

Using visualizations, the following were discovered:
Histograms: Item Weight (looks partially multi-modal because of slight peaks), Item Visibility (right skew--head left side, tail right side), Item MRP (looks multi-modal with four distinct peaks), Item Outlet Sales (right skew--head left side, tail right side)
Boxplots: Item Weight (no outliers), Item MRP (no outliers), Item Outlet Sales (skewed; lots of outliers appear at values greater than 6000)
Heatmap: seems to be correlation between Item Outlet Sales and Item MRP (corr method between Item Outlet Sales and Item MRP shows 0.5676)
Bar charts: Low Fat Items Sold (top three: Household, Snack Foods, Fruits and Vegetables), Regular Items Sold (Fruits and Vegetables, Snack Foods, Frozen Foods), Low Fat Items Sold (Supermarket Type1 sells the most), Regular Items Sold (Supermarket Type1 sells the most)

The following is a summary of the results of using machine learning models for linear regression, simple decision tree, bagged tree, and random forest:

Linear Regression
Attempted to predict for Item Outlet Sales based on all other variables
R^2 scores between training and test data are so far apart with test data score a huge negative value (training is 0.6717 and test is  -2523656567959085.5)!
RMSE for test data was also a large value (83442861607.34908) significantly greater than training RMSE (985.6956)!
Linear regression model was redone with a smaller feature matrix (Item Fat Content, Item Type, Outlet Type, Item Outlet Sales)
R^2 score becomes much lower but are closer together in value (training 0.2370 and test 0.2603); RMSE values are also closer together with no large values (training 1502.67 and test 1428.56)
Using a heatmap, there seems to be a weak correlation to barely moderate correlation between Outlet Type and Item Outlet Sales (weak positive (0.31) for Supermarket Type3 and weak negative for (-0.41) Grocery Store)

Simple Decision Tree
Features matrix and target are the same as Linear Regression
R^2 scores are consistent with those in Linear Regression (training and test data scores are very far apart); but simple decision has R^2 training score at 1.0000 and R^2 test score at 0.2320!
Setting max depth to 5 brings R^2 training and test scores down but at least they're similar in value (0.6042 for training and 0.5961 for test)
RMSE values are close together (1082.28 for training and 1055.69 for test)
Mean for Item Outlet Sales is 2181.29

Bagging Tree
Features matrix and target are the same as Linear Regression
Follows trend of high R^2 training score value and much lower R^2 test score value (0.9184 for training and 0.5304 for test)
Minor fine tuning doesn't significantly increase the values:
	50 estimators has R^2 training 0.9354 and R^2 test 0.5473
	100 estimators has R^2 training 0.9377 and R^2 test 0.5513
RMSE for 100 estimators has training 429.30 and test 1112.58

Random Forest
Features matrix and target are the same as Linear RegressionFollows trend of high R^2 training score value and much lower R^2 test score value (training 0.9379 and test 0.5514)
Setting max depth to 5 results in similar values from simple decision tree (training 0.6122 and test 0.6039)
Increasing max depth estimators realigns values back to the trend:
	Setting max depth to 30 results in R^2 training 0.9089 and R^2 test 0.5588
	Setting estimators to 200 results in R^2 training 0.9390 and R^2 test 0.5514
	Setting estimators to 400 results in R^2 training 0.9395 and R^2 test 0.5508
	Setting estimators to 1000 results in R^2 training 0.9398 and R^2 0.5517
Setting estimators to 1000 and max depth to 5 results in R^2 training 0.6121 and R^2 test 0.6049
RMSE values for 1000 estimators and max depth 5 are training 1071.50 and 1044.10 test
As per above, minor fine tuning doesn't significantly increase the values

Recommendations
For the random forest model, the RMSE values of 1071.50 training and 1044.10 test are the lowest when compared to the linear regression (training 1502.73, test 1429.34) and simple decision tree (training 1082.28, test 1055.69). Random forest has a higher training score when compared with bagging tree (training 429.30) but still a lower test score (test is 1113.58 for bagging tree).

Ultimately, we want to rely on a model that does well for R^2 score (as close to 1.0000 as possible) while maintaining similarity in R^2 scores between the training and test data. Should performance of the model be higher for either the training or test data, then it would better to maintain the integrity between training and test data then favor one over the other.

Since the target variable is Item Outlet Sales, the RMSE would be in target variable units and would indicate "closeness" to the line of best fit for the target variable. Therefore, the lower value for RMSE would be better.

Given the above, I would recommend the random forest model set at max depth of 5 with 1000 estimators. This would give R^2 training of 0.6121 and R^2 test of 0.6049 and RMSE training of 1071.50 and RMSE test of 1044.10.
