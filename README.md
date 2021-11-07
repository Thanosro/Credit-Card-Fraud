# Fraud Detection in Credit Card Transactions
## Introduction
The goal of this project is to detect fraudulent transactions in a real world dataset containing credit card transactions. The dataset contains information about 97,000 transactions, such as transaction date, card number, merchant state, zip and ID etc and a label for each transaction denoting if it is fraudulent or not. We want to design a model in a supervised way to detect/predict fraud on new incoming transactions. 
## Approach
### Data Preprocessing
Initially, we removed any transcations that were marked otherwise (such as card activation, refund etc.), since our focus is on credit card purhcases. Next, after inspecting the dataset, we noticed that more than 8,000 values were missing in the merchant zipcode, merchant number and merchant state. Thus we filled in those values using data imputation techniques i.e., we filled missing merch zipcodes by state and vice versa and merchant number by the merchant description field among other methods. 
### Feature Engineering 
Our next step was to create additional features from the existing data fields. We created new features based on our understanding of the data and different modeling techniques, as well as our domain knowledge of the subject area. Fraud is correlated to the amount, periodicity and frequency of the transactions. Therefore, we built a number of candidate variables based on the data fields in the original dataset as they applied to the concepts of the monetary amount of the transactionsl, the frequency in which transactions were conducted, the number of days since the last time a transaction was seen, and the speed in which transactions were seen over a certain period of time in relation to what was considered normal. Additionally, we created variables based on Benford’s law of large numbers and the fraud rate per day.
### Feature Selection
Our goal in the feature selection step is to choose a subset of the features to use as inputs in our models. We want to choose the 30 most statistically important features to perform an accurate prediction. First, we used the filter method i.e, we measured the correlation of each feature with the fraud distribution, using the Kolmogorov–Smirnov (KS) distance and the FDR @ 3% and we kept 1/3 of the features. Second we used the wrapper method which includes using simple statistical models to evaluate the performance of each feature based on a performance metric. For feature selection with the wrapper method we performed recursive feature elimination using a logistic regression model with L2 regularization and the FDR @ 3% metric and kept 30 features.
## Results
For fraud detection we used 4 models: Logistic regression, Boodted Trees, Neural Networks and Random Forests. We trained the models using different paraemeters and configurations each time. The FDR @ 3% for training, test and validation (out of time (OOT)) sets for the best parameter are presented below:
| Model | Training | Test | OOT |
|---|---|---|---|
| Logistic regression | 0.7146 | 0.7058 | 0.448 |
| Boodted Tree | 0.9343 | 0.8736 | 0.543 |
| Neural Network | 0.6854 | 0.6831 | 0.5911 |
| Random Forests | 0.8635 | 0.8333 | 0.5815 |
## Final Model Analysis
