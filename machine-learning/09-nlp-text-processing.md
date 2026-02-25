# 9. Xử lý văn bản & ngôn ngữ tự nhiên – NLP (Tín hiệu phi cấu trúc)

## 9.1. Tổng quan

**Văn bản** là chuỗi **ký tự / từ** không có cấu trúc lưới như ảnh hay tín hiệu liên tục như âm thanh; thông tin nằm ở **thứ tự** và **ngữ cảnh**. Xử lý ngôn ngữ tự nhiên (NLP) gồm: biểu diễn (tokenization, embedding), mô hình chuỗi (RNN, Transformer), và các nhiệm vụ **phân loại**, **gắn nhãn chuỗi (NER, POS)**, **dịch máy**, **hỏi đáp**, **tóm tắt**, **sinh văn bản**.

---

## 9.2. Biểu diễn văn bản

### 9.2.1. Đơn vị: Ký tự, Từ, Subword

| Đơn vị | Ưu điểm | Nhược điểm |
|--------|---------|------------|
| **Ký tự (character)** | Từ vựng nhỏ, xử lý typo/morphology. | Chuỗi rất dài, khó học ngữ nghĩa từ xa. |
| **Từ (word)** | Đơn vị có nghĩa rõ. | Từ vựng lớn, OOV (từ ngoài từ điển). |
| **Subword** | Cân bằng: từ vựng vừa, giảm OOV, chia được từ mới. | Cần thuật toán tách từ (BPE, WordPiece, Unigram). |

### 9.2.2. Tokenization

- **Mục đích**: Chia câu thành danh sách **token** (ID số) để đưa vào mô hình.
- **BPE (Byte Pair Encoding)**: Bắt đầu từ ký tự, lặp lại: gộp cặp xuất hiện nhiều nhất thành token mới (dùng trong GPT, RoBERTa).
- **WordPiece**: Tương tự BPE, cách chọn cặp gộp dựa trên likelihood (dùng trong BERT).
- **Unigram LM**: Mô hình unigram trên subword; tách từ theo xác suất (SentencePiece, ALBERT).
- **SentencePiece**: Tokenizer độc lập ngôn ngữ (BPE/Unigram), xử lý khoảng trắng đặc biệt.

**Kết quả**: Bộ từ vựng (vocab) + ánh xạ token ↔ ID; thêm token đặc biệt: [PAD], [UNK], [CLS], [SEP], [MASK] tùy mô hình.

### 9.2.3. Embedding (vector hóa từ/token)

- **Embedding layer**: Mỗi token ID → vector $d$-chiều (tham số học được). Shape đầu vào (batch, seq_len) → (batch, seq_len, d).
- **Pre-trained embedding**: Word2Vec, GloVe (từ → vector cố định); hoặc dùng **contextual embedding** từ BERT/GPT (vector phụ thuộc ngữ cảnh).
- **Positional encoding**: Transformer cần thông tin vị trí (sinusoidal hoặc learned); thường cộng vào embedding sau khi nhúng token.

---

## 9.3. Mô hình chuỗi (tóm tắt)

| Loại | Ý tưởng | Hạn chế |
|------|---------|---------|
| **RNN / LSTM** | Duyệt từ trái sang phải, hidden state mang ngữ cảnh. | Khó song song, vanishing gradient, ngữ cảnh xa. |
| **BiLSTM** | Hai LSTM ngược chiều, concat → mỗi vị trí “thấy” cả câu. | Vẫn tuần tự theo thời gian. |
| **Transformer** | Self-attention: mỗi token attend toàn bộ chuỗi; song song hóa tốt. | Chi phí O(L²) theo độ dài L. |

Trong NLP hiện đại, **Transformer** (BERT, GPT, T5, v.v.) là nền tảng chính.

---

## 9.4. Pre-training và mô hình ngôn ngữ

### 9.4.1. Mô hình ngôn ngữ (Language Model – LM)

- **Mục tiêu**: Dự đoán token tiếp theo (hoặc token bị che) từ ngữ cảnh.
- **Causal LM (GPT-style)**: $P(x_t \mid x_1, \ldots, x_{t-1})$; dùng mask causal (chỉ nhìn trái). → Sinh văn bản, completion.
- **Masked LM (BERT-style)**: Che một phần token (ví dụ 15%), dự đoán token bị che từ toàn bộ câu. → Biểu diễn câu/đoạn, phân loại, NER, QA.

### 9.4.2. Các kiến trúc pre-train phổ biến

| Mô hình | Kiến trúc | Pre-train | Ứng dụng chính |
|---------|-----------|-----------|-----------------|
| **BERT** | Chỉ encoder | Masked LM + NSP (Next Sentence Prediction) | Phân loại, NER, QA, embedding câu. |
| **GPT** | Chỉ decoder (causal) | Causal LM | Sinh văn, completion, fine-tune phân loại. |
| **RoBERTa** | Giống BERT | Chỉ Masked LM, dữ liệu + batch lớn hơn. | Giống BERT, thường mạnh hơn. |
| **T5** | Encoder–decoder | “Text-to-text” (span corruption, v.v.). | Dịch, tóm tắt, QA, phân loại dạng text. |
| **ALBERT, ELECTRA, DeBERTa** | Biến thể encoder | Cách pre-train khác (sentence order, replaced token, v.v.). | Giống BERT. |

---

## 9.5. Nhiệm vụ NLP chính

### 9.5.1. Phân loại văn bản (Text Classification)

- **Đầu vào**: Câu/đoạn. **Đầu ra**: Nhãn lớp (sentiment, chủ đề, ý định).
- **Cách làm**: BERT (hoặc encoder khác) → lấy [CLS] hoặc pooling → Linear → Softmax. Loss: Cross-Entropy.

### 9.5.2. Gắn nhãn chuỗi (Sequence Labeling)

- **NER (Named Entity Recognition)**: Gán nhãn từng token (B-PER, I-PER, B-LOC, O, …).
- **POS (Part-of-Speech)**: Danh từ, động từ, …
- **Cách làm**: Encoder (BERT) → từng vị trí → Linear → Softmax (hoặc CRF). Loss: Cross-Entropy per token (có thể mask [PAD]).

### 9.5.3. Hỏi đáp (Question Answering – QA)

- **Extractive QA**: Cho đoạn văn (context) và câu hỏi; tìm **span** (đoạn con) trong context là câu trả lời. Mô hình: BERT → hai đầu ra (start, end) cho mỗi token. Loss: CE cho start và end.
- **Generative QA**: Sinh câu trả lời (T5, GPT). Đầu vào: "question: ... context: ...", đầu ra: câu trả lời.

### 9.5.4. Dịch máy (Machine Translation)

- **Đầu vào**: Câu nguồn (source). **Đầu ra**: Câu đích (target).
- **Kiến trúc**: Encoder–decoder (Transformer); encoder nguồn, decoder sinh đích từng token với cross-attention. Pre-train: T5, mBART; fine-tune trên cặp song ngữ.

### 9.5.5. Tóm tắt (Summarization)

- **Extractive**: Chọn câu có sẵn trong văn bản.
- **Abstractive**: Sinh câu mới (encoder–decoder, T5, BART). Loss: Cross-Entropy trên chuỗi tóm tắt.

### 9.5.6. Sinh văn bản (Text Generation)

- **Causal LM**: GPT; sinh từng token tiếp theo (autoregressive). Ứng dụng: completion, chatbot, viết truyện.
- **Điều khiển**: Thêm điều kiện (prompt, prefix) để điều hướng nội dung.

---

## 9.6. Pipeline xử lý văn bản điển hình

1. **Tokenize** câu → list token ID; **padding** / **truncation** về độ dài cố định (max_length).
2. **Embedding** (trong mô hình): token ID → vector; cộng positional encoding (Transformer).
3. **Encoder** (BERT/GPT/T5) → biểu diễn từng vị trí (và [CLS] nếu dùng).
4. **Head** tùy nhiệm vụ: Linear cho phân loại; Linear per token cho NER; Decoder cho sinh/dịch/tóm tắt.
5. **Loss**: CE (phân loại, NER, QA span), CE trên chuỗi (dịch, tóm tắt). **Metric**: Accuracy, F1, BLEU, ROUGE, v.v.

---

## 9.7. Tăng cường dữ liệu cho văn bản

| Kỹ thuật | Mô tả |
|----------|--------|
| **Back-translation** | Dịch câu sang ngôn ngữ khác rồi dịch lại → câu paraphrase. |
| **Synonym replacement** | Thay một số từ bằng từ đồng nghĩa (WordNet, embedding gần). |
| **Random insert/delete/swap** | Chèn/xóa/hoán đổi từ ngẫu nhiên. |
| **EDA (Easy Data Augmentation)** | Kết hợp replacement, insertion, deletion, swap với xác suất điều khiển. |
| **Mask/span replacement** | Dùng mô hình masked LM thay span bằng từ khác (contextual). |

---

## 9.8. Code mẫu (PyTorch – BERT phân loại với transformers)

```python
from transformers import AutoTokenizer, AutoModelForSequenceClassification
import torch

model_name = "bert-base-uncased"
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForSequenceClassification.from_pretrained(model_name, num_labels=2)

# Ví dụ: sentiment
texts = ["This is great!", "I don't like it."]
batch = tokenizer(texts, padding=True, truncation=True, max_length=128, return_tensors="pt")
outputs = model(**batch)
logits = outputs.logits
preds = logits.argmax(dim=-1)

# Fine-tune: loss = F.cross_entropy(logits, labels)
```

---

## 9.9. Tài liệu tham khảo

- [Natural language processing - Wikipedia](https://en.wikipedia.org/wiki/Natural_language_processing)
- [Byte pair encoding - Wikipedia](https://en.wikipedia.org/wiki/Byte_pair_encoding)
- Devlin et al., *BERT: Pre-training of Deep Bidirectional Transformers*
- Radford et al., *Language Models are Unsupervised Multitask Learners* (GPT-2)
- Hugging Face: transformers, tokenizers, datasets
