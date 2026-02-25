# 1. Xác suất và Thống kê (Probability & Statistics)

Toán xác suất và thống kê là **nền tảng** của Data Science và AI: từ mô hình hóa bất định, ước lượng tham số, kiểm định giả thuyết, đến Bayesian inference, A/B test và đánh giá mô hình.

---

## 1.1. Xác suất cơ bản

### Biến ngẫu nhiên và phân phối

| Khái niệm | Mô tả |
|-----------|--------|
| **Biến ngẫu nhiên (RV)** | Đại lượng nhận giá trị ngẫu nhiên theo một quy luật (phân phối) |
| **Phân phối rời rạc** | X nhận giá trị đếm được; P(X = x) với ∑ P(X=x) = 1 |
| **Phân phối liên tục** | X nhận giá trị trong ℝ (hoặc đoạn); mật độ f(x) ≥ 0, ∫f(x)dx = 1 |
| **Kỳ vọng E[X]** | Trung bình theo xác suất: ∑ x·P(X=x) hoặc ∫ x·f(x)dx |
| **Phương sai Var(X)** | E[(X − E[X])²] = E[X²] − (E[X])²; độ lệch chuẩn σ = √Var(X) |

### Quy tắc quan trọng

- **Xác suất có điều kiện**: P(A|B) = P(A∩B) / P(B).
- **Công thức Bayes**: P(A|B) = P(B|A)·P(A) / P(B) — nền tảng cho Bayesian ML, spam filter, chẩn đoán.
- **Độc lập**: A, B độc lập ⇔ P(A∩B) = P(A)P(B) ⇔ P(A|B) = P(A).
- **Luật xác suất toàn phần**: P(B) = ∑ᵢ P(B|Aᵢ)P(Aᵢ) khi {Aᵢ} là phân hoạch.

---

## 1.2. Một số phân phối thường gặp trong Data Science & AI

| Phân phối | Ký hiệu / Tham số | Ứng dụng |
|-----------|--------------------|----------|
| **Bernoulli** | Ber(p), p = P(X=1) | Nhãn nhị phân (0/1), một lần thử |
| **Binomial** | Bin(n, p) | Số lần “thành công” trong n lần thử độc lập |
| **Categorical / Multinomial** | Cat(p₁,…,pₖ) | Phân loại đa lớp, one-hot, softmax |
| **Gaussian (Chuẩn)** | N(μ, σ²) | Lỗi đo, nhiễu, prior/sufficient statistics trong nhiều mô hình |
| **Laplace** | Lap(μ, b) | Nhiễu, regularization L1, robust regression |
| **Exponential** | Exp(λ) | Thời gian chờ, độ tin cậy |
| **Poisson** | Pois(λ) | Số sự kiện trong khoảng thời gian (số từ, số click) |
| **Beta** | Beta(α, β) | Prior cho xác suất (A/B test, Bayesian update) |
| **Dirichlet** | Dir(α₁,…,αₖ) | Prior cho phân phối xác suất trên k lớp (LDA, topic model) |

**Ví dụ trong AI**:  
- **Softmax** tạo phân phối xác suất trên các lớp (Categorical).  
- **Gaussian noise** thêm vào dữ liệu hoặc latent trong VAE.  
- **Beta** dùng làm prior cho tỷ lệ click (CTR) trong bandit/Bayesian A/B.

---

## 1.3. Thống kê mô tả và ước lượng

### Thống kê mô tả (trên mẫu)

| Đại lượng | Công thức (mẫu) | Ý nghĩa |
|-----------|------------------|---------|
| **Trung bình mẫu** | x̄ = (1/n)∑xᵢ | Ước lượng kỳ vọng |
| **Phương sai mẫu** | s² = (1/(n−1))∑(xᵢ−x̄)² | Ước lượng Var (unbiased) |
| **Độ lệch chuẩn** | s = √s² | Cùng đơn vị với dữ liệu |
| **Trung vị** | Giá trị giữa khi sắp xếp | Robust với outlier |
| **Phần tứ phân vị (Q1, Q3)** | Chia 25%, 75% | Box plot, IQR = Q3−Q1 |
| **Hệ số tương quan Pearson** | r = Cov(X,Y)/(σₓσᵧ) | Đo quan hệ tuyến tính [-1, 1] |

### Ước lượng điểm và khoảng

- **Ước lượng hợp lý tối đa (MLE)**: chọn tham số θ sao cho khả năng xảy ra dữ liệu D là lớn nhất: θ_MLE = argmax P(D|θ). Dùng rộng rãi trong ML (cross-entropy tương đương MLE cho phân loại).
- **Khoảng tin cậy (CI)**: khoảng [L, U] sao cho với xác suất (ví dụ 95%), tham số thật nằm trong [L, U]. Trong ML dùng để báo cáo độ không chắc chắn (ví dụ bootstrap).

---

## 1.4. Kiểm định giả thuyết

- **H₀ (giả thuyết không)** vs **H₁ (đối thuyết)**.  
- **p-value**: Xác suất (khi H₀ đúng) quan sát được dữ liệu “cực đoan” ít nhất như đã thấy.  
- **α (mức ý nghĩa)**: Ngưỡng bác bỏ H₀ (thường 0.05). Nếu p-value < α → bác bỏ H₀.  
- **Lỗi loại I**: Bác bỏ H₀ khi H₀ đúng. **Lỗi loại II**: Không bác bỏ H₀ khi H₁ đúng.

**Ứng dụng**: A/B test (so sánh tỷ lệ chuyển đổi, CTR), so sánh độ chính xác hai mô hình, kiểm tra tính chuẩn của phần dư.

---

## 1.5. Bayes và ước lượng Bayesian

- **Công thức Bayes cho tham số**: P(θ|D) ∝ P(D|θ)·P(θ).  
  - **Prior** P(θ): niềm tin trước về θ.  
  - **Likelihood** P(D|θ): mô hình dữ liệu.  
  - **Posterior** P(θ|D): niềm tin sau khi quan sát D.

- **Ước lượng MAP (Maximum A Posteriori)**: θ_MAP = argmax P(θ|D) = argmax P(D|θ)P(θ). Thêm prior tương đương regularization (ví dụ Gaussian prior ↔ L2).

- **Ứng dụng trong AI**: Bayesian neural network, Gaussian Process, Bayesian optimization (hyperparameter), Thompson Sampling (bandit), topic model (LDA).

---

## 1.6. Tương quan, Covariance, và PCA

- **Covariance**: Cov(X,Y) = E[(X−E[X])(Y−E[Y])]. Đo quan hệ tuyến tính.  
- **Ma trận hiệp phương sai**: Σᵢⱼ = Cov(Xᵢ, Xⱼ). Đối xứng, nửa xác định dương.  
- **PCA (Principal Component Analysis)**: Tìm các hướng phương sai lớn nhất (eigenvector của ma trận hiệp phương sai). Dùng giảm chiều, nhiễu, visualization. Nền tảng cho nhiều kỹ thuật trong ML.

---

## 1.7. Ứng dụng trực tiếp trong Data Science & AI

| Chủ đề | Vai trò |
|--------|---------|
| **Loss & MLE** | Cross-entropy ↔ MLE cho phân phối Categorical; MSE ↔ MLE cho Gaussian noise |
| **Calibration** | Xác suất dự đoán cần phản ánh tần suất thực (reliability diagram, ECE) |
| **Uncertainty** | Bayesian NN, dropout as Bayesian approx., ensemble → uncertainty estimate |
| **A/B test** | So sánh tỷ lệ (z-test, χ²), so sánh mean (t-test), cỡ mẫu, p-value |
| **Evaluation** | Confidence interval cho accuracy/F1; bootstrap; permutation test |
| **Generative model** | VAE, GMM, LDA — đều dựa trên mô hình xác suất |
| **Information theory** | Entropy, KL divergence (distillation, variational inference), mutual information |

---

## 1.8. Code mẫu (Python)

```python
import numpy as np
from scipy import stats

# Mẫu từ phân phối chuẩn, ước lượng mean và CI
np.random.seed(42)
x = np.random.normal(loc=5, scale=2, size=100)
mean_x = np.mean(x)
se = np.std(x, ddof=1) / np.sqrt(len(x))
ci_95 = stats.t.interval(0.95, len(x)-1, loc=mean_x, scale=se)
print(f"Mean: {mean_x:.3f}, 95% CI: {ci_95}")

# Kiểm định t (so sánh mean với 0)
t_stat, p_value = stats.ttest_1samp(x, 0)
print(f"t-statistic: {t_stat:.3f}, p-value: {p_value:.4f}")

# Tương quan và covariance
y = 2 * x + np.random.normal(0, 1, size=100)
print("Correlation:", np.corrcoef(x, y)[0, 1])
print("Covariance matrix:\n", np.cov(x, y))
```

---

## 1.9. Tài liệu tham khảo

- Wasserman, *All of Statistics*
- Bishop, *Pattern Recognition and Machine Learning* (Chương 1–2, 8–10)
- DeGroot & Schervish, *Probability and Statistics*
