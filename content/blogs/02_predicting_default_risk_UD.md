---
title: "Predicting Default Risk"
metaAlignment: center
coverMeta: out
draft: false
date: 2020-04-11
auhtor: "Helena"
toc: true
type: "blogs"    
categories:
- udacity
tags:
- alteryx
- classification models
# thumbnailImagePosition: right
# thumbnailImage: images/catalogs.jpg
---
In this project, we need to decide if a new bank customer is approved for loan or not using a predictive model.

<!--more-->

##### Project overview 
We have the following information to work with:
1.	Data on all past applications for a loan
2.	The list of customers that need to be classified

Because we are classifying new customers to two groups: creditworthy or not - we need to use a binary model to make this decision. We will build and compare classification models such as Logistic Regression, Decision Tree, Random Forest, and Boosted Model models’ performance against each other. Then decide on the best model to classify new bank customers on whether they can be approved for a loan or not.


## Step 1: Data clean up and building the Training Set
I used Field Summary tool to explore given dataset in Alteryx. Based on report below following decisions were made: 
Duration in Current Address has 69% missing data and was removed, while Age Years has missing data as well, but only 2.4% which I decided to impute the missing values with median age because median is not affected by outliers. 
There were two fields with just one value for the whole field: Occupation and Concurrent Credits. Other fields with low-variability where more than 80% of the data was skewed to one side: Guarantors, Number of Dependants and Foreign Worker. All these fields were removed. 
And lastly Telephone field was removed because it is irrelevant to customer’s credit worth.
{{< image classes="fancybox nocaption fig-50" src="/images/Predictors removed.JPG" thumbnail="/images/Predictors removed.JPG" >}}
{{< image classes="fancybox nocaption fig-50" src="/images/Occupation uniform.JPG" thumbnail="/images/Occupation uniform.JPG" >}}  &nbsp;



## Step 2: Training the Classification Models
For creating estimation and validation samples 70% went for estimation(training sample) and 30% of the entire dataset was saved for validation. 
For each model I have highlighted their significant predictor variables with their p-values.  &nbsp;


1.	Logistic Regression model 
Logistic regression model with Stepwise kept these predictor variables: 
{{< image classes="fancybox nocaption fig-33 right" src="/images/LR model predictor p values.JPG" thumbnail="/images/LR model predictor p values.JPG" >}} 
- Account balance
- Payment status
- Purpose
- Credit amount
- Length of current employment
- Instalment percent &nbsp;  
&nbsp;  

2.	Decision Tree model
Decision Tree model found these predictor variables significant:  

{{< image classes="fancybox nocaption fig-33 right" src="/images/DT model predictors VI.JPG" thumbnail="/images/DT model predictors VI.JPG" >}}  
- Account Balance
- Value Savings Stocks
- Duration of Credit in Months &nbsp;  
&nbsp;  
&nbsp;  

3.	Forest Model
The Forest model found these predictor variables most significant:  
{{< image classes="fancybox nocaption fig-25 right" src="/images/F model predictors VI.JPG" thumbnail="/images/F model predictors VI.JPG" >}}  
- Credit Amount
- Age in years
- Duration of credit in months
- Account Balance &nbsp;  
&nbsp;  
&nbsp;  

4.	Boosted Model
Our Boosted Model works with these predictor variables:  
{{< image classes="fancybox nocaption fig-25 right" src="/images/B model predictors VI.JPG" thumbnail="/images/B model predictors VI.JPG" >}}
- Credit amount
- Account balance
- Duration of credit in months
- Payment status of previous credit &nbsp;  
&nbsp;  



## Step 3: Model accuracy comparison against validation set  

{{< image classes="clear center fig-75" src="/images/Comparison report.JPG" thumbnail="/images/Comparison report.JPG" >}}
Model accuracy comparison report tells us the overall accuracy for Logistic Regression model was 75%, Decision Tree model 75%, Forest model 80% and Boosted model 78%. 
&nbsp;  
&nbsp;  
{{< image classes="nocaption fig-75 center clear" src="/images/Comparison report confusion matrix.JPG" thumbnail="/images/Comparison report confusion matrix.JPG" >}}

The Regression and the Decision Tree models predicted a lot of creditworthy applicants as non-creditworthy. Those two models are predicting the creditworthy applicants with more accuracy compared to the non-creditworthy. We can see that the Positive Predictive value *(PPV, also called Precision)* is higher than the Negative Predictive Value *(NPV)* which suggests that the models are biased towards creditworthy applicants which is understandable since our data set is biased - having a lot more creditworthy than non-creditworthy applicants.

Decision tree and regression model both: PPV 79%, NPV 60%.  
Boosted model: PPV 77%, NPV 80%.  
Forest model: PPV 80%, NPV 83%.  

The PPV and NPV are almost equal in Boosted and Forest models - suggesting lack of bias.

## Step 4: Choosing the best model
I chose to use our Forest model to predict loan worthy customers taking into account its highest overall accuracy compared to other models and its high F1 score, which looks for the bias in prediciting "Creditworthy" or "Non-Creditworthy" and calculates a single score: the higher the score, the better. Our Forest model’s overall accuracy is 80%, F1 87%.  

PPV = True Positives / ( True Positives + False Positives ) = 101/ (101 + 26) = 0.8  
NPV = True Negatives / ( True Negatives + False Negatives) = 19 / ( 19 + 4) = 0.83  
ACC = (TP + TN) / (P + N) = (101 + 19) / (127 + 23) = 0.8  
F1 = 2TP / (2TP + FP + FN) = 2x101/ (2x101 + 26 + 4) = 0.87


{{< image classes="nocaption fig-33 right" src="/images/Comparison report ROC.JPG" thumbnail="/images/Comparison report ROC.JPG" >}}
In the ROC curve we see our Forest model with orange line being the highest in graph of all models – meaning that we are getting a higher rate of true positive rates vs. false positives rates. We are looking for a high rate of true positive vs. true negative rates because we do not want to extend loans to people who are not creditworthy. Classifiers that get curves closer to the top-left corner indicate a better performance.  

Our Forest model’s AUC is 0.73 it means there is 73% chance that model will be able to distinguish between positive class and negative class.

### The result  
Out of 500 new applications there are 411 customers who are creditworthy meaning they qualify for getting a loan as per our prediction model.  
{{< image classes="nocaption clear" src="/images/Scoring creditworthy customers.JPG" thumbnail="/images/Scoring creditworthy customers.JPG" >}}  

&nbsp;  
&nbsp;  
##### Materials used:  
*“Understanding AUC - ROC Curve”*  
https://towardsdatascience.com/understanding-auc-roc-curve-68b2303cc9c5  

*“Simple guide to confusion matrix terminology”*  
https://www.dataschool.io/simple-guide-to-confusion-matrix-terminology/  

*“Confusion matrix online calculator”*  
http://onlineconfusionmatrix.com/  
  
&nbsp;  
Alteryx workflows:   
{{< image classes=" fig-75 center clear" src="/images/Comparison workflow.JPG" thumbnail="/images/Comparison workflow.JPG" title="Model comparison workflow">}}
{{< image classes=" fig-75 center clear" src="/images/Scoring forest model.JPG" thumbnail="/images/Scoring forest model.JPG" title="Scoring data with Forest model  ">}}







