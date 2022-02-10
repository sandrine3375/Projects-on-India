

# Project Description 
A consumer reseach based on social background
This analysis will be an indicator of level of living and of the responsiveness of demand and will give a better understanding of the consumer. 

There is a political issue: in a context of Hindu nationalism, the religion and casts are used for political purposes in order to divide Indians (the stigmatization is based food consumption)

The objective will be to see whether social characteristics determine consumption behavior or not.
Machine learning is used in order to classify consumption and see if consumption has a link with the social background.


## Dataset
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

### Website of Mospi
India - Household Consumer Expenditure, NSS 68th Round Sch1.0 Type 2 : July 2011 - June 2012, Type - 2

[link](http://microdata.gov.in/nada43/index.php/catalog/126)

### Procedure of exportation of data

Install and open Nesstar explorer

Export all data set in format sav

Install PSPP

Open file, then nouveau , then syntaxe

Write syntaxe  and execute:

SAVE TRANSLATE
  /OUTFILE="C:\Users\Lenovo\Documents\INDEDATA\block5.csv"  
  /TYPE=CSV
  /FIELDNAMES      
  /CELLS=LABELS.


### Data link - Food Expenditure 

[Data link](https://drive.google.com/file/d/11tMldwJvXpLNfM3SvTQ9qWyHvQ-Hj0vf/view?usp=sharing)

### Data link - Household characteritics
[Data link](https://drive.google.com/file/d/1BUmdb9BMgszLWAe5MJ2ngtb76RFYe-Os/view?usp=sharing)
[Data link](https://drive.google.com/file/d/1jPn8FCzlxtZalEydykIutWupqBrLJsMp/view?usp=sharing)



# Workflow
## After dowloading: data et feature selection 
The columns which presented relevant information for  analysis were the columns containing informations on class level ( revenue), geolocalisation, cast, religion

### Selection of the househoold characterics 
First dataset: 'Sector', 'HH_Size', 'Religion', 'Social_Group', 'whether_Land_owned', 'State_code'
       
Second dataset: 'Regular_salary_earner', 'Possess_ration_card', 'MPCE_MRP'

### Selection of the food items: 
First, selection of columns: 'Item_Code', 'Total_Consumption_Quantity',  'Total_Consumption_Value'

Second, selection of items: 'Cereal', 'Milk & Milk Products',
       'Pulses and Pulse Products', 'beef / buffalo meat', 'beer', 'chicken',
       'coffee, tea, juice', 'country liquor', 'edible oil', 'eggs',
       'fish, prawn', 'foreign/ refined liquor or wine', 'fruits_dry',
       'fruits_fresh', 'goat meat /mutton', 'packaged processed food', 'pan',
       'pork', 'salt & sugar', 'spices', 'tobacco', 'toddy', 'vegetables'

## Data Cleaning and Processing

The selected data where cleaned before Supervised Machine Learning Analysis:

Rename labels of columns

Build a new classification with filter creation on food items

Check for duplicates/ missing value

In numeric columns, exclude outliers. I drawn a boxplot for each items

In non-numeric columns, drop rows with small numbers of household (ex: 'Zoroastrianism)

Check collinearity. A correlation matrix was created and because of high collinearuty between pork and ganja doesn't make sense, I delete ganja ( lower number of rows than pork)

## Merging dataset
I did a pivot table on the food dataset which contains at 5000000 of rows. The items become a columns. 

Then I used the unique identifiant for household to merge the datasets. 

I checked again  collinearity with a heatmap.

## Encoding non-numerical data
I used LabelEncoder from sklearn.preprocessing

## Missing values processing
As the study in base on monthly expenses, there is a lot of missing value. So I implement missing value by zero. Each household with zero is a non-consumer.

## Unsupervised model for clustering 

### PCA
Because of the high number of columns I used PCA

Principal Component Analysis (PCA) is used for dimensionality reduction in machine learning. 

PCA is used to filter noisy datasets, such as image compression.

After I tryed different machine learning algorythms

### KMEANS 
I used KElbowVisualizer(estimator = model, k = (2,5), metric='silhouette')

![image](https://user-images.githubusercontent.com/93095187/153495168-e7b83066-b17a-4f12-8f25-6576f3a86884.png)

#### AgglomerativeClustering
I used KElbowVisualizer (estimator = agc, k = (2,5), metric='silhouette')

![image](https://user-images.githubusercontent.com/93095187/153495478-0027d11d-eaa0-49bb-9fd9-adc77d1c22b0.png)

#### GaussianMixture

The results where:

 gmm2
 
Silhouette Coefficient: 0.620
Variance Ratio Criterion: 291476.437

gmm3

Silhouette Coefficient: 0.578
Variance Ratio Criterion: 382330.351

gmm4

Silhouette Coefficient: 0.555
Variance Ratio Criterion: 469877.177

gmm5

Silhouette Coefficient: 0.539
Variance Ratio Criterion: 552613.912

### DBSCAN

The results were:

db1
Silhouette Coefficient: -0.939
Variance Ratio Criterion: 1.751

db2
Silhouette Coefficient: -0.170
Variance Ratio Criterion: 1.504

db14
DBSCAN(eps=0.02, min_samples=3)
number of clusters:  2

db15
DBSCAN(eps=0.03, min_samples=3)
number of clusters:  2

db16
DBSCAN(eps=0.04, min_samples=3)
number of clusters:  2

db14
Silhouette Coefficient: -0.170
Variance Ratio Criterion: 1.504

db15
Silhouette Coefficient: -0.170
Variance Ratio Criterion: 1.504

db16
Silhouette Coefficient: -0.170
Variance Ratio Criterion: 1.504

## Model comparaison
I choose KMEANS which have better results. 

k-means clustering tries to group similar kinds of items in form of clusters. It finds the similarity between the items and groups them into the clusters. K-means clustering algorithm works in three steps. Let’s see what are these three steps.

- Select the k values.

- Initialize the centroids.

- Select the group and find the average

https://github.com/scikit-learn/scikit-learn/blob/7e1e6d09b/sklearn/cluster/_kmeans.py


## Conclusions

After clustering I explore characteric of the cluster - on python and tableau

Insights: 
- Clustering shows poorer and richer clusters ( cluster0, richer, cluster1, poorer)

![image](https://user-images.githubusercontent.com/93095187/153496758-367c98f6-f1d9-4f85-9137-9be176822b44.png)


- In cluster1 poorer: More Scheduled Castes & Tribes (respectively 67% and 61%) + More Muslims (59% of muslims in cluster1)

![image](https://user-images.githubusercontent.com/93095187/153496796-d026a0a6-9a64-4eb2-83c0-dd36bc088c28.png)


![image](https://user-images.githubusercontent.com/93095187/153496821-cbc3d3f8-e9a5-47d4-adda-ae8e51bc6160.png)


- The main difference seems to be  based on level of revenu


The one who is poor and eats less is in majority from scheduled caste, Tribes and live more in rural sector

There is no difference of the structure of food consumption related to the cast or religion values  ( when they have the same level of expenditure, the consumption is the same, whatever you are hindou, muslim, bouddhist, christian, hight cast or low cast)

![image](https://user-images.githubusercontent.com/93095187/153496904-4b191c28-9382-4ff6-9022-081390465aaf.png)


![image](https://user-images.githubusercontent.com/93095187/153496920-af04c5a7-4cae-4e1d-bef8-3505a42f6e8e.png)

