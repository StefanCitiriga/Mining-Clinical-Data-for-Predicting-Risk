# Mining-Clinical-Data-for-Predicting-Risk
Binary Classification Problem solved with  logistic regression (K-Fold train-test split)

 
The CRISP-DM methodology has the following stages: Business Understanding, Data Understanding, Data Preparation, Modeling, Evaluation and finally, Deployment. In the context of this project:

## Business Understanding 
The business objective in this case is to create a prediction model for a binary classification problem, that would predict whether a patient is at mortality risk or not, considering a few pieces of information. Due to the nature of what needs to be predicted, it would be very important to have a high accuracy rate as primary objective and a low false negatives rate (false negatives for Risk/NoRisk=1/0  would mean low number of cases that we predicted as “NoRisk” but are actually “Risk” cases) as a secondary objective. The secondary objective is important because we are talking about people’s lives in this case.

## Data Understanding
Data collection has already been done, since the data is already given to us in a csv. file.  There is a ‘Random’ attribute, which is used to help randomly sorting the data records (although as we will see on closer inspection, these random numbers are not unique). ‘Id’ attribute is a patient record identifier, should be (and is) unique for each patient. No patient seems to have multiple sessions, since there are 1520 unique values for this attribute (that is the number of rows in the entire dataframe). 

There are a few categorical attributes: 4-category attribute ‘Indication’, YES/NO attributes ‘Diabetes’, ‘IHD’, ‘Hypertension’, ‘Arrhythmia’, ‘History’. Two more attributes, ’IPSI’ and ‘Contra’, are of numerical type. 
Finally we have the label attribute, or our target label, that should only have Risk or NoRisk as value. This is what determines the nature of the problem in the first place, in this case, binary classification.

After exploration it can be seen that there are a few missing values that need to be imputed. No missing value results in dropping records in this case. There are 5 values for our target value that are not Risk or NoRisk so those records will be dropped since they cannot be used for training or testing a model. In ‘Indication’ there are a lot of ‘Asx’ written as ‘ASx’ so we will change one of them in the other so everything is uniform.

When it comes to data quality, it can be stated that the quality is alright. There are a few things to be done during data preparation, but most data is present, free of weird values that should not be there, and there are only a few imputations to be done, which will not affect the model by a lot in case the imputing is done wrong.


## Data Preparation
When selecting data, we could probably drop ‘Random’ and ‘Id’ since they most likely do not provide any useful information. All other features can be kept, especially because there are not many of them which, makes the curse of high dimensionality not a threat.
### Data cleaning consists of dropping some rows because we have to, and imputing. 

The 5 rows that contain values other than Risk/NoRisk in ‘label’ will be dropped (after carefully checking that there have not been typing errors).

The ‘Asx’ values in ‘Indication’ are replaced by ‘ASx’. This merges those two unique values so now there are only 4 possible values mapping to those 4 categories that exist for this categorical attribute.

There is one blank space value for ‘Contra’ (‘ ‘) which is replaced by the median value for ‘Contra’ because this is a numerical attribute.

The missing values in 'Indication', 'Diabetes', 'IHD', 'Hypertension', 'Arrhythmia' and 'History' will be imputed using the mode for each of the attributes because they are categorical values and the missing values in ‘Contra’ and ‘IPSI’ will be imputed by the median value for each of these two attributes because they are numerical values.

There are no duplicate data records and we know this because all ‘Id’ values are unique, which makes the existence of duplicate records impossible.

All yes/no attributes will be replaced by 1/0 values (1=yes/, 0=no), and for ‘Indication’, one-hot-encoding will be used (this creates the need to drop the initial ‘Indication’ column).

Data integration is not relevant for the scope of this project, all data comes from the same source and in the same format.
To my understanding, the values that seem like outliers in ‘IPSI’ and ‘Contra’ are possible to encounter so they will be treated normally.

## Modeling
First, a modeling technique should be picked. Good candidates would be a Logistic Regression model or a Decision Tree model (because this is a binary classification problem). I chose to not overdo it and just pick logistic regression and do K-Fold train-test split (with 80% data for training and 20% for testing, and K = 5).

There were no troubles encountered during Modeling so there was no need to go back to Data preparation.

## Evaluation
In order to be able to evaluate the models, I have printed, for each of the 5, the accuracy scores, the non-normalized confusion matrix (plain text) and printed the normalized version of the confusion matrix (heatmap using seaborn and pyplot). The 5 models’ results are:
 
  
 ![image](https://github.com/StefanCitiriga/Mining-Clinical-Data-for-Predicting-Risk/assets/57890672/a7e68748-d663-48b9-961a-89028e4b32d6)

 ![image](https://github.com/StefanCitiriga/Mining-Clinical-Data-for-Predicting-Risk/assets/57890672/50587a3d-33a9-4a0a-aa27-2e547019b3b2)

 ![image](https://github.com/StefanCitiriga/Mining-Clinical-Data-for-Predicting-Risk/assets/57890672/27122ff3-3a8f-4c1a-829f-64e8170238c4)

 ![image](https://github.com/StefanCitiriga/Mining-Clinical-Data-for-Predicting-Risk/assets/57890672/924d41d6-d6e8-47e7-8a7d-c3e25f0f0e54)

 ![image](https://github.com/StefanCitiriga/Mining-Clinical-Data-for-Predicting-Risk/assets/57890672/321184ba-0918-42f7-ac5b-3ce2880c276d)

Average Accuracy: 0.95
Standard Deviation of Accuracy: 0.01

For this specific problem we are solving, the first priority is high accuracy and the second priority is a low number of false negatives (which means low number of cases that are actually at Risk but the model predicted them as being NoRisk). We find these 2 priorities satisfied in all 5 models, but if we had to pick one, maybe picking one of the models with 97% accuracy and 3% rate of False Negatives. 

## Deployment
 In the hypothetical case this project would eventually be deployed and used: 
 
The prediction models created can only predict on data in a certain format. For deployment, there should also be an input transformer that would check any new records and format them in the only acceptable format, maybe even impute missing values based on the dataframe used to train the model.
