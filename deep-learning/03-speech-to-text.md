# 3. Speech-to-Text (ASR – Nhận dạng giọng nói)

**Speech-to-Text (STT)** hay **ASR (Automatic Speech Recognition)** chuyển **âm thanh giọng nói** thành **văn bản**. Ứng dụng: phụ đề trực tiếp, trợ lý ảo, ghi chú từ giọng nói, accessibility, phân tích cuộc gọi.

---

## 3.1. Tổng quan pipeline

| Bước | Mô tả |
|------|--------|
| **Tiền xử lý** | Resample (thường 16 kHz), chuẩn hóa biên độ, VAD (voice activity detection) để cắt im lặng |
| **Feature** | Waveform hoặc mel-spectrogram (hoặc raw waveform cho mô hình end-to-end) |
| **Mô hình** | Encoder (audio) → Decoder/Head (text); có thể CTC, attention-based, hoặc transducer |
| **Hậu xử lý** | Punctuation, capitalization, inverse text normalization (số, ngày, đơn vị) |

---

## 3.2. Kiến trúc mô hình ASR

| Kiểu | Ý tưởng | Ưu / Nhược |
|------|----------|-------------|
| **CTC (Connectionist Temporal Classification)** | Encoder → logits theo frame → CTC loss (không cần alignment) | Đơn giản, real-time; có thể lặng hoặc lỗi lặp |
| **Attention-based Encoder–Decoder** | Encoder audio, Decoder sinh text từng token với attention vào encoder | Linh hoạt, chất lượng cao; chậm hơn, có thể “nhảy” (alignment không ổn định) |
| **RNN-T / Transducer** | Encoder + predictor (language model) + joint network; decode từng frame có thể emit token hoặc blank | Cân bằng latency/chất lượng, streaming tốt |
| **Transformer / Conformer** | Encoder Transformer (hoặc Conformer: conv + attention) thay RNN | Scale tốt, chất lượng cao; cần nhiều dữ liệu |

---

## 3.3. Mô hình phổ biến

| Mô hình | Kiến trúc | Đặc điểm |
|---------|-----------|----------|
| **Whisper (OpenAI)** | Encoder–decoder Transformer; ảnh mel-spectrogram | Đa ngôn ngữ, robust noise, có thể detect language; open weights |
| **wav2vec 2.0 / XLS-R** | CNN + Transformer encoder; self-supervised rồi fine-tune với CTC | Ít dữ liệu có nhãn, nhiều ngôn ngữ |
| **HuBERT** | Masked prediction trên hidden unit (cluster) | Tương tự wav2vec, pre-train hiệu quả |
| **Conformer** | Convolution + Transformer (macaron-style) | Kết hợp local (conv) và global (attention), dùng trong production nhiều |
| **NeMo ASR** | Bộ công cụ (Conformer, Citrinet, …) + CTC/RNN-T | Dễ train, tối ưu cho streaming |

---

## 3.4. Self-supervised pre-training

- **wav2vec 2.0**: Mask một phần latent, model dự đoán masked (giống BERT). Fine-tune với CTC trên ít dữ liệu có nhãn.
- **HuBERT**: Cluster hidden unit, dự đoán cluster cho vị trí bị mask.
- **WavLM**: Thêm mô phỏng noise, nhiều điều kiện → robust hơn.

→ Giảm nhu cầu dữ liệu gán nhãn; đặc biệt hữu ích cho ngôn ngữ ít tài nguyên.

---

## 3.5. Đánh giá

| Metric | Mô tả |
|--------|--------|
| **WER (Word Error Rate)** | (S + D + I) / N với S=substitution, D=deletion, I=insertion, N=số từ tham chiếu. Chuẩn cho ASR. |
| **CER (Character Error Rate)** | Tương tự WER nhưng ở mức ký tự; dùng khi từ vựng lớn hoặc ngôn ngữ không tách từ rõ. |
| **TER (Token Error Rate)** | Trên subword token; phù hợp mô hình dùng BPE/WordPiece. |

---

## 3.6. Streaming và real-time

- **Streaming ASR**: Chỉ dùng phần âm thanh đã nghe được (chunk), trễ thấp. Kiến trúc: RNN-T, chunk-based attention, hoặc encoder với context window cố định.
- **Whisper**: Mặc định không streaming (encoder toàn bộ); có thể chunk ảnh mel để gần real-time.
- **VAD**: Xác định đoạn có giọng nói → chỉ gửi đoạn đó vào mô hình, giảm tính toán.

---

## 3.7. Ứng dụng

- Phụ đề trực tiếp (meeting, video, livestream).  
- Trợ lý ảo, điều khiển bằng giọng nói.  
- Ghi chú, biên bản họp.  
- Hỗ trợ người khiếm thính (chuyển lời nói thành chữ).  
- Phân tích cuộc gọi, sentiment từ speech.  
- Đầu vào cho RAG/chatbot: voice → STT → text → LLM → TTS → voice.

---

## 3.8. Code mẫu (Whisper)

```python
import whisper

model = whisper.load_model("base")  # base, small, medium, large, large-v3
result = model.transcribe("audio.wav", language="vi", fp16=False)
print(result["text"])

# Từng segment (có timestamp)
for seg in result["segments"]:
    print(seg["start"], seg["end"], seg["text"])
```

---

## 3.9. Tài liệu tham khảo

- Radford et al., *Robust Speech Recognition via Large-Scale Weak Supervision* (Whisper)
- Baevski et al., *wav2vec 2.0: A Framework for Self-Supervised Learning of Speech Representations*
- Gulati et al., *Conformer: Convolution-augmented Transformer for Speech Recognition*
- [OpenAI Whisper](https://github.com/openai/whisper), [Hugging Face Transformers (ASR)](https://huggingface.co/docs/transformers/tasks/asr)
