# 3. Chuỗi số (Sequences and Series)

**Chuỗi số** là tổng vô hạn các số hạng; **dãy số** là danh sách có thứ tự $a_1, a_2, \ldots$. Khái niệm hội tụ, phân kỳ và các chuỗi cơ bản (cấp số nhân, $p$-chuỗi, Taylor) xuất hiện trong giải tích, xác suất và tối ưu (learning rate decay, khai triển hàm loss).

---

## 3.1. Dãy số (Sequence)

### Định nghĩa

- **Dãy số** là ánh xạ từ $\mathbb{N}$ (hoặc tập chỉ số) vào $\mathbb{R}$: $n \mapsto a_n$.
- Ký hiệu: $(a_n)$ hoặc $\{a_n\}_{n=1}^{\infty}$, ví dụ $a_n = \frac{1}{n}$, $a_n = (-1)^n$.

### Giới hạn dãy số

- Dãy $(a_n)$ **hội tụ** về $L$ khi $n \to \infty$ nếu: với mọi $\varepsilon > 0$ tồn tại $N$ sao cho $n \ge N \Rightarrow |a_n - L| < \varepsilon$.
- Ký hiệu: $\displaystyle\lim_{n \to \infty} a_n = L$ hoặc $a_n \to L$.
- Nếu không tồn tại $L$ hữu hạn như vậy thì dãy **phân kỳ** (có thể ra $\pm\infty$ hoặc dao động).

### Một số giới hạn cơ bản

| Dãy | Giới hạn (khi $n \to \infty$) |
|-----|-------------------------------|
| $\dfrac{1}{n^p}$ với $p > 0$ | $0$ |
| $r^n$ với $\|r\| < 1$ | $0$ |
| $r^n$ với $r > 1$ | $+\infty$ |
| $\left(1 + \dfrac{1}{n}\right)^n$ | $e$ |
| $\dfrac{\ln n}{n}$ | $0$ |
| $\dfrac{n^k}{r^n}$ với $r > 1$, $k$ cố định | $0$ |

---

## 3.2. Chuỗi số (Series) – Định nghĩa và hội tụ

### Định nghĩa

- **Chuỗi số** (vô hạn) là tổng hình thức:
  $$
  \sum_{n=1}^{\infty} a_n = a_1 + a_2 + a_3 + \cdots
  $$
- **Tổng riêng thứ $n$**:
  $$
  S_n = \sum_{k=1}^{n} a_k
  $$
- Chuỗi **hội tụ** nếu dãy tổng riêng $(S_n)$ hội tụ; khi đó đặt $\displaystyle S = \lim_{n\to\infty} S_n = \sum_{n=1}^{\infty} a_n$.
- Chuỗi **phân kỳ** nếu $(S_n)$ phân kỳ (ra $\pm\infty$ hoặc không có giới hạn).

### Điều kiện cần để hội tụ

- Nếu $\displaystyle\sum_{n=1}^{\infty} a_n$ hội tụ thì $\displaystyle\lim_{n\to\infty} a_n = 0$.
- **Chú ý**: Điều ngược lại không đúng (ví dụ $\sum \frac{1}{n}$ phân kỳ dù $\frac{1}{n} \to 0$).

---

## 3.3. Chuỗi số dương – Các tiêu chuẩn

Giả sử $a_n \ge 0$. Khi đó $S_n$ tăng; chuỗi hội tụ khi và chỉ khi $(S_n)$ bị chặn trên.

### So sánh (Comparison)

- Nếu $0 \le a_n \le b_n$ và $\sum b_n$ hội tụ thì $\sum a_n$ hội tụ.
- Nếu $0 \le b_n \le a_n$ và $\sum b_n$ phân kỳ thì $\sum a_n$ phân kỳ.

### Tỷ số (Ratio test)

- Đặt $\displaystyle L = \lim_{n\to\infty} \frac{a_{n+1}}{a_n}$ (nếu tồn tại).
  - $L < 1$: chuỗi hội tụ.
  - $L > 1$: chuỗi phân kỳ.
  - $L = 1$: không kết luận.

### Căn (Root test)

- Đặt $\displaystyle L = \limsup_{n\to\infty} \sqrt[n]{|a_n|}$.
  - $L < 1$: chuỗi hội tụ.
  - $L > 1$: chuỗi phân kỳ.
  - $L = 1$: không kết luận.

### Tích phân (Integral test)

- Nếu $f(x) \ge 0$ liên tục, giảm trên $[1,+\infty)$ và $a_n = f(n)$, thì $\sum a_n$ hội tụ khi và chỉ khi $\int_1^{+\infty} f(x)\,dx$ hội tụ.

---

## 3.4. Một số chuỗi quan trọng

### Cấp số nhân (Geometric series)

$$
\sum_{n=0}^{\infty} r^n = 1 + r + r^2 + \cdots = \frac{1}{1-r} \quad \text{nếu } |r| < 1
$$

- Phân kỳ nếu $|r| \ge 1$.
- **Ứng dụng**: Xác suất (phân phối hình học), tài chính (chiết khấu), learning rate decay dạng $r^n$.

### $p$-chuỗi

$$
\sum_{n=1}^{\infty} \frac{1}{n^p} \quad \text{hội tụ khi và chỉ khi } p > 1
$$

- $p = 1$: chuỗi điều hòa $\sum \frac{1}{n}$ **phân kỳ** (đi ra $+\infty$).
- $p > 1$: hội tụ (ví dụ $p=2$: $\sum \frac{1}{n^2} = \frac{\pi^2}{6}$).

### Chuỗi lũy thừa (Power series)

$$
\sum_{n=0}^{\infty} c_n (x - a)^n
$$

- **Bán kính hội tụ** $R$: chuỗi hội tụ khi $|x-a| < R$, phân kỳ khi $|x-a| > R$ (với $R$ có thể $0$ hoặc $+\infty$).
- **Khai triển Taylor/Maclaurin**: Nhiều hàm quen thuộc được viết dưới dạng chuỗi lũy thừa, ví dụ $e^x$, $\ln(1+x)$, $\frac{1}{1-x}$.

---

## 3.5. Khai triển Taylor và chuỗi hàm

### Khai triển Taylor tại $x = a$

$$
f(x) = \sum_{n=0}^{\infty} \frac{f^{(n)}(a)}{n!}(x-a)^n = f(a) + f'(a)(x-a) + \frac{f''(a)}{2!}(x-a)^2 + \cdots
$$

(với điều kiện hội tụ phù hợp).

### Một số khai triển Maclaurin ($a=0$)

| Hàm | Chuỗi (với $|x|$ đủ nhỏ hoặc mọi $x$) |
|-----|---------------------------|
| $e^x$ | $\displaystyle 1 + x + \frac{x^2}{2!} + \frac{x^3}{3!} + \cdots = \sum_{n=0}^{\infty} \frac{x^n}{n!}$ |
| $\ln(1+x)$ | $\displaystyle x - \frac{x^2}{2} + \frac{x^3}{3} - \cdots = \sum_{n=1}^{\infty} \frac{(-1)^{n+1} x^n}{n}$, $|x| < 1$ |
| $\frac{1}{1-x}$ | $\displaystyle 1 + x + x^2 + x^3 + \cdots = \sum_{n=0}^{\infty} x^n$, $|x| < 1$ |
| $(1+x)^\alpha$ | $\displaystyle 1 + \alpha x + \frac{\alpha(\alpha-1)}{2!}x^2 + \cdots$ (nhị thức), $|x| < 1$ |
| $\sin x$ | $\displaystyle x - \frac{x^3}{3!} + \frac{x^5}{5!} - \cdots = \sum_{n=0}^{\infty} \frac{(-1)^n x^{2n+1}}{(2n+1)!}$ |
| $\cos x$ | $\displaystyle 1 - \frac{x^2}{2!} + \frac{x^4}{4!} - \cdots = \sum_{n=0}^{\infty} \frac{(-1)^n x^{2n}}{(2n)!}$ |

---

## 3.6. Chuỗi đan dấu và hội tụ có điều kiện

### Chuỗi đan dấu (Alternating series)

- Dạng $\sum (-1)^n a_n$ hoặc $\sum (-1)^{n+1} a_n$ với $a_n \ge 0$.
- **Tiêu chuẩn Leibniz**: Nếu $a_n$ giảm và $a_n \to 0$ thì chuỗi đan dấu hội tụ.
- **Hội tụ tuyệt đối**: $\sum |a_n|$ hội tụ $\Rightarrow$ $\sum a_n$ hội tụ. Nếu $\sum a_n$ hội tụ nhưng $\sum |a_n|$ phân kỳ thì ta nói chuỗi **hội tụ có điều kiện**.

---

## 3.7. Ứng dụng trong Data Science & AI

| Chủ đề | Liên hệ chuỗi số |
|--------|------------------|
| **Learning rate schedule** | Decay dạng $\eta_n = \eta_0 \cdot r^n$ hoặc $\eta_0 / n^p$; điều kiện hội tụ SGD thường liên quan $\sum \eta_n$ và $\sum \eta_n^2$. |
| **Xác suất** | Phân phối hình học: $\sum_{k\ge 1} (1-p)^{k-1} p = 1$; kỳ vọng dùng chuỗi. |
| **Khai triển hàm** | Xấp xỉ $e^x$, $\ln(1+x)$ trong tính loss, softmax; gradient qua khai triển. |
| **Phân tích thuật toán** | Tổng $\sum_{i=1}^{n} i$, $\sum i^2$ trong độ phức tạp; chuỗi vô hạn khi phân tích trung bình. |
| **Signal / Fourier** | Biểu diễn tín hiệu bằng chuỗi Fourier (sin, cos). |

---

## 3.8. Tóm tắt công thức

- **Cấp số nhân**: $\displaystyle \sum_{n\ge 0} r^n = \frac{1}{1-r}$ với $|r|<1$.
- **$p$-chuỗi**: $\displaystyle \sum_{n\ge 1} \frac{1}{n^p}$ hội tụ $\Leftrightarrow$ $p > 1$.
- **Giới hạn cơ bản**: $\displaystyle \lim_{n\to\infty}\left(1+\frac{1}{n}\right)^n = e$; $\displaystyle \lim_{n\to\infty} \frac{\ln n}{n} = 0$.
- **Taylor**: $e^x = \sum \frac{x^n}{n!}$; $\ln(1+x) = \sum \frac{(-1)^{n+1} x^n}{n}$ ($|x|<1$).

---

## 3.9. Tài liệu tham khảo

- Stewart, *Calculus* (chương Sequences and Series)
- Rudin, *Principles of Mathematical Analysis* (Chương 3)
