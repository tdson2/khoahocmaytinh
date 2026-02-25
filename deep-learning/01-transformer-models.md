# 1. Mô hình Transformer (Transformer Models)

**Transformer** là kiến trúc nền tảng của Deep Learning hiện đại: NLP (BERT, GPT, T5, LLaMA), Vision (ViT), đa modal và nhiều mô hình đặc thù (OCR, ASR, TTS). Chuyên mục này tập trung **các họ mô hình Transformer** và ứng dụng.

---

## 1.1. Tổng quan kiến trúc

Transformer dựa hoàn toàn trên **self-attention** (không RNN, không conv theo chuỗi). Ba dạng chính:

| Kiểu | Thành phần | Ứng dụng điển hình |
|------|------------|---------------------|
| **Encoder-only** | Chỉ encoder (bidirectional attention) | BERT, RoBERTa: phân loại, NER, QA, embedding |
| **Decoder-only** | Chỉ decoder (causal/masked self-attention) | GPT, LLaMA, Mistral: sinh văn, chat, completion |
| **Encoder–Decoder** | Encoder + Decoder (cross-attention) | T5, BART: dịch, tóm tắt, span-to-span |

---

## 1.2. Self-Attention và Multi-Head Attention

### Scaled Dot-Product Attention

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^\top}{\sqrt{d_k}}\right) V$$

- **Q, K, V** từ cùng input qua phép chiếu tuyến tính; **√d_k** để tránh softmax quá nhọn.
- **Mask**: Che vị trí không được nhìn (ví dụ token tương lai trong decoder: causal mask).

### Multi-Head Attention

- Nhiều head song song, mỗi head chiều nhỏ hơn; concat rồi chiếu ra:  
  \(\text{MultiHead}(Q,K,V) = \text{Concat}(\text{head}_1,\ldots,\text{head}_h) W^O\).
- Cho phép học nhiều kiểu quan hệ (cú pháp, tham chiếu, ngữ nghĩa) cùng lúc.

---

## 1.3. Encoder-only: BERT và họ

| Mô hình | Pre-training | Đặc điểm |
|---------|--------------|----------|
| **BERT** | Masked LM + NSP | Bidirectional, [CLS] cho sentence-level task |
| **RoBERTa** | Chỉ Masked LM, dữ liệu lớn hơn | Bỏ NSP, dynamic masking |
| **ALBERT** | Masked LM + SOP, parameter sharing | Giảm tham số, scale tốt hơn |
| **DeBERTa** | Disentangled attention + enhanced mask decoder | Hiệu năng cao trên GLUE/SuperGLUE |
| **ELECTRA** | Replaced token detection | Hiệu quả pre-train, ít bước hơn |

**Ứng dụng**: Phân loại, NER, QA (extractive), embedding câu, làm encoder trong RAG/retriever.

---

## 1.4. Decoder-only: GPT và họ LLM

| Mô hình | Đặc điểm | Ứng dụng |
|---------|----------|----------|
| **GPT-2 / GPT-3** | Causal LM, scale lớn, few-shot | Sinh văn, completion, chat (trước ChatGPT) |
| **LLaMA / LLaMA 2** | Kiến trúc tối ưu (RMSNorm, SwiGLU), open weights | Base cho chat, fine-tune, RAG |
| **Mistral / Mixtral** | MoE (Mixture of Experts), 7B/8x7B | Chat, reasoning, ít tham số active |
| **Qwen, Phi, Gemma** | Các họ open LLM khác | Đa ngôn ngữ, code, chat |

**Causal mask**: Mỗi vị trí chỉ attend vào các token trước đó → phù hợp sinh token từng bước (autoregressive).

**Kỹ thuật thường dùng**: Instruction tuning, RLHF/DPO, context length mở rộng (RoPE, ALiBi), quantization (GPTQ, AWQ) cho deploy.

---

## 1.5. Encoder–Decoder: T5, BART

| Mô hình | Pre-training | Ứng dụng |
|---------|--------------|----------|
| **T5** | “Text-to-text”: span corruption, nhiều nhiệm vụ | Dịch, tóm tắt, QA, phân loại dạng text |
| **BART** | Denoising (shuffle, mask, delete) | Tóm tắt, sinh văn, denoise |
| **mBART** | Multilingual denoising | Dịch đa ngôn ngữ |

**Cross-attention**: Decoder dùng Query từ bước hiện tại, Key/Value từ encoder → “hỏi” nguồn khi sinh từng token.

---

## 1.6. Vision Transformer (ViT) và biến thể

| Mô hình | Ý tưởng | Ứng dụng |
|---------|---------|----------|
| **ViT** | Chia ảnh thành patch → sequence → Transformer encoder | Phân loại ảnh, backbone |
| **Swin Transformer** | Shifted window, hierarchy | Phân loại, detection, segmentation |
| **DETR** | Encoder–decoder + object queries | Object detection (set prediction) |
| **SAM (Segment Anything)** | Prompt (point/box) + image encoder + mask decoder | Phân đoạn theo prompt |

---

## 1.7. Đa modal (Multimodal)

- **CLIP**: Encoder ảnh + encoder text, contrastive learning → embedding chung; dùng cho retrieval, zero-shot.
- **LLaVA / Qwen-VL / GPT-4V**: Kết hợp encoder ảnh với LLM (decoder) → hỏi đáp về ảnh, mô tả, OCR trong ảnh.
- **Whisper**: Encoder–decoder; audio → text (ASR), có thể kèm ngôn ngữ.

---

## 1.8. Kỹ thuật quan trọng

| Kỹ thuật | Mô tả |
|----------|--------|
| **Positional encoding** | Sinusoidal hoặc learned; đưa thông tin vị trí vào (Transformer không có thứ tự nội tại). |
| **RoPE (Rotary)** | Mã hóa vị trí bằng phép quay; scale tốt cho độ dài dài, dùng trong LLaMA, GPT-NeoX. |
| **Layer Norm / RMSNorm** | Chuẩn hóa trước/sau từng sub-layer; RMSNorm không center (LLaMA). |
| **SwiGLU / GELU** | Activation trong FFN; SwiGLU thường tốt hơn ReLU/GELU thuần trong LLM. |
| **Flash Attention** | Tối ưu bộ nhớ và tốc độ attention (O(N) memory), hỗ trợ causal mask. |

---

## 1.9. Liên hệ với các chuyên mục khác

- **OCR**: TrOCR dùng encoder–decoder (image → text).  
- **Speech-to-Text**: Whisper, wav2vec dùng encoder hoặc encoder–decoder.  
- **Text-to-Speech**: Một số mô hình dùng Transformer (FastSpeech, VITS).  
- **RAG & Ứng dụng**: Retriever thường dùng encoder (BERT, E5); generator dùng decoder-only LLM; xem [05-applications-rag-agents.md](05-applications-rag-agents.md).

---

## 1.10. Tài liệu tham khảo

- Vaswani et al., *Attention Is All You Need*
- Devlin et al., *BERT: Pre-training of Deep Bidirectional Transformers*
- Touvron et al., *LLaMA: Open and Efficient Foundation Language Models*
- [Hugging Face Transformers](https://huggingface.co/docs/transformers)
