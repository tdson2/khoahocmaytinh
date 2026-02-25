# 3. Transformer và Attention

## 3.1. Tổng quan

**Transformer** là kiến trúc dựa hoàn toàn trên **cơ chế attention** (không dùng recurrence hay convolution theo chuỗi). Nền tảng cho BERT, GPT, ViT và nhiều mô hình đa modal. Input là tập **token** (từ, patch ảnh); mỗi token tương tác với mọi token khác qua **self-attention**.

**Ứng dụng thực tế:** BERT, GPT (NLP); ViT (ảnh); mô hình đa modal (CLIP, Flamingo); dịch máy; tóm tắt, hỏi đáp, sinh văn bản.

| Thành phần chính | Chức năng |
|------------------|-----------|
| **Self-Attention** | Mỗi token “hỏi” tất cả token khác để tổng hợp ngữ cảnh |
| **Multi-Head Attention** | Nhiều head attention song song, concat rồi chiếu tuyến tính |
| **Positional Encoding** | Đưa thông tin vị trí vào (vì không có RNN) |
| **Feed-Forward** | MLP 2 lớp sau mỗi block attention |
| **Layer Norm** | Chuẩn hóa trước/sau từng sub-layer |

---

## 3.2. Scaled Dot-Product Attention

Với **Query Q**, **Key K**, **Value V** (đều từ cùng tập token qua phép chiếu tuyến tính):

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^\top}{\sqrt{d_k}}\right) V$$

- **Q, K, V**: shape (batch, num_heads, seq_len, d_k).  
- **d_k**: chiều của key (dùng √d_k để scale, tránh softmax quá nhọn).  
- **Softmax** theo chiều key → trọng số cho từng vị trí; nhân với V để lấy tổng hợp theo value.

**Mask (optional)**: Che các vị trí không được “nhìn” (ví dụ token tương lai trong decoder) bằng cách cộng −∞ trước softmax.

---

## 3.3. Multi-Head Attention

- Tạo nhiều bộ (Qᵢ, Kᵢ, Vᵢ) từ cùng input (mỗi head một chiều nhỏ hơn).
- Mỗi head tính Scaled Dot-Product Attention.
- **Concat** các đầu ra rồi nhân với ma trận **W^O** (output projection).

$$\text{MultiHead}(Q,K,V) = \text{Concat}(\text{head}_1,\dots,\text{head}_h) W^O$$

- Cho phép mô hình học nhiều kiểu quan hệ (cú pháp, tham chiếu xa, …) song song.

---

## 3.4. Transformer Encoder

- **Input**: Embedding token + positional encoding.
- **N block** lặp lại:
  - **Multi-Head Self-Attention** (+ residual, Layer Norm).
  - **Feed-Forward** (2 linear với activation ở giữa) (+ residual, Layer Norm).
- Đầu ra encoder: biểu diễn theo từng vị trí (dùng cho BERT, phân loại sequence, v.v.).

---

## 3.5. Transformer Decoder (cho sinh chuỗi)

- **Masked Self-Attention**: Mỗi vị trí chỉ attend vào các vị trí trước đó (mask phần tương lai).
- **Cross-Attention**: Query từ decoder, Key/Value từ encoder (để “hỏi” nguồn).
- **Feed-Forward** + residual + Layer Norm giống encoder.

Kiến trúc **Encoder–Decoder** (như mô hình dịch máy gốc): encoder xử lý nguồn, decoder sinh đích từng bước với cross-attention vào encoder.

---

## 3.6. Positional Encoding

- Transformer không có thứ tự nội tại; cần bổ sung vị trí.
- **Sinusoidal** (gốc): PE(pos, 2i) = sin(pos/10000^(2i/d)), PE(pos, 2i+1) = cos(...).
- **Learned**: embedding vị trí học được (thường dùng trong BERT, GPT).

---

## 3.7. Các biến thể phổ biến

| Mô hình | Kiến trúc | Ứng dụng |
|---------|-----------|----------|
| **BERT** | Chỉ encoder, masked LM + NSP | Pre-training NLP, phân loại, NER, QA |
| **GPT** | Chỉ decoder (masked self-attention) | Sinh văn bản, completion |
| **T5** | Encoder–decoder | Dịch, tóm tắt, QA dạng “text-to-text” |
| **ViT** | Transformer trên patch ảnh | Phân loại ảnh, backbone vision |
| **DETR** | Encoder–decoder + object queries | Object detection |

---

## 3.8. Code mẫu (PyTorch – Self-Attention đơn giản)

```python
import torch
import torch.nn as nn
import math

class MultiHeadAttention(nn.Module):
    def __init__(self, d_model, num_heads, dropout=0.1):
        super().__init__()
        assert d_model % num_heads == 0
        self.d_k = d_model // num_heads
        self.num_heads = num_heads
        self.W_q = nn.Linear(d_model, d_model)
        self.W_k = nn.Linear(d_model, d_model)
        self.W_v = nn.Linear(d_model, d_model)
        self.W_o = nn.Linear(d_model, d_model)
        self.dropout = nn.Dropout(dropout)

    def forward(self, x, mask=None):
        B, T, C = x.shape
        Q = self.W_q(x).view(B, T, self.num_heads, self.d_k).transpose(1, 2)
        K = self.W_k(x).view(B, T, self.num_heads, self.d_k).transpose(1, 2)
        V = self.W_v(x).view(B, T, self.num_heads, self.d_k).transpose(1, 2)
        scores = torch.matmul(Q, K.transpose(-2, -1)) / math.sqrt(self.d_k)
        if mask is not None:
            scores = scores.masked_fill(mask == 0, float('-inf'))
        attn = self.dropout(torch.softmax(scores, dim=-1))
        out = torch.matmul(attn, V)
        out = out.transpose(1, 2).contiguous().view(B, T, C)
        return self.W_o(out)
```

---

## 3.9. Tài liệu tham khảo

- Vaswani et al., *Attention Is All You Need* (NeurIPS 2017)
- [Transformer - Wikipedia](https://en.wikipedia.org/wiki/Transformer_(machine_learning_model))
- [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/)
