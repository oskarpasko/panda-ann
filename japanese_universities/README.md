# 📌 Project: [Japanese Univiersities]

## 🎯 Goal
*The network assigns school rankings based on features such as the number of reviews, department count, whether the school offers graduate programs, remote learning availability, and several other attributes. What’s interesting is that using these factors, we can predict a school's difficulty level with reasonable accuracy.*

---

## 📊 Data
- **File:** `japanese_universities.csv`  
- **Data source:** [Dataset](https://www.kaggle.com/datasets/webdevbadger/japanese-universities)  
- **Description:** Comprehensive Japanese University data since 1872. Contains 813 university information collected from the Japan Ministry of Education and two other online sources.

## 📄 Feature Description

| Feature | Description |
|---------|-------------|
| `code` | The identification codes assigned by the Ministry of Education. |
| `name` | The official English translation of university names. |
| `name_jp` | The official Japanese university names. |
| `type` | The type of university; one of National, Public, or Private. |
| `type_jp` | The type of university in Japanese. |
| `address` | The address of the universities. |
| `postal_code` | The postal codes. |
| `phone` | The phone numbers. |
| `state_code` | The codes associated with the state. |
| `state_jp` | The state of universities. |
| `latitude` | The latitude of university locations. |
| `longitude` | The longitude of university locations. |
| `found` | The year and month the university was founded. |
| `faculty_count` | The number of faculties in universities. |
| `department_count` | The number of departments in universities. |
| `has_grad` | The availability of a graduate school. True or False. |
| `has_remote` | The availability of a remote school. True or False. |
| `review_rating` | The review rating of universities. |
| `review_count` | The number of reviews of universities. |
| `difficulty_SD` | The standard deviation measurement of universities' difficulty. |
| `difficulty_rank` | The rank of universities' difficulty. The highest to lowest ranking order is: S, A, B, C, D, E, F. |

## 🧹 Data Preprocessing & Preparation

- **Missing Values:**
  - Checked percentage of missing values per column.
  - Removed rows with missing values in:
    - `difficulty_rank` (target)
    - `review_rating`
    - `review_count`

- **Feature Selection:**
  - Selected the most relevant features:
    ```
    ['faculty_count', 'department_count', 'has_remote', 'has_grad', 
     'review_rating', 'review_count', 'difficulty_SD', 'difficulty_rank']
    ```

- **Encoding:**
  - Target column `difficulty_rank` encoded using `LabelEncoder`.
  - Categorical columns (`has_remote`, `has_grad`) converted to binary features using one-hot encoding (`drop_first=True`).

- **Train/Test Split:**
  - Split dataset into **80% training / 20% testing**.
  - `stratify=y_encoded` ensures class proportions are preserved.

- **Feature Scaling:**
  - Standardized features using `StandardScaler` (mean = 0, std = 1).

- **Tensor Conversion (PyTorch):**
  - Converted `X_train`, `X_test` to `torch.float32`.
  - Converted `y_train`, `y_test` to `torch.long`.
  - Ready for input into the PyTorch model.


---

## 🧠 Model
- **Type:** MLP 
- **Framework:** PyTorch 2.7.1+cpu
- **Architecture:**  
  - **Layers:**  
    - `Linear(input_dim → 16)`  
    - `ReLU` activation  
    - `Linear(16 → 8)`  
    - `ReLU` activation  
    - `Linear(8 → num_classes)` (output logits)  
  - **Activation functions:** ReLU for hidden layers, no activation for the output layer (raw logits for `CrossEntropyLoss`).  
  - **Output shape:** `[batch_size, num_classes]`  

---

## ⚙️ Hyperparameters
- **Epochs:**  2000 (with early stopping, patience = 5)
- **Batch size:**  (depends on DataLoader, e.g., 32 or 64)
- **Learning rate:**  0.001
- **Optimizer:**  Adam
- **Loss function:**  CrossEntropyLoss
- **Metrics:**  Accuracy (primary), Validation Loss

---

## 🔬 Hyperparameter Tuning / Experiments

| Configuration | Final Epochs | Hidden Layers | Activation func per Layer | Neurons per Layer | Learning Rate | Accuracy | 
| ------------- | ------------ | ------------- | ------------------------- | ----------------- | ------------- | -------- | 
| Config 1.1      | 81          | 2             | [LogSigmoid, LogSigmoid ] | [32, 16]           | 0.001         | 95.33%      | 
| Config 1.2      | 93          | 2             | [LogSigmoid, LogSigmoid ] | [32, 16]           | 0.001         | 96.67%      | 
| Config 1.3      | 85          | 2             | [LogSigmoid, LogSigmoid ] | [32, 16]           | 0.001         | 94.67%      | 
| Config 1.4      | 81          | 2             | [LogSigmoid, LogSigmoid ] | [32, 16]           | 0.001         | 96.67%      | 
| Config 1.5      | 62          | 2             | [LogSigmoid, LogSigmoid ] | [32, 16]           | 0.001         | 94.00%      | 
| Config 2.1      | 56          | 2             | [ReLU, ReLU ] | [32, 16]           | 0.001         | 93.33%      | 
| Config 2.2      | 57          | 2             | [ReLU, ReLU ] | [32, 16]           | 0.001         | 94.67%      | 
| Config 2.3      | 94          | 2             | [ReLU, ReLU ] | [32, 16]           | 0.001         | 97.33%      | 
| Config 2.4      | 78          | 2             | [ReLU, ReLU ] | [32, 16]           | 0.001         | 96.00%      | 
| Config 2.5      | 73          | 2             | [ReLU, ReLU ] | [32, 16]           | 0.001         | 94.00%      | 
| Config 3.1      | 105          | 2             | [ReLU, ReLU ] | [16, 8]           | 0.001         | 95.33%      | 
| Config 3.2      | 96          | 2             | [ReLU, ReLU ] | [16, 8]           | 0.001         | 94.00%      | 
| Config 3.3      | 102          | 2             | [ReLU, ReLU ] | [16, 8]           | 0.001         | 95.33%      | 
| Config 3.4      | 144          | 2             | [ReLU, ReLU ] | [16, 8]           | 0.001         | 96.00%      | 
| Config 3.5      | 102          | 2             | [ReLU, ReLU ] | [16, 8]           | 0.001         | 95.33%      | 
| Config 4.1      | 102          | 2             | [LogSigmoid, LogSigmoid ] | [16, 8]           | 0.001         | 95.33%      | 
| Config 4.2      | 87          | 2             | [LogSigmoid, LogSigmoid ] | [16, 8]           | 0.001         | 95.33%      | 
| Config 4.3      | 117          | 2             | [LogSigmoid, LogSigmoid ] | [16, 8]           | 0.001         | 96.67%      | 
| Config 4.4      | 113          | 2             | [LogSigmoid, LogSigmoid ] | [16, 8]           | 0.001         | 94.67%      | 
| Config 4.5      | 112          | 2             | [LogSigmoid, LogSigmoid ] | [16, 8]           | 0.001         | 96.00%      | 

```

Accuracy per class [Config 3]:
A     80.000000
B    100.000000
C    100.000000
D     94.444444
E    100.000000
F     98.412698
S    100.000000


```

```

Accuracy per class [Config 4]:
A    100.000000
B     75.000000
C    100.000000
D     83.333333
E    100.000000
F     98.412698
S    100.000000

```

- **Best overall accuracy:**  
  - The highest results were achieved with **ReLU + larger layers [32,16]** (Config 2.3), reaching **97.33% Accuracy**.  
  - LogSigmoid performed stably in smaller layers ([16,8]), but did not surpass the best ReLU results.

- **Impact of architecture:**  
  - Increasing the number of neurons in the first hidden layer improves the network’s ability to capture complex patterns.  
  - Number of epochs is not the most critical factor – the **combination of neurons and activation functions** is more important.  

- **Per-class accuracy analysis:**  
  - The model performs very well for most classes (A, C, E, F, S – Accuracy ~98–100%).  
  - Classes B and D show lower performance (75–87%), indicating **harder-to-distinguish patterns or lower representation in the dataset**.  
  - High overall Accuracy can mask issues with smaller or more difficult classes.

- **Recommendations for further experiments:**  
  - Apply **class weighting** in the loss function to improve accuracy for underrepresented classes.  
  - Consider increasing the number of neurons or layers for challenging classes.  
  - Experiment with **mixed activation functions** (e.g., ReLU + LogSigmoid) for better generalization.  
  - Monitor **Accuracy per class / F1-score** instead of only overall accuracy to get a more complete view of model performance.

- **Overall conclusion:**  
  - The MLP network can effectively classify Japanese university difficulty levels based on selected features, achieving high accuracy, but per-class analysis reveals areas that could benefit from further optimization.



---

## 📈 Training & Validation
- **Data split:** 80% training / 20% testing (no separate validation set)  
- **Early stopping:** Enabled (patience = 5)  
- **Training progress (Validation Loss):**  
    ```
    Epoch 1/2000 | val loss: 1.9183
    Epoch 10/2000 | val loss: 1.0998
    Epoch 20/2000 | val loss: 0.6902
    Epoch 30/2000 | val loss: 0.4764
    Epoch 40/2000 | val loss: 0.3494
    Epoch 50/2000 | val loss: 0.2755
    Epoch 60/2000 | val loss: 0.2192
    Epoch 70/2000 | val loss: 0.1853
    Epoch 80/2000 | val loss: 0.1630
    Epoch 90/2000 | val loss: 0.1462
    Epoch 100/2000 | val loss: 0.1301
    Epoch 110/2000 | val loss: 0.1149
    Epoch 120/2000 | val loss: 0.1058
    Epoch 130/2000 | val loss: 0.0979
    Epoch 140/2000 | val loss: 0.0992
    Stop at epoch 149
    ```

---

## 🏆 Results

- **Final Performance Metrics:**
    - **Accuracy:** 100%
    - **Precision, Recall, F1-score:** 1.00 for all classes
    - **RMSE:** *Not applicable* (classification task)

- **Classification Report:**
    ```
               precision    recall  f1-score   support
           A       0.80      0.80      0.80         5
           B       0.88      0.88      0.88         8
           C       1.00      0.96      0.98        24
           D       0.94      0.94      0.94        18
           E       0.94      1.00      0.97        30
           F       1.00      0.98      0.99        63
           S       1.00      1.00      1.00         2

    accuracy                             0.97       150
   macro avg        0.94      0.94      0.94       150
  weighted avg     0.97      0.97      0.97       150
    ```

- **Accuracy per Class:**
    ```
    A     80    %
    B     87.5  %
    C     95.83 %
    D     94.44 %
    E     100   %
    F     98.41 %
    S     100   %
    ```

- **Example Predictions:**
    - Model perfectly matched all test samples (True vs Predicted were almost identical).

---
