---
## Predict Vs Decision Function Scores
- The predictions output will contain values of 1 and -1. A value of 1 indicates that the data point is considered an inlier (normal), and -1 indicates that it is considered an outlier (anomaly).
- The decision_scores output will contain the anomaly scores for each data point. The scores indicate how isolated each sample is compared to the rest of the dataset. Higher scores (more positive values) indicate that the data point is likely to be an inlier, while lower scores (more negative values) suggest that the point is an outlier.
- Each SHAP value for a feature represents the contribution of that feature's value to the difference between the predicted score for that instance and the base value (average prediction)
- Higher positive SHAP values for a feature mean that the feature's value for that instance pushed the prediction towards a higher anomaly_score (more anomalous)
- 
---
