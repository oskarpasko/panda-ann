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


- **Preprocessing:** Any steps you applied to clean or transform the data.  

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

## 📈 Training & Validation
- **Data split:** 80% training / 20% testing (no separate validation set)  
- **Early stopping:** Enabled (patience = 5)  
- **Training progress (Validation Loss):**  
    ```
    Epoch 1/2000  | val loss: 1.9096
    Epoch 10/2000 | val loss: 0.9492
    Epoch 20/2000 | val loss: 0.5991
    Epoch 30/2000 | val loss: 0.4034
    Epoch 40/2000 | val loss: 0.2945
    Epoch 50/2000 | val loss: 0.2221
    Epoch 60/2000 | val loss: 0.1814
    Epoch 70/2000 | val loss: 0.1547
    Epoch 80/2000 | val loss: 0.1343
    Epoch 90/2000 | val loss: 0.1177
    Epoch 100/2000| val loss: 0.1077
    Epoch 110/2000| val loss: 0.0945
    Stop at epoch 119
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
    A              1.00      1.00      1.00         2
    B              1.00      1.00      1.00         4
    C              1.00      1.00      1.00        12
    D              1.00      1.00      1.00         9
    E              1.00      1.00      1.00        15
    F              1.00      1.00      1.00        32
    S              1.00      1.00      1.00         1

    accuracy                           1.00        75
    macro avg      1.00      1.00      1.00        75
    weighted avg   1.00      1.00      1.00        75
    ```

- **Accuracy per Class:**
    ```
    A    100.0%
    B    100.0%
    C    100.0%
    D    100.0%
    E    100.0%
    F    100.0%
    S    100.0%
    ```

- **Example Predictions:**
    - Model perfectly matched all test samples (True vs Predicted were identical).

- **Training Visualization (Validation Loss):**
    ```
    Epoch 1   | val loss: 1.9096
    Epoch 10  | val loss: 0.9492
    Epoch 50  | val loss: 0.2221
    Epoch 100 | val loss: 0.1077
    Epoch 110 | val loss: 0.0945
    Stop at epoch 119
    ```

 

---
