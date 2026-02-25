
## 📖 Giới thiệu

Kho lưu trữ gồm:

- **Toán học ứng dụng** — xác suất thống kê, đạo hàm & giải tích, **chuỗi số** (cho Data Science & AI)
- **Thuật toán** — cơ bản (tìm kiếm, sắp xếp, đồ thị, DP, BST) và nâng cao (đồ thị nâng cao, chuỗi, cấu trúc dữ liệu, DP nâng cao)
- **Học máy (nền tảng)** — tổng quan ML, Deep Learning cơ bản, Loss, Optimizer, CV / Audio / NLP
- **Deep Learning** — Transformer, OCR, Speech-to-Text, Text-to-Speech, RAG & Agents

Mỗi tài liệu có: ý tưởng, giải thích, ví dụ, công thức/mã giả, độ phức tạp và code mẫu (Python/C++) khi phù hợp.

---

## 🧭 Mục lục nhanh

| Chuyên mục                                                      | Nội dung                                                                        |
| ----------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| [📊 Toán học ứng dụng](#-toán-học-ứng-dụng-data-science--ai) | Xác suất thống kê · Đạo hàm & Giải tích ·**Chuỗi số**         |
| [📐 Thuật toán](#-thuật-toán)                                    | Cơ bản (1–6) · Nâng cao: Đồ thị (7–12), Chuỗi & CTDL (13–17), DP (18) |
| [⚙️ Học máy (nền tảng)](#️-học-máy-nền-tảng--công-cụ)   | Tổng quan ML · DL · Attention · Loss · Optimizer · CV · Audio · NLP      |
| [🧠 Deep Learning](#-deep-learning-mô-hình--ứng-dụng)            | Transformer · OCR · STT · TTS · RAG & Agents                                 |
| [🌐 Ứng dụng thực tế](#-ứng-dụng-thực-tế-ví-dụ-dễ-hiểu)         | Ví dụ ứng dụng của từng phương pháp, thuật toán, mô hình                    |
| [📁 Cấu trúc thư mục](#-cấu-trúc-thư-mục)                    | Sơ đồ thư mục dự án                                                       |

---

## 📊 Toán học ứng dụng (Data Science & AI)

Toán nền tảng cho Data Science và AI: xác suất thống kê, đạo hàm & giải tích (gradient, backprop), chuỗi số (hội tụ, cấp số nhân, Taylor).

| # | Chủ đề                  | Nội dung chính                                                           | Tài liệu                                                            |
| -: | -------------------------- | -------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| 1 | Xác suất và Thống kê  | Biến ngẫu nhiên, phân phối, Bayes, MLE, kiểm định, A/B test        | [01-probability-statistics.md](math-for-ds/01-probability-statistics.md) |
| 2 | Đạo hàm và Giải tích | Gradient, chain rule, backprop, matrix calculus, loss & activation         | [02-derivatives-calculus.md](math-for-ds/02-derivatives-calculus.md)     |
| 3 | Chuỗi số                 | Dãy số, chuỗi số, hội tụ/phân kỳ, cấp số nhân, p-chuỗi, Taylor | [03-sequences-series.md](math-for-ds/03-sequences-series.md)             |

---

## 📐 Thuật toán Computer Science

### Thuật toán cơ bản

| # | Thuật toán                     | Mô tả                                            | Tài liệu                                                     |
| -: | -------------------------------- | -------------------------------------------------- | -------------------------------------------------------------- |
| 1 | Tìm kiếm nhị phân            | Tìm phần tử trong mảng đã sắp xếp          | [01-binary-search.md](algorithms/01-binary-search.md)             |
| 2 | Quick Sort                       | Sắp xếp nhanh chia để trị                     | [02-quick-sort.md](algorithms/02-quick-sort.md)                   |
| 3 | Dijkstra                         | Đường đi ngắn nhất (trọng số không âm)   | [03-dijkstra.md](algorithms/03-dijkstra.md)                       |
| 4 | BFS & DFS                        | Duyệt đồ thị theo chiều rộng và chiều sâu | [04-bfs-dfs.md](algorithms/04-bfs-dfs.md)                         |
| 5 | Quy hoạch động                | DP cơ bản: Fibonacci, Knapsack, Min Path         | [05-dynamic-programming.md](algorithms/05-dynamic-programming.md) |
| 6 | Cây nhị phân tìm kiếm (BST) | Cấu trúc dữ liệu BST                           | [06-binary-search-tree.md](algorithms/06-binary-search-tree.md)   |

### Thuật toán nâng cao

**Đồ thị**

|  # | Thuật toán                  | Mô tả                                             | Tài liệu                                                                         |
| -: | ----------------------------- | --------------------------------------------------- | ---------------------------------------------------------------------------------- |
|  7 | Bellman-Ford                  | Đường đi ngắn nhất, cạnh âm, chu trình âm | [07-bellman-ford.md](algorithms/07-bellman-ford.md)                                   |
|  8 | Floyd-Warshall                | Đường đi ngắn nhất mọi cặp đỉnh           | [08-floyd-warshall.md](algorithms/08-floyd-warshall.md)                               |
|  9 | Minimum Spanning Tree         | Kruskal & Prim — cây bao trùm nhỏ nhất         | [09-minimum-spanning-tree.md](algorithms/09-minimum-spanning-tree.md)                 |
| 10 | Topological Sort              | Sắp xếp tô-pô trên DAG                         | [10-topological-sort.md](algorithms/10-topological-sort.md)                           |
| 11 | Strongly Connected Components | SCC — Kosaraju (và Tarjan)                        | [11-strongly-connected-components.md](algorithms/11-strongly-connected-components.md) |
| 12 | Maximum Flow                  | Luồng cực đại — Ford-Fulkerson, Edmonds-Karp   | [12-maximum-flow.md](algorithms/12-maximum-flow.md)                                   |

**Chuỗi & Cấu trúc dữ liệu**

|  # | Thuật toán             | Mô tả                                          | Tài liệu                                                     |
| -: | ------------------------ | ------------------------------------------------ | -------------------------------------------------------------- |
| 13 | KMP                      | Tìm kiếm chuỗi Knuth-Morris-Pratt             | [13-kmp-string-matching.md](algorithms/13-kmp-string-matching.md) |
| 14 | Segment Tree             | Cây phân đoạn — truy vấn/cập nhật đoạn | [14-segment-tree.md](algorithms/14-segment-tree.md)               |
| 15 | Disjoint Set Union (DSU) | Union-Find — gộp tập, kiểm tra cùng tập    | [15-disjoint-set-union.md](algorithms/15-disjoint-set-union.md)   |
| 16 | A*                       | Tìm đường heuristic A*                       | [16-a-star.md](algorithms/16-a-star.md)                           |
| 17 | Trie                     | Cây tiền tố — từ điển, autocomplete       | [17-trie.md](algorithms/17-trie.md)                               |

**Quy hoạch động nâng cao**

|  # | Thuật toán | Mô tả                              | Tài liệu                                     |
| -: | ------------ | ------------------------------------ | ---------------------------------------------- |
| 18 | Advanced DP  | Bitmask DP (TSP), LIS, LCS, Digit DP | [18-advanced-dp.md](algorithms/18-advanced-dp.md) |

---

## ⚙️ Học máy (Nền tảng & Công cụ)

### Nền tảng & Công cụ

| # | Chủ đề                  | Nội dung chính                                                             | Tài liệu                                                                       |
| -: | -------------------------- | ---------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| 1 | Tổng quan Học máy       | Supervised/Unsupervised, Regression/Classification, Overfitting, Metrics     | [01-machine-learning-overview.md](machine-learning/01-machine-learning-overview.md) |
| 2 | Deep Learning              | MLP, Backprop, Activation, Batch/Layer Norm, Dropout, Optimizer              | [02-deep-learning.md](machine-learning/02-deep-learning.md)                         |
| 3 | Transformer & Attention    | Self-Attention, Multi-Head, Encoder/Decoder, BERT/GPT/ViT                    | [03-transformer-attention.md](machine-learning/03-transformer-attention.md)         |
| 4 | Loss Functions             | MSE, MAE, Huber, BCE, CE, Focal, Triplet, Contrastive, KL, Wasserstein, Dice | [04-loss-functions.md](machine-learning/04-loss-functions.md)                       |
| 5 | Knowledge Distillation     | Teacher-Student, Soft labels, Temperature, Response/Feature Distill          | [05-knowledge-distillation.md](machine-learning/05-knowledge-distillation.md)       |
| 6 | Optimizer & Regularization | SGD, Adam, AdamW, LAMB, Warmup, Dropout, Augmentation                        | [06-optimizers-regularization.md](machine-learning/06-optimizers-regularization.md) |

### Xử lý tín hiệu phi cấu trúc (Ảnh · Âm thanh · Văn bản)

| # | Chủ đề                  | Nội dung chính                                                  | Tài liệu                                                                   |
| -: | -------------------------- | ----------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| 7 | Computer Vision — Ảnh    | CNN, ResNet/ViT, augmentation, Detection, Segmentation            | [07-computer-vision-image.md](machine-learning/07-computer-vision-image.md)     |
| 8 | Âm thanh & Giọng nói    | Waveform, Spectrogram, Mel, ASR, TTS, wav2vec                     | [08-audio-speech-processing.md](machine-learning/08-audio-speech-processing.md) |
| 9 | NLP — Văn bản & Ký tự | Tokenization, BPE, Embedding, BERT/GPT, NER, QA, Dịch, Tóm tắt | [09-nlp-text-processing.md](machine-learning/09-nlp-text-processing.md)         |

---

## 🧠 Deep Learning (Mô hình & Ứng dụng)

Transformer, mô hình đặc thù (OCR, STT, TTS) và ứng dụng (RAG, Agents).

| # | Chủ đề             | Nội dung chính                                                       | Tài liệu                                                                |
| -: | --------------------- | ---------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| 1 | Mô hình Transformer | Encoder/Decoder-only, BERT, GPT, LLaMA, T5, ViT, đa modal             | [01-transformer-models.md](deep-learning/01-transformer-models.md)           |
| 2 | OCR                   | Text detection (CRAFT, DBNet), recognition (TrOCR), PaddleOCR, EasyOCR | [02-ocr.md](deep-learning/02-ocr.md)                                         |
| 3 | Speech-to-Text (ASR)  | Whisper, wav2vec, Conformer, CTC/attention, streaming                  | [03-speech-to-text.md](deep-learning/03-speech-to-text.md)                   |
| 4 | Text-to-Speech (TTS)  | Tacotron, FastSpeech, VITS, HiFi-GAN, Bark, voice cloning              | [04-text-to-speech.md](deep-learning/04-text-to-speech.md)                   |
| 5 | RAG & Agents          | Retrieve + generate, tool use, ReAct, chatbot tài liệu               | [05-applications-rag-agents.md](deep-learning/05-applications-rag-agents.md) |

---

## 🌐 Ứng dụng thực tế (ví dụ dễ hiểu)

Các phương pháp, thuật toán và mô hình trong repo được dùng trong đời sống và sản phẩm như sau.

### 📊 Toán học ứng dụng

| Chủ đề | Ứng dụng thực tế |
|--------|------------------|
| **Xác suất & Thống kê** | **A/B test**: So sánh tỷ lệ click (CTR) giữa hai giao diện web để chọn bản tốt hơn. **Bayes**: Lọc spam (P(spam\|từ) từ tần suất từ trong thư spam/không spam). **MLE**: Ước lượng tham số mô hình từ dữ liệu (ví dụ xác suất chuyển đổi từ log). |
| **Đạo hàm & Giải tích** | **Gradient descent**: Cách mọi mô hình ML (từ hồi quy đến neural net) “tìm điểm loss thấp nhất”. **Backprop**: Tính gradient từng layer trong mạng để cập nhật trọng số khi train. **Chain rule**: Nền tảng của autograd (PyTorch, TensorFlow). |
| **Chuỗi số** | **Learning rate decay**: Giảm learning rate theo bước train (dạng $\eta_n = \eta_0 \cdot r^n$ hoặc $\eta_0/n$) để hội tụ ổn. **Cấp số nhân**: Tính xác suất “lần đầu thành công” trong phân phối hình học (ví dụ số lần quảng cáo đến khi có click). |

### 📐 Thuật toán

| Thuật toán | Ứng dụng thực tế |
|------------|------------------|
| **Tìm kiếm nhị phân** | Tìm tên trong danh bạ đã sắp theo ABC; tìm giá trong danh sách giá đã sort; game “đoán số trong khoảng 1–100” với gợi ý lớn hơn/nhỏ hơn. |
| **Quick Sort** | Sắp xếp danh sách sản phẩm theo giá, ngày, tên; chuẩn bị dữ liệu trước khi dùng tìm kiếm nhị phân hoặc merge. |
| **Dijkstra** | **Google Maps / Waze**: Đường đi ngắn nhất (theo thời gian hoặc km). Định tuyến trong mạng (router chọn đường ít trễ nhất). |
| **BFS / DFS** | **BFS**: Tìm đường ngắn nhất theo số cạnh (mê cung, mạng xã hội “bạn của bạn”). **DFS**: Duyệt cây thư mục, crawl web, kiểm tra chu trình trong dependency (build phần mềm). |
| **Quy hoạch động** | **Knapsack**: Chọn gói hàng tối ưu (trọng lượng/giá). **Min path**: Tổng chi phí nhỏ nhất qua nhiều bước. **Fibonacci**: Cơ sở để hiểu memoization và DP. |
| **BST** | Index trong database (tìm, thêm, xóa theo khóa nhanh); từ điển trong bộ nhớ; cây tìm kiếm trong game (object theo vùng). |
| **Bellman-Ford / Floyd-Warshall** | Đường đi khi có cạnh “âm” (ví dụ arbitrage tiền tệ: đổi A→B→C→A để lời). Floyd: bảng khoảng cách giữa mọi cặp sân bay. |
| **MST (Kruskal / Prim)** | Thiết kế mạng (điện, cáp, nước) nối N điểm với tổng chiều dài/cost nhỏ nhất; clustering theo khoảng cách. |
| **Topological Sort** | Thứ tự build các module phần mềm (module A phụ thuộc B → build B trước). Thứ tự học môn (môn tiên quyết trước). |
| **SCC** | Phân tích mạng: nhóm trang link lẫn nhau; nhóm tài khoản giao dịch liên quan; chu trình phụ thuộc trong code. |
| **Maximum Flow** | Luồng tối đa trong mạng (ống dẫn, băng thông); ghép cặp (job–worker, sinh viên–đề tài) khi mỗi bên có capacity. |
| **KMP** | Tìm kiếm chuỗi con trong văn bản (Ctrl+F nâng cao, so khớp mẫu DNA), không cần quay lại từ đầu khi không khớp. |
| **Segment Tree** | Truy vấn min/max/sum trên đoạn và cập nhật điểm (điểm số theo thời gian, giá cổ phiếu theo khoảng ngày). |
| **DSU (Union-Find)** | “Hai người có cùng nhóm không?” khi gộp nhóm theo quan hệ (bạn bè, kết nối mạng); tìm thành phần liên thông động. |
| **A\*** | Game: NPC tìm đường tránh chướng ngại. Robot: đường đi nhanh có thông tin heuristic (ước lượng khoảng cách đến đích). |
| **Trie** | Gợi ý từ khi gõ (autocomplete), kiểm tra chính tả, từ điển tiền tố, đếm số từ có tiền tố cho trước. |
| **Advanced DP** | **TSP**: Lộ trình giao hàng qua N điểm mỗi điểm đúng 1 lần. **LIS/LCS**: So sánh chuỗi (diff văn bản, genome). **Digit DP**: Đếm số trong khoảng thỏa điều kiện (ví dụ không chứa 4). |

### ⚙️ Học máy (nền tảng)

| Chủ đề | Ứng dụng thực tế |
|--------|------------------|
| **Tổng quan ML** | **Regression**: Dự đoán giá nhà, doanh số, nhiệt độ. **Classification**: Spam/không spam, nhận diện ảnh (chó/mèo), sentiment (tích cực/tiêu cực). **Clustering**: Nhóm khách hàng, gom tin tức cùng chủ đề. |
| **Deep Learning cơ bản** | Mạng MLP làm bộ dự đoán (tabular), layer ẩn học đặc trưng; Backprop + optimizer là nền tảng train mọi mô hình neural. |
| **Transformer & Attention** | BERT/GPT dùng trong search, gợi ý, phân loại văn bản; attention giúp mô hình “nhìn” đúng từ/cụm quan trọng trong câu. |
| **Loss Functions** | MSE cho dự báo số; Cross-entropy cho phân loại; Focal Loss cho detection (nhiều nền, ít vật thể); Contrastive cho embedding (tìm ảnh tương tự). |
| **Optimizer & Regularization** | Adam/AdamW: train transformer, CNN. Dropout, augmentation: giảm overfitting. L2: trọng số không quá lớn. |
| **Computer Vision** | Nhận diện ảnh (Face ID, phân loại sản phẩm); detection (xe, người trên camera); segmentation (làm phông ảnh, y tế). |
| **Âm thanh & Giọng nói** | Nhận diện giọng nói (trợ lý ảo, phụ đề); phân loại âm thanh (nhạc cụ, tiếng khóc); TTS (đọc tin, sách nói). |
| **NLP** | Phân loại (sentiment, intent); NER (trích tên người, địa điểm); dịch máy; tóm tắt; chatbot, search semantic. |

### 🧠 Deep Learning (mô hình & ứng dụng)

| Chủ đề | Ứng dụng thực tế |
|--------|------------------|
| **Transformer (BERT, GPT, LLaMA)** | **ChatGPT, Claude, Gemini**: Chat, viết văn, code. **Tìm kiếm**: Embedding câu (BERT) để tìm tài liệu giống nghĩa. **Phân loại/NER**: Fine-tune BERT cho sentiment, trích thông tin. |
| **OCR** | Scan CMND, hóa đơn, sách → chữ có thể copy/search; đọc biển số; số hóa tài liệu cũ; app đọc chữ qua camera. |
| **Speech-to-Text (ASR)** | Phụ đề video/meeting trực tiếp; trợ lý ảo (Alexa, Siri, Google); ghi chú từ giọng nói; hỗ trợ người khiếm thính. |
| **Text-to-Speech (TTS)** | Trợ lý ảo trả lời bằng giọng; sách nói; đọc tin; chatbot có giọng; clone giọng cho video/podcast. |
| **RAG & Agents** | **RAG**: Chatbot hỏi đáp trên PDF/công văn nội bộ (retrieve đoạn liên quan rồi LLM trả lời). **Agents**: Trợ lý đặt lịch, tra thời tiết, tìm kiếm web, gọi API thay người dùng. |

---
