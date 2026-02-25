# 2. Deep Learning (Học sâu)

## 2.1. Tổng quan

**Deep Learning** là tập con của học máy dựa trên **mạng neural nhiều lớp (deep neural networks)**. Mô hình học biểu diễn đa tầng từ dữ liệu thô (ảnh, văn bản, âm thanh) nhờ **backpropagation** và **gradient descent**.

**Ứng dụng thực tế:** Nhận dạng ảnh (CNN); dịch máy, BERT/GPT (Transformer); nhận dạng giọng nói; xe tự lái; chẩn đoán ảnh y khoa; recommendation.

| Thành phần | Vai trò |
|------------|--------|
| **Layer** | Biến đổi đầu vào (linear + activation) |
| **Loss function** | Đo lỗi dự đoán so với nhãn |
| **Optimizer** | Cập nhật tham số theo gradient |
| **Backpropagation** | Tính gradient theo quy tắc chain |

---

## 2.2. Kiến trúc cơ bản: MLP (Multi-Layer Perceptron)

- **Input layer** → **Hidden layers** (kích thước ẩn) → **Output layer**.
- Mỗi layer: **y = activation(Wx + b)**.  
- **Backprop**: Tính ∂L/∂W, ∂L/∂b từ output ngược về input (chain rule).

---

## 2.3. Hàm kích hoạt (Activation)

| Hàm | Công thức | Đặc điểm |
|-----|-----------|----------|
| **ReLU** | max(0, x) | Đơn giản, tránh vanishing gradient; không zero-centered |
| **GELU** | x Φ(x) (gần với x·σ(1.7x)) | Dùng nhiều trong Transformer |
| **Sigmoid** | 1/(1+e⁻ˣ) | Đầu ra (0,1); dễ bão hòa |
| **Tanh** | (eˣ−e⁻ˣ)/(eˣ+e⁻ˣ) | Zero-centered; vẫn bão hòa |
| **Softmax** | e^xᵢ / Σⱼ e^xⱼ | Dùng ở output phân loại đa lớp |

**Lưu ý**: Ẩn layer thường dùng ReLU/GELU; output phân loại đa lớp dùng softmax; output nhị phân có thể dùng sigmoid.

---

## 2.4. Chuẩn hóa (Normalization)

| Loại | Áp dụng | Công thức (ý tưởng) |
|------|---------|---------------------|
| **Batch Norm** | Theo batch (channel) | Chuẩn hóa mean=0, var=1 trên từng channel theo batch |
| **Layer Norm** | Theo từng mẫu (toàn bộ feature) | Mean/var trên từng mẫu, dùng nhiều trong Transformer |
| **Instance Norm** | Từng mẫu từng channel | Ảnh style transfer |
| **Group Norm** | Chia channel thành group | Thay Batch Norm khi batch nhỏ |

---

## 2.5. Regularization

- **Dropout**: Trong lúc train, ngẫu nhiên “tắt” một tỷ lệ neuron (ví dụ 0.5) để tránh overfitting.
- **Weight decay (L2)**: Penalty λ‖W‖² thêm vào loss → đẩy trọng số về gần 0.
- **Early stopping**: Dừng train khi validation loss không cải thiện.
- **Data augmentation**: Xoay, crop, flip, thay đổi màu (ảnh); thay thế từ (NLP) để tăng dữ liệu ảo.

---

## 2.6. Optimizer (Tối ưu hóa)

| Optimizer | Công thức / ý tưởng | Ghi chú |
|-----------|----------------------|---------|
| **SGD** | θ ← θ − η∇L | Đơn giản; thường thêm momentum |
| **SGD + Momentum** | v ← μv + ∇L; θ ← θ − ηv | Giảm dao động, hội tụ nhanh hơn |
| **Adam** | Cập nhật m (momentum) và v (bình phương gradient), rồi θ | Thích nghi learning rate từng tham số |
| **AdamW** | Adam + weight decay tách riêng (decoupled) | Thường tốt hơn Adam cho transformer/CNN |
| **Lion** | Kết hợp momentum và sign(gradient) | Ít bộ nhớ, phù hợp scale lớn |

Learning rate có thể dùng **schedule**: Step decay, Cosine annealing, Warmup + decay (phổ biến trong transformer).

---

## 2.7. Kiến trúc mạng thường gặp

- **CNN (Convolutional NN)**: Ảnh – convolution, pooling, sau đó có thể flatten + MLP hoặc global pooling.
- **RNN / LSTM**: Chuỗi thời gian hoặc văn bản (trước khi có transformer).
- **Transformer**: Dựa trên cơ chế attention (xem file Transformer); dùng cho NLP, vision (ViT), đa modal.

---

## 2.8. Code mẫu (PyTorch – MLP đơn giản)

```python
import torch
import torch.nn as nn

class MLP(nn.Module):
    def __init__(self, input_dim=784, hidden=[256, 128], num_classes=10, dropout=0.2):
        super().__init__()
        layers = []
        prev = input_dim
        for h in hidden:
            layers.extend([nn.Linear(prev, h), nn.ReLU(), nn.Dropout(dropout)])
            prev = h
        self.features = nn.Sequential(*layers)
        self.classifier = nn.Linear(prev, num_classes)

    def forward(self, x):
        x = x.view(x.size(0), -1)
        x = self.features(x)
        return self.classifier(x)

# Training loop (minimal)
# model = MLP()
# opt = torch.optim.Adam(model.parameters(), lr=1e-3)
# for x, y in loader:
#     opt.zero_grad()
#     loss = F.cross_entropy(model(x), y)
#     loss.backward()
#     opt.step()
```

---

## 2.9. Tài liệu tham khảo

- [Deep learning - Wikipedia](https://en.wikipedia.org/wiki/Deep_learning)
- Goodfellow et al., *Deep Learning* (MIT Press)
- PyTorch / TensorFlow documentation
