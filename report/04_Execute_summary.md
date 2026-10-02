## Executive Summary
- What did I do?  
    This project investigated whether machine learning could support the classification of TikTok videos as claims or opinions.  
- What did I find?  
    Two tree-based classification models, Random Forest and XGBoost, were developed and tuned using five-fold cross-validation.  
- What does it means?  
    The tuned Random Forest model achieved approximately 100% accuracy, precision, recall, and F1-score on the held-out test set.  
- What should happen next?  
    The results suggest that machine learning could potentially support automated content classification and help moderation teams prioritize videos for further review.
- What is the limitation of the project?  
    The dataset is synthetic and designed for educational purposes, so the observed performance should not be interpreted as evidence of production-level performance.  
## Key Findings
### Finding 1 — Model Performance
Random Forest was selected as the champion model based on validation performance. It achieved approximately 100% accuracy, precision, recall, and F1-score on the held-out test set. This indicates that the model was highly effective at distinguishing the two classes within the available dataset.  
### Finding 2 — Feature Importance
Engagement-related variables were among the most important predictors in the Random Forest model. These four features are `video_view_count`, `video_like_count`, `video_share_count`, `video_comment_count` respectively.This suggests that engagement patterns contain useful predictive information for distinguishing claims from opinions within this dataset. Feature importance should not be interpreted as causal evidence.  
### Finding 3 — Business Implications
The model could potentially be used as a decision-support tool for content moderation workflows. By automatically identifying videos that are more likely to contain claims, the model could help moderation teams prioritize content for further review and potentially reduce manual screening workload.
## Champion Model
| Model         | Validation Accuracy | Selection    |
| ------------- | ------------------: | ------------ |
| Random Forest |               ~1.00 | **Champion** |
| XGBoost       |               ~0.99 | Alternative  |
Random Forest was selected as the champion model because it achieved slightly stronger validation performance than XGBoost while maintaining similarly strong performance across precision, recall, and F1-score.
## Business Recommendations
There are three valuable recommendations:
1. Evaluate the model using representative real-world moderation data before considering deployment.
2. Use model predictions to support prioritization rather than treating them as final moderation decisions.
3. Monitor model performance over time because user behaviour and content patterns may change.
## Limitations
There are three limitations as well:
1. The dataset is synthetic and was designed for educational purposes.
2. The available feature set is relatively limited compared with a production content moderation system.
3. The test set comes from the same underlying dataset, so the results do not establish performance under future distribution shifts.
## Next Steps
Given that the dataset used in this project is for educational purposes, further work will be conducted in a follow-up project:
1. Validate using representative real-world data.
2. Add richer linguistic and behavioural features.
3. Evaluate performance across relevant content/user groups.
4. Establish model monitoring procedures.
5. Assess deployment readiness after validation.