<div align="center">

# 📚 Thuật toán & Khoa học Máy tính

**Tài liệu thuật toán, toán ứng dụng, Học máy và Deep Learning** — ý tưởng, công thức, ví dụ và code mẫu.

[![License](https://img.shields.io/badge/license-Open%20for%20learning-green.svg)](./README.md)

</div>

---

## 📖 Giới thiệu

Kho lưu trữ gồm:

- **Thuật toán** — cơ bản (tìm kiếm, sắp xếp, đồ thị, DP, BST) và nâng cao (đồ thị nâng cao, chuỗi, cấu trúc dữ liệu, DP nâng cao)
- **Toán học ứng dụng** — xác suất thống kê, đạo hàm & giải tích (cho Data Science & AI)
- **Deep Learning** — Transformer, OCR, Speech-to-Text, Text-to-Speech, RAG & Agents
- **Học máy (nền tảng)** — tổng quan ML, Deep Learning cơ bản, Loss, Optimizer, CV / Audio / NLP

Mỗi tài liệu có: ý tưởng, giải thích, ví dụ, công thức/mã giả, độ phức tạp và code mẫu (Python/C++) khi phù hợp.

---

## 🧭 Mục lục nhanh

| Chuyên mục | Nội dung |
|------------|----------|
| [📐 Thuật toán](#-thuật-toán) | Cơ bản (1–6) · Nâng cao: Đồ thị (7–12), Chuỗi & CTDL (13–17), DP (18) |
| [📊 Toán học ứng dụng](#-toán-học-ứng-dụng-data-science--ai) | Xác suất thống kê · Đạo hàm & Giải tích |
| [🧠 Deep Learning](#-deep-learning-mô-hình--ứng-dụng) | Transformer · OCR · STT · TTS · RAG & Agents |
| [⚙️ Học máy (nền tảng)](#️-học-máy-nền-tảng--công-cụ) | Tổng quan ML · DL · Attention · Loss · Optimizer · CV · Audio · NLP |
| [📁 Cấu trúc thư mục](#-cấu-trúc-thư-mục) | Sơ đồ thư mục dự án |

---

## 📐 Thuật toán

### Thuật toán cơ bản

| # | Thuật toán | Mô tả | Tài liệu |
|--:|------------|-------|----------|
| 1 | Tìm kiếm nhị phân | Tìm phần tử trong mảng đã sắp xếp | [01-binary-search.md](algorithms/01-binary-search.md) |
| 2 | Quick Sort | Sắp xếp nhanh chia để trị | [02-quick-sort.md](algorithms/02-quick-sort.md) |
| 3 | Dijkstra | Đường đi ngắn nhất (trọng số không âm) | [03-dijkstra.md](algorithms/03-dijkstra.md) |
| 4 | BFS & DFS | Duyệt đồ thị theo chiều rộng và chiều sâu | [04-bfs-dfs.md](algorithms/04-bfs-dfs.md) |
| 5 | Quy hoạch động | DP cơ bản: Fibonacci, Knapsack, Min Path | [05-dynamic-programming.md](algorithms/05-dynamic-programming.md) |
| 6 | Cây nhị phân tìm kiếm (BST) | Cấu trúc dữ liệu BST | [06-binary-search-tree.md](algorithms/06-binary-search-tree.md) |

### Thuật toán nâng cao

**Đồ thị**

| # | Thuật toán | Mô tả | Tài liệu |
|--:|------------|-------|----------|
| 7 | Bellman-Ford | Đường đi ngắn nhất, cạnh âm, chu trình âm | [07-bellman-ford.md](algorithms/07-bellman-ford.md) |
| 8 | Floyd-Warshall | Đường đi ngắn nhất mọi cặp đỉnh | [08-floyd-warshall.md](algorithms/08-floyd-warshall.md) |
| 9 | Minimum Spanning Tree | Kruskal & Prim — cây bao trùm nhỏ nhất | [09-minimum-spanning-tree.md](algorithms/09-minimum-spanning-tree.md) |
| 10 | Topological Sort | Sắp xếp tô-pô trên DAG | [10-topological-sort.md](algorithms/10-topological-sort.md) |
| 11 | Strongly Connected Components | SCC — Kosaraju (và Tarjan) | [11-strongly-connected-components.md](algorithms/11-strongly-connected-components.md) |
| 12 | Maximum Flow | Luồng cực đại — Ford-Fulkerson, Edmonds-Karp | [12-maximum-flow.md](algorithms/12-maximum-flow.md) |

**Chuỗi & Cấu trúc dữ liệu**

| # | Thuật toán | Mô tả | Tài liệu |
|--:|------------|-------|----------|
| 13 | KMP | Tìm kiếm chuỗi Knuth-Morris-Pratt | [13-kmp-string-matching.md](algorithms/13-kmp-string-matching.md) |
| 14 | Segment Tree | Cây phân đoạn — truy vấn/cập nhật đoạn | [14-segment-tree.md](algorithms/14-segment-tree.md) |
| 15 | Disjoint Set Union (DSU) | Union-Find — gộp tập, kiểm tra cùng tập | [15-disjoint-set-union.md](algorithms/15-disjoint-set-union.md) |
| 16 | A* | Tìm đường heuristic A* | [16-a-star.md](algorithms/16-a-star.md) |
| 17 | Trie | Cây tiền tố — từ điển, autocomplete | [17-trie.md](algorithms/17-trie.md) |

**Quy hoạch động nâng cao**

| # | Thuật toán | Mô tả | Tài liệu |
|--:|------------|-------|----------|
| 18 | Advanced DP | Bitmask DP (TSP), LIS, LCS, Digit DP | [18-advanced-dp.md](algorithms/18-advanced-dp.md) |

---

## 📊 Toán học ứng dụng (Data Science & AI)

Toán nền tảng cho Data Science và AI: xác suất thống kê, đạo hàm và giải tích (gradient, backprop, tối ưu).

| # | Chủ đề | Nội dung chính | Tài liệu |
|--:|--------|----------------|----------|
| 1 | Xác suất và Thống kê | Biến ngẫu nhiên, phân phối, Bayes, MLE, kiểm định, A/B test | [01-probability-statistics.md](math-for-ds/01-probability-statistics.md) |
| 2 | Đạo hàm và Giải tích | Gradient, chain rule, backprop, matrix calculus, loss & activation | [02-derivatives-calculus.md](math-for-ds/02-derivatives-calculus.md) |

---

## 🧠 Deep Learning (Mô hình & Ứng dụng)

Transformer, mô hình đặc thù (OCR, STT, TTS) và ứng dụng (RAG, Agents).

| # | Chủ đề | Nội dung chính | Tài liệu |
|--:|--------|----------------|----------|
| 1 | Mô hình Transformer | Encoder/Decoder-only, BERT, GPT, LLaMA, T5, ViT, đa modal | [01-transformer-models.md](deep-learning/01-transformer-models.md) |
| 2 | OCR | Text detection (CRAFT, DBNet), recognition (TrOCR), PaddleOCR, EasyOCR | [02-ocr.md](deep-learning/02-ocr.md) |
| 3 | Speech-to-Text (ASR) | Whisper, wav2vec, Conformer, CTC/attention, streaming | [03-speech-to-text.md](deep-learning/03-speech-to-text.md) |
| 4 | Text-to-Speech (TTS) | Tacotron, FastSpeech, VITS, HiFi-GAN, Bark, voice cloning | [04-text-to-speech.md](deep-learning/04-text-to-speech.md) |
| 5 | RAG & Agents | Retrieve + generate, tool use, ReAct, chatbot tài liệu | [05-applications-rag-agents.md](deep-learning/05-applications-rag-agents.md) |

---

## ⚙️ Học máy (Nền tảng & Công cụ)

### Nền tảng & Công cụ

| # | Chủ đề | Nội dung chính | Tài liệu |
|--:|--------|----------------|----------|
| 1 | Tổng quan Học máy | Supervised/Unsupervised, Regression/Classification, Overfitting, Metrics | [01-machine-learning-overview.md](machine-learning/01-machine-learning-overview.md) |
| 2 | Deep Learning | MLP, Backprop, Activation, Batch/Layer Norm, Dropout, Optimizer | [02-deep-learning.md](machine-learning/02-deep-learning.md) |
| 3 | Transformer & Attention | Self-Attention, Multi-Head, Encoder/Decoder, BERT/GPT/ViT | [03-transformer-attention.md](machine-learning/03-transformer-attention.md) |
| 4 | Loss Functions | MSE, MAE, Huber, BCE, CE, Focal, Triplet, Contrastive, KL, Wasserstein, Dice | [04-loss-functions.md](machine-learning/04-loss-functions.md) |
| 5 | Knowledge Distillation | Teacher-Student, Soft labels, Temperature, Response/Feature Distill | [05-knowledge-distillation.md](machine-learning/05-knowledge-distillation.md) |
| 6 | Optimizer & Regularization | SGD, Adam, AdamW, LAMB, Warmup, Dropout, Augmentation | [06-optimizers-regularization.md](machine-learning/06-optimizers-regularization.md) |

### Xử lý tín hiệu phi cấu trúc (Ảnh · Âm thanh · Văn bản)

| # | Chủ đề | Nội dung chính | Tài liệu |
|--:|--------|----------------|----------|
| 7 | Computer Vision — Ảnh | CNN, ResNet/ViT, augmentation, Detection, Segmentation | [07-computer-vision-image.md](machine-learning/07-computer-vision-image.md) |
| 8 | Âm thanh & Giọng nói | Waveform, Spectrogram, Mel, ASR, TTS, wav2vec | [08-audio-speech-processing.md](machine-learning/08-audio-speech-processing.md) |
| 9 | NLP — Văn bản & Ký tự | Tokenization, BPE, Embedding, BERT/GPT, NER, QA, Dịch, Tóm tắt | [09-nlp-text-processing.md](machine-learning/09-nlp-text-processing.md) |

---

## 📁 Cấu trúc thư mục

<details>
<summary>Click để mở rộng / thu gọn</summary>

```
khoahocmaytinh/
├── README.md
├── .gitignore
├── algorithms/          # Thuật toán cơ bản & nâng cao (01–18)
├── math-for-ds/         # Toán ứng dụng: Xác suất, Đạo hàm
├── deep-learning/      # Transformer, OCR, STT, TTS, RAG & Agents
└── machine-learning/   # Nền tảng ML: Tổng quan, Loss, Optimizer, CV, Audio, NLP
```

</details>

---

## ✅ Yêu cầu & Giấy phép

| | |
|---|---|
| **Xem tài liệu** | Trình duyệt hoặc công cụ đọc Markdown (GitHub, VS Code, …). |
| **Chạy code mẫu** | **Python 3** (PyTorch / sklearn cho ML) hoặc **C++** (thuật toán). |
| **Giấy phép** | Tài liệu mở, dùng cho học tập và tham khảo. |

---

<div align="center">

*Nếu thấy hữu ích, hãy ⭐ star repo.*

</div>
