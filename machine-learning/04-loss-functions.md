# 4. Các loại Hàm Loss (Loss Functions)

## 4.1. Tổng quan

**Hàm loss (hàm mất mát)** đo độ “sai lệch” giữa dự đoán của mô hình và giá trị đích. Mục tiêu huấn luyện là **cực tiểu hóa** loss trên tập train. Chọn loss phù hợp với **nhiệm vụ** (hồi quy, phân loại nhị phân/đa lớp, ranking, embedding) và **phân bố dữ liệu** (cân bằng lớp, nhiễu, ngoại lai).

---

**Ứng dụng thực tế:** MSE/MAE cho hồi quy giá, nhiệt độ; Cross-entropy cho phân loại ảnh, spam; focal loss khi lớp mất cân bằng; contrastive loss cho embedding, retrieval.

---

## 4.2. Loss cho Hồi quy (Regression)

| Loss | Công thức | Đặc điểm |
|------|-----------|----------|
| **MSE** (L2) | $\frac{1}{n}\sum_i (y_i - \hat{y}_i)^2$ | Phạt mạnh lỗi lớn; nhạy outlier |
| **MAE** (L1) | $\frac{1}{n}\sum_i \|y_i - \hat{y}_i\|$ | Ít nhạy outlier; gradient không đổi |
| **Huber** | $\frac{1}{2}(y-\hat{y})^2$ nếu \|e\|≤δ; nếu không: δ(\|e\|−δ/2) | Kết hợp MSE (nhỏ) và MAE (lớn), ít nhạy outlier |
| **Log-Cosh** | $\sum_i \log(\cosh(\hat{y}_i - y_i))$ | Gần MSE khi lỗi nhỏ, gần MAE khi lỗi lớn |
| **Quantile** | $\sum_i \rho_\tau(y_i - \hat{y}_i)$, $\rho_\tau(e)=\tau e$ nếu e≥0, $(\tau-1)e$ nếu e<0 | Ước lượng quantile (median τ=0.5) |

---

## 4.3. Loss cho Phân loại

### Phân loại nhị phân (Binary Classification)

| Loss | Công thức / Mô tả | Ghi chú |
|------|-------------------|---------|
| **Binary Cross-Entropy (BCE)** | $-\frac{1}{n}\sum_i [y_i \log p_i + (1-y_i)\log(1-p_i)]$ | p = xác suất lớp 1; dùng với sigmoid |
| **BCE with logits** | Kết hợp sigmoid + BCE trong một biểu thức ổn định số | Tránh overflow; PyTorch: `F.binary_cross_entropy_with_logits` |

### Phân loại đa lớp (Multi-class)

| Loss | Công thức / Mô tả | Ghi chú |
|------|-------------------|---------|
| **Cross-Entropy (CE)** | $-\sum_i y_i \log p_i$ (one-hot y) | p từ softmax; dùng khi mỗi mẫu thuộc đúng 1 lớp |
| **Sparse CE** | Cùng công thức, y là chỉ số lớp (integer) | Tránh one-hot khi số lớp lớn |
| **Focal Loss** | $-\alpha_t(1-p_t)^\gamma \log p_t$ | Giảm trọng số mẫu dễ (p_t lớn); tốt cho mất cân bằng lớp (object detection) |
| **Label Smoothing** | Thay one-hot (0,1) bằng (ε/(K-1), 1-ε) | Tránh overconfident; thường ε≈0.1 |

### Đa nhãn (Multi-label)

- **BCE** trên từng lớp: coi mỗi lớp là bài nhị phân độc lập; tổng loss = tổng BCE từng lớp (hoặc trung bình).

---

## 4.4. Loss cho Ranking & Metric Learning

| Loss | Mô tả | Ứng dụng |
|------|--------|----------|
| **Triplet Loss** | $\max(0, d(a,p) - d(a,n) + margin)$ | a=anchor, p=positive, n=negative; kéo a gần p, đẩy xa n |
| **Contrastive Loss** | Cặp (x₁,x₂): nếu cùng lớp thì giảm d(x₁,x₂); khác lớp thì tăng (hoặc margin) | Siamese network, embedding |
| **InfoNCE / NT-Xent** | $-\log \frac{\exp(sim(z_i,z_j)/\tau)}{\sum_k \exp(sim(z_i,z_k)/\tau)}$ | SimCLR, MoCo; nhiệt độ τ |
| **ArcFace / CosFace** | Góc hoặc cosine có margin giữa embedding và trọng số lớp | Face recognition, classification có margin |

---

## 4.5. Loss cho Phân phối & Generative

| Loss | Mô tả | Dùng trong |
|------|--------|------------|
| **KL Divergence** | $D_{KL}(P\|Q) = \sum P\log(P/Q)$ | So sánh phân phối; variational inference |
| **JS Divergence** | $\frac{1}{2}D_{KL}(P\|M) + \frac{1}{2}D_{KL}(Q\|M)$, M=(P+Q)/2 | Một số GAN |
| **Wasserstein** | Khoảng cách vận chuyển tối ưu giữa P và Q | WGAN; ổn định hơn GAN thường |
| **Reconstruction (MSE/BCE)** | Lỗi tái tạo (ảnh/vector) | Autoencoder, VAE (phần reconstruction) |
| **VAE total** | Reconstruction + β·KL(q(z|x) \| p(z)) | VAE; β cho β-VAE |

---

## 4.6. Loss đặc biệt & Kết hợp

| Loss | Mô tả |
|------|--------|
| **Distillation** | KL(softmax(z_T/T) \| softmax(z_s/T)) giữa teacher và student (xem file Distillation) |
| **Dice Loss** | $1 - \frac{2\lvert A\cap B\rvert}{\lvert A\rvert+\lvert B\rvert}$ | Segmentation; cân bằng foreground/background |
| **IoU Loss** | 1 − IoU(pred, target) | Detection / segmentation |
| **Focal + Dice** | Kết hợp Focal và Dice | Segmentation mất cân bằng |
| **Multi-task** | $\sum_k \lambda_k L_k$ | Nhiều đầu ra (classification + regression + segmentation) |

---

## 4.7. Code mẫu (PyTorch)

```python
import torch
import torch.nn as nn
import torch.nn.functional as F

# Regression
mse = nn.MSELoss()
mae = nn.L1Loss()
huber = nn.HuberLoss(delta=1.0)

# Classification
bce = nn.BCELoss()           # input đã sigmoid
bce_logits = nn.BCEWithLogitsLoss()
ce = nn.CrossEntropyLoss()   # logits, target class index
ce_smooth = nn.CrossEntropyLoss(label_smoothing=0.1)

# Focal (minimal)
def focal_loss(logits, targets, gamma=2.0, alpha=0.25):
    ce = F.cross_entropy(logits, targets, reduction='none')
    pt = torch.exp(-ce)
    return (alpha * (1 - pt) ** gamma * ce).mean()

# Triplet (minimal)
def triplet_loss(anchor, positive, negative, margin=0.2):
    d_pos = F.pairwise_distance(anchor, positive)
    d_neg = F.pairwise_distance(anchor, negative)
    return F.relu(d_pos - d_neg + margin).mean()
```

---

## 4.8. Tài liệu tham khảo

- [Loss functions - Wikipedia](https://en.wikipedia.org/wiki/Loss_function)
- PyTorch: `torch.nn` loss layers
- Lin et al., *Focal Loss for Dense Object Detection* (RetinaNet)
- Chen et al., *A Simple Framework for Contrastive Learning* (SimCLR)
