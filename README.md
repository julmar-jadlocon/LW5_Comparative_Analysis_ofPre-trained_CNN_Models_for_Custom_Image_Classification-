# LW5_Comparative_Analysis_ofPre-trained_CNN_Models_for_Custom_Image_Classification-

Colab Link - https://colab.research.google.com/drive/13MWT6wP2TU-nA2e9kyaZQ_c87HPobzTB?usp=sharing


# PART 12: MODEL PERFORMANCE COMPARISON TABLE

| Model Sample | Train Accuracy | Train Loss | Test Accuracy | Test Loss | Precision | Recall | F1-score | ROC AUC |
|---|---|---|---|---|---|---|---|---|
| Pre-Trained Model 1 (VGG16) | 32.49% | 2.1898 | 37.08% | 2.1150 | 0.3750 | 0.3826 | 0.3658 | 0.8561 |
| Pre-Trained Model 2 (ResNet50) | 70.47% | 0.9897 | 68.78% | 1.1002 | 0.6905 | 0.6976 | 0.6888 | 0.9608 |
| Pre-Trained Model 3 (EfficientNetB0 / MobileNetV2) | 6.33% | 2.9951 | 7.01% | 2.9940 | 0.0030 | 0.0500 | 0.0057 | 0.5082 |
| Model from Teachable Machine | — | — | — | — | — | — | — | — |
| Your 1st Model (LW3 Baseline) | N/A (loaded) | N/A | 33.91% | N/A | 0.3709 | 0.3550 | 0.3443 | 0.8655 |
| Your 2nd Model (LW3 Enhanced) | N/A (loaded) | N/A | 33.91% | N/A | 0.3709 | 0.3550 | 0.3443 | 0.8655 |
| Your 3rd Model - The Good Model (LW4) | N/A (loaded) | N/A | 33.72% | N/A | 0.3676 | 0.3569 | 0.3290 | 0.8630 |


## Guide Questions: Answers

### A. Model Performance

**1. Which pre-trained model achieved the highest accuracy? Why?**

ResNet50 achieved the highest test accuracy at **68.78%**. 

ResNet50 is a deeper and more complex architecture than VGG16, and its residual connections help mitigate the vanishing gradient problem, allowing it to learn more intricate features from the data. This often leads to better performance on complex image classification tasks.

**2. Which model had the lowest performance? What could be the reason?**

EfficientNetB0 had the lowest performance, with a test accuracy of **7.01%**. 

The training logs for EfficientNetB0 show very low accuracy and high loss throughout its training, with the learning rate being reduced quickly (Epoch 5 and 7 warnings). This suggests that the model likely failed to converge effectively or learn meaningful features for this specific dataset and training setup. It's possible the default hyperparameters (e.g., learning rate, optimizer) were not suitable for this model on this dataset, or it may require more fine-tuning than the other models in this particular transfer learning setup.

**3. How did loss values compare across models?**

ResNet50 had the lowest test loss at **1.1002**, indicating it learned the data distribution best. VGG16 had a test loss of **2.1150**, which is higher than ResNet50. EfficientNetB0 had the highest test loss at **2.9940**, reinforcing its poor performance and lack of learning.

### B. Evaluation Metrics

**4. Why is accuracy not enough to evaluate a model?**

Accuracy alone can be misleading, especially in multi-class classification or with imbalanced datasets. For example, if 90% of the data belongs to one class, a model that always predicts that class would have 90% accuracy but would be useless for identifying other classes. Accuracy doesn't provide insights into the types of errors made (false positives vs. false negatives) or how well the model performs on individual classes.

**5. Which model had the best F1-score? What does it indicate?**

ResNet50 had the best F1-score of **0.6888**. The F1-score is the harmonic mean of precision and recall. A high F1-score indicates that the model has a good balance between precision (correct positive predictions relative to all positive predictions) and recall (correct positive predictions relative to all actual positives). In multi-class, a macro F1-score (as used here) means the model performs well across all classes, not just the majority ones.

**6. How did Precision and Recall differ across models?**

*   **ResNet50** generally showed the highest precision (0.6905) and recall (0.6976), indicating its ability to correctly identify positive instances while also minimizing false positives.
*   **VGG16** had lower precision (0.3750) and recall (0.3826) compared to ResNet50, suggesting a more moderate performance in correctly identifying positive instances.
*   **EfficientNetB0** had extremely low precision (0.0030) and recall (0.0500), which aligns with its very poor accuracy and loss values. This indicates it struggled significantly with both correctly identifying positive classes and avoiding false alarms.

### C. Confusion Matrix Analysis

**7. Which classes were frequently misclassified?**

Without the visual confusion matrix, we can infer from the classification reports for VGG16 and especially EfficientNetB0. For VGG16, classes with low precision and recall (e.g., Scented_Geranium, Holy_Basil, Lilac, Petunias, Chrysanthemum, Thymes) were frequently misclassified. For EfficientNetB0, almost all classes were misclassified, as evidenced by near-zero precision and recall for most, except for 'Sunflower' which had a recall of 1.00 but a precision of only 0.06, indicating it likely predicted 'Sunflower' for many images across different classes.

**8. What patterns did you observe in the confusion matrix?**

*   **ResNet50's** confusion matrix (implied by its high metrics) would likely show a strong diagonal, indicating that it correctly classified most images into their true classes. Off-diagonal elements would be relatively small.
*   **VGG16's** confusion matrix would likely show a less pronounced diagonal and more scattered off-diagonal elements, reflecting its moderate performance and some common misclassifications between visually similar flowers.
*   **EfficientNetB0's** confusion matrix would likely show a very weak diagonal and potentially one row (e.g., 'Sunflower') having a high recall but receiving many false positives from other classes, indicating it biased its predictions heavily towards one or a few classes due to failed training.

### D. ROC and AUC

**9. Which model had the highest AUC score?**

ResNet50 had the highest Mean ROC AUC score of **0.9608**.

**10. What does AUC tell us about model performance?**

AUC (Area Under the Receiver Operating Characteristic curve) measures a classifier's ability to distinguish between classes. A higher AUC indicates that the model is better at separating positive and negative classes across various classification thresholds. An AUC of 1.0 represents a perfect classifier, while an AUC of 0.5 represents a classifier no better than random guessing. ResNet50's high AUC score suggests it has excellent discriminatory power for the given task.

### E. Explainability (Grad-CAM)

**11. What did Grad-CAM reveal about model decision-making?**

The Grad-CAM visualizations were not successfully generated for any test images, as indicated by the output: `[001] No test image found — skipping.` for all numbers in the `GRADCAM_START` to `GRADCAM_END` range. Therefore, we cannot determine what Grad-CAM revealed about the model's decision-making from the current output.

**12. Did the model focus on relevant image regions?**

Cannot be determined due to the absence of Grad-CAM heatmaps.

**13. Which model produced the most meaningful heatmaps?**

Cannot be determined due to the absence of Grad-CAM heatmaps.

### F. Model Comparison & Improvement

**14. Which model would you recommend for deployment? Why?**

Based on the evaluation metrics (Test Accuracy, F1-score, and ROC AUC), **ResNet50** is the clear recommendation for deployment. It consistently outperformed VGG16 and significantly outperformed EfficientNetB0 across all key metrics. Its high accuracy and AUC suggest it is the most reliable and robust model for this image classification task.

**15. How can you further improve your best-performing model?**

To further improve the ResNet50 model, several strategies can be considered:

1.  **Fine-tuning:** Instead of just training the dense layers, unfreeze some of the later convolutional layers of the pre-trained ResNet50 base and fine-tune them with a very small learning rate. This allows the model to adapt the pre-trained features more specifically to the new dataset.
2.  **More Data/Data Augmentation:** While data augmentation (like random flips, rotations, zooms) was likely implicitly used by `image_dataset_from_directory`, exploring more aggressive or specific augmentations could help. Also, acquiring more diverse training data, if available, is almost always beneficial.
3.  **Hyperparameter Tuning:** Systematically tune hyperparameters like learning rate, optimizer (e.g., trying SGD with momentum, RMSprop), batch size, and dropout rates using techniques like grid search or random search.
4.  **Learning Rate Schedules:** Experiment with more sophisticated learning rate schedules (e.g., cosine decay, cyclical learning rates) beyond `ReduceLROnPlateau`.
5.  **Ensemble Methods:** Combine predictions from multiple models (e.g., trained ResNet50 and VGG16) to potentially achieve higher accuracy than any single model.
6.  **Transfer Learning from a larger, more diverse dataset:** If the current ImageNet weights are not fully representative, using a model pre-trained on an even larger and more diverse image dataset could be beneficial.

### G. Real-World Application

**16. How can your model be applied in real-world scenarios?**

This image classification model, particularly the ResNet50 version, could be applied in various real-world scenarios:

*   **Automated Plant Identification:** For gardeners, farmers, or nature enthusiasts, a mobile app could use the model to instantly identify plant species from a photo, helping with care instructions, avoiding poisonous plants, or learning about local flora.
*   **Agricultural Disease Detection:** With specialized datasets, similar models could identify plant diseases from images, allowing for early intervention and preventing crop loss.
*   **Educational Tools:** Interactive learning applications for botany or environmental science could use this technology to engage users in identifying plants.
*   **Smart Gardening Systems:** Integrated into smart cameras, the model could monitor plant growth, identify weeds, or alert users to specific plant needs.

**17. What are the risks of deploying an inaccurate model?**

Deploying an inaccurate model in real-world scenarios can have significant risks:

*   **Incorrect Plant Care:** Misidentifying a plant could lead to wrong watering, fertilizing, or pruning practices, potentially harming or killing the plant.
*   **Health and Safety Hazards:** If used for identifying edible vs. poisonous plants, an inaccurate model could lead to severe health consequences or even fatalities.
*   **Economic Loss:** In agriculture, misidentifying crops, weeds, or diseases could lead to significant financial losses for farmers due to improper treatment or delayed action.
*   **Loss of Trust:** Users will quickly lose trust in an application or system that frequently provides incorrect identifications, leading to low adoption and negative reputation.
*   **Resource Misallocation:** Incorrect predictions might lead to wasted resources (e.g., applying pesticides to healthy plants, or not applying them when needed).

**18. How can this system be integrated into a mobile/web app?**

To integrate this system into a mobile or web application, you would typically follow these steps:

1.  **Model Export:** Convert the trained Keras model (e.g., `lw5_resnet50.keras`) into a format suitable for deployment. For mobile, this might involve converting it to TensorFlow Lite (`.tflite`). For web, it could be TensorFlow.js (`.tfjs`) format.
2.  **API Development (for web/cloud):**
    *   Create a RESTful API endpoint (e.g., using Flask, FastAPI, or cloud functions like Google Cloud Functions/AWS Lambda).
    *   The API would receive an image (e.g., as a base64 encoded string or a file upload) from the client.
    *   The backend would preprocess the image to match the model's expected input (resizing, normalization).
    *   Load the deployed model and make a prediction.
    *   Return the prediction (e.g., class name, confidence scores) as a JSON response to the client.
3.  **Mobile Application Development (iOS/Android):**
    *   Use platform-specific SDKs (e.g., TensorFlow Lite Android/iOS library).
    *   The mobile app would capture an image, preprocess it on-device, and then either send it to the cloud API or run the `.tflite` model locally on the device for inference.
    *   Display the prediction results to the user.
4.  **Web Application Development:**
    *   A frontend framework (e.g., React, Angular, Vue.js) would handle user interaction and image capture.
    *   Images would be sent to the backend API for inference (or processed directly in the browser using TensorFlow.js if the model is small enough).
    *   The results from the API or local inference would be displayed to the user.
