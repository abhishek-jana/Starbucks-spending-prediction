# Starbucks-spending-prediction
Every data tells a story! As a part of Udacity’s Data Science nano-degree program, I was fortunate enough to have a look at Starbuck’s sales data. In this capstone project, I was free to analyze the data in my way. So, in this blog, I will try to explain what I did.

# Dataset overview

The data was created to get an overview of the following things:

- To observe the purchase decision of people based on different promotional offers.
- There are three types of offers: BOGO ( buy one get one ), discount, and informational. I wanted to see the influence of these offers on purchases.
- Finally, I wanted to see how the offers influence a particular group of people.

There are 3 files in the dataset:

### profile.json

Rewards program users (17000 users x 5 fields)

- **gender:** (categorical) M, F, O, or null
- **age:** (numeric) missing value encoded as 118
- **id:** (string/hash) id of each user.
- **became_member_on:** (date) format YYYYMMDD
- **income:** (numeric)

### portfolio.json

Offers sent during the 30-day test period (10 offers x 6 fields)

- **reward:** (numeric) money awarded for the amount spent
- **channels:** (list) web, email, mobile, social
- **difficulty:** (numeric) money required to be spent to receive a reward
- **duration:** (numeric) time for the offer to be open, in days
- **offer_type:** (string) BOGO, discount, informational
- **id:** (string/hash) id of the offers

### transcript.json

Event log (306648 events x 4 fields)

- **person:** (string/hash)
- **event:** (string) offer received, offer viewed, transaction, offer completed
- **value:** (dictionary) different values depending on event type
- **offer id:** (string/hash) not associated with any “transaction”
- **amount:** (numeric) money spent in “transaction”
- **reward:** (numeric) money gained from “offer completed”
- **time:** (numeric) hours after the start of the test


## Problem Statement
There are three main questions I attempted to answer.

**1. What is the spending pattern based on offer type and demographics?**

**2. How to recommend coupons/offers to current customers based on their spending pattern?**

**3. How to recommend coupons/offers to new customers?**

## Project Set Up and Installation

This project is done on anaconda platform using jupyter notebook jupyter notebook. The detailed instruction of how to install anaconda can be found [here](https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html).
To create a virtual environment see [here](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html).

Python Packages used for this project are:
```
Numpy
Pandas
Scikit-learn
XGBoost
Seaborn
pickle

```

## Description of the repository.

In the repository please look into the jupyter notebook **"Starbucks_Capstone_notebook.ipynb"** for the analysis.

There are three types of datasets used for this project. **"portfolio.json", "profile.json", and "transcript.json"** stored under **data** directory.

**finalmodel.bin** is the final trained model.

## Results

For Question 1. **What are the popular types of offers?**

Offer by Event 

![offer_by_event](https://github.com/abhishek-jana/Starbucks-spending-prediction/blob/main/images/offer_by_event.png)

Offer by Gender

![offer_by_gender](https://github.com/abhishek-jana/Starbucks-spending-prediction/blob/main/images/offer_by_gender.png)

Transaction data

![transaction](https://github.com/abhishek-jana/Starbucks-spending-prediction/blob/main/images/transaction.png)

From the plots above, the possible answer is,

- Although BOGO offers were viewed more, **Discount offers were more popular in terms of completion.**
- Given an offer, the chance of redeeming the offer is higher among **Females and Other** genders!
- **Women** tend to spend the most.
- Spending increases with age and income.

For Question 2. **How to recommend coupons/offers to current customers based on their spending pattern?**

- I used **rank-based recommendation system** to filter out top potential users who have a high chance of viewing and redeeming an existing or future offer.
- I created a **user-user based collaborative filtering** method to find out the top potential users specific to an offer.

For Question 3. **How to recommend coupons/offers to new customers?**

I used the Machine Learning approach to recommend offers to new users. For this, I set a rule for sending out coupons to new users.

- A person with no demographic data is expected to spend \$1.71, so we can send out an **Informational offer or No offer!**
- If a user is expected to spend < \$3 or > \$22, send out an **Informational offer or No offer!**
- If a user is expected to spend ≥ \$3 and < \$5, send out the **\$5 coupon offers.**
- If a user is expected to spend ≥ \$5 and < \$7, send out the **\$7 coupon offer.**
- If a user is expected to spend ≥ \$7 and < \$10, send out the **\$10 coupons.**
- Lastly, If a user is expected to spend ≥ \$10 and < \$20, send out the **\$20 coupon.**

For example,

```
person_demo = {
    'gender' : ['M','F','O'],
    'income' : [60000.0,72000.0,80000.0],
    'age' : [36,26,30]
}

predict_expense(person_demo, final_model)
>>> array([ 3.78, 10.66, 22.26], dtype=float32)
```
- Person 1: gender = Male, income = 60000.00, age = 36. Expected to spend \$3.78, so send out a **\$5 coupon.**
- Person 2: gender = Female, income = 72000.00, age = 26. Expected to spend \$10.66, so send out a **\$20 coupon.**
- Person 3: gender = Other, income = 80000.00, age = 30. Expected to spend \$22.26, so send out a **Informational coupon/no coupon at all!**

## Future Work

In the future,

- I am planning to deploy the model and build a data dashboard on the web.
- I will try to improve the model performance by performing more feature engineering.

## References:

- https://www.udacity.com/course/data-scientist-nanodegree--nd025
- https://scikit-learn.org/stable/
- https://xgboost.readthedocs.io/en/latest/
- https://harshit-tyagi.medium.com/end-to-end-machine-learning-project-tutorial-part-1-ea6de9710c0
- https://www.youtube.com/watch?v=vtm35gVP8JU

## Acknowledgements

I am thankful to Udacity Data Science Nanodegree program for motivating me in this project.

I am also grateful to Starbucks for making  the dataset publicly available.