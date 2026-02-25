# 5. Knowledge Distillation (Ép mật / Nén tri thức)

## 5.1. Tổng quan

**Knowledge Distillation** là kỹ thuật huấn luyện một mô hình **học trò (student)** nhỏ, nhanh sao cho bắt chước hành vi của mô hình **thầy (teacher)** lớn, mạnh. Mục tiêu: **nén** tri thức từ teacher vào student mà vẫn giữ được phần lớn hiệu năng, phục vụ triển khai trên thiết bị có tài nguyên hạn chế.

| Thành phần | Vai trò |
|------------|--------|
| **Teacher** | Mô hình đã train (cố định hoặc freeze), cho logits/features |
| **Student** | Mô hình cần train, học từ teacher và (thường) từ nhãn thật |
| **Temperature T** | Làm mềm phân phối xác suất để student học “dark knowledge” (quan hệ giữa các lớp) |

---

## 5.2. Ý tưởng “Dark Knowledge”

- Teacher không chỉ cho biết lớp đúng mà còn cho **phân phối xác suất** trên toàn bộ lớp (ví dụ: “rất giống chó, hơi giống mèo, gần như không phải xe”).
- Phân phối **mềm** (softmax với **nhiệt độ T > 1**) làm bớt nhọn → student học được quan hệ tương đối giữa các lớp, thường giúp generalize tốt hơn chỉ học one-hot.

---

## 5.3. Công thức cơ bản (Response-based)

- **Soft labels** từ teacher: \(p_i^T = \text{softmax}(z^T / T)_i\) với \(z^T\) = logits teacher.
- **Soft predictions** từ student: \(q_i^T = \text{softmax}(z^S / T)_i\).
- **Loss distillation** (KL divergence):
  $$\mathcal{L}_{\text{distill}} = T^2 \cdot D_{KL}(p^T \| q^T)$$
  (nhân \(T^2\) để scale gradient khi T lớn.)
- **Loss với nhãn thật** (hard label): Cross-entropy giữa student và one-hot \(y\).
- **Tổng loss**:
  $$\mathcal{L} = \alpha \mathcal{L}_{\text{CE}}(y, q^S) + (1-\alpha) T^2 D_{KL}(p^T \| q^T)$$
  với \(q^S = \text{softmax}(z^S)\) (temperature 1). Thường \(\alpha \in [0.3, 0.5]\).

---

## 5.4. Feature-based Distillation

- Student không chỉ bắt chước logits mà còn bắt chước **biểu diễn trung gian** (feature maps, hidden states).
- **Loss**: Đồng nhất feature teacher và student qua một lớp chiếu (nếu khác chiều) rồi dùng MSE hoặc cosine:
  $$\mathcal{L}_{\text{feat}} = \|f_S - W \cdot f_T\|^2 \quad \text{hoặc} \quad 1 - \cos(f_S, W \cdot f_T)$$
- Áp dụng: FitNet, PKD, nhiều phương pháp BERT distillation (hidden states từng layer).

---

## 5.5. Relation-based Distillation

- Bắt chước **quan hệ** giữa các mẫu (ví dụ ma trận tương đồng trong batch) thay vì từng mẫu riêng lẻ.
- Ví dụ: RKD (Relational Knowledge Distillation) dùng distance-wise và angle-wise giữa các cặp/tam giác embedding.

---

## 5.6. Self-Distillation

- **Teacher và student cùng kiến trúc** (hoặc teacher = student ở epoch trước); không cần mô hình thầy cố định riêng.
- Ví dụ: Deep Mutual Learning, Be Your Own Teacher (BYOT); student học từ bản thân mình ở giai đoạn trước hoặc từ nhiều nhánh.

---

## 5.7. Một số biến thể

| Phương pháp | Ý tưởng ngắn |
|-------------|--------------|
| **Hinton et al. (2015)** | Soft labels + temperature, KL loss |
| **FitNet** | Fit intermediate feature (hidden layer) |
| **DistilBERT** | Distillation BERT → student nhỏ hơn, loss = CE + cosine hidden + KL logits |
| **TinyBERT** | Distill cả attention và hidden nhiều layer |
| **DeBERTa / MiniLM** | Distill với nhiều layer, chỉ một số layer student tương ứng teacher |

---

## 5.8. Code mẫu (PyTorch – Response-based)

```python
import torch
import torch.nn as nn
import torch.nn.functional as F

def distillation_loss(logits_student, logits_teacher, labels, temperature=4.0, alpha=0.5):
    """
    logits_student, logits_teacher: (B, C)
    labels: (B,) class indices
    """
    soft_teacher = F.softmax(logits_teacher / temperature, dim=-1)
    soft_student = F.log_softmax(logits_student / temperature, dim=-1)
    kd_loss = F.kl_div(soft_student, soft_teacher, reduction='batchmean') * (temperature ** 2)
    ce_loss = F.cross_entropy(logits_student, labels)
    return alpha * ce_loss + (1 - alpha) * kd_loss

# Trong training loop (minimal):
# with torch.no_grad():
#     logits_teacher = teacher(batch_x)
# logits_student = student(batch_x)
# loss = distillation_loss(logits_student, logits_teacher, labels, T=4.0, alpha=0.5)
```

---

## 5.9. Tài liệu tham khảo

- Hinton et al., *Distilling the Knowledge in a Neural Network* (2015)
- [Knowledge distillation - Wikipedia](https://en.wikipedia.org/wiki/Knowledge_distillation)
- Sanh et al., *DistilBERT*
- Jiao et al., *TinyBERT*
- Gou et al., *Knowledge Distillation: A Survey*
