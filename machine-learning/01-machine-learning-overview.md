# 1. Tổng quan Học máy (Machine Learning)

## 1.1. Định nghĩa

**Học máy (Machine Learning)** là lĩnh vực nghiên cứu các thuật toán cho phép máy tính **học từ dữ liệu** (kinh nghiệm) để cải thiện hiệu năng trên một nhiệm vụ mà không cần lập trình tường minh từng bước.

---

## 1.2. Phân loại chính

| Loại | Mô tả | Ví dụ |
|------|--------|------|
| **Supervised Learning** | Học từ dữ liệu có nhãn (X, y) | Phân loại ảnh, hồi quy giá nhà |
| **Unsupervised Learning** | Dữ liệu không nhãn, tìm cấu trúc | Clustering (K-means), giảm chiều (PCA) |
| **Reinforcement Learning** | Học qua tương tác, nhận phần thưởng/penalty | Game, robot, tự lái |
| **Semi-supervised** | Một phần có nhãn, phần còn lại không | Gán nhãn một phần rồi học |
| **Self-supervised** | Tự tạo nhãn từ dữ liệu (masking, contrast) | Pre-training BERT, SimCLR |

---

## 1.3. Nhiệm vụ cơ bản

### Regression (Hồi quy)
- **Mục tiêu**: Dự đoán giá trị liên tục y ∈ ℝ.
- **Ví dụ**: Giá nhà, nhiệt độ, doanh số.
- **Đánh giá**: MSE, MAE, RMSE, R².

### Classification (Phân loại)
- **Nhị phân**: 2 lớp (0/1, âm/ dương).
- **Đa lớp**: nhiều lớp (ví dụ 10 lớp ảnh CIFAR).
- **Đa nhãn**: mỗi mẫu có thể thuộc nhiều lớp.
- **Đánh giá**: Accuracy, Precision, Recall, F1, AUC-ROC.

### Clustering (Phân cụm)
- Nhóm các mẫu tương tự mà không dùng nhãn.
- **Ví dụ**: K-means, DBSCAN, hierarchical clustering.

### Dimensionality Reduction (Giảm chiều)
- Giảm số chiều dữ liệu, giữ thông tin quan trọng.
- **Ví dụ**: PCA, t-SNE, UMAP.

---

## 1.4. Quy trình học máy điển hình

1. **Thu thập & làm sạch dữ liệu**  
2. **Chia dữ liệu**: Train / Validation / Test (tránh rò rỉ nhãn).  
3. **Chọn mô hình** (linear, tree, neural net, …).  
4. **Chọn hàm loss** và **optimizer**.  
5. **Huấn luyện** trên train set, điều chỉnh hyperparameter theo validation.  
6. **Đánh giá cuối** trên test set (chỉ một lần, không tune theo test).  
7. **Triển khai** (deploy), giám sát (monitoring).

---

## 1.5. Overfitting và Underfitting

| Hiện tượng | Nguyên nhân | Hướng xử lý |
|------------|-------------|-------------|
| **Overfitting** | Mô hình quá phức tạp, học “thuộc” train | Regularization, dropout, early stopping, thêm dữ liệu, giảm độ phức tạp |
| **Underfitting** | Mô hình quá đơn giản | Tăng capacity, train lâu hơn, bớt regularization |

**Bias–Variance trade-off**: Mô hình đơn giản → bias cao, variance thấp; mô hình phức tạp → bias thấp, variance cao. Cần cân bằng (validation set giúp chọn điểm tốt).

---

## 1.6. Một số metric đánh giá

### Phân loại
- **Accuracy** = (TP + TN) / (TP + TN + FP + FN).  
- **Precision** = TP / (TP + FP).  
- **Recall (Sensitivity)** = TP / (TP + FN).  
- **F1** = 2 × (Precision × Recall) / (Precision + Recall).  
- **AUC-ROC**: Diện tích dưới đường ROC (khả năng phân biệt lớp).

### Hồi quy
- **MSE** = mean((y - ŷ)²).  
- **MAE** = mean(|y - ŷ|).  
- **RMSE** = √MSE.  
- **R²** (hệ số xác định): tỷ lệ phương sai được giải thích bởi mô hình.

---

## 1.7. Code mẫu (Python – train/val split, metric)

```python
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score, f1_score, classification_report

# Giả sử X, y đã có
X_train, X_val, y_train, y_val = train_test_split(X, y, test_size=0.2, random_state=42)
model = LogisticRegression(max_iter=1000)
model.fit(X_train, y_train)
y_pred = model.predict(X_val)
print("Accuracy:", accuracy_score(y_val, y_pred))
print("F1:", f1_score(y_val, y_pred, average='weighted'))
print(classification_report(y_val, y_pred))
```

---

## 1.8. Tài liệu tham khảo

- [Machine learning - Wikipedia](https://en.wikipedia.org/wiki/Machine_learning)
- Bishop, *Pattern Recognition and Machine Learning*
- Coursera: Machine Learning (Andrew Ng)
