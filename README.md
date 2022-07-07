# Phishing-Classifier

## PROBLEM STATEMENT

To build a classification methodology to predict whether a website is a phishing website based on a given set of predictors. 

## OBJECTIVE

Phishing is a method in which the attackers trick users into revealing sensitive information to be used in committing fraudulent activities. Phishers have used many sophisticated techniques to deceive unaware users such as leveraging social engineering tactics and technology to deliver well-crafted emails to delude users into that emails are legitimate.
The objective of the project is to classify the website as phishing and legitimate.

## **Tools/Software Used:**

  Jupyter notebook 
->Pycharm 
->Postman
->Chrome Browser 

## Architecture:
The architecture that I followed for this project is :

![image](https://user-images.githubusercontent.com/69865169/177703864-b5ddf4c5-66f0-4968-933a-ddcc68f54dd2.png)

## Steps Followed: 
These are the steps that are followed in a sequential order  to implement this project : 
## Data Validation 
In this step, I performed different sets of validation on the training files.  
1.	 Name Validation- I validate the name of the files based on the given name in the schema file. I have created a regex pattern as per the name given in the schema file to use for validation. After validating the pattern in the name,  checked for the length of date in the file name as well as the length of time in the file name. If all the values are as per requirement, I move such files to "Good_Data_Folder" else move such files to "Bad_Data_Folder."

2.	 Number of Columns - I validate the number of columns present in the files, and if it doesn't match with the value given in the schema file, then the file is moved to "Bad_Data_Folder."


3.	 Name of Columns - The name of the columns is validated and should be the same as given in the schema file. If not, then the file is moved to "Bad_Data_Folder".

4.	 The datatype of columns - The datatype of columns is given in the schema file. This is validated when I insertED the files into Database. If the data type is wrong, then the file is moved to "Bad_Data_Folder".


5.	Null values in columns - If any of the columns in a file have all the values as NULL or missing, I discarded such a file and move it to "Bad_Data_Folder".

## Data Insertion in Database
 
1) Database Creation and connection - Create a database with the given name passed. If the database is already created, open the connection to the database. 
2) Table creation in the database - A table with the name - "Good_Data", is created in the database for inserting the files in the "Good_Data_Folder" based on given column names and data type in the schema file. If the table is already present, then the new table is not created and new files are inserted in the already present table as I want training to be done on new as well as old training files.     
3) Insertion of files in the table - All the files in the "Good_Data_Folder" are inserted in the above-created table. If any file has an invalid data type in any of the columns, the file is not loaded in the table and is moved to "Bad_Data_Folder".
 
## Model Training 
1) Data Export from Db - The data in a stored database is exported as a CSV file to be used for model training.
2) Data Preprocessing   
   a) Replace the invalid values with NumPy “nan” so I can use imputer on such values.
   b) Check for null values in the columns. If present, impute the null values using the KNN imputer.
3) Clustering - KMeans algorithm is used to create clusters in the preprocessed data. The optimum number of clusters is selected by plotting the elbow plot, and for the dynamic selection of the number of clusters, I am using the "KneeLocator" function. The idea behind clustering is to implement different algorithms
   To train data in different clusters. The Kmeans model is trained over preprocessed data and the model is saved for further use in prediction.
4) Model Selection - After clusters are created, I found the best model for each cluster. I am using two algorithms, "SVM" and "XGBoost". For each cluster, both the algorithms are passed with the best parameters derived from GridSearch.I calculated the AUC scores for both models and select the model with the best score. Similarly, the model is selected for each cluster. All the models for every cluster are saved for use in prediction. 
 
## Prediction Data Description
 
The client will send the data in multiple sets of files in batches at a given location. Data will contain predictors in 30 columns.
Apart from prediction files, I also require a "schema" file from the client which contains all the relevant information about the training files such as:
Name of the files, Length of Date value in FileName, Length of Time value in FileName, Number of Columns, Name of the Columns and their datatype.
 Data Validation  
In this step, I performed different sets of validation on the training files.  
1) Name Validation- I validated the name of the files based on the given Name in the schema file. I have created a regex pattern as per the name given in the schema file, to use for validation. After validating the pattern in the name, I checked for the length of date in the file name as well as the length of time in the file name. If all the values are as per requirement, I moved such files to "Good_Data_Folder" else moved such files to "Bad_Data_Folder". 
2) Number of Columns - I validated the number of columns present in the files, if it doesn't match with the value given in the schema file then the file is moved to "Bad_Data_Folder". 
3) Name of Columns - The name of the columns is validated and should be the same as given in the schema file. If not, then the file is moved to "Bad_Data_Folder". 
4) Datatype of columns - The datatype of columns is given in the schema file. This is validated when I inserted the files into Database. If the data type is wrong then the file is moved to "Bad_Data_Folder". 
5) Null values in columns - If any of the columns in a file has all the values as NULL or missing, I discarded such file and moved it to "Bad_Data_Folder".  
## Data Insertion in Database 
1) Database Creation and connection - Create the database with the given name passed. If the database is already created, open the connection to the database. 
2) Table creation in the database - A table with the name - "Good_Data", is created in the database for inserting the files in the "Good_Data_Folder" based on given column names and data type in the schema file. If the table is already present then the new table is not created, and new files are inserted into the already present table as I wanted training to be done on new as well as old training files.     
3) Insertion of files in the table - All the files in the "Good_Data_Folder" are inserted in the above-created table. If any file has an invalid data type in any of the columns, the file is not loaded in the table and is moved to "Bad_Data_Folder".
## Prediction 
 
1) Data Export from Db - The data in the stored database is exported as a CSV file to be used for prediction.
2) Data Preprocessing   
   a) Replace the invalid values with NumPy “nan” so I can use imputer on such values.
   b) Check for null values in the columns. If present, impute the null values using the Single imputer.
3) Clustering - KMeans model created during training is loaded, and clusters for the preprocessed prediction data are predicted.
4) Prediction - Based on the cluster number, the respective model is loaded and is used to predict the data for that cluster.
5) Once the prediction is made for all the clusters, the predictions along with the original names before the label encoder are saved in a CSV file at a given location and the location is returned to the client.
 
## Implementation and Testing 

![image](https://user-images.githubusercontent.com/69865169/177703995-c5e88546-e544-4bba-9b55-53dc8c2e3ce3.png)
## Output (Screenshots)

![image](https://user-images.githubusercontent.com/69865169/177704058-e95bc560-a31e-4f76-a7f3-a5e27ae81432.png)

## Snapshot of My Interactive Website:

![image](https://user-images.githubusercontent.com/69865169/177704157-99320b56-0762-4a5e-9c96-563abb32852f.png)

## CONCLUSION
As we say “What we learn with pleasure, we never forget”, and the joy we had doing this project was boundless.
My objective to take on this project was to fabricate a working phishing website detection model. I used various models to make predictions on a given file. Further with the help of GUI, created an interactive page where the user can give a default file path or custom file path and evaluate if the website is phishing or a legitimate one. Accuracy for the model came out to be 98.5%  using the XGBoost model. Other models were also used like SVM for training purposes but accuracy did not come out well. 
By the end of this project, I was able to have a good understanding of how to implement the lifecycle of a machine learning project. With the means of this project, I also learned how to make a GUI/website using HTML, and CSS and further connect it with our python model. With all the knowledge we gained from this project, I can now easily make many more similar projects on any data. 

## FUTURE SCOPE

As it is said “Knowledge is power - never stop learning”, I aim to do the same. With the means of this project, I gained plenty of knowledge, but there is yet a lot to learn. In the future, I would like to use different models like KNN, logistic regression, naïve Bayes, etc. By using different models my main aim would be to increase the accuracy and a better front end of my website.
Another major step, I wish to add to my project is to deploy our model on Heroku Cloud Services, right now it is deployed on GitHub using GitHub pages.

