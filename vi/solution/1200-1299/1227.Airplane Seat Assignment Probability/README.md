---
comments: true
difficulty: Medium
tags:
    - Brainteaser
    - Math
    - Dynamic Programming
    - Probability and Statistics
---

<!-- problem:start -->

# [1227. Airplane Seat Assignment Probability](https://leetcode.com/problems/airplane-seat-assignment-probability)

[中文文档](/solution/1200-1299/1227.Airplane%20Seat%20Assignment%20Probability/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> hành khách lên máy bay với đúng <code>n</code> ghế. Hành khách đầu tiên làm mất vé nên chọn ngẫu nhiên một ghế. Sau đó, các hành khách còn lại sẽ:</p>

<ul>
	<li>Ngồi vào ghế của mình nếu ghế đó còn trống; nếu không,</li>
	<li>Chọn ngẫu nhiên một ghế khác khi phát hiện ghế của mình đã có người ngồi.</li>
</ul>

<p>Trả về <em>xác suất để hành khách thứ </em><code>n<sup>th</sup></code><em> ngồi đúng ghế của mình</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> 1.00000
<strong>Giải thích: </strong>Hành khách đầu tiên chỉ có thể ngồi vào ghế đầu tiên.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> 0.50000
<strong>Giải thích: </strong>Hành khách thứ hai có xác suất 0.5 ngồi được ghế thứ hai (khi hành khách đầu tiên ngồi vào ghế đầu tiên).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> $n$ có thể lên đến $10^5$, nên không thể mô phỏng từng hành khách. Xác suất hành khách thứ $n$ ngồi ghế thứ $n$ bằng $1$ khi $n=1$; với các trường hợp lớn hơn, hành khách đầu tiên có thể ngồi đúng ghế, chiếm ghế thứ $n$, hoặc chọn một ghế ở giữa để đưa bài toán về một trường hợp nhỏ hơn có cấu trúc tương tự.
>
> Công thức truy hồi rút gọn thành $1/2$ với mọi $n\ge 2$. Vì vậy, trả về $1$ nếu $n=1$, nếu không thì trả về $0.5$; không cần lặp.

<!-- thinking:end -->

Gọi $f(n)$ là xác suất hành khách thứ $n$ ngồi đúng ghế của mình khi có $n$ hành khách lên máy bay. Xét các trường hợp đơn giản nhất trước:

Khi $n=1$, chỉ có 1 hành khách và 1 ghế, nên hành khách đầu tiên chắc chắn ngồi ghế đầu tiên, $f(1)=1$.

Khi $n=2$, có 2 ghế và mỗi ghế có xác suất 0.5 được hành khách đầu tiên chọn. Sau khi hành khách đầu tiên chọn ghế, hành khách thứ hai chỉ có thể chọn ghế còn lại, nên xác suất người này ngồi đúng ghế của mình là 0.5, $f(2)=0.5$.

Khi $n>2$, ta tính $f(n)$ như thế nào? Xét ghế mà hành khách đầu tiên chọn; có ba trường hợp:

- Hành khách đầu tiên có xác suất $\frac{1}{n}$ chọn ghế đầu tiên. Khi đó, mọi hành khách đều có thể ngồi đúng ghế của mình, nên xác suất hành khách thứ $n$ ngồi đúng ghế là 1.0.

- Hành khách đầu tiên có xác suất $\frac{1}{n}$ chọn ghế thứ $n$. Khi đó, các hành khách từ thứ hai đến thứ $(n-1)$ có thể ngồi đúng ghế của mình, còn hành khách thứ $n$ chỉ có thể ngồi ghế đầu tiên; vì vậy xác suất người thứ $n$ ngồi đúng ghế là 0.0.

- Hành khách đầu tiên có xác suất $\frac{n-2}{n}$ chọn một trong các ghế còn lại; mỗi ghế có xác suất $\frac{1}{n}$ được chọn.
  Giả sử hành khách đầu tiên chọn ghế thứ $i$, với $2 \le i \le n-1$. Khi đó, các hành khách từ thứ hai đến thứ $(i-1)$ có thể ngồi đúng ghế của mình; ghế dành cho các hành khách từ thứ $i$ đến thứ $n$ vẫn chưa xác định. Hành khách thứ $i$ sẽ chọn ngẫu nhiên một trong $n-(i-1)=n-i+1$ ghế còn lại (gồm ghế đầu tiên và các ghế từ thứ $(i+1)$ đến thứ $n$). Vì còn $n-i+1$ hành khách và ghế, trong đó một hành khách sẽ chọn ghế ngẫu nhiên, kích thước bài toán giảm từ $n$ xuống $n-i+1$.

Kết hợp ba trường hợp trên, ta thu được công thức truy hồi của $f(n)$:

$$
\begin{aligned}
f(n) &= \frac{1}{n} \times 1.0 + \frac{1}{n} \times 0.0 + \frac{1}{n} \times \sum_{i=2}^{n-1} f(n-i+1) \\
&= \frac{1}{n}(1.0+\sum_{i=2}^{n-1} f(n-i+1))
\end{aligned}
$$

Trong công thức truy hồi trên có $n-2$ giá trị của $i$. Số lượng giá trị của $i$ phải là số nguyên không âm, nên công thức chỉ đúng khi $n-2 \ge 0$, tức là $n \ge 2$.

Nếu áp dụng trực tiếp công thức truy hồi trên để tính $f(n)$, độ phức tạp thời gian là $O(n^2)$ và không thể vượt qua mọi test case, nên cần tối ưu.

Thay $n$ bằng $n-1$ trong công thức truy hồi trên, ta được:

$$
f(n-1) = \frac{1}{n-1}(1.0+\sum_{i=2}^{n-2} f(n-i))
$$

Công thức truy hồi trên có $n-3$ giá trị của $i$ và chỉ đúng khi $n-3 \ge 0$, tức là $n \ge 3$.

Khi $n \ge 3$, có thể viết hai công thức truy hồi trên như sau:

$$
\begin{aligned}
n \times f(n) &= 1.0+\sum_{i=2}^{n-1} f(n-i+1) \\
(n-1) \times f(n-1) &= 1.0+\sum_{i=2}^{n-2} f(n-i)
\end{aligned}
$$

Lấy công thức thứ nhất trừ công thức thứ hai:

$$
\begin{aligned}
&~~~~~ n \times f(n) - (n-1) \times f(n-1) \\
&= (1.0+\sum_{i=2}^{n-1} f(n-i+1)) - (1.0+\sum_{i=2}^{n-2} f(n-i)) \\
&= \sum_{i=2}^{n-1} f(n-i+1) - \sum_{i=2}^{n-2} f(n-i) \\
&= f(2)+f(3)+...+f(n-1) - (f(2)+f(3)+...+f(n-2)) \\
&= f(n-1)
\end{aligned}
$$

Rút gọn, ta thu được công thức truy hồi:

$$
\begin{aligned}
n \times f(n) &= n \times f(n-1) \\
f(n) &= f(n-1)
\end{aligned}
$$

Ta biết $f(1)=1$ và $f(2)=0.5$. Với $n \ge 3$, từ $f(n) = f(n-1)$ suy ra $f(n)=0.5$. Do $f(2)=0.5$, vậy với mọi số nguyên dương $n \ge 2$, ta có $f(n)=0.5$.

Từ đó, ta có kết quả của $f(n)$:

$$
f(n) = \begin{cases}
1.0, & n = 1 \\
0.5, & n \ge 2
\end{cases}
$$

Độ phức tạp thời gian của lời giải là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def nthPersonGetsNthSeat(self, n: int) -> float:
        return 1 if n == 1 else 0.5
```

#### Java

```java
class Solution {
    public double nthPersonGetsNthSeat(int n) {
        return n == 1 ? 1 : .5;
    }
}
```

#### C++

```cpp
class Solution {
public:
    double nthPersonGetsNthSeat(int n) {
        return n == 1 ? 1 : .5;
    }
};
```

#### Go

```go
func nthPersonGetsNthSeat(n int) float64 {
	if n == 1 {
		return 1
	}
	return .5
}
```

#### TypeScript

```ts
function nthPersonGetsNthSeat(n: number): number {
    return n === 1 ? 1 : 0.5;
}
```

#### Rust

```rust
impl Solution {
    pub fn nth_person_gets_nth_seat(n: i32) -> f64 {
        return if n == 1 { 1.0 } else { 0.5 };
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
