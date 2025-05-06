Project Explanation: Cancer Presence Prediction Using Gene Expression Data
Overview
In this project, I aimed to predict cancer presence (0 for absent, 1 for present) using gene expression data (Gene One and Gene Two) from a genomic dataset. I used a combination of machine learning models, including a Neural Network, a Voting Classifier, and a Stacking Classifier, to build robust predictions. My dataset had 3000 samples, with a training set of 2400 samples and a test set of 600 samples, each with 5 features after preprocessing. The class distribution in the training set was balanced, with 1172 samples for class 0 and 1228 for class 1.
Data Preprocessing and Exploration
I started by downloading the dataset from Kaggle and selecting Gene One and Gene Two as my features, with Cancer Present as the target variable. I normalized the features using StandardScaler and added polynomial features (degree 2) to capture non-linear relationships, resulting in 5 features: Gene One, Gene Two, Gene One², Gene Two², and Gene One * Gene Two.
To understand the data, I visualized the distributions of Gene One and Gene Two (see the "Distribution of Gene One Expression" and "Distribution of Gene Two Expression" plots). Both genes showed multimodal distributions, with distinct patterns for cancer presence (blue for 0, orange for 1). The scatter plot ("Gene One vs. Gene Two by Cancer Presence") showed some clustering, with cancer-present samples (red) tending toward higher Gene One values and cancer-absent samples (blue) toward higher Gene Two values. I also computed the correlation between Gene One and Gene Two, which was -0.375, indicating a moderate negative relationship.
Model Development
I implemented three models to predict cancer presence:

Neural Network: I built a Sequential model with four layers (64, 32, 16 neurons, and a sigmoid output layer) using ReLU activations and dropout (0.2) for regularization. I trained it for 180 epochs with early stopping (patience=20), using the Adam optimizer (learning rate 0.0005) and binary cross-entropy loss. The training process showed steady improvement, with validation accuracy reaching 0.9383 and validation loss dropping to 0.1693 by epoch 12.

Voting Classifier: I combined an SVM (RBF kernel, C=100, gamma=0.01) and an XGBoost model (learning rate 0.05, max depth 3, 200 estimators) using soft voting to leverage their strengths.

Stacking Classifier: I used the Voting Classifier as a base estimator and added a Logistic Regression model as the final estimator, with 5-fold cross-validation to improve generalization.


Model Evaluation
I evaluated all models on the test set (600 samples) using accuracy, classification reports, AUC-ROC, confusion matrices, and ROC curves:

Neural Network: Achieved an accuracy of 0.9367, with precision, recall, and F1-score around 0.94 for both classes (see classification report). The confusion matrix ("Confusion Matrix: Neural Network") shows 309 true negatives, 253 true positives, 19 false positives, and 19 false negatives. The AUC-ROC was 0.9824.

Voting Classifier: Also achieved an accuracy of 0.9367, with similar precision, recall, and F1-scores (0.94). Its confusion matrix ("Confusion Matrix: Voting Classifier") shows 310 true negatives, 252 true positives, 18 false positives, and 20 false negatives. The AUC-ROC was 0.9802.

Stacking Classifier: Performed slightly better with an accuracy of 0.9400. Its confusion matrix ("Confusion Matrix: Stacking Classifier") shows 312 true negatives, 252 true positives, 16 false positives, and 20 false negatives. The AUC-ROC was 0.9802.


The ROC curves ("ROC Curves") confirm that all models performed similarly well, with AUCs of 0.98, indicating excellent discriminative ability.
Feature Importance
Using SHAP values on the XGBoost model within the Voting Classifier, I analyzed feature importance (see "SHAP Summary Plot"). Gene Two had the highest impact on predictions, followed by Gene One. Polynomial features like Gene One * Gene Two also contributed significantly, especially when their values were high (red dots), pushing the model toward predicting cancer presence.
Conclusion
My models successfully predicted cancer presence with high accuracy (0.94) and AUC (0.98). The Stacking Classifier slightly outperformed the others, likely due to its ability to learn from the Voting Classifier's predictions. The negative correlation between Gene One and Gene Two, combined with their distinct distributions, helped the models differentiate between classes. This project demonstrates the effectiveness of combining neural networks and ensemble methods for genomic data classification.
