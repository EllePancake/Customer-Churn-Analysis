# Customer-Churn-Analysis

> - [Video Presentation](https://youtu.be/JIqCr8k8z-8)
> - [Summary Documentation](https://github.com/EllePancake/Customer-Churn-Analysis/blob/main/Customer%20Churn%20Summary%20Document.pdf) 
> - [Tableau Dashboard](https://public.tableau.com/views/Capstone2-BankCustomerChurnDashboard/Dashboard1?:language=en-US&publish=yes&:display_count=n&:origin=viz_share_link)

# Introduction:

## The story Behind The Data
A bank is concerned that more and more customers are leaving its credit card services. They would really appreciate if someone could analyze it for them, in order to understand the main reasons for leaving the services, and to come up with recommendations for how the bank can mitigate that. Eventually, the bank would like to proactively implement these recommendations in order to keep their customers happy.

**A full ERD can be found [here](https://dbdiagram.io/d/638cdd8abae3ed7c45449eed)**

## Data Description
In this task, few datasets are provided:

1. **`BankChurners.csv`**   - this file contains basic information about each client (10 columns). The columns are:
    - `CLIENTNUM` - Client number. Unique identifier for the customer holding the account;
    - `Attrition Flag` - Internal event (customer activity) variable - if the client had churned (attrited) or not (existing).
    - `Dependent Count` - Demographic variable - Number of dependents
    - `Card_Category` - Product Variable - Type of Card (Blue, Silver, Gold, Platinum)
    - `Months_on_book` - Period of relationship with bank
    - `Months_Inactive_12_mon` - No. of months inactive in the last 12 months
    - `Contacts_Count_12_mon` - No. of Contacts in the last 12 months
    - `Credit_Limit` - Credit Limit on the Credit Card
    - `Avg_Open_To_Buy` - Open to Buy Credit Line (Average of last 12 months)
    - `Avg_Utilization_Ratio` - Average Card Utilization Ratio
2. **`basic_client_info.csv`** - this file contains some basic client info per each client (6 columns) -
    - `CLIENTNUM` - Client number. Unique identifier for the customer holding the account
    - `Customer Age` - Demographic variable - Customer's Age in Years
    - `Gender` - Demographic variable - M=Male, F=Female
    - `Education_Level` - Demographic variable - Educational Qualification of the account holder (example: high school, college graduate, etc.`
    - `Marital_Status` - Demographic variable - Married, Single, Divorced, Unknown
    - `Income_Category` - Demographic variable - Annual Income Category of the account holder (< $40K, $40K - 60K, $60K - $80K, $80K-$120K, > $120K, Unknown)
3. **`enriched_churn_data.csv`** - this file contains some enriched data about each client (7 columns) -
    - `CLIENTNUM` - Client number. Unique identifier for the customer holding the account
    - `Total_Relationship_Count` - Total no. of products held by the customer
    - `Total_Revolving_Bal` - Total Revolving Balance on the Credit Card
    - `Total_Amt_Chng_Q4_Q1` - Change in Transaction Amount (Q4 over Q1)
    - `Total_Trans_Amt` - Total Transaction Amount (Last 12 months)
    - `Total_Trans_Ct` - Total Transaction Count (Last 12 months)
    - `Total_Ct_Chng_Q4_Q1` - Change in Transaction Count (Q4 over Q1)

## Process

> See SQL questions & answers [here](https://github.com/EllePancake/Customer-Churn-Analysis/blob/main/Customer%20Churn%20Analysis%20-%20SQL%20Questions.ipynb)
> See Jupyter Notebook with initial analysis & exploration [here](https://github.com/EllePancake/Customer-Churn-Analysis/blob/main/Customer%20Churn%20-%20Initial%20Analysis.ipynb)
> See Jupyter Notebook with additional analysis [here]()

Python libraries used: NumPy, Pandas, Matplotlib, Seaborn, scipy, sklearn, 

### Initial Analysis

I set out to investigate each of the three datasets, of 10k rows each, before combining them horizontally using the Unique ID column. Next, I cleaned the dataframe and looked into outliers. Then I dove into exploratory data analysis, utilizing univariate, bivariate, and multivariate graphs to further understand the data and the relationships between various points. Lastly, I zoomed in by asking specific questions and using the exploration to find answers. 

### Additional Analysis

For the group portion my partner, [Kubra Sirin](https://www.linkedin.com/in/sirinmeydanli/), and I decided to use statistical analysis and predictive modeling to compare attrited customers to existing customers and define the most important attributes. I used weight of evidence encoding to test categorical columns for how likely each variable leads to churn and we created a list of variables we wanted to test. I ran our initial dataframe through a variety of classifiers to determine which one offered the highest AUC, which led me to choose the Gradient Boosting Classifier and run grid search to find the best parameters. Later, we used this model to test various combinations of data points and graphs to reaffirm our findings. 

## Questions & Findings

### Initial Analysis Questions

1. What do the bank's churned customers look like? 
> Okay! Just over half of churned customers are women, most have 2-3 dependents, and nearly half fall within the 40-49 age group. The vast majority are married or single, with 40% making less than 40k and most with a graduate degree or below.

![image](https://user-images.githubusercontent.com/107210379/218325647-1ba5db46-6a77-48a4-a0a6-f7bf2d47d2c8.png)

2. What does a churned customer's relationship with the bank look like?
> The vast majority of churned customers had been with the bank for 3 years and had 2-4 contacts with the bank. The vast majority also had blue cards, despite there being a more balanced distribution in credit limits and even a spike in higher credit limits (the same also holds true for average open to buy). This leads me to question if these customers would have had a better relationship with the bank (and thus less likely to leave) if they had been given better cards. I also notice that the vast majority of churned customers have 0 average utilization ratio.

![image](https://user-images.githubusercontent.com/107210379/218325632-d84f879a-361b-4354-85c3-6cba0b069612.png)

3. Are there differences in credit limits, average open to buy, and average utilization ratio between existing and attrited customers?
> Looks like the average utilization ratio is a huge indicator of potential churn which makes their willingness to buy very important.

![image](https://user-images.githubusercontent.com/107210379/218325604-8ff464f2-ee6c-4195-a2b9-2a7e22c55903.png)

4. Are there other factors that affect willingness to buy?
> Blue card customers are much less willing to buy! The more relationship counts a customer has the less likely they are to be open to buy, while the opposite peaks true for attrited customers. Customers with two relationships total tend to have the highest willingness to buy. Why is this? For existing customers, contact counts don't have much effect on them, however, contact counts largely affect attrited customers.

![image](https://user-images.githubusercontent.com/107210379/218325587-7e533616-295f-4085-be8e-30a64bb5188b.png)

### Additional Analysis

> For this part of the project I had the opportunity to work with the talented [Kubra Sirin](https://www.linkedin.com/in/sirinmeydanli/). Our goal was to zero in on what factors are most likely to cause customer churn and understand how important different points of the data truly are. We also aimed to create customer profiles for the most loyal customer and the churned customers. 

The most impactful insight we found was that **transaction counts and transaction amounts alone were able to predict customer churn with 92% accuracy.** Therefore, all actions should focus on increasing these two factors.

## Recommendations

- Create a reward system to keep loyal customers happy, encourage spending, & increase card utilization
- Address distribution of card types. 93% of customers have blue cards despite a great variety in income and willingness to buy
