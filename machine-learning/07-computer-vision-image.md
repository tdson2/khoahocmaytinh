# 7. Xử lý ảnh – Computer Vision (Tín hiệu phi cấu trúc)

## 7.1. Tổng quan

**Ảnh** là tín hiệu **phi cấu trúc** dạng lưới 2D (hoặc 3D với kênh màu). Xử lý ảnh bằng học máy gồm: biểu diễn ảnh, tiền xử lý, tăng cường dữ liệu, và các nhiệm vụ như **phân loại**, **phát hiện đối tượng**, **phân đoạn** (semantic/instance). Kiến trúc chủ đạo: **CNN** và **Vision Transformer (ViT)**.

---

## 7.2. Biểu diễn ảnh số

| Khái niệm | Mô tả |
|-----------|--------|
| **Pixel** | Điểm ảnh; cường độ (grayscale) hoặc (R,G,B) (màu). |
| **Kích thước** | Chiều cao H × chiều rộng W × số kênh C (1: xám, 3: RGB). |
| **Độ sâu** | 8 bit (0–255), 16 bit, float chuẩn hóa [0,1]. |
| **Tensor** | Trong framework: shape thường (B, C, H, W) (PyTorch) hoặc (B, H, W, C) (TensorFlow). |

**Tiền xử lý thường dùng**: Resize về kích thước cố định, chuẩn hóa mean/std (theo ImageNet hoặc theo dataset), đôi khi chuẩn hóa [0,1] rồi trừ mean.

---

## 7.3. Convolution và CNN cơ bản

### 7.3.1. Convolution 2D

- **Bộ lọc (kernel)** kích thước K×K (hoặc K_h×K_w) trượt trên ảnh; tại mỗi vị trí tính **tích chập** (tổng có trọng số) → map đặc trưng (feature map).
- **Stride**: Bước nhảy khi trượt (1 hoặc 2 để giảm kích thước).
- **Padding**: Thêm viền 0 (hoặc reflect) để giữ kích thước hoặc kiểm soát output size.
- **Công thức output**: $H_{out} = \lfloor (H + 2P - K)/S \rfloor + 1$ (tương tự cho W).

### 7.3.2. Các lớp cơ bản trong CNN

| Lớp | Chức năng |
|-----|-----------|
| **Conv2d** | Trích đặc trưng không gian (cạnh, texture, pattern). |
| **Pooling (Max/Avg)** | Giảm kích thước không gian, tăng tính bất biến với dịch chuyển nhỏ. |
| **Batch Norm** | Chuẩn hóa theo batch sau conv. |
| **ReLU / GELU** | Phi tuyến sau conv. |

**Kiến trúc điển hình**: Khối (Conv → BN → ReLU → Pool) xếp chồng nhiều lần, cuối Flatten → MLP (hoặc Global Average Pooling → Linear) cho phân loại.

---

## 7.4. Kiến trúc CNN kinh điển

| Kiến trúc | Ý tưởng chính | Ứng dụng |
|-----------|----------------|----------|
| **LeNet** | Conv sâu đầu tiên (chữ viết tay). | Phân loại chữ số. |
| **AlexNet** | Nhiều conv + dropout, ReLU; ảnh ImageNet. | Phân loại ảnh. |
| **VGG** | Chỉ 3×3 conv, stack sâu (VGG-16, VGG-19). | Backbone đặc trưng. |
| **ResNet** | **Skip connection** (residual): $y = F(x) + x$; train được mạng rất sâu. | Backbone phổ biến. |
| **Inception** | Nhiều nhánh (1×1, 3×3, 5×5, pool) trong một block, concat. | Giảm tham số, tăng width. |
| **EfficientNet** | Scale đồng thời depth/width/resolution theo compound scaling. | Cân bằng accuracy/efficiency. |
| **ConvNeXt** | CNN “hiện đại hóa” (kernel 7×7, Layer Norm, GELU) gần transformer. | Backbone ảnh. |

---

## 7.5. Data Augmentation cho ảnh

Mục đích: Tăng đa dạng dữ liệu, giảm overfitting, cải thiện khả năng tổng quát hóa.

| Loại | Cách làm |
|------|----------|
| **Geometric** | Xoay (rotate), dịch (translate), lật ngang/dọc (flip), scale (zoom in/out), cắt (crop). |
| **Color** | Thay đổi độ sáng/độ tương phản, saturation; thêm nhiễu Gaussian; đổi kênh. |
| **Cutout / Random Erasing** | Xóa ngẫu nhiên vùng vuông trên ảnh. |
| **Mixup** | $\tilde{x} = \lambda x_1 + (1-\lambda)x_2$, $\tilde{y} = \lambda y_1 + (1-\lambda)y_2$. |
| **CutMix** | Dán vùng cắt từ ảnh khác lên ảnh gốc; nhãn tỉ lệ theo diện tích. |
| **AutoAugment / RandAugment** | Tìm hoặc dùng tập chính sách augmentation tự động. |

---

## 7.6. Nhiệm vụ chính trong Computer Vision

### 7.6.1. Phân loại ảnh (Image Classification)

- **Đầu vào**: Ảnh. **Đầu ra**: Nhãn lớp (1 trong C lớp).
- **Mô hình**: CNN hoặc ViT → Global Average Pooling (hoặc [CLS]) → Linear → Softmax.
- **Loss**: Cross-Entropy. **Metric**: Top-1 / Top-5 accuracy.

### 7.6.2. Phát hiện đối tượng (Object Detection)

- **Đầu vào**: Ảnh. **Đầu ra**: Hộp giới hạn (bounding box) + nhãn lớp cho từng đối tượng.
- **Hai hướng**: (1) **Two-stage**: RCNN, Fast/Faster R-CNN (region proposal rồi phân loại/chỉnh box). (2) **One-stage**: YOLO, RetinaNet (dự đoán trực tiếp từ grid/anchor).
- **Loss**: Classification (CE/Focal) + Localization (L1/smooth L1, IoU/GIoU). **Metric**: mAP (mean Average Precision), IoU.

### 7.6.3. Phân đoạn (Segmentation)

- **Semantic segmentation**: Gán nhãn từng **pixel** (lớp ngữ nghĩa: đường, người, trời…). Kiến trúc: U-Net, FCN, DeepLab (atrous conv, ASPP).
- **Instance segmentation**: Phân biệt từng **đối tượng** (nhiều người → nhiều vùng khác nhau). Ví dụ: Mask R-CNN, YOLACT.
- **Loss**: Cross-Entropy per pixel, Dice, IoU loss. **Metric**: mIoU, Dice.

### 7.6.4. Vision Transformer (ViT)

- Chia ảnh thành **patch** (ví dụ 16×16), mỗi patch flatten → linear embedding + positional encoding → **Transformer encoder** (chỉ self-attention, không decoder).
- Đầu ra: [CLS] token hoặc average patch → head phân loại.
- **Ưu điểm**: Scale tốt khi dữ liệu lớn; **nhược**: Cần nhiều dữ liệu hoặc pre-train, ít inductive bias không gian hơn CNN.

---

## 7.7. Pipeline xử lý ảnh điển hình

1. **Load ảnh** → resize, crop (train/val có thể khác).  
2. **Chuẩn hóa** (mean/std hoặc [0,1]).  
3. **Augmentation** (chỉ trên train).  
4. **Mô hình** (CNN/ViT) → logits.  
5. **Loss** (CE/Focal…) + **backprop** + optimizer.  
6. **Đánh giá**: accuracy, mAP, mIoU tùy nhiệm vụ.

---

## 7.8. Code mẫu (PyTorch – CNN đơn giản + augmentation)

```python
import torch
import torch.nn as nn
from torchvision import transforms

# Augmentation
train_tf = transforms.Compose([
    transforms.RandomResizedCrop(224),
    transforms.RandomHorizontalFlip(),
    transforms.ColorJitter(0.4, 0.4, 0.4),
    transforms.ToTensor(),
    transforms.Normalize([0.485, 0.456, 0.406], [0.229, 0.224, 0.225]),
])
val_tf = transforms.Compose([
    transforms.Resize(256),
    transforms.CenterCrop(224),
    transforms.ToTensor(),
    transforms.Normalize([0.485, 0.456, 0.406], [0.229, 0.224, 0.225]),
])

# Mini CNN
class SimpleCNN(nn.Module):
    def __init__(self, num_classes=10):
        super().__init__()
        self.features = nn.Sequential(
            nn.Conv2d(3, 32, 3, padding=1), nn.BatchNorm2d(32), nn.ReLU(), nn.MaxPool2d(2),
            nn.Conv2d(32, 64, 3, padding=1), nn.BatchNorm2d(64), nn.ReLU(), nn.MaxPool2d(2),
            nn.Conv2d(64, 128, 3, padding=1), nn.BatchNorm2d(128), nn.ReLU(), nn.AdaptiveAvgPool2d(1),
        )
        self.classifier = nn.Linear(128, num_classes)
    def forward(self, x):
        x = self.features(x)
        x = x.view(x.size(0), -1)
        return self.classifier(x)
```

---

## 7.9. Tài liệu tham khảo

- [Convolutional neural network - Wikipedia](https://en.wikipedia.org/wiki/Convolutional_neural_network)
- He et al., *Deep Residual Learning for Image Recognition* (ResNet)
- Dosovitskiy et al., *An Image is Worth 16x16 Words* (ViT)
- torchvision: models, transforms, datasets
