---
title: "Predicting Catalog Demand"
date: 2020-04-04
categories:
- udacity
tags:
- alteryx
thumbnailImagePosition: right
thumbnailImage: images/catalogs.jpg
---
In this project, we will analyze a fictional business problem in the mail-order catalog business. We're tasked with predicting how much money your company can expect to earn from sending out a catalog to new customers. 
<!--more-->

This task will involve building a linear regression model with Alteryx and applying the results in order to provide a recommendation to management.


###### The Business Problem  
Last year the company sent out its first print catalog, and is preparing to send out this year's catalog in the coming months. The company has 250 new customers from their mailing list that they want to send the catalog to.
Determine how much profit the company can expect from sending a catalog to these customers. Management does not want to send the catalog out to these new customers unless the expected profit contribution exceeds $10,000.


## Step 1: Business and Data Understanding
1. We need to predict if this catalog campaign is a good idea. 
Should the company send out these catalogs to new customers? How much revenue can this company expect to make if they send their new catalog to these 250 customers? Is it profitable considering all the costs and possibility that customer might not make a purchase at all? 

2. 	What data is needed to inform those decisions?
- Possible revenue from each client.
- Cost of each catalog.
- Average past sales. 
- Average gross margin for each product.
- Average number of products purchased.
- Probability that customer will buy.
- Customer segment.


## Step 2: Analysis, Modeling, and Validation

Variables for each customer given in the data: Name, Customer Segment, Customer ID, Address, Average Sale Amount, Response to Last Catalog, Average Number of Products Purchased, Years as a Customer.

On my first model I chose to use these predictor variables: Average number of products purchased, Years as a Customer and Customer segment. 
For Numeric variables we can use scatterplots between an individual variable and the target variable to see if a variable might be a good candidate for a predictor variable. For categorical variables we need to run them through the model to see their if they are significant. We don’t want to include any variables that aren’t statistically significant. If the p-value is less than 0.05, we can be 95% confident that there exists a relationship between the predictor and target variable.

{{< image classes="fancybox nocaption fig-50" src="/images/avg number of products purchased vs avg sale amount.JPG" thumbnail="/images/avg number of products purchased vs avg sale amount.JPG" >}}  
Average number of products purchased as a continuous variable is easy to 
see the correlation between the predictor variable and target variable by visualizing it. I put Average number of products purchased and Average sale amount on a scatterplot to visualize this.

To use the categorical variable of Customer segment in our model we have to create dummy variables. Because we can’t check categorical variables’ relationship visually, we run it through the model and check their p-values. Looking for p-values to be smaller than 0.05 which makes them statistically significant. Reading the regression model report we see this is true for our Customer segments.
 
{{< image classes="fancybox nocaption " src="/images/2nd model coefficients.png" thumbnail="/images/2nd model coefficients.png" >}}

From Linear Regression Model Report we can also see the p-value of Average number of products purchased is 2.2e-16 *(this can be written as 2.2 * 10^-16 which can also be written as < .00000000000000022 This is very small!)*, which makes it statistically significant.

Because the trendline on scatterplot between Years as a Customer and Average Sale Amount came out flat *(for it being a discrete variable)*, I decided to run it through the linear regression model to see if it has any significance. The p-value of this turned out to be 0.056 and we can consider it to be of insignificant value to the model and remove it from the equation.


###### Why I think it is a good model
 
First, with all the p-values of predictors used in this model being less than 0.05 we consider all of them significant, which means the relationship is not likely to be occurring randomly. 
Second, we need to look at the adjusted R-squared value. *Adjusted* R-squared because it is a multiple linear regression. In our results it  is 0.84 and this is considered a very good result, generally higher is better.
Together the low p-values of predictor variables and R-squared value show us the model is highly predictive which suggests it to be a good model for the results we need and the available data we have.

Best linear regression equation based on the available data is the following:   
Y = 303.46 + 66.98 * Avg_Num_Products_Purchased - 149.36 * (If Type: Loyalty Club Only) + 281.84 * (If Type: Loyalty Club and Credit Card) - 245.42 * (If Type: Store Mailing List) + 0 * (If Type: Credit Card Only)


## Step 3: Presentation/Visualization

###### My recommendation
Based on my predictive model and calculations made considering costs, as the management wanted to send out the catalog to these 250 customers only if the expected profit exceeds $10,000 - I can recommend them to do so. 

My recommendation to send out the catalogs is based on the use of created predictive model:
Predicted_Revenue = 303.46 + 66.98 * Avg_Num_Products_Purchased - 149.36 * (If Type: Loyalty Club Only) + 281.84 * (If Type: Loyalty Club and Credit Card) - 245.42 * (If Type: Store Mailing List) + 0 * (If Type: Credit Card Only)

Then we multiply the predicted revenue with given customer’s probability of making the purchase:
Probable_Revenue = (Predicted_Revenue) * (Probability to buy)  
This is multiplied with average gross margin of all products, which is 50% and deduct the cost of each catalog which is $6.50:
	Probable_Revenue * 0.5 – 6.50

The Final Profit from the new catalog is expected to be $21,987.44 which would be the probable profit made from sending new catalogs out to given 250 customers.

