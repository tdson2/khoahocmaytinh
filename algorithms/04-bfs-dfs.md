# 4. BFS và DFS (Duyệt đồ thị)

## 4.1. Tổng quan

**BFS (Breadth-First Search)** và **DFS (Depth-First Search)** là hai cách duyệt cơ bản trên đồ thị (vô hướng hoặc có hướng). BFS dùng **hàng đợi**, DFS dùng **ngăn xếp** (hoặc đệ quy).

| Thuật toán | Cấu trúc dữ liệu | Thời gian | Không gian |
|------------|------------------|-----------|------------|
| BFS | Hàng đợi (Queue) | O(V + E) | O(V) |
| DFS | Ngăn xếp / Đệ quy | O(V + E) | O(V) |

---

## 4.2. BFS (Duyệt theo chiều rộng)

### Ý tưởng

- Bắt đầu từ một đỉnh, thăm tất cả đỉnh kề trước khi đi sâu hơn.
- Dùng hàng đợi: đẩy đỉnh vào queue, lấy ra và thăm các đỉnh chưa thăm kề với nó rồi đẩy vào queue.

### Mã giả BFS

```
BFS(graph, start):
    visited = set()
    queue = [start]
    visited.add(start)
    while queue not empty:
        u = queue.dequeue()
        process(u)
        for each neighbor v of u:
            if v not in visited:
                visited.add(v)
                queue.enqueue(v)
```

### Ứng dụng BFS

- Tìm đường đi ngắn nhất (số cạnh) trên đồ thị **không trọng số**.
- Kiểm tra đồ thị hai phía (tô màu 2 màu).
- Tìm các thành phần liên thông.

---

## 4.3. DFS (Duyệt theo chiều sâu)

### Ý tưởng

- Đi sâu theo một nhánh đến hết, rồi quay lại (backtrack) và thử nhánh khác.
- Có thể cài bằng đệ quy hoặc stack.

### Mã giả DFS (đệ quy)

```
DFS(graph, u, visited):
    visited.add(u)
    process(u)
    for each neighbor v of u:
        if v not in visited:
            DFS(graph, v, visited)
```

### Ứng dụng DFS

- Kiểm tra chu trình, tìm đường đi.
- Topological sort (trên DAG).
- Tìm thành phần liên thông mạnh (SCC) khi kết hợp với thứ tự kết thúc.

---

## 4.4. Ví dụ đồ thị

Đồ thị vô hướng (danh sách kề):

```
0 -- 1
|    |
2 -- 3
```

- **BFS từ 0**: 0 → 1 → 2 → 3 (hoặc 0 → 2 → 1 → 3 tùy thứ tự cạnh).
- **DFS từ 0**: 0 → 1 → 3 → 2 (hoặc 0 → 2 → 3 → 1 tùy thứ tự).

---

## 4.5. Code mẫu

### Python – BFS

```python
from collections import deque

def bfs(graph, start):
    n = len(graph)
    visited = [False] * n
    queue = deque([start])
    visited[start] = True
    order = []
    while queue:
        u = queue.popleft()
        order.append(u)
        for v in graph[u]:
            if not visited[v]:
                visited[v] = True
                queue.append(v)
    return order

# Đồ thị trong 4.4: 0-1, 0-2, 1-3, 2-3
graph = [[1, 2], [0, 3], [0, 3], [1, 2]]
print(bfs(graph, 0))   # [0, 1, 2, 3] (hoặc tương tự)
```

### Python – DFS (đệ quy)

```python
def dfs_recursive(graph, start, visited=None):
    if visited is None:
        visited = [False] * len(graph)
    visited[start] = True
    order = [start]
    for v in graph[start]:
        if not visited[v]:
            order.extend(dfs_recursive(graph, v, visited))
    return order

def dfs_iterative(graph, start):
    visited = [False] * len(graph)
    stack = [start]
    order = []
    while stack:
        u = stack.pop()
        if visited[u]:
            continue
        visited[u] = True
        order.append(u)
        for v in reversed(graph[u]):  # reversed để thứ tự giống DFS đệ quy
            if not visited[v]:
                stack.append(v)
    return order
```

### C++ – BFS

```cpp
#include <vector>
#include <queue>
using namespace std;

vector<int> bfs(const vector<vector<int>>& graph, int start) {
    int n = graph.size();
    vector<bool> visited(n, false);
    queue<int> q;
    q.push(start);
    visited[start] = true;
    vector<int> order;
    while (!q.empty()) {
        int u = q.front();
        q.pop();
        order.push_back(u);
        for (int v : graph[u]) {
            if (!visited[v]) {
                visited[v] = true;
                q.push(v);
            }
        }
    }
    return order;
}
```

---

## 4.6. So sánh nhanh

| Tiêu chí | BFS | DFS |
|----------|-----|-----|
| Thứ tự thăm | Theo tầng (gần nguồn trước) | Theo nhánh (đi sâu) |
| Đường đi ngắn nhất (số cạnh) | Có | Không (trực tiếp) |
| Chu trình | Có thể dùng | Thường dùng |
| Stack overflow | Ít gặp (queue) | Có thể (đệ quy sâu) |

---

## 4.7. Tài liệu tham khảo

- [BFS - Wikipedia](https://en.wikipedia.org/wiki/Breadth-first_search)
- [DFS - Wikipedia](https://en.wikipedia.org/wiki/Depth-first_search)
- CLRS, Chương 22: Elementary Graph Algorithms
