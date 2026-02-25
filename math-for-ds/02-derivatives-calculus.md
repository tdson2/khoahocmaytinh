# 2. Đạo hàm và Giải tích (Derivatives & Calculus)

Đạo hàm và giải tích là **cốt lõi** của tối ưu trong ML/AI: gradient descent, backpropagation, và mọi optimizer đều dựa trên việc tính gradient (đạo hàm) của hàm loss theo tham số. Chuyên mục này tập trung **ứng dụng cho Data Science và AI**.

---

## 2.1. Đạo hàm một biến

### Định nghĩa và ý nghĩa

- **Đạo hàm** của \(f(x)\) tại \(x_0\):  
  \(f'(x_0) = \lim_{h \to 0} \frac{f(x_0 + h) - f(x_0)}{h}\).

- **Ý nghĩa hình học**: Hệ số góc (slope) của tiếp tuyến với đồ thị \(y = f(x)\) tại điểm \((x_0, f(x_0))\).

- **Ý nghĩa trong tối ưu**:  
  - \(f'(x) > 0\) → tăng \(x\) thì \(f\) tăng.  
  - \(f'(x) < 0\) → tăng \(x\) thì \(f\) giảm.  
  - Cực tiểu địa phương thường tại điểm \(f'(x) = 0\) (điều kiện cần).

### Một số đạo hàm cơ bản

| Hàm \(f(x)\) | Đạo hàm \(f'(x)\) |
|--------------|-------------------|
| \(x^n\) | \(n x^{n-1}\) |
| \(e^x\) | \(e^x\) |
| \(\ln x\) | \(1/x\) |
| \(\sin x\) | \(\cos x\) |
| \(\cos x\) | \(-\sin x\) |
| \(\tan x\) | \(1/\cos^2 x\) |
| \(a^x\) | \(a^x \ln a\) |

### Quy tắc tính đạo hàm

- **Tổng/hiệu**: \((u \pm v)' = u' \pm v'\).
- **Tích**: \((uv)' = u'v + uv'\).
- **Thương**: \(\left(\frac{u}{v}\right)' = \frac{u'v - uv'}{v^2}\).
- **Hàm hợp (chain rule)**: \(\frac{d}{dx}f(g(x)) = f'(g(x))\, g'(x)\).  
  **Đây là nền tảng của backpropagation**: đạo hàm loss theo từng layer được lan truyền ngược bằng chain rule.

---

## 2.2. Đạo hàm nhiều biến và Gradient

### Đạo hàm riêng

- Hàm \(f(x_1, x_2, \ldots, x_n)\): **đạo hàm riêng** theo \(x_i\) là đạo hàm khi coi mọi biến khác là hằng số:  
  \(\frac{\partial f}{\partial x_i}\).

### Gradient (vector đạo hàm)

- **Gradient** của \(f\) là vector các đạo hàm riêng:  
  \(\nabla f = \left( \frac{\partial f}{\partial x_1}, \ldots, \frac{\partial f}{\partial x_n} \right)^T\).

- **Ý nghĩa**: Hướng tăng nhanh nhất của \(f\); ngược lại, \(-\nabla f\) là hướng **giảm nhanh nhất** → cơ sở của gradient descent.

### Gradient Descent (cơ bản)

- Cập nhật tham số: \(\theta_{t+1} = \theta_t - \eta \cdot \nabla L(\theta_t)\).  
- \(\eta\): learning rate.  
- Trong ML: \(L\) là loss; \(\theta\) là vector tham số (weights, biases).  
- **Stochastic GD (SGD)**: Dùng gradient ước lượng trên một mini-batch thay vì toàn bộ dữ liệu → nhanh, có nhiễu, giúp thoát khỏi minimum kém.

---

## 2.3. Đạo hàm ma trận (Matrix Calculus)

Trong neural network, tham số thường là **ma trận** (ví dụ \(\mathbf{W}\)) và **vector** (ví dụ \(\mathbf{b}\)). Cần quy ước đạo hàm theo vector/ma trận.

### Quy ước thường dùng (numerator layout)

- \(f\) vô hướng, \(\mathbf{x}\) vector: \(\frac{\partial f}{\partial \mathbf{x}}\) là **vector cột** cùng shape với \(\mathbf{x}\).
- \(\frac{\partial \mathbf{y}}{\partial \mathbf{x}}\): ma trận Jacobi, phần tử \((i,j)\) là \(\frac{\partial y_i}{\partial x_j}\).

### Một số công thức hay dùng trong ML

| Biểu thức | Đạo hàm (gradient / Jacobian) |
|-----------|-------------------------------|
| \(\mathbf{a}^T \mathbf{x}\) | \(\nabla_{\mathbf{x}} = \mathbf{a}\) |
| \(\mathbf{x}^T \mathbf{A} \mathbf{x}\) | \(\nabla_{\mathbf{x}} = (\mathbf{A} + \mathbf{A}^T)\mathbf{x}\); nếu \(\mathbf{A}\) đối xứng: \(2\mathbf{A}\mathbf{x}\) |
| \(\|\mathbf{x}\|^2 = \mathbf{x}^T \mathbf{x}\) | \(\nabla_{\mathbf{x}} = 2\mathbf{x}\) |
| \(\mathbf{W}\mathbf{x} + \mathbf{b}\) (theo \(\mathbf{W}\), \(\mathbf{x}\), \(\mathbf{b}\)) | \(\frac{\partial}{\partial \mathbf{x}} = \mathbf{W}^T\); \(\frac{\partial}{\partial \mathbf{W}} = \ldots\) (shape phù hợp cho backprop) |

**Trong backprop**: Loss \(L\) vô hướng; cần \(\frac{\partial L}{\partial \mathbf{W}}\) và \(\frac{\partial L}{\partial \mathbf{b}}\) để cập nhật từng layer. Chain rule: \(\frac{\partial L}{\partial \mathbf{W}} = \frac{\partial L}{\partial \mathbf{z}} \cdot \frac{\partial \mathbf{z}}{\partial \mathbf{W}}\) với \(\mathbf{z} = \mathbf{W}\mathbf{x} + \mathbf{b}\).

---

## 2.4. Chain Rule và Backpropagation

- **Chain rule nhiều biến**: Nếu \(L = f(g_1(\theta), \ldots, g_k(\theta))\) thì  
  \(\frac{\partial L}{\partial \theta} = \sum_i \frac{\partial L}{\partial g_i} \frac{\partial g_i}{\partial \theta}\).

- **Backpropagation**:  
  1. **Forward pass**: Tính từ input → output từng layer, lưu giá trị trung gian (activation).  
  2. **Backward pass**: Tính \(\frac{\partial L}{\partial \text{activation}}\) từ output ngược về input; tại mỗi layer dùng chain rule để có \(\frac{\partial L}{\partial \mathbf{W}}\) và \(\frac{\partial L}{\partial \mathbf{b}}\).  
  Các framework (PyTorch, TensorFlow) tự động tính gradient nhờ **automatic differentiation (autograd)**.

- **Vanishing / exploding gradient**: Đạo hàm qua nhiều lớp tích lũy (tích của nhiều Jacobi). Nếu trị riêng < 1 → gradient tiến về 0 (vanishing); > 1 → bùng nổ (exploding). Giải pháp: ReLU, Batch/Layer Norm, residual connection, kiểm soát initialization.

---

## 2.5. Đạo hàm các hàm loss và activation thường gặp

### Hàm kích hoạt (activation)

| Hàm | Công thức | Đạo hàm (theo input) |
|-----|-----------|----------------------|
| **Sigmoid** | \(\sigma(x) = \frac{1}{1+e^{-x}}\) | \(\sigma'(x) = \sigma(x)(1-\sigma(x))\) |
| **Tanh** | \(\tanh(x)\) | \(1 - \tanh^2(x)\) |
| **ReLU** | \(\max(0, x)\) | \(1\) nếu \(x>0\), \(0\) nếu \(x<0\) |
| **Softmax** | \(p_i = \frac{e^{z_i}}{\sum_j e^{z_j}}\) | \(\frac{\partial p_i}{\partial z_j} = p_i(\delta_{ij} - p_j)\) (Jacobian) |

### Loss thường dùng

| Loss | Công thức | Đạo hàm (gradient theo \(\hat{y}\) hoặc logit) |
|------|-----------|------------------------------------------------|
| **MSE** | \(\frac{1}{n}\sum (y - \hat{y})^2\) | \(\frac{\partial L}{\partial \hat{y}} \propto (\hat{y} - y)\) |
| **Cross-entropy (nhị phân)** | \(-[y\ln\hat{p} + (1-y)\ln(1-\hat{p})]\) | \(\hat{p} - y\) (khi output là xác suất sigmoid) |
| **Cross-entropy (đa lớp)** | \(-\sum_c y_c \ln p_c\) với softmax | \(\frac{\partial L}{\partial \mathbf{z}} = \mathbf{p} - \mathbf{y}\) (one-hot \(\mathbf{y}\)) |

Công thức **softmax + CE**: gradient đặc biệt gọn \(\mathbf{p} - \mathbf{y}\), nên trong phân loại đa lớp thường gộp softmax vào trong hàm loss để tính gradient ổn định.

---

## 2.6. Đạo hàm bậc hai: Hessian và tối ưu

- **Hessian**: Ma trận đạo hàm bậc hai \(H_{ij} = \frac{\partial^2 f}{\partial x_i \partial x_j}\). Đối xứng nếu \(f\) khả vi liên tục hai lần.

- **Ý nghĩa**:  
  - **Cực tiểu địa phương**: \(\nabla f = 0\) và Hessian **xác định dương**.  
  - **Newton method**: Cập nhật \(\theta_{t+1} = \theta_t - H^{-1} \nabla L\). Hội tụ nhanh gần minimum nhưng tốn chi phí tính và lưu \(H\); trong deep learning thường dùng các phương pháp giống Newton nhưng gần đúng (Adam, quasi-Newton như L-BFGS khi có thể).

---

## 2.7. Ứng dụng tóm tắt trong Data Science & AI

| Chủ đề | Vai trò của đạo hàm / giải tích |
|--------|---------------------------------|
| **Gradient descent** | \(-\nabla L\) là hướng giảm loss nhanh nhất |
| **Backprop** | Chain rule → gradient theo từng trọng số từ output về input |
| **Optimizer** | SGD, Adam, AdamW đều dùng gradient (và momentum/second moment) |
| **Regularization** | L2: gradient của \(\|\theta\|^2\) là \(2\theta\); L1: subgradient của \(\|\theta\|_1\) |
| **GAN** | Cân bằng min-max; gradient của discriminator/generator |
| **VAE** | Reparameterization trick để đạo hàm qua biến ngẫu nhiên |
| **Meta-learning (MAML)** | Đạo hàm theo meta-parameter qua nhiều bước gradient nội bộ |
| **Neural ODE** | Đạo hàm theo thời gian; adjoint method để backprop qua ODE solver |

---

## 2.8. Code mẫu (PyTorch: gradient thủ công và autograd)

```python
import torch

# Autograd: PyTorch tự tính gradient
x = torch.tensor([1.0, 2.0, 3.0], requires_grad=True)
y = x.pow(2).sum()  # x1^2 + x2^2 + x3^2
y.backward()
print("Gradient of sum(x^2) w.r.t. x:", x.grad)  # [2, 4, 6]

# Gradient của loss theo tham số mô hình
model = torch.nn.Linear(3, 2)
x = torch.randn(1, 3)
loss = model(x).sum()
loss.backward()
print("Gradient of weight:\n", model.weight.grad)
print("Gradient of bias:", model.bias.grad)
```

---

## 2.9. Tài liệu tham khảo

- Stewart, *Calculus* (chương đạo hàm, đạo hàm riêng)
- Goodfellow et al., *Deep Learning* (Chương 6: Optimization, Chương 8: Backpropagation)
- Matrix calculus: [The Matrix Cookbook](https://www.math.uwaterloo.ca/~hwolkowi/matrixcookbook.pdf)
