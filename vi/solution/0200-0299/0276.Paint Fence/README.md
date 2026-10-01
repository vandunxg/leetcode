---
comments: true
difficulty: Medium
tags:
    - Dynamic Programming
---

<!-- problem:start -->

# [276. Paint Fence 🔒](https://leetcode.com/problems/paint-fence)

[中文文档](/solution/0200-0299/0276.Paint%20Fence/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn cần sơn hàng rào gồm <code>n</code> cọc bằng <code>k</code> màu khác nhau, theo các quy tắc sau:</p>

<ul>
	<li>Mỗi cọc phải được sơn <strong>đúng một</strong> màu.</li>
	<li>Không được có từ ba cọc <strong>liên tiếp</strong> trở lên cùng màu.</li>
</ul>

<p>Cho hai số nguyên <code>n</code> và <code>k</code>, hãy trả về <em><strong>số cách</strong> sơn hàng rào</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0200-0299/0276.Paint%20Fence/images/paintfenceex1.png" style="width: 507px; height: 313px;" />
<pre>
<strong>Đầu vào:</strong> n = 3, k = 2
<strong>Đầu ra:</strong> 6
<strong>Giải thích: </strong>Tất cả các cách sơn được minh họa ở trên.
Lưu ý: cách sơn tất cả các cọc màu đỏ hoặc tất cả các cọc màu xanh lá đều không hợp lệ vì không được có ba cọc liên tiếp cùng màu.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1, k = 1
<strong>Đầu ra:</strong> 1
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 7, k = 2
<strong>Đầu ra:</strong> 42
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 50</code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>5</sup></code></li>
	<li>Các test được tạo sao cho đáp án nằm trong khoảng <code>[0, 2<sup>31</sup> - 1]</code> với <code>n</code> và <code>k</code> đã cho.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Tối đa hai cọc liền kề có thể cùng màu. Cọc $i$ hoặc được sơn khác màu với cọc $i-1$, hoặc cùng màu với cọc $i-1$ nếu cọc $i-1$ khác màu với cọc $i-2$.
>
> $f[i]$ đếm số cách mà hai cọc cuối khác màu, còn $g[i]$ đếm số cách mà chúng cùng màu: $f[i]=(f[i-1]+g[i-1])(k-1)$, $g[i]=f[i-1]$.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là số cách sơn các cọc từ $[0..i]$ sao cho hai cọc cuối khác màu, còn $g[i]$ là số cách sơn các cọc từ $[0..i]$ sao cho hai cọc cuối cùng màu. Ban đầu, $f[0] = k$ và $g[0] = 0$.

Khi $i > 0$, ta có các công thức chuyển trạng thái:

$$
\begin{aligned}
f[i] & = (f[i - 1] + g[i - 1]) \times (k - 1) \\
g[i] & = f[i - 1]
\end{aligned}
$$

Đáp án cuối cùng là $f[n - 1] + g[n - 1]$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số cọc hàng rào.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numWays(self, n: int, k: int) -> int:
        f = [0] * n
        g = [0] * n
        f[0] = k
        for i in range(1, n):
            f[i] = (f[i - 1] + g[i - 1]) * (k - 1)
            g[i] = f[i - 1]
        return f[-1] + g[-1]
```

#### Java

```java
class Solution {
    public int numWays(int n, int k) {
        int[] f = new int[n];
        int[] g = new int[n];
        f[0] = k;
        for (int i = 1; i < n; ++i) {
            f[i] = (f[i - 1] + g[i - 1]) * (k - 1);
            g[i] = f[i - 1];
        }
        return f[n - 1] + g[n - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numWays(int n, int k) {
        vector<int> f(n);
        vector<int> g(n);
        f[0] = k;
        for (int i = 1; i < n; ++i) {
            f[i] = (f[i - 1] + g[i - 1]) * (k - 1);
            g[i] = f[i - 1];
        }
        return f[n - 1] + g[n - 1];
    }
};
```

#### Go

```go
func numWays(n int, k int) int {
	f := make([]int, n)
	g := make([]int, n)
	f[0] = k
	for i := 1; i < n; i++ {
		f[i] = (f[i-1] + g[i-1]) * (k - 1)
		g[i] = f[i-1]
	}
	return f[n-1] + g[n-1]
}
```

#### TypeScript

```ts
function numWays(n: number, k: number): number {
    const f: number[] = Array(n).fill(0);
    const g: number[] = Array(n).fill(0);
    f[0] = k;
    for (let i = 1; i < n; ++i) {
        f[i] = (f[i - 1] + g[i - 1]) * (k - 1);
        g[i] = f[i - 1];
    }
    return f[n - 1] + g[n - 1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động (Tối ưu không gian)

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi trạng thái chỉ dùng cặp giá trị của bước trước, nên chỉ cần hai biến.

<!-- thinking:end -->

Ta thấy $f[i]$ và $g[i]$ chỉ phụ thuộc vào $f[i - 1]$ và $g[i - 1]$. Vì vậy, có thể dùng hai biến $f$ và $g$ lần lượt lưu các giá trị $f[i - 1]$ và $g[i - 1]$, qua đó giảm độ phức tạp không gian xuống $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numWays(self, n: int, k: int) -> int:
        f, g = k, 0
        for _ in range(n - 1):
            ff = (f + g) * (k - 1)
            g = f
            f = ff
        return f + g
```

#### Java

```java
class Solution {
    public int numWays(int n, int k) {
        int f = k, g = 0;
        for (int i = 1; i < n; ++i) {
            int ff = (f + g) * (k - 1);
            g = f;
            f = ff;
        }
        return f + g;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numWays(int n, int k) {
        int f = k, g = 0;
        for (int i = 1; i < n; ++i) {
            int ff = (f + g) * (k - 1);
            g = f;
            f = ff;
        }
        return f + g;
    }
};
```

#### Go

```go
func numWays(n int, k int) int {
	f, g := k, 0
	for i := 1; i < n; i++ {
		f, g = (f+g)*(k-1), f
	}
	return f + g
}
```

#### TypeScript

```ts
function numWays(n: number, k: number): number {
    let [f, g] = [k, 0];
    for (let i = 1; i < n; ++i) {
        const ff = (f + g) * (k - 1);
        g = f;
        f = ff;
    }
    return f + g;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
