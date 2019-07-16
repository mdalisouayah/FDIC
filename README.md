## <center> Report - FDIC Data</center>


### 1. Introduction of the real-world problem:

FDIC-insured Financial institutions report information about the status of their Loan Portfolios, on a quarterly basis. The task of this exercise is to build a model that predicts the value of future past-due loans based on the current value of the loan portfolio. The solution to this exercise would provide banks with tools, based on which actionable recommendations can be made, including:

* **Risk management** for capital adequacy monitoring, capital allocation, budgeting, etc., and 
* **Performance benchmarking** against peers and/or the industry as a whole 


### 2. Data Science problem: 

Given that we are predicting a continuous numerical variable (past due loans), the problem is identified as a regression problem. As such, We will test several regression models and compare their performance. 

### 3. Data Collection: 
The [FDIC data](./loan-performance_aggregate.xls) provides:
* **the aggregate quarterly amounts of the overall loan portfolio of all Banks**, which is our `predictor/independent variable`
* **the aggregate amount of the past due loans**, which is our `target/dependent variable`

There are 141 observations, which correspond to the number of all quarterly reports made available by the FDIC, since 1984.


### 4. Data Cleaning: 

During the data cleaning process, the **`Total Loans & Leases`** section was located. It contains the  information we need, which is the **`Total outstanding`** and the **`90 days or more past due`** rows. Given the forward looking nature of the problem, all past due loan amounts have been shifted two quarters up, so that each loan portfolio amount corresponds to its respective (in-6-months) 90 days+ past due loan. 

### 5. Data Exploration and Analysis

The correlation (r) between the two variables is 0.67, suggesting a strong linear relationship. However, the resulting coefficient of determination (r-squared), of 0.45, suggests that the change in total loan portfolio only explains 45% of the change in past dues. This might be a hint that the linear regression model might not be the strongest predictive model. 

The chart below depicts the evolution of the overall loan amounts and the six-month past due amounts. It also suggests a strong linear relationship, albeit inverted since 2008, marking the start of the financial crisis. Indeed, the chart, as expected, shows a spike in the amount of past dues during that period. The chart also shows, for the same period, a shrinkage in the amount of loans, reflecting the credit crunch (and also loan write-offs).

![Variables Trend](./images/VariablesTrend.png)

### 6. Model Selection 

Several regression algorithms were tried, including `linear regression`, `decision tree`, and other `ensemble models`. The models were evaluated based on two metrics: `the coefficient of determination (r2)` and `the Root Mean Square Error (RMSE)`, which measures the error between the actual and predicted past dues. The best model would be the one that results in the highest r2 and the lowest RMSE. A special attention was paid to overfitting, which is a common issue with decision tree and ensemble models. 

### 7. Model Evaluation

The evaluation of the models shows that the Decision tree and Adaboost are the best models on both metrics (r2 and RMSE), with a slight edge for Adaboost in terms of (less) relative overfitting, as inferred by the difference between the train and the test scores.

**<center>r2 scores</center>**
![r2 scores](./images/r2_score.png)


**<center>RMSE scores</center>**
![RMSE scores](./images/RMSE_score.png)




### 8. Solution to the real-world problem

Below is the outcome of the decision tree model when applied to unseen Portfolio loan amounts (the validation set). The chart provides a comparison between predicted past dues and actual past due numbers. 

![predictions vs. actuals](./images/validation_data.png)

The same model was also applied to the loan amounts of the last two quarters (2018Q4 and 2019Q1), which 6-month past due numbers will come out in 2019Q2 and 2019Q3, respectively.  
![model predictions](./images/test_data.png)
Note: the predicted amounts are the same due to (1) the step-wise approach of the decision tree model, and (2) the loan amounts being close.

### 9. Ideas for model improvement 

The following areas are highlighted for further consideration: 
* Expand the analysis to particular loan types and compare it to the analysis done on the overall loan portfolio. The question would be: is the credit analyst better off using a specific model by type of loan, or does this general model provide a stronger predictive tool?
* Expand the analysis to a particular bank and compare the resulting model performance with the general model (or an alternative model covering a group of peers). The question would be: Is the credit analyst better off using the bank’s specific model, or does the general model provide a stronger predictive tool?
* An expanded analysis would be particularly worthwhile if we are looking for an interpretable model. In the current exercise we resorted to complex (black box) models due to the relatively low performance of the linear regression model, which is a comparatively  highly interpretable model. If the expanded analysis results in a strong enough linear regression model, the credit analyst might elect to trade off stronger models for an interpretable one, if such a requirement exists. 
