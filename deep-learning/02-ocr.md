# 2. OCR – Nhận dạng ký tự quang học (Optical Character Recognition)

**OCR** chuyển ảnh chứa chữ (tài liệu, biển báo, ảnh chụp màn hình) thành **văn bản có thể chỉnh sửa**. Pipeline điển hình: **phát hiện vùng chữ (text detection)** → **nhận dạng chữ (text recognition)**; có thể thêm **chỉnh góc/skew**, **tách dòng/chữ**.

**Ứng dụng thực tế:** Số hóa tài liệu, form, hóa đơn; đọc biển số xe; trích văn bản từ ảnh (camera, scan); trợ lý đọc cho người khiếm thị.

---

## 2.1. Tổng quan pipeline

| Bước | Mục tiêu | Kỹ thuật thường dùng |
|------|----------|----------------------|
| **Tiền xử lý** | Chuẩn hóa ảnh, chỉnh góc | Deskew, binarization, denoise |
| **Text detection** | Tìm vùng chứa chữ (bounding box) | CRAFT, DBNet, EAST, DB++ |
| **Text recognition** | Từ ảnh crop vùng chữ → chuỗi ký tự | CRNN+CTC, TrOCR, ABINet, PARSeq |
| **Hậu xử lý** | Sửa lỗi, định dạng | Spell check, rule-based, LLM |

---

## 2.2. Text detection (Phát hiện vùng chữ)

### Mục tiêu

- Đầu vào: ảnh. Đầu ra: **bounding box** (hộp chữ nhật hoặc polygon) cho từng dòng/chữ.
- Có thể trả về **region map** (pixel-level) rồi tách vùng (contour, connected component).

### Mô hình phổ biến

| Mô hình | Ý tưởng | Ghi chú |
|---------|---------|---------|
| **CRAFT** | Dự đoán character region + affinity (liên kết ký tự); không cần box gán sẵn | Chính xác, phù hợp nhiều ngôn ngữ |
| **DBNet / DBNet++** | Segmentation + threshold bất định (differentiable binarization) | Nhanh, ổn định |
| **EAST** | Dự đoán pixel thuộc text + geometry (box/quads) | One-stage, nhanh |
| **PSENet** | Progressive scale expansion cho instance segmentation chữ | Tách được chữ gần nhau |

---

## 2.3. Text recognition (Nhận dạng chữ)

### Mục tiêu

- Đầu vào: ảnh **một dòng chữ** (crop từ detection). Đầu ra: **chuỗi ký tự** (string).

### Kiến trúc điển hình

| Loại | Mô tả | Ví dụ |
|------|--------|-------|
| **CNN + RNN + CTC** | CNN trích đặc trưng, RNN (BiLSTM) theo chuỗi, CTC để không cần alignment | CRNN, Deep Text Recognition |
| **Encoder–Decoder** | Encoder ảnh (CNN/Transformer), Decoder sinh từng ký tự (attention) | TrOCR, SAR |
| **Transformer-only** | Ảnh → patch embedding → Transformer encoder–decoder | TrOCR (image encoder + decoder) |
| **Vision + LM** | Encoder ảnh + language model (spelling, context) | ABINet, PARSeq (iterative correction) |

### TrOCR (Transformer OCR)

- **Encoder**: Chia ảnh dòng chữ thành patch → ViT-like encoder (hoặc CNN backbone + transformer).
- **Decoder**: Transformer decoder, sinh chuỗi token (character hoặc subword) với causal mask; có thể dùng cross-attention vào encoder.
- Pre-train: ảnh → text (synthetic + real); fine-tune theo ngôn ngữ/tập ký tự.

### CTC (Connectionist Temporal Classification)

- Cho phép map chuỗi feature (theo thời gian/frame) → chuỗi nhãn **không cần alignment** (thêm token blank). Loss: CTC loss. Decode: greedy hoặc beam search.

---

## 2.4. End-to-end và mô hình gộp

- **End-to-end**: Một mô hình từ ảnh full → text (có thể nhiều dòng): nhận diện vùng + nhận diện chữ trong cùng kiến trúc (attention-based E2E).
- **Two-stage**: Detection riêng, recognition riêng; linh hoạt, dễ thay từng bước.

---

## 2.5. Công cụ và thư viện

| Công cụ | Mô tả |
|---------|--------|
| **Tesseract** | Engine OCR truyền thống (không deep learning), nhiều ngôn ngữ |
| **PaddleOCR** | Pipeline đầy đủ: detection (DB) + recognition (CRNN/SVTR), support nhiều ngôn ngữ, lightweight |
| **EasyOCR** | PyTorch, dùng CRAFT + CRNN hoặc tương đương, dễ cài, hỗ trợ 80+ ngôn ngữ |
| **TrOCR (Hugging Face)** | Transformer-based recognition, fine-tune cho tiếng Anh/đa ngôn ngữ |
| **Keras-OCR** | Pipeline detection + recognition, dùng Keras/TF |

---

## 2.6. Ứng dụng

- Số hóa tài liệu (PDF, scan) → text.  
- Extract thông tin từ form, hóa đơn, CMND.  
- Đọc biển số, biển báo (kết hợp detection đối tượng).  
- RAG từ tài liệu ảnh: OCR → chunk → embed → retrieve.  
- Accessibility: đọc chữ trên ảnh cho người khiếm thị.

---

## 2.7. Code mẫu (PaddleOCR / EasyOCR)

```python
# PaddleOCR
from paddleocr import PaddleOCR
ocr = PaddleOCR(use_angle_cls=True, lang="en")
result = ocr.ocr("document.png", cls=True)
for line in result[0]:
    box, (text, conf) = line[0], line[1]
    print(text, conf)

# EasyOCR
import easyocr
reader = easyocr.Reader(["en", "vi"])
results = reader.readtext("image.png")
for (bbox, text, prob) in results:
    print(text, prob)
```

---

## 2.8. Tài liệu tham khảo

- Baek et al., *Character Region Awareness for Text Detection* (CRAFT)
- Li et al., *Real-time Scene Text Detection with Differentiable Binarization* (DBNet)
- Li et al., *TrOCR: Transformer-based Optical Character Recognition*
- [PaddleOCR](https://github.com/PaddlePaddle/PaddleOCR), [EasyOCR](https://github.com/JaidedAI/EasyOCR)
