# 4. Text-to-Speech (TTS – Tổng hợp giọng nói)

**Text-to-Speech (TTS)** chuyển **văn bản** thành **âm thanh giọng nói** (waveform). Ứng dụng: trợ lý ảo, sách nói, phụ đề có đọc, accessibility, podcast/nội dung đa phương tiện.

**Ứng dụng thực tế:** Trợ lý ảo đọc đáp án; sách nói, podcast tổng hợp; phụ đề có đọc cho người khiếm thị; tin nhắn thoại; video/nội dung đa phương tiện.

---

## 4.1. Tổng quan pipeline

| Kiểu pipeline | Mô tả |
|---------------|--------|
| **Hai giai đoạn** | (1) Text → intermediate representation (mel-spectrogram hoặc acoustic feature); (2) Vocoder: mel → waveform |
| **End-to-end** | Text → waveform (hoặc gần như trực tiếp, với ít bước trung gian) |

---

## 4.2. Giai đoạn 1: Text → Mel (hoặc acoustic feature)

### Nhiệm vụ

- Đầu vào: chuỗi ký tự hoặc phoneme. Đầu ra: **mel-spectrogram** (hoặc feature tương đương) theo thời gian.
- Cần **alignment** text–audio (hoặc mô hình học alignment ẩn).

### Mô hình phổ biến

| Mô hình | Ý tưởng | Ghi chú |
|---------|---------|---------|
| **Tacotron 2** | Encoder (text) + Decoder (attention) sinh mel từng frame; attention học alignment | Nền tảng nhiều hệ TTS |
| **FastSpeech / FastSpeech 2** | Dự đoán **duration** cho mỗi phoneme → expand → sinh mel (không autoregressive) | Nhanh, ổn định, ít lỗi skip/repeat |
| **VITS** | Text → latent (flow) → mel; kết hợp với vocoder trong một mô hình; có thể end-to-end | Chất lượng cao, một mô hình |
| **Bark (Suno)** | Transformer; text → semantic token (audio codec) rồi codec → waveform | Đa ngôn ngữ, nhiều giọng, có thể thêm âm thanh (cười, thở) |
| **XTTS (Coqui)** | Clone giọng ít mẫu; text + reference audio → waveform | Zero-shot / few-shot voice cloning |

---

## 4.3. Giai đoạn 2: Vocoder (Mel → Waveform)

- **Waveform** có tần số lấy mẫu cao (22 kHz, 24 kHz) → số mẫu rất lớn; sinh trực tiếp từ text khó.
- **Mel-spectrogram** nén thông tin theo thời gian (frame 10–12.5 ms) → vocoder sinh waveform từ mel.

| Vocoder | Ý tưởng | Ưu / Nhược |
|---------|---------|-------------|
| **WaveNet** | Autoregressive, sinh từng mẫu; dilated conv | Chất lượng cao; rất chậm |
| **WaveGlow** | Flow-based: mel → latent → waveform bằng invertible network | Nhanh (parallel), chất lượng tốt |
| **HiFi-GAN** | GAN: generator sinh waveform từ mel, discriminator đa scale | Nhanh, chất lượng cao, dùng rộng rãi |
| **BigVGAN** | GAN với anti-aliasing, generator lớn hơn | Chất lượng và độ ổn định cao |

---

## 4.4. Một số hướng phát triển

- **Neural codec**: Nén speech thành discrete code (EnCodec, SoundStream); TTS sinh code rồi decode → waveform (Bark, VALL-E style).
- **Zero-shot / few-shot voice cloning**: Cho vài giây giọng mẫu → TTS nói với giọng đó (XTTS, VALL-E, YourTTS).
- **Multilingual & emotion**: Một mô hình nhiều ngôn ngữ, nhiều giọng; điều khiển tốc độ, cao độ, cảm xúc.
- **Streaming**: Sinh audio theo chunk để giảm latency (cho trợ lý ảo).

---

## 4.5. Ứng dụng

- Trợ lý ảo (Alexa, Google Assistant, Siri).  
- Sách nói, đọc bản tin.  
- Phụ đề có đọc (video accessibility).  
- Chatbot có giọng nói: LLM → text → TTS → audio.  
- Podcast, nội dung đa phương tiện với giọng nhân tạo hoặc clone.

---

## 4.6. Code mẫu (Coqui TTS / Bark)

```python
# Coqui TTS (ví dụ: model đa ngôn ngữ + clone giọng)
# pip install TTS
from TTS.api import TTS
tts = TTS(model_name="tts_models/multilingual/multi-dataset/xtts_v2", gpu=True)
tts.tts_to_file(text="Xin chào, đây là giọng tổng hợp.", file_path="out.wav", language="vi")

# Bark (Hugging Face) – cần transformers, bark
# from bark import SAMPLE_RATE, generate_audio, preload_models
# preload_models()
# audio = generate_audio("Hello, this is synthetic speech.")
```

---

## 4.7. Tài liệu tham khảo

- Shen et al., *Natural TTS Synthesis by Conditioning WaveNet on Mel Spectrogram Predictions* (Tacotron 2)
- Kim et al., *FastSpeech: Fast, Robust and Controllable Text to Speech*
- Kong et al., *HiFi-GAN: Generative Adversarial Networks for Efficient and High Fidelity Speech Synthesis*
- [Coqui TTS](https://github.com/coqui-ai/TTS), [Bark](https://github.com/suno-ai/bark)
