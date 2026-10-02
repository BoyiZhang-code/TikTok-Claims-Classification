## 01. Business Context
TikTok users have the ability to submit reports that identify videos and comments that contain user claims. These reports identify content that needs to be reviewed by moderators. The process generates a large number of user reports that are challenging to consider in a timely manner. 

TikTok is working on the development of a predictive model that can determine whether a video contains a claim or offers an opinion. With a successful prediction model, TikTok can reduce the backlog of user reports and prioritize them more efficiently.

## 02. Business Problem
There are too many user-reported videos for human moderators to review efficiently. The platform needs an automated ML classifier to identify videos with claims, so these high-risk videos can be prioritized for human review. Opinion videos will be deprioritized, to save moderation resources.

## 03. Analytical Problem
After finishing EDA, hypothesis testing and the baseline logistic regression model, the analytics team will build the final machine learning model for the claims classification dataset. The team will lead these final-stage tasks, including feature engineering, model development, and model evaluation.

## 04. Project Objective
According to the results of previous baseline logistic regression model, Willow Jaffey (**fictional character**), a data science lead, suggests that the random forest is more efficient model for classification task than Naive Bayes model and logistic regression model. Therefore, the project objectives are:

- Conduct EDA

- Perform feature engineering

- Build random forest model and XGBoost model

- Evaluate both models

- select champion model to predict test data

- Analyse feature importance

## 05. Key Questions
For this task, there are some questions need to be considered by data professionals:

1. Which modeling solution did the team choose for the final claims model?

2. What criteria did the data team use to determine their modeling approach?

3. What are the ethical implications of the final claims model?

4. What are the consequences if our claims model makes errors?

5. Do the benefits of such a model outweigh the potential problems?

6. After evaluating the model, should TikTok proceed with using this model?

7. Consider the impact that "banned authors" have on claims or opinions. To what extent do those authors make claims or opinions? 

## 06. Success Criteria
The project will be considered successful if it:

- establishes a clear and reproducible analytical workflow;

- identifies meaningful patterns in the TikTok data;

- develops classification models that outperform a baseline;

- evaluates model performance using multiple appropriate metrics;

- identifies important predictive features;

- translates analytical findings into relevant business implications.