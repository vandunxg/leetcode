---
comments: true
difficulty: Medium
tags:
    - Math
---

<!-- problem:start -->

# [3958. Minimum Cost to Split into Ones II 🔒](https://leetcode.com/problems/minimum-cost-to-split-into-ones-ii)

[中文文档](/solution/3900-3999/3958.Minimum%20Cost%20to%20Split%20into%20Ones%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code>.</p>

<p>Trong một thao tác, bạn có thể tách một số nguyên <code>x</code> thành hai số nguyên dương <code>a</code> và <code>b</code> sao cho <code>a + b = x</code>.</p>

<p>Chi phí của thao tác này là <code>a * b</code>.</p>

<p>Trả về <strong>tổng chi phí nhỏ nhất</strong> cần thiết để tách số nguyên <code>n</code> thành <code>n</code> số 1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một chuỗi thao tác tối ưu là:</p>

<table border="1" bordercolor="#ccc" cellpadding="5" cellspacing="0" style="border-collapse:collapse;">
	<tbody>
		<tr>
			<th><code>x</code></th>
			<th><code>a</code></th>
			<th><code>b</code></th>
			<th><code>a + b</code></th>
			<th><code>a * b</code></th>
			<th>Chi phí</th>
		</tr>
		<tr>
			<td>3</td>
			<td>1</td>
			<td>2</td>
			<td>3</td>
			<td>2</td>
			<td>2</td>
		</tr>
		<tr>
			<td>2</td>
			<td>1</td>
			<td>1</td>
			<td>2</td>
			<td>1</td>
			<td>1</td>
		</tr>
	</tbody>
</table>

<p>Vì vậy, tổng chi phí nhỏ nhất là <code>2 + 1 = 3</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:​​​​​​​</strong></p>

<p>Một chuỗi thao tác tối ưu là:</p>

<table border="1" bordercolor="#ccc" cellpadding="5" cellspacing="0" style="border-collapse:collapse;">
	<tbody>
		<tr>
			<th><code>x</code></th>
			<th><code>a</code></th>
			<th><code>b</code></th>
			<th><code>a + b</code></th>
			<th><code>a * b</code></th>
			<th>Chi phí</th>
		</tr>
		<tr>
			<td>4</td>
			<td>2</td>
			<td>2</td>
			<td>4</td>
			<td>4</td>
			<td>4</td>
		</tr>
		<tr>
			<td>2</td>
			<td>1</td>
			<td>1</td>
			<td>2</td>
			<td>1</td>
			<td>1</td>
		</tr>
	</tbody>
</table>

<p>Vì vậy, tổng chi phí nhỏ nhất là <code>4 + 1 + 1 = 6</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 5 * 10<sup>7</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n$ có thể lên tới $5\times 10^7$, tách từng bước sẽ quá chậm. Tách $x$ thành $1$ và $x-1$ có chi phí $x-1$; lặp lại cho đến khi tất cả đều là số $1$, tổng chi phí là $1+2+\cdots+(n-1)$.
>
> Chiến lược đó cho công thức đóng $\dfrac{n(n-1)}{2}$. Tách cân bằng hơn cũng không tốt hơn: mỗi số 1 cuối cùng vẫn phải trả cho một hiệu kề bên, và tổng không đổi.
>
> Trả về công thức trong $O(1)$.

<!-- thinking:end -->

Để tối thiểu hóa chi phí, trước tiên ta nên tách $n$ thành $1$ và $n - 1$, với chi phí $n - 1$; sau đó tách $n - 1$ thành $1$ và $n - 2$, với chi phí $n - 2$. Theo quy luật này, tổng chi phí là $1 + 2 + \dots + (n - 1) = \frac{n \times (n - 1)}{2}$.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCost(self, n: int) -> int:
        return n * (n - 1) // 2
```

#### Java

```java
class Solution {
    public long minCost(int n) {
        return 1L * n * (n - 1) / 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minCost(int n) {
        return 1LL * n * (n - 1) / 2;
    }
};
```

#### Go

```go
func minCost(n int) int64 {
	return int64(n * (n - 1) / 2)
}
```

#### TypeScript

```ts
function minCost(n: number): number {
    return (n * (n - 1)) / 2;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
