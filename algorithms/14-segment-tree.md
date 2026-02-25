# 14. Cây phân đoạn (Segment Tree)

## 14.1. Tổng quan

**Cây phân đoạn (Segment Tree)** là cấu trúc dữ liệu dùng để **truy vấn trên đoạn** (range query) và **cập nhật điểm** (point update) trong thời gian **O(log n)**. Ví dụ: tổng đoạn, min/max đoạn, GCD đoạn.

| Thao tác | Thời gian | Ghi chú |
|----------|-----------|--------|
| Xây cây | O(n) | Mảng gốc n phần tử |
| Truy vấn đoạn [l, r] | O(log n) | Tổng / min / max ... |
| Cập nhật 1 phần tử | O(log n) | Cập nhật và đẩy lên cha |

---

## 14.2. Cấu trúc

- Cây nhị phân: nút gốc quản lý đoạn [0, n-1]; nút con trái [l, mid], nút con phải [mid+1, r].
- Mảng `tree` lưu giá trị “tổng hợp” của đoạn (tổng, min, max...). Thường dùng mảng 4n phần tử (chỉ số gốc = 1 hoặc 0).

---

## 14.3. Hàm tổng hợp

- **Tổng**: `tree[i] = tree[2*i] + tree[2*i+1]`.
- **Min**: `tree[i] = min(tree[2*i], tree[2*i+1])`.
- **Max**: tương tự.

---

## 14.4. Mã giả (tổng đoạn)

```
BUILD(arr, node, l, r):
    if l == r:
        tree[node] = arr[l]
        return
    mid = (l + r) / 2
    BUILD(arr, 2*node, l, mid)
    BUILD(arr, 2*node+1, mid+1, r)
    tree[node] = tree[2*node] + tree[2*node+1]

QUERY(node, l, r, ql, qr):
    if qr < l or ql > r: return 0
    if ql <= l and r <= qr: return tree[node]
    mid = (l + r) / 2
    return QUERY(2*node, l, mid, ql, qr) + QUERY(2*node+1, mid+1, r, ql, qr)

UPDATE(node, l, r, idx, value):
    if l == r:
        tree[node] = value
        return
    mid = (l + r) / 2
    if idx <= mid: UPDATE(2*node, l, mid, idx, value)
    else: UPDATE(2*node+1, mid+1, r, idx, value)
    tree[node] = tree[2*node] + tree[2*node+1]
```

---

## 14.5. Code mẫu (Python – tổng đoạn)

```python
class SegmentTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.size = 4 * self.n
        self.tree = [0] * self.size
        self._build(arr, 1, 0, self.n - 1)

    def _build(self, arr, node, l, r):
        if l == r:
            self.tree[node] = arr[l]
            return
        mid = (l + r) // 2
        self._build(arr, 2 * node, l, mid)
        self._build(arr, 2 * node + 1, mid + 1, r)
        self.tree[node] = self.tree[2 * node] + self.tree[2 * node + 1]

    def query(self, ql, qr, node=1, l=0, r=None):
        if r is None:
            r = self.n - 1
        if qr < l or ql > r:
            return 0
        if ql <= l and r <= qr:
            return self.tree[node]
        mid = (l + r) // 2
        return self.query(ql, qr, 2 * node, l, mid) + self.query(ql, qr, 2 * node + 1, mid + 1, r)

    def update(self, idx, value, node=1, l=0, r=None):
        if r is None:
            r = self.n - 1
        if l == r:
            self.tree[node] = value
            return
        mid = (l + r) // 2
        if idx <= mid:
            self.update(idx, value, 2 * node, l, mid)
        else:
            self.update(idx, value, 2 * node + 1, mid + 1, r)
        self.tree[node] = self.tree[2 * node] + self.tree[2 * node + 1]

# Ví dụ
arr = [1, 3, 5, 7, 9]
st = SegmentTree(arr)
print(st.query(1, 3))   # 3+5+7 = 15
st.update(2, 10)
print(st.query(1, 3))   # 3+10+7 = 20
```

---

## 14.6. Biến thể

- **Lazy propagation**: Cập nhật cả đoạn [l, r] trong O(log n) (ví dụ cộng v vào cả đoạn).
- **Segment tree 2D**: Truy vấn trên ma trận.
- **Persistent segment tree**: Lưu nhiều phiên bản sau mỗi cập nhật.

---

## 14.7. Tài liệu tham khảo

- [Segment tree - Wikipedia](https://en.wikipedia.org/wiki/Segment_tree)
- [CP-Algorithms: Segment Tree](https://cp-algorithms.com/data_structures/segment_tree.html)
