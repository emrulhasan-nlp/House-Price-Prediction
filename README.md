# Do you want to know how much your house worth? Here is my approach.

As you can guess the price of any house varies on different features like size, area, age and so son. And it's always difficult to find the accurate price of the house without analysing properly. In this project, I examine 79 explotory variables describing (almost) every aspect of residential homes in Ames, Iowa, and predict the price of each house.

# Data 
I collected data from Kaggle competition. Kaggler's were challenged to predict the house of residential homes in Ames, Iowa. There are two data files. One of them is train data and the other one is test data. Both of them have same number of rows and columns except the target columns on test data. The dataset has total of 79 features and 1460 records. There is no duplicate entries but missing value. 

# File Guide
I have created two separate notebooks to keep the data preprocessing and data modeling separate. 
House_Price_Prediction_EDA - This notebook describes obtaining data, EDA and data preprocessing
House_Price_Prediction - This notebook contains data preprocessing, modeling, parameter tuning and predictions.

# Data Preprocessing
As a part of data cleaning I have impelemented following steps:
* Removed the id columns because, id column does not have any contribution to prediction.
* Removed suspicious outliers at salaries above 700,000. Box plot indicates salaries above 700,000 are very far from the 4th quartile.
![SalePrice](https://user-images.githubusercontent.com/33338872/74671615-3032a080-5171-11ea-9340-1f8b6c63e7c0.jpg)


* After further analysis, i also determined to remove following entries:
'GrLivArea'>4000 and 'SalePrice'<300000
'LotFrontage'>250  & 'SalePrice'<300000
'BsmtFinSF1'>1400 & 'SalePrice'<400000
'TotalBsmtSF'>5000 & 'SalePrice'<300000
'1stFlrSF'>4000   & 'SalePrice'<300000

![GrLivArea](https://user-images.githubusercontent.com/33338872/74672525-11cda480-5173-11ea-887e-b6a044112636.jpg)
![LotFrontage](https://user-images.githubusercontent.com/33338872/74672354-b69bb200-5172-11ea-9fd8-0cbb5c542491.jpg)
![BsmtFinSF1](https://user-images.githubusercontent.com/33338872/74672407-d337ea00-5172-11ea-9e1f-fe159049e960.jpg)
![TotalBsmtSF](https://user-images.githubusercontent.com/33338872/74672576-3164cd00-5173-11ea-9baf-631d267a3e0d.jpg)
![1stFlrSF](https://user-images.githubusercontent.com/33338872/74672414-d632da80-5172-11ea-9290-0b72eb6f3e3c.jpg)


Before checking the missing data I merge the train and test data together. Because to evalute models test data also need to be cleaned. So i cleaned them all together.

All together, there are total of 35 entries with missing values. PoolQC, MiscFeature, and Alley have more than 90% data missing. Following detail are applied to impute missing values.
* Missing values indicate None is replaced with zero(0).
*'LotFrontage'-Linear feet of street connected to property has been replaced with neighborhood house.
*  'Electrical'-Electrical system, replaceed with most frequent occuring event. It has very few amount of data missing.



# Exploratory data analysis
First of all, looking tat categorical data, lotshape indicates SalePrice increases slowly with lotshape. Lot configuration doesnot much depend on SalePrice. It is obvious that SalePrice increases smoothly with Heating Quality. Average saleprice is pretty much same for BsmtFintype2.

![LotShape](https://user-images.githubusercontent.com/33338872/74752622-946a6880-5234-11ea-918d-c17c25f534b1.jpg)
![HeatingQC](https://user-images.githubusercontent.com/33338872/74752706-b8c64500-5234-11ea-8c9d-de07dd85f35b.jpg)
![BsmtFinType2](https://user-images.githubusercontent.com/33338872/74752858-ec08d400-5234-11ea-8f05-17cad48bef73.jpg)
![LotConfig](https://user-images.githubusercontent.com/33338872/74756531-5112f880-523a-11ea-930f-c1f063c6b64c.jpg)

Moving into numerical data, TotalBsmtSF, 1stFlrSF, 2ndFlrSF, GarageArea and GrLivArea are linearly correlated with SalePric. Finally looking at the data correlation plot, it is obvious that OverallQual is the highly correlated feautre followed by GrLivArea, GarageCars, Garage Area and TotalBsmtSF. 1stFlrSF, FullBath and YearBuilt have same correlation with SalePrice. Least correlated feature is BsmtFinSF1.

Since GrLivArea and Garage Area have high influence on Saleprice we can create a new features call total area.
Some of the columns are numerical but they are actually categorical. They are 'MSSubClass', 'OverallCond', 'YrSold', 'MoSold'. These columns are converted into categorical columns.

Converting categorical data to numerical data:  Some features are ordinal and some of them are nominal. For ordinal data i applied label encoding and for nominal data i did one hot encoding

Finally, I separated the test and train data. New cleaned test and train data have shape of (1459, 234) and (1421, 234).


# Modeling
Based on EDA, I  chose following models to try:
Lasso, 
RandomForestRegressor,
ElasticNet, 
KernelRidge and
GradientBoostingRegressor

At first I tried with defult parameter and used RMSE as evaluation metric. KernelRidge regression gives lowest RMSE which is 0.120578.
I used grid search method, to find the best parameter. With the best parameters lowest MRSE is given by Lasso and the improved  RMSE is 0.113286. 

Thanks for reading my project!
