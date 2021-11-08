# Fraud Detection in Credit Card Transactions
## Introduction
The goal of this project is to detect fraudulent transactions in a real world dataset containing credit card transactions. The dataset contains information about 97,000 transactions, such as transaction date, card number, merchant state, zip and ID etc and a label for each transaction denoting if it is fraudulent or not. We want to design a model in a supervised way to detect/predict fraud on new incoming transactions. Our metric of success was the Fraud Detection Rate at 3% (FDR @ 3%) which is the precentage of frauds detected in the top 3% records ranked as frauds by our models.
## Approach
### Data Preprocessing
Initially, we removed any transcations that were marked otherwise (such as card activation, refund etc.), since our focus is on credit card purhcases. Next, after inspecting the dataset, we noticed that more than 8,000 values were missing in the merchant zipcode, merchant number and merchant state. Thus we filled in those values using data imputation techniques i.e., we filled missing merch zipcodes by state and vice versa and merchant number by the merchant description field among other methods. 
### Feature Engineering 
Our next step was to create additional features from the existing data fields. We created new features based on our understanding of the data and different modeling techniques, as well as our domain knowledge of the subject area. Fraud is correlated to the amount, periodicity and frequency of the transactions. Therefore, we built a number of candidate variables based on the data fields in the original dataset as they applied to the concepts of the monetary amount of the transactionsl, the frequency in which transactions were conducted, the number of days since the last time a transaction was seen, and the speed in which transactions were seen over a certain period of time in relation to what was considered normal. Additionally, we created variables based on Benford’s law of large numbers and the fraud rate per day.
### Feature Selection
Our goal in the feature selection step is to choose a subset of the features to use as inputs in our models. We want to choose the 30 most statistically important features to perform an accurate prediction. First, we used the filter method i.e, we measured the correlation of each feature with the fraud distribution, using the Kolmogorov–Smirnov (KS) distance and the FDR @ 3% and we kept 1/3 of the features. Second we used the wrapper method which includes using simple statistical models to evaluate the performance of each feature based on a performance metric. For feature selection with the wrapper method we performed recursive feature elimination using a logistic regression model with L2 regularization and the FDR @ 3% metric and kept 30 features.
## Results
For fraud detection we used 4 models: Logistic regression, Boodted Trees, Neural Networks and Random Forests. We trained the models using different paraemeters and configurations each time and our evaluation metric was the FDR @ 3$. The FDR results for training, test and validation (out of time (OOT)) sets for the best parameters chosen are presented below:
| Model | Training | Test | OOT |
|---|---|---|---|
| Logistic regression | 0.7146 | 0.7058 | 0.448 |
| Boodted Tree | 0.9343 | 0.8736 | 0.543 |
| Neural Network | 0.6854 | 0.6831 | 0.5911 |
| Random Forests | 0.8635 | 0.8333 | 0.5815 |  

Seemingly the above models are overfit since the training and test set results are significantly higher than the validation ones. Therefore, we tried altering the model paramters to generalize the models and reduce the overfit, but this led to significantly lower (more than 5%) validation set FDR. Finally, we chose as our final model the random forest, since it produced on average better test set results, with lower variance. The averrage 10-fold cross validation results for our final model after hyperparameter tuning were the following:  

| Model | Training | Test | OOT |
|---|---|---|---|
| Random Forests | 0.8635 | 0.8333 | 0.5815 |  

## Final Model Analysis
Based on the results of our model, we wanted to find an optimal cutoff point for the FDR, i.e. any transaction with a score above the threshold at that cutoff point would be classified as fraudulent. A low cutoff point would mean that we would detect a lot of fraudulent transactions, but also misclassify non-fraudulent ones as frauds (false-positive, FP). Similarly, a high cutoff point would not detect many frauds, but would also misclassify a small number of transactions. Assuming $2,000 gain for every fraud that is detected (TP) and $50 loss for every non-fraud that is flagged as a fraud (FP), we plotted the Fraud Savings (blue), the Lost Sales (orange) and the Fraud Savings (green) for the out of time dataset. According to our analysis, we recommend a cutoff point at 5% as this threshold maximizes the profits (P) calculated as follows: P = 2000*TP - 50*FP. 

![image](https://user-images.githubusercontent.com/39418469/140681513-c732c283-dafa-4e5e-99bf-d93c12cc7f31.png)

From our analysis and interpretation of our model's results, we found there was a correlation between transaction counts and fraud scores. When there is a burst of transaction activity, the fraud score rises rapidly. We illustrate this with the “Cardnum” value of “5142235211” in the following plot. There was a burst of activity on 11/25/2010 containing 18 transactions and an additional 15 transactions on 11/26/2010. From the graph above, we can see the fraud score rose steeply in accordance with the time period.

![image](https://user-images.githubusercontent.com/39418469/140683111-01734def-d893-4af7-9678-bc29346bea70.png)



