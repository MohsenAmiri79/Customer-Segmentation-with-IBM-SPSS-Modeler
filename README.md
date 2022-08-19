
# Banking Customer Segmentation (IBM SPSS Modeler)

This is a simple data cleaning and clustering project done for educational purposes.

In this project, I have used multiple related customer information datasets and combined them together to make a customer segmentation dataset for others. This projects creates a dataset for Customer Segmentation and Customer Status Prediction.


## Usage Instructions
After cloning the main branch, open the ```'SPSS Stream/BCS.str'``` file in IBM SPSS Modeler.
Then, Open the 'Importing Data' stream and for each input module, change the file directories to your own directiory, click 'refresh' and click ok.
You also may want to change the directory to which the output file is saved. You can do so by opening the last module in the main stream and changing the saving path.
## Description

This projects consists of 4 major parts that I am going to explain in this section.
1. Importing the input data
2. Cleaning the data
3. K-Means clustering
4. Preparing the output

### 1. Importing the data
In this project we use 2 data files that have been included in the repository:
- The 'customer_personal_info.csv' file:
> This file contains 700 rows of number indexed records each containing 5 personal features about the customer. it also comtains some missing values that need to be dealt with.
- The 'customer_credit_info.xlsx' file:
> This file consists of two excel sheets which in total, contain 700 rows of credit related info about customers. this file is also number indexed and is matched with the previous file on the 'ID' feature (index).

In this section the two sheets are appended to each other and then merges with the csv file on their matching 'ID' features. The result is a table of 700 records each with an index and 9 other features.

### 2. Cleaning the data
This part is divided into three subtasks:
1. Managing outliers
2. Managing missing values
3. Rescaling the data

#### 2.1. Managing Outliers:
First an estimate is made of each features distribution using the 'Sim Fits' module.
As it can be seen, none of the inputs have normal or normal-like distribution.
** BUT ** for the educational purposes of this project, we will assume that the 'age' and 'debt_to_income' features have been assumed to have normal distribution.

In the second step, outliers of the two aforementioned features are detected using the z-score method in the 'Data Audit' module.
For the z-score method, a score of *3.0* and higher was considered an outlier and a scores above *5.0* were considered extreme cases.
These numbers were human-decided using graphs over the z-scores of records.
Since the total number of outliers detected in this section were quite low, all of the cases were coerced using Data Audit's "Action" module.

Third step is to detect and manage all the outliers in the other features.
Since the distributions of the remaining features aren't normal, *feature IQR* was used for outlier detection.
Cases with a distance of *1.5IQR* and above from the first and last quartile were considered outliers while those with a distance higher than *3.0IQR* were marked as extreme cases.
These numbers were decided from convention and quick analysis of the distribution of the features.
As the number of cases are high for these features, outliers were coerced while extreme cases were nullified.

#### 2.2. Managing Missing Values:
As the last step of this stage, missing values are managed, again using the 'Data Audit' module.
For the features with higher number of missing values (above 5%), the C&RT algorithm was used to fill in the dataset.
As for features with low number of missing values (below 1%), the value of the median of the dataset was chosen to fill the missing values.
As for the rest of the features, random values with the same distribution as the original data were generated and added to the dataset.

#### 2.3. Rescaling The Data:
Since we are going to use a distance-based algorithm on the data, it is best practice to rescale the data so different ranges of features doesn't affect the result.
For this purpose, the rescaling option of the 'Auto Data-prep' module was used. all the features were rescaled from *0.0* to *50.0* depending on their value in respect to the minimum and maximum values of that feature.

### 3. K-Means Clustering

In this part, we first have to get rid of the anomalies that might worsen the results of K-Means clustering, as this algorithm is anomaly-sensetive.
For detection of anomalies, we use the 'Anomaly' module of the program to get a anomaly index;
A value which indicates the distance of each record from the center of their hypothetical low-accuracy clusters.
Next, we plot the anomaly index of records in each hypothetical cluster and decide what the anomaly-limit should be for each group.
Then we eliminate the anomalies from the data.

After managing anomalies, we eliminate the unneeded features that were resulted in the anomaly detection phase.
Then, weapply a 'Auto Cluser' module to the data to see which number of clusters gives the best results.
To evaluate the cases, we use silhouette's value.
In the case of this project, the value of K=2 gets a silhouette's value above 4 which is the best of the tested numbers.
Next, we use the 'K-Means' module with K=2 with its default options.

### 4. Preparing The Output
The resulted clusters are inspected and renamed as 'Premium' and 'Normal' since one cluster must have much higher value for
the bank since they have higher financial status and credit.
Next, all of the features are removed from the data except the cluster labels.
Then they are merged again with the data after 2.2.
This is done to bring the data back to their unscaled state.
The data are then sorted by their index.
At last, many graphs are plotted from data, the data is shown as a table and is also saved to a ```.csv``` file.





