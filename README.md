## Analysis of the UNSW-NB15 Dataset  
---

### 1. Dataset Overview  
- The **UNSW-NB15** dataset combines real modern network traffic with nine types of synthetic attacks: Fuzzers, Analysis, Backdoors, DoS, Exploits, Generic, Reconnaissance, Shellcode, and Worms.  
- Each record has **49 features** plus a binary class label (0 = normal, 1 = attack).  
- Feature selection reduced the feature set from 49 to **42**.  
- Data split:  
  - **Training set:** 175,341 records  
  - **Test set:** 82,322 records :contentReference[oaicite:2]{index=2}:contentReference[oaicite:3]{index=3}  

---

### 2. Preprocessing Steps  
1. Drop the `attack_cat` (attack category) column.  
2. Split into `train_X`, `train_Y`, `test_X`, `test_Y`.  
3. Remove any null records.  
4. One-hot encode all categorical features.  
5. Align test features to the training set (add missing columns, drop extras).  
6. Normalize numeric features via z-score.  
7. Sort feature columns alphabetically in both sets :contentReference[oaicite:4]{index=4}:contentReference[oaicite:5]{index=5}.

---

### 3. Classification Models  
Four classifiers were trained and evaluated on the original (unbalanced) data:  
- **Logistic Regression**  
- **Decision Tree**  
- **Random Forest** (max depth = 20)  
- **Neural Network** (two hidden layers with 10 and 5 neurons) :contentReference[oaicite:6]{index=6}:contentReference[oaicite:7]{index=7}

---

### 4. Performance on Unbalanced Data  

| Metric     | Class | Logistic Reg. | Decision Tree | Random Forest | Neural Net |
|------------|-------|---------------|---------------|---------------|------------|
| **Precision** | 0     | 0.95          | 0.93          | 0.98          | 0.98       |
|            | 1     | 0.75          | 0.82          | 0.91          | 0.80       |
| **Recall**    | 0     | 0.61          | 0.75          | 0.71          | 0.70       |
|            | 1     | 0.97          | 0.96          | 0.99          | 0.99       |
| **F₁-Score**  | 0     | 0.74          | 0.83          | 0.83          | 0.81       |
|            | 1     | 0.85          | 0.86          | 0.89          | 0.88       | :contentReference[oaicite:8]{index=8}:contentReference[oaicite:9]{index=9}

---

### 5. Addressing Class Imbalance  

- The training set is skewed: **119,341 attacks** vs. **56,000 normals**.  
- Two rebalancing strategies were applied:

  1. **Random Undersampling:** keep all normals; randomly remove attacks.  
  2. **Random Oversampling:** replicate normals (with replacement) to match attacks. :contentReference[oaicite:10]{index=10}:contentReference[oaicite:11]{index=11}

---

#### 5.1. Results After Undersampling  

| Metric     | Class | Logistic Reg. | Decision Tree | Random Forest | Neural Net |
|------------|-------|---------------|---------------|---------------|------------|
| **Precision** | 0     | 0.89          | 0.93          | 0.95          | 0.93       |
|            | 1     | 0.80          | 0.85          | 0.88          | 0.87       |
| **Recall**    | 0     | 0.72          | 0.80          | 0.84          | 0.82       |
|            | 1     | 0.93          | 0.95          | 0.97          | 0.95       |
| **F₁-Score**  | 0     | 0.80          | 0.86          | 0.89          | 0.87       |
|            | 1     | 0.86          | 0.90          | 0.92          | 0.91       |
| **Accuracy**  | —     | 0.834         | 0.882         | 0.910         | 0.891     | :contentReference[oaicite:12]{index=12}:contentReference[oaicite:13]{index=13}

---

#### 5.2. Results After Oversampling  

| Metric     | Class | Logistic Reg. | Decision Tree | Random Forest | Neural Net |
|------------|-------|---------------|---------------|---------------|------------|
| **Precision** | 0     | 0.90          | 0.94          | 0.96          | 0.95       |
|            | 1     | 0.80          | 0.82          | 0.87          | 0.87       |
| **Recall**    | 0     | 0.72          | 0.74          | 0.82          | 0.82       |
|            | 1     | 0.93          | 0.96          | 0.97          | 0.96       |
| **F₁-Score**  | 0     | 0.80          | 0.83          | 0.89          | 0.88       |
|            | 1     | 0.86          | 0.89          | 0.92          | 0.91       |
| **Accuracy**  | —     | 0.835         | 0.862         | 0.904         | 0.900     | :contentReference[oaicite:14]{index=14}:contentReference[oaicite:15]{index=15}

---

### 6. 10-Fold Cross-Validation & Comparative Statistics  

- **10-fold CV** was run on both rebalanced datasets to assess stability.  
- A paired **t-test** (α = 0.05) on sensitivity and accuracy distributions found **no significant differences** between the top two classifiers in either scenario (all p-values > 0.4). :contentReference[oaicite:16]{index=16}:contentReference[oaicite:17]{index=17}

---

### 7. Key Takeaways  
- **Random Forest** generally achieved the highest precision and accuracy across all setups.  
- Rebalancing (both under- and oversampling) improved specificity and precision for the minority (normal) class at the cost of a small drop in sensitivity.  
- Statistical tests confirm that differences among the best models are not significant at the 5% level.

---

### References  
1. Moustafa, N., & Slay, J. (2015). UNSW-NB15: a comprehensive data set for network intrusion detection systems… :contentReference[oaicite:18]{index=18}:contentReference[oaicite:19]{index=19}  
2. Moustafa, N., & Slay, J. (2016). The evaluation of Network Anomaly Detection Systems… :contentReference[oaicite:20]{index=20}:contentReference[oaicite:21]{index=21}  
3. Moustafa, N., et al. (2017). Novel geometric area analysis technique for anomaly detection… :contentReference[oaicite:22]{index=22}:contentReference[oaicite:23]{index=23}
