# 7. Bellman-Ford (Đường đi ngắn nhất – có cạnh âm)

## 7.1. Tổng quan

**Bellman-Ford** tìm đường đi ngắn nhất từ một đỉnh nguồn đến mọi đỉnh trên đồ thị **có trọng số bất kỳ** (kể cả âm). Thuật toán còn **phát hiện chu trình âm** (chu trình mà tổng trọng số < 0).

**Ứng dụng thực tế:** Đường đi khi có cạnh "âm" (ví dụ arbitrage tiền tệ: đổi A→B→C→A để lời); phát hiện chu trình âm trong mạng tài chính, ràng buộc.

| Thuộc tính | Giá trị |
|------------|--------|
| **Độ phức tạp thời gian** | O(V × E) |
| **Độ phức tạp không gian** | O(V) |
| **Cạnh âm** | Hỗ trợ |
| **Chu trình âm** | Phát hiện được |

---

## 7.2. Ý tưởng

- Khởi tạo: `dist[source] = 0`, các đỉnh khác = +∞.
- **Lặp (V - 1) lần**: Mỗi lần duyệt toàn bộ cạnh (u, v) với trọng số w; nếu `dist[u] + w < dist[v]` thì **nới lỏng** (relax): `dist[v] = dist[u] + w`.
- Sau V-1 vòng, nếu vẫn còn cạnh có thể nới lỏng → tồn tại **chu trình âm** và không có đường đi ngắn nhất hữu hạn từ nguồn đến một số đỉnh.

**Tại sao V-1 lần?** Đường đi đơn không có chu trình có tối đa V-1 cạnh; mỗi vòng nới lỏng tối thiểu một cạnh trên đường đó.

---

## 7.3. Mã giả (Pseudocode)

```
BELLMAN_FORD(graph, source):
    dist[source] = 0
    for each vertex v != source:
        dist[v] = INFINITY
    for i = 1 to V - 1:
        for each edge (u, v) with weight w:
            if dist[u] + w < dist[v]:
                dist[v] = dist[u] + w
    for each edge (u, v) with weight w:
        if dist[u] + w < dist[v]:
            return "Chu trình âm tồn tại"
    return dist
```

---

## 7.4. Ví dụ minh họa

Đồ thị: 0 → 1 (4), 0 → 2 (1), 2 → 1 (2), 1 → 3 (1), 3 → 2 (-3). Nguồn = 0.

- Sau vòng 1: dist = [0, 3, 1, 4]
- Sau vòng 2: cạnh (3,2) với w=-3 → dist[2] = 4-3 = 1 (đã bằng 1). Tiếp tục nới lỏng (2,1), (1,3), (3,2) → dist[2] có thể giảm xuống 2, 1, 0, -1... qua các vòng.
- Chu trình 2→1→3→2 có tổng trọng số 2+1-3 = 0; nếu có cạnh âm khác sẽ có chu trình âm.

---

## 7.5. Code mẫu

### Python

```python
def bellman_ford(n, edges, source):
    """
    n: số đỉnh (0..n-1)
    edges: list of (u, v, w)
    """
    INF = float('inf')
    dist = [INF] * n
    dist[source] = 0
    for _ in range(n - 1):
        for u, v, w in edges:
            if dist[u] != INF and dist[u] + w < dist[v]:
                dist[v] = dist[u] + w
    for u, v, w in edges:
        if dist[u] != INF and dist[u] + w < dist[v]:
            return None  # Chu trình âm
    return dist
```

### C++

```cpp
const int INF = 1e9;
vector<int> bellman_ford(int n, vector<tuple<int,int,int>>& edges, int source) {
    vector<int> dist(n, INF);
    dist[source] = 0;
    for (int i = 0; i < n - 1; i++)
        for (auto [u, v, w] : edges)
            if (dist[u] != INF && dist[u] + w < dist[v])
                dist[v] = dist[u] + w;
    for (auto [u, v, w] : edges)
        if (dist[u] != INF && dist[u] + w < dist[v])
            return {};  // Chu trình âm
    return dist;
}
```

---

## 7.6. So sánh với Dijkstra

| Tiêu chí | Bellman-Ford | Dijkstra |
|----------|--------------|----------|
| Cạnh âm | Có | Không |
| Chu trình âm | Phát hiện | Không áp dụng |
| Thời gian | O(V×E) | O((V+E) log V) với heap |
| Ứng dụng | Đồ thị có cạnh âm, phát hiện chu trình âm | Đồ thị trọng số không âm |

---

## 7.7. Tài liệu tham khảo

- [Bellman–Ford algorithm - Wikipedia](https://en.wikipedia.org/wiki/Bellman%E2%80%93Ford_algorithm)
- CLRS, Chương 24: Single-Source Shortest Paths
