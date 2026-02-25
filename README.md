# 📚 Thuật toán Khoa học Máy tính

Kho lưu trữ tài liệu chi tiết về các thuật toán **cơ bản**, **nâng cao** và chuyên mục **Học máy / Deep Learning / Transformer**, bao gồm **xử lý tín hiệu phi cấu trúc** (hình ảnh, âm thanh, văn bản/ký tự). Mỗi file trình bày đầy đủ: ý tưởng, giải thích, ví dụ, mã giả (hoặc công thức), phân tích độ phức tạp và code mẫu khi phù hợp.

---

## 📑 Mục lục – Thuật toán cơ bản

| # | Thuật toán | Mô tả ngắn | File |
|---|------------|------------|------|
| 1 | Tìm kiếm nhị phân | Tìm phần tử trong mảng đã sắp xếp | [01-binary-search.md](algorithms/01-binary-search.md) |
| 2 | Quick Sort | Sắp xếp nhanh chia để trị | [02-quick-sort.md](algorithms/02-quick-sort.md) |
| 3 | Dijkstra | Đường đi ngắn nhất (trọng số không âm) | [03-dijkstra.md](algorithms/03-dijkstra.md) |
| 4 | BFS & DFS | Duyệt đồ thị theo chiều rộng và chiều sâu | [04-bfs-dfs.md](algorithms/04-bfs-dfs.md) |
| 5 | Quy hoạch động | DP cơ bản: Fibonacci, Knapsack, Min Path | [05-dynamic-programming.md](algorithms/05-dynamic-programming.md) |
| 6 | Cây nhị phân tìm kiếm (BST) | Cấu trúc dữ liệu BST | [06-binary-search-tree.md](algorithms/06-binary-search-tree.md) |

---

## 📑 Mục lục – Thuật toán nâng cao

### Đồ thị nâng cao

| # | Thuật toán | Mô tả ngắn | File |
|---|------------|------------|------|
| 7 | Bellman-Ford | Đường đi ngắn nhất, cạnh âm, chu trình âm | [07-bellman-ford.md](algorithms/07-bellman-ford.md) |
| 8 | Floyd-Warshall | Đường đi ngắn nhất mọi cặp đỉnh | [08-floyd-warshall.md](algorithms/08-floyd-warshall.md) |
| 9 | Minimum Spanning Tree | Kruskal & Prim – cây bao trùm nhỏ nhất | [09-minimum-spanning-tree.md](algorithms/09-minimum-spanning-tree.md) |
| 10 | Topological Sort | Sắp xếp tô-pô trên DAG | [10-topological-sort.md](algorithms/10-topological-sort.md) |
| 11 | Strongly Connected Components | SCC – Kosaraju (và Tarjan) | [11-strongly-connected-components.md](algorithms/11-strongly-connected-components.md) |
| 12 | Maximum Flow | Luồng cực đại – Ford-Fulkerson, Edmonds-Karp | [12-maximum-flow.md](algorithms/12-maximum-flow.md) |

### Chuỗi & cấu trúc dữ liệu nâng cao

| # | Thuật toán | Mô tả ngắn | File |
|---|------------|------------|------|
| 13 | KMP | Tìm kiếm chuỗi Knuth-Morris-Pratt | [13-kmp-string-matching.md](algorithms/13-kmp-string-matching.md) |
| 14 | Segment Tree | Cây phân đoạn – truy vấn/cập nhật đoạn | [14-segment-tree.md](algorithms/14-segment-tree.md) |
| 15 | Disjoint Set Union (DSU) | Union-Find – gộp tập, kiểm tra cùng tập | [15-disjoint-set-union.md](algorithms/15-disjoint-set-union.md) |
| 16 | A* | Tìm đường heuristic A* | [16-a-star.md](algorithms/16-a-star.md) |
| 17 | Trie | Cây tiền tố – từ điển, autocomplete | [17-trie.md](algorithms/17-trie.md) |

### Quy hoạch động nâng cao

| # | Thuật toán | Mô tả ngắn | File |
|---|------------|------------|------|
| 18 | Advanced DP | Bitmask DP (TSP), LIS, LCS, Digit DP | [18-advanced-dp.md](algorithms/18-advanced-dp.md) |

---

## 📑 Chuyên mục: Học máy, Deep Learning & Transformer

### Nền tảng & Công cụ

| # | Chủ đề | Mô tả ngắn | File |
|---|--------|------------|------|
| 1 | Tổng quan Học máy | Supervised/Unsupervised, Regression/Classification, Overfitting, Metrics | [01-machine-learning-overview.md](machine-learning/01-machine-learning-overview.md) |
| 2 | Deep Learning | MLP, Backprop, Activation, Batch/Layer Norm, Dropout, Optimizer | [02-deep-learning.md](machine-learning/02-deep-learning.md) |
| 3 | Transformer & Attention | Self-Attention, Multi-Head, Encoder/Decoder, BERT/GPT/ViT | [03-transformer-attention.md](machine-learning/03-transformer-attention.md) |
| 4 | Loss Functions | MSE, MAE, Huber, BCE, CE, Focal, Triplet, Contrastive, KL, Wasserstein, Dice | [04-loss-functions.md](machine-learning/04-loss-functions.md) |
| 5 | Knowledge Distillation | Teacher-Student, Soft labels, Temperature, Response/Feature Distill | [05-knowledge-distillation.md](machine-learning/05-knowledge-distillation.md) |
| 6 | Optimizer & Regularization | SGD, Adam, AdamW, LAMB, Warmup, Dropout, Augmentation | [06-optimizers-regularization.md](machine-learning/06-optimizers-regularization.md) |

### Xử lý tín hiệu phi cấu trúc (ảnh, âm thanh, văn bản)

| # | Chủ đề | Mô tả ngắn | File |
|---|--------|------------|------|
| 7 | Computer Vision – Ảnh | CNN, ResNet/ViT, augmentation, Detection, Segmentation | [07-computer-vision-image.md](machine-learning/07-computer-vision-image.md) |
| 8 | Âm thanh & Giọng nói | Waveform, Spectrogram, Mel, ASR, TTS, wav2vec, Audio classification | [08-audio-speech-processing.md](machine-learning/08-audio-speech-processing.md) |
| 9 | NLP – Văn bản & Ký tự | Tokenization, BPE, Embedding, BERT/GPT, NER, QA, Dịch, Tóm tắt | [09-nlp-text-processing.md](machine-learning/09-nlp-text-processing.md) |

---

## Cấu trúc thư mục

```
khoahocmaytinh/
├── README.md
├── .gitignore
├── algorithms/          # Thuật toán cơ bản & nâng cao
│   ├── 01-binary-search.md
│   ├── ... (02–18)
│   └── 18-advanced-dp.md
└── machine-learning/   # Học máy, Deep Learning, Transformer
    ├── 01-machine-learning-overview.md
    ├── 02-deep-learning.md
    ├── 03-transformer-attention.md
    ├── 04-loss-functions.md
    ├── 05-knowledge-distillation.md
    ├── 06-optimizers-regularization.md
    ├── 07-computer-vision-image.md    # Ảnh
    ├── 08-audio-speech-processing.md # Âm thanh, giọng nói
    └── 09-nlp-text-processing.md     # Văn bản, ký tự
```

---

## Yêu cầu

- Chỉ cần trình duyệt hoặc công cụ đọc Markdown để xem trên GitHub.
- Code mẫu có thể chạy bằng **Python 3** (PyTorch/sklearn cho ML) hoặc **C++** (cho thuật toán).

## Giấy phép

Tài liệu mở, sử dụng cho mục đích học tập và tham khảo.
