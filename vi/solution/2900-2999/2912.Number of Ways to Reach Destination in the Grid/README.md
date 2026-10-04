---
comments: true
difficulty: Hard
tags:
    - Math
    - Dynamic Programming
    - Combinatorics
---

<!-- problem:start -->

# [2912. Number of Ways to Reach Destination in the Grid 🔒](https://leetcode.com/problems/number-of-ways-to-reach-destination-in-the-grid)

[中文文档](/solution/2900-2999/2912.Number%20of%20Ways%20to%20Reach%20Destination%20in%20the%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai số nguyên <code>n</code> và <code>m</code> biểu diễn kích thước của một lưới <strong>được đánh chỉ số từ 1</strong>. Bạn cũng được cho một số nguyên <code>k</code>, một <strong>mảng số nguyên được đánh chỉ số từ 1</strong> <code>source</code> và một <strong>mảng số nguyên được đánh chỉ số từ 1</strong> <code>dest</code>, trong đó <code>source</code> và <code>dest</code> có dạng <code>[x, y]</code> biểu diễn một ô trên lưới đã cho.</p>

<p>Bạn có thể di chuyển trong lưới theo cách sau:</p>

<ul>
	<li>Bạn có thể đi từ ô <code>[x<sub>1</sub>, y<sub>1</sub>]</code> đến ô <code>[x<sub>2</sub>, y<sub>2</sub>]</code> nếu <code>x<sub>1</sub> == x<sub>2</sub></code> hoặc <code>y<sub>1</sub> == y<sub>2</sub></code>.</li>
	<li>Lưu ý rằng bạn <strong>không thể</strong> di chuyển đến ô mà mình đang đứng, chẳng hạn khi <code>x<sub>1</sub> == x<sub>2</sub></code> và <code>y<sub>1</sub> == y<sub>2</sub></code>.</li>
</ul>

<p>Hãy trả về <em>số cách bạn có thể đi đến</em> <code>dest</code> <em>từ</em> <code>source</code> <em>bằng cách di chuyển trong lưới</em> <strong>chính xác</strong> <code>k</code> <em>lần.</em></p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, m = 2, k = 2, source = [1,1], dest = [2,2]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có 2 chuỗi di chuyển khả dĩ từ [1,1] đến [2,2]:
- [1,1] -&gt; [1,2] -&gt; [2,2]
- [1,1] -&gt; [2,1] -&gt; [2,2]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, m = 4, k = 3, source = [1,2], dest = [2,3]
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Có 9 chuỗi di chuyển khả dĩ từ [1,2] đến [2,3]:
- [1,2] -&gt; [1,1] -&gt; [1,3] -&gt; [2,3]
- [1,2] -&gt; [1,1] -&gt; [2,1] -&gt; [2,3]
- [1,2] -&gt; [1,3] -&gt; [3,3] -&gt; [2,3]
- [1,2] -&gt; [1,4] -&gt; [1,3] -&gt; [2,3]
- [1,2] -&gt; [1,4] -&gt; [2,4] -&gt; [2,3]
- [1,2] -&gt; [2,2] -&gt; [2,1] -&gt; [2,3]
- [1,2] -&gt; [2,2] -&gt; [2,4] -&gt; [2,3]
- [1,2] -&gt; [3,2] -&gt; [2,2] -&gt; [2,3]
- [1,2] -&gt; [3,2] -&gt; [3,3] -&gt; [2,3]
</pre>

<p>&nbsp;</p>
<p><strong>Các ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n, m &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k&nbsp;&lt;= 10<sup>5</sup></code></li>
	<li><code>source.length == dest.length == 2</code></li>
	<li><code>1 &lt;= source[1], dest[1] &lt;= n</code></li>
	<li><code>1 &lt;= source[2], dest[2] &lt;= m</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước thay đổi hàng hoặc cột, và ta muốn ở tại $dest$ sau $k$ bước. Mỗi cạnh của lưới có thể dài đến $10^9$, nên không thể dùng trạng thái cho từng ô. So với $source$, chỉ có bốn loại ô: chính nó, cùng cột, cùng hàng hoặc khác cả hàng lẫn cột.
>
> Một vector bốn phần tử $f$ lưu số cách đến từng loại; mỗi bước chỉ trộn bốn loại này với các hệ số phụ thuộc vào $n$ và $m$. Sau $k$ lần lặp, ta chọn thành phần tương ứng với vị trí của $dest$ so với $source$.

<!-- thinking:end -->

Ta định nghĩa các trạng thái sau:

- $f[0]$ biểu diễn số cách đi từ `source` đến chính `source`;
- $f[1]$ biểu diễn số cách đi từ `source` đến một hàng khác trong cùng cột;
- $f[2]$ biểu diễn số cách đi từ `source` đến một cột khác trong cùng hàng;
- $f[3]$ biểu diễn số cách đi từ `source` đến một hàng khác và một cột khác.

Ban đầu, $f[0] = 1$, còn các trạng thái khác đều bằng $0$.

Với mỗi trạng thái, ta có thể tính trạng thái hiện tại dựa trên trạng thái trước đó như sau:

$$
\begin{aligned}
g[0] &= (n - 1) \times f[1] + (m - 1) \times f[2] \\
g[1] &= f[0] + (n - 2) \times f[1] + (m - 1) \times f[3] \\
g[2] &= f[0] + (m - 2) \times f[2] + (n - 1) \times f[3] \\
g[3] &= f[1] + f[2] + (n - 2) \times f[3] + (m - 2) \times f[3]
\end{aligned}
$$

Ta lặp $k$ lần, cuối cùng kiểm tra xem `source` và `dest` nằm trên cùng hàng hay cùng cột, rồi trả về trạng thái tương ứng.

Độ phức tạp thời gian là $O(k)$, trong đó $k$ là số lần di chuyển. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfWays(
        self, n: int, m: int, k: int, source: List[int], dest: List[int]
    ) -> int:
        mod = 10**9 + 7
        f = [1, 0, 0, 0]
        for _ in range(k):
            g = [0] * 4
            g[0] = ((n - 1) * f[1] + (m - 1) * f[2]) % mod
            g[1] = (f[0] + (n - 2) * f[1] + (m - 1) * f[3]) % mod
            g[2] = (f[0] + (m - 2) * f[2] + (n - 1) * f[3]) % mod
            g[3] = (f[1] + f[2] + (n - 2) * f[3] + (m - 2) * f[3]) % mod
            f = g
        if source[0] == dest[0]:
            return f[0] if source[1] == dest[1] else f[2]
        return f[1] if source[1] == dest[1] else f[3]
```

#### Java

```java
class Solution {
    public int numberOfWays(int n, int m, int k, int[] source, int[] dest) {
        final int mod = 1000000007;
        long[] f = new long[4];
        f[0] = 1;
        while (k-- > 0) {
            long[] g = new long[4];
            g[0] = ((n - 1) * f[1] + (m - 1) * f[2]) % mod;
            g[1] = (f[0] + (n - 2) * f[1] + (m - 1) * f[3]) % mod;
            g[2] = (f[0] + (m - 2) * f[2] + (n - 1) * f[3]) % mod;
            g[3] = (f[1] + f[2] + (n - 2) * f[3] + (m - 2) * f[3]) % mod;
            f = g;
        }
        if (source[0] == dest[0]) {
            return source[1] == dest[1] ? (int) f[0] : (int) f[2];
        }
        return source[1] == dest[1] ? (int) f[1] : (int) f[3];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfWays(int n, int m, int k, vector<int>& source, vector<int>& dest) {
        const int mod = 1e9 + 7;
        vector<long long> f(4);
        f[0] = 1;
        while (k--) {
            vector<long long> g(4);
            g[0] = ((n - 1) * f[1] + (m - 1) * f[2]) % mod;
            g[1] = (f[0] + (n - 2) * f[1] + (m - 1) * f[3]) % mod;
            g[2] = (f[0] + (m - 2) * f[2] + (n - 1) * f[3]) % mod;
            g[3] = (f[1] + f[2] + (n - 2) * f[3] + (m - 2) * f[3]) % mod;
            f = move(g);
        }
        if (source[0] == dest[0]) {
            return source[1] == dest[1] ? f[0] : f[2];
        }
        return source[1] == dest[1] ? f[1] : f[3];
    }
};
```

#### Go

```go
func numberOfWays(n int, m int, k int, source []int, dest []int) int {
	const mod int = 1e9 + 7
	f := []int{1, 0, 0, 0}
	for i := 0; i < k; i++ {
		g := make([]int, 4)
		g[0] = ((n-1)*f[1] + (m-1)*f[2]) % mod
		g[1] = (f[0] + (n-2)*f[1] + (m-1)*f[3]) % mod
		g[2] = (f[0] + (m-2)*f[2] + (n-1)*f[3]) % mod
		g[3] = (f[1] + f[2] + (n-2)*f[3] + (m-2)*f[3]) % mod
		f = g
	}

	if source[0] == dest[0] {
		if source[1] == dest[1] {
			return f[0]
		}
		return f[2]
	}

	if source[1] == dest[1] {
		return f[1]
	}
	return f[3]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
