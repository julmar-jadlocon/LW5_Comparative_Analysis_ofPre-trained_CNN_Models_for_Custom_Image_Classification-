# LW5_Comparative_Analysis_ofPre-trained_CNN_Models_for_Custom_Image_Classification-

Colab Link - https://colab.research.google.com/drive/13MWT6wP2TU-nA2e9kyaZQ_c87HPobzTB?usp=sharing

A. Model Performance1. 

1. Which pre-trained model achieved the highest accuracy? Why?
   
ResNet50 achieved the highest test accuracy ($77.34%). Its skip-connections (residual blocks) allow it to train much deeper architectures effectively 
without suffering from the vanishing gradient problem. This enables the model to extract highly complex, hierarchical spatial features from the bonsai images compared to VGG16's shallower, sequential structure.

3. Which model had the lowest performance? What could be the reason?
EfficientNetB0 performed the worst, with an abysmal test accuracy of just $6.73\%$ (worse than random guessing for a 20-class dataset).This is a classic mismatch between Keras's default BatchNormalization layer  behavior and frozen pre-trained weights during transfer learning. If the training data distribution differs significantly from ImageNet and the batch statistics are locked or corrupted during feature extraction, the model fails entirely to converge.

4. How did loss values compare across models?
   
The loss values perfectly mirrored the accuracy metrics: ResNet50 had the lowest and most stable loss (Train: $0.7141$, Test: $0.7713$). VGG16 showed moderate loss values (Train: $1.7906$, Test: $1.6225$), steadily decreasing over epochs. EfficientNetB0 flatlined at a massive loss value of roughly $2.99$, indicating it never found a viable gradient descent path.

B. Evaluation Metrics

4. Why is accuracy not enough to evaluate a model?
   
Accuracy treats all classes and types of errors equally. In a multi-class dataset, if your data is even  slightly imbalanced, a model can cheat by scoring well on highly populated classes while failing completely on rare classes. Metrics like Precision, Recall, and F1-score reveal if a model is biased or choking on specific categories.

6. Which model had the best F1-score? What does it indicate?
   
ResNet50 had the best F1-score ($0.7661$). This high harmonic mean indicates a robust balance between Precision and Recall across all 20 bonsai classes, meaning it minimizes both false positives and false negatives consistently.

7. How did Precision and Recall differ across models?
   
For ResNet50, they were highly balanced (Precision: $0.7727$, Recall: $0.7663$). For VGG16, Precision ($0.5423$) was slightly higher than Recall ($0.5228$). 
This indicates VGG16 was slightly more conservative—when it predicted a specific class, it was often right, but it missed several actual instances (higher false negatives).

9. Which classes were frequently misclassified?
    
Looking closely at your earlier VGG16/ResNet50 classification reports: Holy Basil (VGG16 Recall: $0.18$, ResNet50 Recall: $0.64$) and Sweet Bail were frequently misclassified. They were likely confused with other needle/evergreen or fine-leaf varieties like Scecnted Geranium. Pot Marigold and Yarrow also saw cross-misclassification due to highly similar leaf geometry shapes.

11. What patterns did you observe in the confusion matrix?
    
Visual Congruence: Misclassifications heavily clustered among botanical look-alikes (e.g., evergreen conifers confused  with other conifers; deciduous broadleaf trees confused with other broadleaf trees).ResNet50 Sparsity: ResNet50's matrix would show a stark, strong diagonal line with minimal off-diagonal scatter, whereas VGG16's matrix would show wider, hazy horizontal scatter across visually ambiguous classes.

D. ROC and AUC9. Which model had the highest AUC score?
ResNet50 had the highest ROC AUC score ($0.9806$).10. What does AUC tell us about model performance?The Area Under the ROC Curve (AUC) measures the model's ability to 
distinguish between classes across all possible probability thresholds. ResNet50’s score of $0.9806$ means there is a $98.06\%$ chance that the model will rank a randomly chosen positive instance higher than a randomly chosen negative one. It proves the model has excellent class-separation mechanics, even if the absolute class cutoff choice isn't perfectly calibrated.

E. Explainability (Grad-CAM)

11. What did Grad-CAM reveal about model decision-making?
    
Grad-CAM heatmaps reveal whether the neural network is looking at semantic features (like leaf shape, trunk curvature, or pot style) or background noise (like wall textures or lighting) to make its final prediction.

13. Did the model focus on relevant image regions?
    
ResNet50: Yes. It likely focused tightly on distinctive botanical regions—the branch canopy structure, leaf textures, and unique trunk styles. VGG16: Moderately. It often catches the overall silhouette of the tree but can sometimes bleed its attention out into the background canvas or the container pot.

15. Which model produced the most meaningful heatmaps?
    
ResNet50 produced the most meaningful heatmaps. Its cleaner feature extraction isolates fine-grained regions of interest (like individual leaf structures) far better than VGG16's coarser, deeper layers.

F. Model Comparison & Improvement
14. Which model would you recommend for deployment? Why?
ResNet50 is the definitive recommendation. It dominates every single performance benchmark (Accuracy, F1-score, and ROC AUC), demonstrating high reliability and predictable behavior across all 20 distinct target categories.15. How can you further improve your best-performing model?Unfreeze and Fine-tune: Unfreeze the top dense layers of ResNet50 and train with an incredibly low learning rate (e.g., $10^{-5}$) to subtly adapt base layers to bonsai-specific details.Targeted Augmentation: Implement specialized data augmentation configurations (e.g., random cropping, color jittering, and scaling) to force the network to prioritize leaf textures over image framing.

G. Real-World Application

16. How can your model be applied in real-world scenarios?
    
E-Commerce & Nurseries: Automated inventory sorting and digital catalog tagging for large plant nurseries. Mobile Identification Apps: An interactive mobile guide for hobbyists to instantly identify bonsai styles and species using a smartphone camera.

18. What are the risks of deploying an inaccurate model?
    
Misidentifying a species could lead an enthusiast to follow the wrong care guide (e.g., watering schedules, sunlight exposure, or pruning styles), which could easily damage or kill a highly valuable, delicate specimen plant.

20. How can this system be integrated into a mobile/web app?
    
The trained ResNet50 model can be converted to an optimized format like TensorFlow Lite (TFLite) or ONNX for lightweight execution. It can then be embedded directly on-device within a mobile app framework (like Flutter or React Native) or hosted as a cloud-based microservice API using FastAPI paired with Docker.

