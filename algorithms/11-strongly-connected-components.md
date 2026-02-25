# 11. Thành phần liên thông mạnh (Strongly Connected Components - SCC)

## 11.1. Tổng quan

**Thành phần liên thông mạnh (SCC)** của đồ thị có hướng là tập đỉnh lớn nhất sao cho **giữa hai đỉnh bất kỳ trong cùng một SCC đều có đường đi hai chiều**. Hai thuật toán phổ biến: **Kosaraju** và **Tarjan**.

| Thuật toán | Thời gian | Không gian | Ý tưởng chính |
|------------|-----------|------------|----------------|
| Kosaraju | O(V + E) | O(V) | DFS 2 lần: G, rồi G^T theo thứ tự kết thúc |
| Tarjan | O(V + E) | O(V) | Một lần DFS với low-link |

---

## 11.2. Kosaraju

### Ý tưởng

1. **Bước 1**: Chạy DFS trên G, lưu **thứ tự kết thúc** (đỉnh nào “xong” sau cùng thì ghi sau).
2. **Bước 2**: Xây đồ thị **chuyển vị** G^T (đảo chiều tất cả cạnh).
3. **Bước 3**: DFS trên G^T theo thứ tự **ngược** thứ tự kết thúc (đỉnh kết thúc muộn nhất trước). Mỗi lần DFS từ một đỉnh chưa thăm sẽ tìm được một SCC.

### Mã giả Kosaraju

```
KOSARAJU(graph):
    stack = []
    visited = set()
    for each vertex u:
        if u not visited: DFS1(u, graph, visited, stack)
    GT = transpose(graph)
    visited = set()
    SCCs = []
    while stack not empty:
        u = stack.pop()
        if u not visited:
            component = []
            DFS2(u, GT, visited, component)
            SCCs.append(component)
    return SCCs
```

### Code Kosaraju (Python)

```python
def kosaraju(n, edges):
    """edges: list of (u, v). Trả về list các SCC (mỗi SCC là list đỉnh)."""
    adj = [[] for _ in range(n)]
    for u, v in edges:
        adj[u].append(v)
    # DFS1: thứ tự kết thúc
    visited = [False] * n
    stack = []
    def dfs1(u):
        visited[u] = True
        for v in adj[u]:
            if not visited[v]:
                dfs1(v)
        stack.append(u)
    for i in range(n):
        if not visited[i]:
            dfs1(i)
    # Đồ thị chuyển vị
    rev = [[] for _ in range(n)]
    for u, v in edges:
        rev[v].append(u)
    # DFS2 trên G^T
    visited = [False] * n
    sccs = []
    def dfs2(u, comp):
        visited[u] = True
        comp.append(u)
        for v in rev[u]:
            if not visited[v]:
                dfs2(v, comp)
    while stack:
        u = stack.pop()
        if not visited[u]:
            comp = []
            dfs2(u, comp)
            sccs.append(comp)
    return sccs
```

---

## 11.3. Tarjan (tóm tắt)

- Một lần DFS, mỗi đỉnh u có `disc[u]` (thứ tự thăm) và `low[u]` (đỉnh có disc nhỏ nhất có thể đến được từ nhánh DFS của u).
- Đỉnh u là **gốc của một SCC** khi `disc[u] == low[u]`. Dùng stack để lưu đỉnh đang xử lý; khi gặp gốc SCC thì pop stack đến u và lấy ra một SCC.

---

## 11.4. Ví dụ minh họa

Đồ thị: 0→1, 1→2, 2→0 (chu trình); 2→3, 3→4, 4→3. SCC: {0,1,2} và {3,4}.

- **Kosaraju**: DFS1 trên G → stack (thứ tự kết thúc). DFS2 trên G^T từ đỉnh kết thúc muộn nhất → lần đầu DFS2 thăm được {0,1,2}, lần sau {3,4}.

---

## 11.5. Ứng dụng

- Nén đồ thị thành DAG của các SCC (đồ thị thành phần).
- Giải bài toán 2-SAT (cùng SCC thì cùng giá trị chân lý).
- Phân tích phụ thuộc trong mã nguồn.

---

## 11.6. Tài liệu tham khảo

- [Strongly connected component - Wikipedia](https://en.wikipedia.org/wiki/Strongly_connected_component)
- [Kosaraju's algorithm](https://en.wikipedia.org/wiki/Kosaraju%27s_algorithm)
- [Tarjan's strongly connected components algorithm](https://en.wikipedia.org/wiki/Tarjan%27s_strongly_connected_components_algorithm)
