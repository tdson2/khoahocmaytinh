# 13. KMP – Tìm kiếm chuỗi (Knuth-Morris-Pratt)

## 13.1. Tổng quan

**KMP** là thuật toán tìm **tất cả vị trí xuất hiện** của chuỗi con (pattern) P trong chuỗi văn bản T trong thời gian **O(|T| + |P|)** nhờ bảng **hàm failure / prefix** (pi), tránh quay lui không cần thiết.

| Thuộc tính | Giá trị |
|------------|--------|
| **Thời gian** | O(n + m) với n = |T|, m = |P| |
| **Không gian** | O(m) cho bảng prefix |
| **Ý tưởng** | Khi không khớp, dịch pattern theo độ dài tiền tố khớp dài nhất |

---

## 13.2. Bảng prefix (failure function)

Với pattern P[0..m-1], `pi[i]` = độ dài **tiền tố thực sự** (không bằng cả đoạn) dài nhất của P[0..i] đồng thời là **hậu tố** của P[0..i].

- Dùng khi tại T[j] và P[i] không khớp: không cần so sánh lại từ đầu P mà chỉ cần so sánh P[pi[i-1]] với T[j] (hoặc tiếp tục từ i = pi[i-1]).

### Cách xây pi

- `pi[0] = 0`.
- Với i ≥ 1: giữ biến `len = pi[i-1]`; trong khi `len > 0` và `P[len] != P[i]` thì `len = pi[len-1]`; nếu `P[len] == P[i]` thì `pi[i] = len + 1`, ngược lại `pi[i] = 0`.

---

## 13.3. Mã giả

```
BUILD_PREFIX(P):
    m = length(P)
    pi[0] = 0
    len = 0
    for i = 1 to m - 1:
        while len > 0 and P[len] != P[i]:
            len = pi[len - 1]
        if P[len] == P[i]:
            len++
        pi[i] = len
    return pi

KMP_SEARCH(T, P):
    pi = BUILD_PREFIX(P)
    n = length(T), m = length(P)
    j = 0  // chỉ số trong P
    for i = 0 to n - 1:
        while j > 0 and T[i] != P[j]:
            j = pi[j - 1]
        if T[i] == P[j]:
            j++
        if j == m:
            report match at i - m + 1
            j = pi[j - 1]
```

---

## 13.4. Ví dụ minh họa

P = "ABABC". Bảng pi: A(0), B(0), A(1), B(2), C(0) → pi = [0,0,1,2,0].

T = "ABABABC", P = "ABABC": Khớp tại chỉ số 2 (AB**ABABC**).

---

## 13.5. Code mẫu

### Python

```python
def build_prefix(p):
    m = len(p)
    pi = [0] * m
    length = 0
    for i in range(1, m):
        while length > 0 and p[length] != p[i]:
            length = pi[length - 1]
        if p[length] == p[i]:
            length += 1
        pi[i] = length
    return pi

def kmp_search(text, pattern):
    if not pattern:
        return []
    pi = build_prefix(pattern)
    n, m = len(text), len(pattern)
    j = 0
    matches = []
    for i in range(n):
        while j > 0 and text[i] != pattern[j]:
            j = pi[j - 1]
        if text[i] == pattern[j]:
            j += 1
        if j == m:
            matches.append(i - m + 1)
            j = pi[j - 1]
    return matches

# Ví dụ
print(kmp_search("ABABABC", "ABABC"))   # [2]
print(kmp_search("aaaa", "aa"))         # [0, 1, 2]
```

### C++ (trích đoạn)

```cpp
vector<int> buildPrefix(const string& p) {
    int m = p.size();
    vector<int> pi(m);
    for (int i = 1, len = 0; i < m; i++) {
        while (len > 0 && p[len] != p[i]) len = pi[len - 1];
        if (p[len] == p[i]) len++;
        pi[i] = len;
    }
    return pi;
}
```

---

## 13.6. So sánh với tìm kiếm naive

- **Naive**: O(n×m), mỗi vị trí có thể so sánh lại từ đầu pattern.
- **KMP**: O(n + m), mỗi ký tự T và P được xử lý tối đa vài lần nhờ bảng pi.

---

## 13.7. Tài liệu tham khảo

- [Knuth–Morris–Pratt algorithm - Wikipedia](https://en.wikipedia.org/wiki/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm)
- CLRS, Chương 32: String Matching
