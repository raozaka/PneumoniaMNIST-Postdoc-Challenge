# Task 1 — CNN Classification Report

---

## 1. Objective

The objective of Task 1 is to build an end-to-end convolutional neural network (CNN) for binary classification of chest X-ray images from **PneumoniaMNIST (MedMNIST v2)**, distinguishing:

- **0 — Normal**
- **1 — Pneumonia**

The system is designed to achieve high diagnostic sensitivity while maintaining reasonable specificity.

---

## 2. Model Architecture

A lightweight CNN architecture was implemented to balance performance and computational efficiency (no GPU required).

### Architecture Overview

- **Input:** 28×28 grayscale images  
- **3 Convolutional Blocks:**
  - Conv2D → Batch Normalization → ReLU → MaxPooling  
- **Global Average Pooling**
- **Fully Connected Layer (64 units)**
- **Dropout Regularization**
- **Final Dense Layer:** 1 neuron (sigmoid activation)

### Model Size

| Parameter Type | Count |
|---------------|--------|
| Total Parameters | 101,889 |
| Trainable Parameters | 101,441 |
| Non-trainable Parameters | 448 |

The model remains compact (~100K parameters) and suitable for standard hardware.

---

## 3. Training Strategy

To improve generalization and handle class imbalance, the following strategies were applied:

### Optimization

- **Optimizer:** Adam  
- **Learning Rate:** 3e-4  

### Loss Function

- **Binary Crossentropy with Label Smoothing (0.05)**  
  Label smoothing improves probability calibration and reduces overconfident predictions.

### Class Imbalance Handling

- Class weights computed using `compute_class_weight`
- Encourages balanced learning across Normal and Pneumonia classes

### Learning Rate Scheduling

- `ReduceLROnPlateau` monitored on validation AUC
- Adaptive reduction when performance plateaus

### Model Selection

- Best model saved using `ModelCheckpoint` (based on validation AUC)
- Final evaluation performed using the best checkpoint

---

## 4. Test Set Performance

Evaluation was performed on **624 test samples**.

### Overall Metrics

| Metric | Value |
|--------|--------|
| Accuracy | **0.8942** |
| Precision | 0.9197 |
| Recall | 0.9103 |
| F1-score | 0.9149 |
| ROC-AUC | **0.9543** |
| Sensitivity (Pneumonia Recall) | **0.9103** |
| Specificity (Normal Recall) | **0.8675** |

The model achieves strong discriminative capability with AUC = 0.9543.

---

## 5. Confusion Matrix Analysis

### Confusion Matrix

|                | Predicted Normal | Predicted Pneumonia |
|----------------|------------------|---------------------|
| **True Normal**     | 203 | 31 |
| **True Pneumonia**  | 35  | 355 |

### Interpretation

- The model correctly detects **91% of pneumonia cases**.
- **35 pneumonia cases** were missed (False Negatives).
- **31 normal cases** were incorrectly flagged as pneumonia (False Positives).
- The operating point prioritizes pneumonia detection, which is clinically desirable in screening scenarios.

---

## 6. ROC Curve Analysis

- **AUC = 0.9543**

The ROC curve demonstrates strong class separability.  
Performance is significantly above the random baseline (AUC = 0.5), indicating effective probability ranking.

---

## 7. Error Analysis

A total of **66 misclassified samples** were identified and saved for further analysis.

Common failure patterns include:

- Subtle pneumonia cases with weak opacity
- Normal images containing ambiguous brightness regions
- Reduced spatial detail due to low image resolution (28×28)

These cases were later used in Task 2 to assess whether a Visual Language Model (VLM) could provide complementary interpretative insight.

---

## 8. Strengths

- High ROC-AUC (0.9543)
- Strong pneumonia sensitivity (0.9103)
- Balanced precision–recall tradeoff
- Lightweight architecture (~100K parameters)
- Robust training pipeline (class weighting + label smoothing + LR scheduling)

---

## 9. Limitations

- Low image resolution (28×28) restricts fine-grained radiographic feature extraction
- Dataset relatively small
- Binary classification only (no multi-pathology detection)
- No external validation dataset

---

## 10. Conclusion

The CNN baseline achieves strong discriminative performance on PneumoniaMNIST, with high sensitivity and robust probability ranking (AUC 0.95). The saved misclassified samples provide valuable insight for multimodal evaluation in Task 2 and potential extensions in semantic retrieval systems.

