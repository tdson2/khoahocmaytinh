# 8. Xử lý âm thanh & giọng nói (Tín hiệu phi cấu trúc)

## 8.1. Tổng quan

**Âm thanh** là tín hiệu **một chiều theo thời gian** (waveform): áp suất không khí được lấy mẫu tại tần số lấy mẫu (sampling rate, ví dụ 16 kHz, 44.1 kHz). Xử lý bằng học máy gồm: biểu diễn (waveform, phổ, mel-spectrogram), tiền xử lý, và các nhiệm vụ **nhận dạng giọng nói (ASR)**, **phân loại âm thanh**, **tổng hợp giọng nói (TTS)**, **nhận dạng loa (speaker)**.

**Ứng dụng thực tế:** Trợ lý ảo (Alexa, Siri); chuyển giọng nói thành văn bản; TTS cho sách nói, video; nhận diện nhạc, âm thanh môi trường; gọi điện thoại tự động.

---

## 8.2. Biểu diễn tín hiệu âm thanh

### 8.2.1. Waveform (dạng sóng)

- **Đầu vào**: Mảng 1D $x[n]$, $n = 0, \ldots, N-1$.
- **Sampling rate** $f_s$ (Hz): Số mẫu mỗi giây. Định lý Nyquist: cần $f_s \ge 2 f_{max}$ để không mất thông tin (với $f_{max}$ là tần số cao nhất).
- **Độ sâu**: 16 bit (âm thanh thường), float [-1, 1] sau chuẩn hóa.
- **Ưu**: Đầy đủ thông tin. **Nhược**: Dài, khó mô hình trực tiếp (gần đây có mô hình waveform như WaveNet, GAN-based).

### 8.2.2. Spectrogram (phổ thời gian)

- **Short-Time Fourier Transform (STFT)**: Chia waveform thành các **frame** (cửa sổ, ví dụ 25 ms), áp dụng **FFT** trên từng frame → biên độ/pha theo tần số và thời gian.
- **Spectrogram**: $|STFT(t,f)|^2$ hoặc $\log(1 + |STFT|^2)$; trục ngang: thời gian, trục dọc: tần số.
- **Cửa sổ**: Hamming, Hanning. **Hop length**: Khoảng cách giữa hai frame (overlap).
- **Ưu**: Thể hiện nội dung tần số theo thời gian; nhiều mô hình dùng spectrogram làm đầu vào.

### 8.2.3. Mel-spectrogram

- **Thang Mel**: Mô phỏng cảm nhận tần số của tai người: tần số thấp phân giải tốt hơn tần số cao.
- **Mel filter bank**: Nhóm các bin tần số tuyến tính thành **mel band** (số band, ví dụ 80). Công thức mel: $m = 2595 \log_{10}(1 + f/700)$.
- **Mel-spectrogram**: Áp dụng mel filter bank lên power spectrogram → (thời gian × mel bins). Chuẩn hóa log thường dùng: $\log(1 + \text{mel})$.
- **Ứng dụng**: ASR, phân loại âm thanh, TTS (mel làm target hoặc feature).

### 8.2.4. MFCC (Mel-frequency cepstral coefficients)

- **Cepstrum**: IFFT của log power spectrum → hệ số cepstral.
- **MFCC**: Lấy log mel power → DCT → lấy K hệ số đầu (ví dụ 13). Dùng nhiều trong ASR truyền thống; deep learning thường dùng mel-spectrogram trực tiếp.

---

## 8.3. Tiền xử lý và tăng cường dữ liệu

| Kỹ thuật | Mô tả |
|----------|--------|
| **Resampling** | Đưa về tần số lấy mẫu cố định (ví dụ 16 kHz cho speech). |
| **Normalization** | Chuẩn hóa biên độ (peak hoặc RMS) để ổn định scale. |
| **Trimming / Padding** | Cắt im lặng đầu cuối; pad/crop về độ dài cố định. |
| **Noise injection** | Cộng nhiễu trắng hoặc nhiễu nền (room, street) để tăng độ robust. |
| **Time stretch / Pitch shift** | Thay đổi tốc độ hoặc cao độ (trong miền waveform hoặc spec). |
| **SpecAugment** | Mask ngẫu nhiên theo thời gian (time mask) và theo tần số (freq mask) trên spectrogram. |
| **Mixup** | Trộn hai mẫu (waveform hoặc mel) và nhãn tương ứng. |

---

## 8.4. Nhiệm vụ chính

### 8.4.1. Nhận dạng giọng nói (ASR – Automatic Speech Recognition)

- **Đầu vào**: Âm thanh (waveform hoặc mel-spectrogram). **Đầu ra**: Chuỗi từ/ký tự (text).
- **Kiến trúc**: (1) **CTC** (Connectionist Temporal Classification): Mạng encoder (CNN+RNN hoặc transformer) → logits theo frame → CTC loss để căn chỉnh không cần alignment. (2) **Attention-based encoder-decoder**: Encoder từ audio, decoder sinh text từng bước với attention. (3) **Transducer**: Kết hợp CTC và autoregressive.
- **Mô hình phổ biến**: wav2vec 2.0 / XLSR (self-supervised), Whisper (encoder-decoder), Conformer.

### 8.4.2. Phân loại âm thanh (Audio Classification)

- **Đầu vào**: Đoạn âm thanh. **Đầu ra**: Nhãn lớp (âm thanh môi trường, nhạc cụ, cảm xúc, v.v.).
- **Pipeline**: Waveform hoặc mel-spectrogram → CNN (2D trên spec) hoặc 1D CNN / RNN / transformer trên waveform hoặc spec.
- **Ví dụ**: CNN 2D trên mel (giống ảnh); hoặc mô hình pre-train (PANNs, YAMNet, wav2vec) rồi fine-tune.

### 8.4.3. Tổng hợp giọng nói (TTS – Text-to-Speech)

- **Đầu vào**: Văn bản. **Đầu ra**: Waveform (hoặc mel rồi vocoder).
- **Hai giai đoạn**: (1) **Text → Mel** (hoặc feature trung gian): Tacotron, FastSpeech, VITS. (2) **Mel → Waveform** (vocoder): WaveNet, HiFi-GAN, WaveGlow.
- **End-to-end**: VITS, Bark (text → waveform trực tiếp hoặc gần trực tiếp).

### 8.4.4. Nhận dạng loa (Speaker Recognition / Verification)

- **Embedding**: Mạng (CNN hoặc encoder) ánh xạ đoạn âm thanh → vector đặc trưng (d-vector, x-vector).
- **Verification**: So sánh embedding (cosine similarity hoặc metric learning). **Identification**: Phân loại trong tập loa đã biết.
- **Loss**: Triplet, Contrastive, hoặc Softmax với margin (ArcFace).

---

## 8.5. Mô hình self-supervised cho âm thanh

| Mô hình | Ý tưởng | Ứng dụng |
|--------|---------|----------|
| **wav2vec 2.0** | Mask một phần waveform, encoder + transformer dự đoán masked (giống BERT). | Pre-train ASR, fine-tune với ít nhãn. |
| **HuBERT** | Mask hidden unit, cluster rồi dự đoán cluster. | Tương tự wav2vec. |
| **WavLM** | Thêm mô phỏng noise/denoise, nhiều điều kiện. | ASR, downstream. |

---

## 8.6. Code mẫu (PyTorch – Mel-spectrogram + CNN đơn giản)

```python
import torch
import torch.nn as nn
import torchaudio.transforms as T

# Mel-spectrogram
mel_spec = T.MelSpectrogram(
    sample_rate=16000,
    n_fft=512,
    win_length=400,
    hop_length=160,
    n_mels=80,
    f_min=0,
    f_max=8000,
)
# Dùng: spec = mel_spec(waveform)  -> (C, n_mels, time)
# Log: spec = torch.log(spec + 1e-5)

class AudioCNN(nn.Module):
    """Phân loại từ mel-spectrogram (C, n_mels, time)."""
    def __init__(self, n_mels=80, num_classes=10):
        super().__init__()
        self.conv = nn.Sequential(
            nn.Conv2d(1, 32, 3, padding=1), nn.BatchNorm2d(32), nn.ReLU(), nn.MaxPool2d(2),
            nn.Conv2d(32, 64, 3, padding=1), nn.BatchNorm2d(64), nn.ReLU(), nn.MaxPool2d(2),
            nn.Conv2d(64, 128, 3, padding=1), nn.BatchNorm2d(128), nn.ReLU(),
            nn.AdaptiveAvgPool2d(1),
        )
        self.fc = nn.Linear(128, num_classes)
    def forward(self, x):
        if x.dim() == 3:
            x = x.unsqueeze(1)
        x = self.conv(x)
        x = x.view(x.size(0), -1)
        return self.fc(x)
```

---

## 8.7. Tài liệu tham khảo

- [Speech recognition - Wikipedia](https://en.wikipedia.org/wiki/Speech_recognition)
- [Mel-frequency cepstrum - Wikipedia](https://en.wikipedia.org/wiki/Mel-frequency_cepstrum)
- Baevski et al., *wav2vec 2.0*
- torchaudio: transforms, models, datasets
