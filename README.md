

#Project Description: a consumer reseach based on social background
This analysis will be an indicator of level of living and of the responsiveness of demand and will give a better understanding of the consumer. 

There is a political issue: in a context of Hindu nationalism, the religion and casts are used for political purposes in order to divide Indians (the stigmatization is based food consumption)

The objective will be to see whether social characteristics determine consumption behavior or not.
Machine learning is used in order to classify consumption and see if consumption has a link with the social background.


##Dataset
I will build a database from gouvernemental datasets: 
India - Household Consumer Expenditure, NSS 68th Round Sch1.0 Type 2 : July 2011 
This study has been conduct by the Ministry of Statistics and Programme Implementation came into existence as an Independent Ministry on 15.10.1999 after the merger of the Department of Statistics and the Department of Programme Implementatation. This ministry is the nodal agency for the planned and organized development of the statistical system in the country and coordination of statistical activities among different stakeholders in Government of India, State Governments as well as meeting requirements of the International Agencies.

The NSS consumer expenditure survey aims at generating estimates of household Monthly Per Capita Consumer Expenditure (MPCE) and the distribution of households and persons over the MPCE range separately for the rural and urban sectors of the country, for States and Union Territories, and for different socio-economic groups.

This is a part of a programme of quinquennial surveys on consumer expenditure and employment & unemployment, adopted by the National Sample Survey Organisation (NSSO) since 1972-73, provides a time series of household consumer expenditure data. 

Period of survey and work programme: one year duration starting on 1st July 2011 and ending on 30th June 2012. 
Randomly selected households based on sampling procedure and members of the household.
I used : three part of the database: 

Block 3 : Household characteristics.

Block 5.1 : Consumption of cereals, pulses, milk and milk products, sugar and salt.

Block 5.2 : Consumption of edible oil, egg, fish and meat, vegetables, fruits, spices, beverages and processed food and pan, tobacco and intoxicants.


##workflow
After dowloading, data et feature selection: 
The columns which presented relevant information for  analysis were the columns containing informations on class level ( revenue), geolocalisation, cast, religion

### selection of the househoold characterics 
First dataset: 'Sector', 'HH_Size', 'Religion', 'Social_Group', 'whether_Land_owned', 'State_code'
       
Second dataset: 'Regular_salary_earner', 'Possess_ration_card', 'MPCE_MRP'

### selection of the food items: 
First, selection of columns: 'Item_Code', 'Total_Consumption_Quantity',  'Total_Consumption_Value'
Second, slection of items: 'Cereal', 'Milk & Milk Products',
       'Pulses and Pulse Products', 'beef / buffalo meat', 'beer', 'chicken',
       'coffee, tea, juice', 'country liquor', 'edible oil', 'eggs',
       'fish, prawn', 'foreign/ refined liquor or wine', 'fruits_dry',
       'fruits_fresh', 'goat meat /mutton', 'packaged processed food', 'pan',
       'pork', 'salt & sugar', 'spices', 'tobacco', 'toddy', 'vegetables'

### Data Cleaning and Processing

The selected data where cleaned before Supervised Machine Learning Analysis:
Rename labels of columns
Build a new classification with filter creation on food items
Check for duplicates/ missing value
In numeric columns, exclude outliers. I drwn a boxplot for each items
In non-numeric columns, drop rows with small numbers of household (ex: 'Zoroastrianism)
Check collinearity 

###merging dataset. 
I did a pivot table on the dataset on food which contains at 5000000 of rows.
Then I used the unique identifiant for household to merge the datasets. 




missing values processing:
columns 'so2' and 'co' were deleted due to the high number of missing values
separate missing values were replaced by the average values of each pollutant for the specified month and year.
for Paris there was a period of 3 months of missing data in 2017. In this case I decided to input the missing values of each day and each pollutant as an average of these values in 2016 and 2018.
for Lyon there was a period of 6 months missing in the second half of 2021. Fortunately, these values, were presented in the other csv file at the air quality data source. Therefore, I imputed the missing values from the different file.
Outliers. In order to check for outliers, I draw a boxplot graph for each pollutant. Basing on that visual representation of the data I could state that there were no outliers in data recorded both in Paris and Lyon.
Cleaning for Supervised ML and EDA
The aim of second stage of cleaning was to prepare the data for Supervised Machine Learning and EDA. This process included:

exchanging columns with information about air quality for the clean ones from the previous stage of cleaning
changing date type to datetime
dropping columns 'latitude', 'longitude', 'address', 'datetimeEpoch', 'description' as they contain wither the same information or too various information, so they are not useful for machine learning model
Missing values processing:
columns 'snow', 'snowdepth', 'solarradiation', and 'solarenergy' were dropped due to the high number of missing values.
1 missing value for precipitation was filled with 0, as from other columns it was clear that it did not rain that day.
'windgust' contained 1 missing value and it was dropped due to the high correlation with windspeed column
1 missing value of pressure was filled with the value from the proceeding day as it presented similar weather conditions.
'Conditions' column needed encoding as it was the only categorical column. The column was encoded by creating columns with 0/1 values for each weather condition.
New columns containing information about month and day of the week were created, as they might be useful for ML model. After that column with date information was dropped.
The data was checked for collinearity between columns. The correlation matrix was created and one by one columns with the correlation above 90% were dropped. This included: 'tempmax', 'tempmin' (high correlation to 'temp').
Outliers. The boxplot for numerical columns were drawn. Basing on this visual representation I can conclude that all of the values are in the reasonable range and there were no outliers.
Column with a target variable was created basing on the recorded values of PM2.5 concentration. This pollutant is often used as an air quality indicator as its concentration is related to the concentration of other pollutants. Therefore, I created three classes basing on the values of PM2.5:
Class 0: day with good air quality, PM2.5 concentration below 50 µg/m³.
Class 1: day with moderate air quality, PM2.5 concentration between 50 and 100 µg/m³.
Class 2: day with bad air quality, PM concentration above 100 µg/m³.
After creating the target column, the columns with the concentration of pollutants ('pm25', 'pm10', 'o3' and 'no2') were dropped, but this was done before ML, as they were also used for EDA.
The cleaned data ready for Time Series and ML was saved in the separate files.

Using : python, pandas, numpy, matplotlib, seaborn

05 - Time Series
Time Series Analysis was performed separately for each pollutant (PM2.5, PM10, ozone and nitrogen dioxide) in each city. The analysis included:

Stationarity test - all analysed time series data was stationary.
Autocorrelation check - graphs showing autocorrelation and partial autocorrelation were drawn for each pollutant. These graphs pointed that there is similarity between values for high number of lags.
Decomposition to see trend line.
Train/test split in proportion 80/20 to evaluate chosen model.
Implementation of model proposed by FbProphet liabry firstly for tarin data.
Calculation of a model error (RMSE - Root Mean Square Deviation)
Fitting the model on the whole available data.
Forecasting pollutant concentration for 2022.
Plotting important figures.
The results of the Time Series Analysis are presented here: https://air-quality-final-project.herokuapp.com

The model errors for each model are given in the Table below:

Paris	Lyon
PM2.5	20.6	19.9
PM10	18.2	9.2
Ozone	9.2	9
NO2	14.7	4.2
The comparison of the created models with the real data points that the predictions of the model are correct.

Using : python, pandas, numpy, matplotlib, seaborn, statmodels, pdmarima, fbprohet

06 - Machine Learning
The purpose of this part of the project was to create a supervised machine learning model, which would correctly predict the air quality class (Good, Moderate or Bad) basing on the weather conditions during that day. For the Supervised Machine Learning part 4 models were evaluated:

Logistic Regression,
Random Forest Classifier,
Balanced Random Forest Classifier, and
pipeline with models proposed by TPOT library.
The process of Machine Learning Analysis included:

random train/test split of data in proportion 80/20.
standardization of the data for Logistic Regression model.
Features selection using Select From Model, Recursive Feature Elimination and Recursive Feature Elimination with Cross-Validation.
The results of feature selection for all models were similar. They proposed following features which were further used for models: 'temp', 'humidity', 'precip', 'windspeed', 'pressure', 'cloudcover', 'visibility', 'uvindex'.
Hyperparameters Tunning using grid search and randomized search. Interestingly the logistic regression and random forest classifier models were giving better performance using default parameters than the ones proposed from hyperparameters tunning for Paris data, therefore default parameters were used for these models evaluation for Paris. On the other hand, for Lyon the results for hyperparameters tunning were used.
Evaluations of the Logistic Regression, Random Forest Classifier and Balanced Random Forest Classifier models.
TOPT implementation:
Paris: TPOT proposed Stacking Estimator using Extra Trees Classifier and GaussianNB
Lyon: TPOT proposed Stacking Estimator using Extra Trees Classifier and XGBClassifier
Comparison of obtained results.
For both cities also Unsupervised Machine Learning models (KMeans, Agglomerative Clustering and DBSCAN) were checked in order to verify if data can be properly clustered from the perspective of air pollution. However, the obtain results were not satisfying (Silhouette Coefficient below 0.3). Thus, this part of the project was not developed further.

Results
Paris
The comparison of different models is presented in the Table below and in the Figure showing confusion matrix.

Model	Accuracy, %	Balanced Accuracy, %	f1_score, %
Logistic Regression	76.2	67.7	75.3
Random Forest Classifier	77.05	64.9	76.2
Balanced RF	71.8	78.5	72.1
Stacking Estimator	69.7	74.7	70.3
Paris_CM

Basing on the obtained results I picked Balanced random Forest as the best model for Paris data, as it has high accuracy and the highest balanced Accuracy. Looking at the confusion matrix, this model correctly classified the highest number of days with bad air quality. Thus, I recommend using this model to predict air quality from weather data in Paris.

Lyon
The comparison of different models is presented in the Table below and in the Figure showing confusion matrix.

Model	Accuracy, %	Balanced Accuracy, %	f1_score, %
Logistic Regression	79.1	69.1	78.3
Random Forest Classifier	81.7	69.9	81.1
Balanced RF	76.9	79.3	76.7
Stacking Estimator	83.4	73.2	83.4
Lyon_CM

The similar case as for Paris can be observed for Lyon. Here the highest balanced accuracy was obtained with Balanced random Forest model. However, the model proposed by TPOT i.e. Stacking Estimator also shows very good results. Therefore, I would recommend one of these two models to predict air quality in Lyon using weather data.

Using : python, pandas, numpy, matplotlib, seaborn, sklearn, imblearn, tpot, yellowbrick

07 - EDA
Exploratory Data Analysis was divided into two parts:

firstly, the historical data on air pollution and weather in Paris and Lyon was analysed and compared
secondly, the results of Time Series with the forecast for 2022 were analysed.
The appropriate graphs were prepared. They can be found in the Jupyter notebook as well as on the Streamlit dashboard.

Using : python, pandas, matplotlib, seaborn, plotly, folium

08 - Streamlit Dashboard
The results of EDA were used in order to prepare a dashboard app using Streamlit library.

The app can be visited here:

Stramlit_App

Using : Streamlit, plotly, pillow

Deploying to Heroku
The prepared dashboard/app was published using Heroku

heroku git:remote -a air-quality-final-project
git subtree push --prefix "08 - Streamlit" heroku main
Dashboard

Using : git, heroku

Conclusions
Air quality is better in Lyon than in Paris.
Trend in both cities is decreasing for all pollutants.
All pollutants show yearly and weekly seasonality.
Applied model correctly forecast pollution concentration.
Supervised ML model correctly predicts air quality basing on the weather information in both Paris and Lyon.
Created database and pipeline allows to perform the same analysis for other cities in Europe.
