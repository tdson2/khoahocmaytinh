# 18. Quy hoạch động nâng cao (Advanced DP)

## 18.1. Tổng quan

Phần này bổ sung các kỹ thuật **quy hoạch động nâng cao**: DP bitmask (trạng thái dạng tập hợp), dãy con tăng dài nhất (LIS), dãy con chung dài nhất (LCS), và một số mẫu thường gặp trong thi đấu/lập trình.

| Kỹ thuật | Ứng dụng điển hình | Độ phức tạp |
|----------|--------------------|-------------|
| Bitmask DP | TSP, gán công việc, tập con | O(2^n × poly(n)) |
| LIS | Dãy con tăng dài nhất | O(n log n) |
| LCS | So sánh chuỗi, diff | O(n×m) |
| Digit DP | Đếm số trong khoảng thỏa điều kiện | O(số chữ số × trạng thái) |

---

## 18.2. DP Bitmask (trạng thái tập hợp)

Trạng thái được mã hóa bằng **bitmask**: bit thứ i = 1 nghĩa là phần tử i thuộc tập. Ví dụ tập {0, 2} → mask = 5 (101₂).

### Ví dụ: Bài toán người du lịch (TSP)

- `dp[mask][i]` = chi phí nhỏ nhất để đã thăm tập đỉnh `mask` và đang ở đỉnh i.
- Khởi tạo: `dp[1<<start][start] = 0`.
- Chuyển: Với mỗi j chưa thăm (bit j = 0), `dp[mask | (1<<j)][j] = min(..., dp[mask][i] + cost[i][j])`.
- Kết quả: min over `dp[(1<<n)-1][i] + cost[i][start]` (quay về start).

### Code TSP (Python – trích đoạn)

```python
def tsp(n, cost):
    """cost[i][j] = chi phí từ i đến j. Trả về chi phí đường đi Hamilton ngắn nhất."""
    INF = float('inf')
    dp = [[INF] * n for _ in range(1 << n)]
    dp[1][0] = 0
    for mask in range(1 << n):
        for i in range(n):
            if dp[mask][i] == INF:
                continue
            for j in range(n):
                if mask & (1 << j):
                    continue
                new_mask = mask | (1 << j)
                dp[new_mask][j] = min(dp[new_mask][j], dp[mask][i] + cost[i][j])
    ans = INF
    for i in range(n):
        ans = min(ans, dp[(1 << n) - 1][i] + cost[i][0])
    return ans
```

---

## 18.3. Dãy con tăng dài nhất (LIS – Longest Increasing Subsequence)

**Bài toán**: Cho mảng A, tìm độ dài (hoặc bản thân dãy) dãy con tăng dài nhất (các phần tử giữ thứ tự, không cần liên tiếp).

### O(n²) – DP truyền thống

- `dp[i]` = độ dài LIS kết thúc tại A[i].
- `dp[i] = 1 + max{ dp[j] : j < i và A[j] < A[i] }`, nếu không có j thì dp[i] = 1.
- Kết quả: max(dp).

### O(n log n) – Binary search + mảng đuôi

- Duy trì mảng `tail`: `tail[len]` = phần tử nhỏ nhất mà có thể kết thúc một LIS độ dài `len`.
- Duyệt A[i]: dùng binary search tìm vị trí đầu tiên mà `tail[k] >= A[i]`, cập nhật `tail[k] = A[i]` (và có thể parent để in lại dãy).

### Code LIS O(n log n) – Python

```python
import bisect

def lis_length(arr):
    tail = []
    for x in arr:
        i = bisect.bisect_left(tail, x)
        if i == len(tail):
            tail.append(x)
        else:
            tail[i] = x
    return len(tail)

# Ví dụ
print(lis_length([10, 9, 2, 5, 3, 7, 101, 18]))  # 4  [2,3,7,101]
```

---

## 18.4. Dãy con chung dài nhất (LCS – Longest Common Subsequence)

**Bài toán**: Cho hai chuỗi X, Y; tìm độ dài (hoặc một dãy) dãy con chung dài nhất (các ký tự giữ thứ tự trong X và Y).

### Công thức DP

- `dp[i][j]` = độ dài LCS của X[0..i-1] và Y[0..j-1].
- Nếu X[i-1] == Y[j-1]: `dp[i][j] = 1 + dp[i-1][j-1]`.
- Ngược lại: `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`.
- Cơ sở: dp[0][j] = dp[i][0] = 0.

### Code LCS – Python

```python
def lcs_length(x, y):
    n, m = len(x), len(y)
    dp = [[0] * (m + 1) for _ in range(n + 1)]
    for i in range(1, n + 1):
        for j in range(1, m + 1):
            if x[i - 1] == y[j - 1]:
                dp[i][j] = 1 + dp[i - 1][j - 1]
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    return dp[n][m]

# Ví dụ
print(lcs_length("abcde", "ace"))  # 3  ("ace")
```

---

## 18.5. Digit DP (tóm tắt)

- Đếm số lượng số trong đoạn [L, R] thỏa điều kiện (ví dụ: không chứa chữ số 4, chia hết cho k, …).
- Trạng thái: (vị trí chữ số, tight/limit, có thể thêm các tham số như remainder mod k).
- Duyệt từ chữ số cao xuống thấp; tight = 1 nghĩa là prefix đang bằng prefix của upper bound.

---

## 18.6. Tài liệu tham khảo

- CLRS, Chương 15 (DP), 14 (LCS).
- [CP-Algorithms: Dynamic Programming](https://cp-algorithms.com/dynamic_programming/)
- [Bitmask DP](https://cp-algorithms.com/dynamic_programming/profile-dynamics.html)
