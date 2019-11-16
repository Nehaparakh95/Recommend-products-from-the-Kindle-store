ABSTRACT
This report focuses on the recommendation of products based on the Kindle store. The system predicts the ratings of the Kindle products based on the model trained on the Kindle store dataset. This model is trained using Alternate Least Squares Matrix Factorization (ALS) which is based on the Spark Framework. The model is trained on 70% of the dataset and tested on 30 % on the dataset. Evaluation parameter used is Root Mean Square Error (RMSE).
PREPROCESSING
The training and testing of the model are done using the following steps:
Dataset- The training dataset was a csv file of approximately 456MB and consisted of 9 columns:
•	reviewerID - ID of the reviewer
•	asin - ID of the product
•	reviewerName - name of the reviewer
•	helpful - helpfulness rating of the review, e.g. 2/3
•	overall - rating of the product
•	summary - summary of the review
•	unixReviewTime - time of the review (unix time)
•	reviewTime - time of the review (raw)
The combination of reviewerID-asin formed the composite key or the unique value for the identification of an entry in the dataset. The dataset was read using the pandas dataframe and followed by reading it as a spark data frame.
 Preprocessing-
•	Dropping of columns: The columns review, unixReviewTime , reviewTime, helpful, summary and  reviewerName  were dropped as they were not used for collaborative filtering.
•	Mean Centering: The overall/rating column was mean centered which helps in reducing multicollinearity of the column. The mean centering is done by averaging the ratings given by a user whose unique value is determined by the reviewerID.
The final data frame which is used for training is as shown below:
 
ALGORITHM
Library: PySpark library for Python is used to implement the MapReduce approach and use the ALS regression algorithm.
Recommendation Algorithm: The collaborative filtering approach is implemented using the Alternate Least Squares (ALS) algorithm and set the following parameters as:
•	Rank: or the number of latent factors is assumed as 150
•	maxIter: is the maximum number of iterations to run is assumed as 100.
•	itemCol: asin or itemID is used as the item column
•	userCol: reviewerID is used as the user column
•	regParam: Set as 0.01 for ALS
•	coldStartStrategy: is dropped
•	nonnegative: is marked as True
Training: The model is trained on 687833 rows with the following columns:
•	asin- Amazon Standard Identification Number which is the productID
•	reviewerID: the ID of the customer
•	Mean centered rating
•	Evaluation Parameter: Regressor Evaluator for the ALS regression model is used with the metric as RMSE. The RMSE mean squares the difference between the actual and the predicted ratings as follows:
 
Testing: A test data of 291098 rows is tested and the mean centering of the predictions is inversed. The regression evaluation is done using RMSE. The final dataframe consists of the reviewerID, asin and the predicted rating for the combination of reviewerID-asin. The final predictions are mapped to the reviewerID-asin composite key as a {key,overall} pair. They are finally stored into .csv file.
 

The final RMSE after uploading on Kaggle was 0.47062
References:
•	Recommendation System In Python using ALS Algorithm and Apache Spark-https://medium.com/@patelneha1495/recommendation-system-in-python-using-als-algorithm-and-apache-spark-27aca08eaab3 [Reference Section: Creating ALS model and fitting data]


