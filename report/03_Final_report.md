# TikTok Claims Classification — Final Report

## 1. Executive Summary

This project develops a machine learning classification model to distinguish TikTok videos containing claims from those containing opinions. The analysis follows an end-to-end machine learning workflow, including exploratory data analysis, data cleaning, feature engineering, model development, hyperparameter tuning, model evaluation, and feature importance analysis.

Two tree-based classification models were developed and compared: Random Forest and XGBoost. A 60/20/20 train-validation-test split was used. Five-fold cross-validation with GridSearchCV was applied to tune both models, with recall selected as the refit criterion.

The Random Forest model was selected as the champion model based on its validation performance. It achieved approximately 100% accuracy, precision, recall, and F1-score on both the validation and held-out test sets. The XGBoost model produced similarly strong results but showed slightly lower validation accuracy.

Feature importance analysis indicated that `video_view_count`, `video_like_count`, `video_share_count`, and `video_comment_count` were among the most important predictors used by the Random Forest model.

Although the model achieved very high predictive performance, the results should be interpreted with caution. The dataset is a synthetic educational dataset rather than real-world platform data. Therefore, the observed performance may not generalize to a real content moderation environment, where data may contain greater noise, additional behavioural signals, changing user patterns, and more complex classification challenges.

---

## 2. Business Context

TikTok hosts a large volume of user-generated video content. From a content moderation perspective, distinguishing between factual claims and subjective opinions can help platform teams identify content that may require different levels of review.

A claim presents information that can potentially be verified as true or false, whereas an opinion expresses a personal view or judgement.

Automated classification can potentially support content moderation teams by identifying videos that are more likely to contain claims and prioritizing them for further review.

This project therefore investigates whether machine learning can be used to classify TikTok videos based on available video-level and engagement-related features.

---

## 3. Problem Definition

### 3.1 Business Problem

The business team needs an efficient method for identifying TikTok videos that contain claims rather than opinions.

Manual review of large volumes of content can require substantial human resources. A machine learning model could potentially support this process by automatically classifying videos and helping moderation teams prioritize content for further review.

### 3.2 Analytical Problem

The analytical task is formulated as a binary classification problem.

The target variable is:

- `claim` — the video contains a claim.
- `opinion` — the video contains an opinion.

The objective is to develop a classification model that can accurately distinguish between the two classes.

---

## 4. Data Understanding

The project uses a TikTok dataset provided for the Google Advanced Data Analytics course.

Initial data exploration was conducted to examine:

- Dataset dimensions
- Data types
- Missing values
- Duplicate records
- Descriptive statistics
- Class distribution
- Potential outliers

After removing rows containing missing values, the resulting dataset contained approximately balanced classes.

The class distribution was:

| Class | Proportion |
|---|---:|
| Claim | 50.35% |
| Opinion | 49.65% |

The relatively balanced target distribution reduces the concern that the model is simply exploiting a highly imbalanced target variable.

The average transcription length also differed between the two classes:

| Class | Average text length |
|---|---:|
| Claim | 95.38 characters |
| Opinion | 82.72 characters |

However, text length alone was not used as the target classification mechanism. It was retained as an engineered numerical feature alongside other available variables.

---

## 5. Data Preparation and Feature Engineering

### 5.1 Missing Values

Rows containing missing values were removed from the dataset.

After this step, no missing values remained in the modeling dataset.

### 5.2 Duplicate Records

Duplicate observations were checked after removing missing values. No duplicated records were identified.

### 5.3 Outliers

The dataset contained a number of extreme values in engagement-related variables.

Because the selected models were tree-based models, explicit outlier transformation was not applied. Random Forest and XGBoost are generally less sensitive to extreme numerical values than many distance-based or linear modeling approaches.

### 5.4 Feature Engineering

The `video_transcription_text` variable was originally stored as text. Rather than directly using the raw transcription, the project extracted its character length to create:

`text_length`

This provided a numerical representation of the transcription.

Categorical variables were transformed using one-hot encoding. The following variables were encoded:

- `verified_status`
- `author_ban_status`

Several identifier or non-predictive fields were removed:

- `#`
- `video_id`
- `video_transcription_text`

The target variable `claim_status` was encoded as:

- `claim` → 1
- `opinion` → 0

The feature engineering process was intentionally relatively simple because the dataset contains a limited number of available variables.

---

## 6. Model Development

### 6.1 Train-Validation-Test Split

The dataset was divided into three subsets:

- Training set: 60%
- Validation set: 20%
- Test set: 20%

The training set was further divided into a sub-training set and validation set.

The final test set was kept separate from model selection and was used only to evaluate the selected champion model.

This approach provides a held-out dataset for assessing final model performance after model selection.

---

## 7. Random Forest Model

A Random Forest classifier was developed as one of the primary models.

GridSearchCV with five-fold cross-validation was used to search across multiple hyperparameter combinations, including:

- `max_depth`
- `max_features`
- `max_samples`
- `min_samples_split`
- `min_samples_leaf`
- `n_estimators`

Multiple evaluation metrics were recorded:

- Accuracy
- Precision
- Recall
- F1-score

Recall was selected as the refit criterion during GridSearchCV.

The selected Random Forest configuration was:

| Parameter | Value |
|---|---:|
| `max_depth` | 5 |
| `max_features` | 0.6 |
| `max_samples` | 0.7 |
| `random_state` | 42 |

The best cross-validation recall score was approximately **0.9914**.

---

## 8. XGBoost Model

An XGBoost classifier was developed as a second tree-based model.

GridSearchCV with five-fold cross-validation was again used for hyperparameter tuning.

The parameters included:

- `max_depth`
- `n_estimators`
- `learning_rate`
- `min_child_weight`

The selected XGBoost configuration was:

| Parameter | Value |
|---|---:|
| `learning_rate` | 0.10 |
| `max_depth` | 4 |
| `min_child_weight` | 3 |
| `n_estimators` | 500 |

The best cross-validation recall score was approximately **0.9898**.

---

## 9. Model Evaluation

### 9.1 Validation Performance

The two tuned models were evaluated on the validation set.

#### Random Forest

| Metric | Opinion | Claim |
|---|---:|---:|
| Precision | 0.99 | 1.00 |
| Recall | 1.00 | 0.99 |
| F1-score | 1.00 | 0.99 |
| Overall Accuracy | **1.00** | |

The Random Forest model produced extremely strong validation performance, with only a small number of classification errors.

#### XGBoost

| Metric | Opinion | Claim |
|---|---:|---:|
| Precision | 0.99 | 1.00 |
| Recall | 1.00 | 0.99 |
| F1-score | 0.99 | 0.99 |
| Overall Accuracy | **0.99** | |

XGBoost also performed strongly, but its validation accuracy was slightly lower than that of the Random Forest model.

Based on the validation results, Random Forest was selected as the champion model.

---

## 10. Final Test Performance

The selected Random Forest model was subsequently evaluated on the held-out test set.

The test set contained 3,817 observations.

| Metric | Opinion | Claim |
|---|---:|---:|
| Precision | 0.99 | 1.00 |
| Recall | 1.00 | 0.99 |
| F1-score | 1.00 | 1.00 |
| Overall Accuracy | **1.00** | |

The model therefore maintained its strong performance on the held-out test set.

The test results were slightly stronger than the validation results. However, this should not be interpreted as evidence that the model will achieve similar performance on future real-world TikTok data, because the dataset is synthetic and educational in nature.

---

## 11. Feature Importance

Feature importance was examined using the Random Forest model's built-in feature importance measure.

The most important variables included:

1. `video_view_count`
2. `video_like_count`
3. `video_share_count`
4. `video_comment_count`

These engagement-related variables contributed strongly to the model's classification decisions.

From a business perspective, this suggests that engagement patterns may contain useful information for distinguishing between claims and opinions within this dataset.

However, feature importance should not be interpreted as a causal relationship. A high feature importance indicates that a variable was useful to the model, but it does not establish that changing the variable would directly change whether a video is classified as a claim or opinion.

---

## 12. Business Implications

The analysis demonstrates the potential of machine learning to support automated content classification.

If similar performance could be achieved on representative real-world data, such a model could potentially support moderation workflows by:

- Automatically classifying incoming content.
- Prioritizing videos for human review.
- Reducing the volume of content requiring manual screening.
- Supporting more consistent initial classification.

The model could therefore be considered as a decision-support tool rather than a replacement for human moderation.

In a real deployment environment, additional validation would be required before using model predictions to make operational decisions.

---

## 13. Ethical Considerations

Automated content classification can affect how user-generated content is reviewed and prioritized.

Several considerations should therefore be addressed before deployment:

### Transparency

Moderation teams should understand the purpose and limitations of the classification model.

### Human Oversight

Automated predictions should not necessarily be treated as final decisions. Human review may be required for uncertain or high-impact cases.

### Model Bias

The model may perform differently across different types of users, content, or language patterns. Performance should therefore be monitored across relevant subgroups before deployment.

### Data Quality

Changes in user behaviour, content styles, and platform trends may reduce model performance over time.

---

## 14. Limitations

Several limitations should be considered.

### 14.1 Synthetic Dataset

The dataset was created for educational purposes through the Google/Coursera course. It does not fully represent the complexity and noise of a production content moderation environment.

Therefore, the near-perfect model performance should not be interpreted as evidence that a production system would achieve the same results.

### 14.2 Limited Feature Set

The dataset contains a relatively limited number of variables compared with a real-world platform.

Additional information such as temporal behaviour, user history, linguistic characteristics, content metadata, and contextual signals could potentially provide additional predictive information.

### 14.3 Model Interpretability

Random Forest and XGBoost are tree-based ensemble models that are less directly interpretable than simple linear models.

Further interpretability techniques, such as SHAP or partial dependence analysis, could be used in future work to investigate individual predictions and feature effects.

### 14.4 Generalizability

The train, validation, and test sets are drawn from the same underlying dataset. Performance on this held-out test set therefore provides useful internal validation but does not establish performance under future distribution shifts or changing platform behaviour.

---

## 15. Recommendations

Based on the analysis, the following steps would be appropriate before considering real-world deployment:

1. Validate the model using representative production-like data.
2. Evaluate performance across different user and content groups.
3. Monitor model performance over time to detect changes in data distribution.
4. Introduce additional linguistic and behavioural features.
5. Apply model explainability techniques such as SHAP for more detailed interpretation.
6. Maintain human review for cases where model predictions may have significant consequences.

---

## 16. Conclusion

This project developed an end-to-end machine learning workflow for classifying TikTok videos as claims or opinions.

The analysis included data exploration, data cleaning, feature engineering, train-validation-test splitting, hyperparameter tuning, Random Forest and XGBoost modeling, model comparison, final testing, and feature importance analysis.

Random Forest was selected as the champion model based on its validation performance. It achieved approximately 100% accuracy, precision, recall, and F1-score on the held-out test set.

The project demonstrates how machine learning can be applied to a business classification problem and provides a foundation for further work in model development, interpretability, and deployment validation.

However, because the dataset is synthetic and designed for educational purposes, the results should be viewed as evidence of the modeling workflow rather than evidence of production-level performance.