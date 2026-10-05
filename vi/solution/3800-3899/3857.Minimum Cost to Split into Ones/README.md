---
comments: true
difficulty: Medium
rating: 1322
source: Weekly Contest 491 Q2
tags:
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [3857. Minimum Cost to Split into Ones](https://leetcode.com/problems/minimum-cost-to-split-into-ones)

[中文文档](/solution/3800-3899/3857.Minimum%20Cost%20to%20Split%20into%20Ones/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code>.</p>

<p>Trong một phép toán, bạn có thể tách một số nguyên <code>x</code> thành hai số nguyên dương <code>a</code> và <code>b</code> sao cho <code>a + b = x</code>.</p>

<p>Chi phí của phép toán này là <code>a * b</code>.</p>

<p>Trả về một số nguyên biểu thị <strong>tổng chi phí nhỏ nhất</strong> cần thiết để tách số nguyên <code>n</code> thành <code>n</code> số 1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một chuỗi phép toán tối ưu là:</p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;"><code>x</code></th>
			<th style="border: 1px solid black;"><code>a</code></th>
			<th style="border: 1px solid black;"><code>b</code></th>
			<th style="border: 1px solid black;"><code>a + b</code></th>
			<th style="border: 1px solid black;"><code>a * b</code></th>
			<th style="border: 1px solid black;">Chi phí</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">2</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
	</tbody>
</table>

<p>Vậy tổng chi phí nhỏ nhất là <code>2 + 1 = 3</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<div class="example-block">
<p>Một chuỗi phép toán tối ưu là:</p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;"><code>x</code></th>
			<th style="border: 1px solid black;"><code>a</code></th>
			<th style="border: 1px solid black;"><code>b</code></th>
			<th style="border: 1px solid black;"><code>a + b</code></th>
			<th style="border: 1px solid black;"><code>a * b</code></th>
			<th style="border: 1px solid black;">Chi phí</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">4</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
	</tbody>
</table>

<p>Vậy tổng chi phí nhỏ nhất là <code>4 + 1 + 1 = 6</code>.</p>
</div>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 500</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Tách $n$ thành $a+b=n$ với chi phí $a \cdot b$ cho đến khi mọi phần đều là $1$, đồng thời tối thiểu hóa tổng chi phí. Vì $n \le 500$, ta có thể dùng DP, nhưng đáp án tối ưu có dạng công thức đóng.
>
> Tích $a(n-a)$ đạt nhỏ nhất khi $a=1$. Vì vậy, luôn tách một phần tử $1$, để lại lần lượt $n-1,n-2,\ldots,2$.
>
> Tổng chi phí là $1+2+\cdots+(n-1)=n(n-1)/2$.
>
> Không cần tìm các cách tách khác.

<!-- thinking:end -->

Để tối thiểu hóa tổng chi phí, trước tiên ta tách $n$ thành $1$ và $n-1$, với chi phí là $1 \cdot (n-1) = n-1$. Tiếp theo, ta tách $n-1$ thành $1$ và $n-2$, với chi phí là $1 \cdot (n-2) = n-2$.

Ta tiếp tục quá trình này cho đến khi tách $2$ thành $1$ và $1$, với chi phí là $1 \cdot 1 = 1$. Do đó, tổng chi phí là $(n-1) + (n-2) + \ldots + 2 + 1 = \frac{n(n-1)}{2}$.

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
    public int minCost(int n) {
        return n * (n - 1) / 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minCost(int n) {
        return n * (n - 1) / 2;
    }
};
```

#### Go

```go
func minCost(n int) int {
	return n * (n - 1) / 2
}
```

#### TypeScript

```ts
function minCost(n: number): number {
    return (n * (n - 1)) >> 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
