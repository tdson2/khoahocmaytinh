# 8. Floyd-Warshall (Đường đi ngắn nhất mọi cặp đỉnh)

## 8.1. Tổng quan

**Floyd-Warshall** tìm **đường đi ngắn nhất giữa mọi cặp đỉnh** trên đồ thị có hướng, có trọng số (cho phép cạnh âm, không cho phép chu trình âm). Thuật toán dùng **quy hoạch động** trên tập đỉnh trung gian.

**Ứng dụng thực tế:** Bảng khoảng cách giữa mọi cặp sân bay/thành phố; tổng hợp đường đi ngắn nhất toàn cục khi không cần cập nhật thường xuyên.

| Thuộc tính | Giá trị |
|------------|--------|
| **Độ phức tạp thời gian** | O(V³) |
| **Độ phức tạp không gian** | O(V²) |
| **Đồ thị** | Có hướng hoặc vô hướng, có trọng số |
| **Chu trình âm** | Phát hiện được (đường chéo âm) |

---

## 8.2. Ý tưởng

- `dist[i][j]` = độ dài đường đi ngắn nhất từ i đến j (chỉ đi qua các đỉnh trung gian trong tập {0, 1, ..., k}).
- Khởi tạo: `dist[i][j]` = trọng số cạnh (i→j), không có cạnh thì +∞; `dist[i][i] = 0`.
- Với mỗi đỉnh trung gian `k` từ 0 đến V-1: cập nhật  
  `dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])`.
- Sau khi chạy xong, nếu tồn tại `dist[i][i] < 0` thì có **chu trình âm** đi qua i.

---

## 8.3. Mã giả (Pseudocode)

```
FLOYD_WARSHALL(graph):
    dist = ma trận kề (không có cạnh = INF, dist[i][i] = 0)
    for k = 0 to V - 1:
        for i = 0 to V - 1:
            for j = 0 to V - 1:
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
    return dist
```

---

## 8.4. Ví dụ minh họa

Đồ thị 4 đỉnh: 0→1(3), 0→2(6), 1→2(2), 2→3(1), 3→1(1). Ma trận ban đầu (INF = ∞):

```
    0   1   2   3
0   0   3   6   ∞
1   ∞   0   2   ∞
2   ∞   ∞   0   1
3   ∞   1   ∞   0
```

Sau khi chạy Floyd-Warshall, dist[0][3] = 0→1→2→3 = 6, dist[3][0] vẫn ∞ (đồ thị có hướng).

---

## 8.5. Code mẫu

### Python

```python
def floyd_warshall(n, graph):
    """
    graph: ma trận kề n×n, graph[i][j] = trọng số (i->j), INF nếu không có cạnh
    """
    INF = float('inf')
    dist = [row[:] for row in graph]
    for i in range(n):
        dist[i][i] = min(0, dist[i][i])
    for k in range(n):
        for i in range(n):
            for j in range(n):
                if dist[i][k] != INF and dist[k][j] != INF:
                    dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
    # Kiểm tra chu trình âm
    for i in range(n):
        if dist[i][i] < 0:
            return None  # Chu trình âm
    return dist
```

### C++

```cpp
const int INF = 1e9;
vector<vector<int>> floyd_warshall(vector<vector<int>> graph) {
    int n = graph.size();
    auto dist = graph;
    for (int k = 0; k < n; k++)
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                if (dist[i][k] != INF && dist[k][j] != INF)
                    dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
    return dist;
}
```

---

## 8.6. Ứng dụng

- Đường đi ngắn nhất mọi cặp trong đồ thị dày (nhiều cạnh).
- Phát hiện chu trình âm.
- **Đóng bắc cầu** (transitive closure): biến thành 0/1 (có đường đi hay không).

---

## 8.7. Tài liệu tham khảo

- [Floyd–Warshall algorithm - Wikipedia](https://en.wikipedia.org/wiki/Floyd%E2%80%93Warshall_algorithm)
- CLRS, Chương 25: All-Pairs Shortest Paths
