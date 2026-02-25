# 6. Optimizer và Regularization (Tóm tắt)

## 6.1. Optimizer (Bộ tối ưu)

Mục tiêu: Cập nhật tham số θ để **giảm loss** $L(\theta)$. Công thức chung: $\theta_{t+1} = \theta_t - \eta \cdot g_t$ với $g_t$ hướng cập nhật (gradient hoặc biến thể).

| Optimizer | Cập nhật (ý tưởng) | Ghi chú |
|-----------|---------------------|---------|
| **SGD** | $g_t = \nabla L$ | Đơn giản; dao động khi learning rate lớn |
| **SGD + Momentum** | $v_t = \mu v_{t-1} + \nabla L$; $\theta \leftarrow \theta - \eta v_t$ | Giảm dao động, hội tụ ổn hơn |
| **Nesterov** | Momentum nhưng gradient tính tại $\theta + \mu v$ | Thường hội tụ nhanh hơn momentum thường |
| **AdaGrad** | Scale từng chiều: $s_i \mathrel{+}= g_i^2$; $\theta_i \leftarrow \theta_i - \eta g_i/\sqrt{s_i}$ | Học chậm chiều có gradient lớn; có thể $s$ quá lớn |
| **RMSprop** | $s = \rho s + (1-\rho)g^2$; $\theta \leftarrow \theta - \eta g/\sqrt{s}$ | Giảm vấn đề AdaGrad |
| **Adam** | $m = \beta_1 m + (1-\beta_1)g$, $v = \beta_2 v + (1-\beta_2)g^2$; bias-correct; $\theta \leftarrow \theta - \eta \hat{m}/\sqrt{\hat{v}}$ | Thích nghi từng tham số; phổ biến |
| **AdamW** | Adam + weight decay **tách** khỏi gradient (decoupled) | Thường tốt hơn Adam cho transformer |
| **Lion** | Kết hợp sign(g) và momentum | Ít bộ nhớ, dùng cho mô hình rất lớn |
| **LAMB** | Layer-wise Adaptive Rate (cho BERT lớn) | Scale learning rate theo norm từng layer |

**Learning rate schedule** thường dùng:
- **Warmup**: Tăng LR từ 0 lên peak trong N bước (tránh bùng nổ gradient đầu train).
- **Linear / Cosine decay**: Giảm LR từ peak xuống 0 theo bước (hoặc theo epoch).
- **OneCycle**: Tăng rồi giảm trong một chu kỳ.

---

## 6.2. Regularization (Điều hòa)

Mục đích: **Giảm overfitting**, cải thiện generalization.

| Kỹ thuật | Cách làm |
|----------|----------|
| **L2 (Weight decay)** | Thêm $\lambda \|\theta\|^2$ vào loss; đẩy trọng số về gần 0 |
| **L1** | $\lambda \|\theta\|_1$; có thể cho sparse weights |
| **Dropout** | Train: ngẫu nhiên “tắt” tỷ lệ neuron; test: dùng toàn bộ (scale nếu cần) |
| **DropBlock** | Drop vùng liên tiếp (cho CNN) thay vì từng neuron |
| **Early stopping** | Dừng khi validation loss không cải thiện sau k epoch |
| **Data augmentation** | Xoay, crop, flip, color jitter (ảnh); synonym replace, back-translation (NLP) |
| **Label smoothing** | Thay one-hot bằng phân phối mềm (ε cho lớp sai) |
| **Stochastic Depth** | Ngẫu nhiên bỏ qua một số layer (như dropout cho layer) |
| **Mixup / CutMix** | Trộn ảnh và nhãn: $\tilde{x}=\lambda x_i+(1-\lambda)x_j$, $\tilde{y}=\lambda y_i+(1-\lambda)y_j$ |

---

## 6.3. Code mẫu (PyTorch)

```python
import torch.optim as optim

# Optimizer
opt = optim.AdamW(model.parameters(), lr=1e-4, weight_decay=0.01)

# Scheduler: warmup 500 bước, rồi cosine decay
def get_lr(step, warmup=500, peak=1e-4, total_steps=10000):
    if step < warmup:
        return peak * step / warmup
    progress = (step - warmup) / (total_steps - warmup)
    return peak * 0.5 * (1 + math.cos(math.pi * progress))
# Hoặc dùng torch.optim.lr_scheduler.OneCycleLR / CosineAnnealingLR
```

---

## 6.4. Tài liệu tham khảo

- [Stochastic gradient descent - Wikipedia](https://en.wikipedia.org/wiki/Stochastic_gradient_descent)
- Kingma & Ba, *Adam: A Method for Stochastic Optimization*
- Loshchilov & Hutter, *Decoupled Weight Decay Regularization* (AdamW)
