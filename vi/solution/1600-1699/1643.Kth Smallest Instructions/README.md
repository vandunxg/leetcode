---
comments: true
difficulty: Hard
rating: 2079
source: Weekly Contest 213 Q4
tags:
    - Array
    - Math
    - Dynamic Programming
    - Combinatorics
---

<!-- problem:start -->

# [1643. Kth Smallest Instructions](https://leetcode.com/problems/kth-smallest-instructions)

[中文文档](/solution/1600-1699/1643.Kth%20Smallest%20Instructions/README.md)

## Mô tả

<!-- description:start -->

<p>Bob đang đứng tại ô <code>(0, 0)</code> và muốn đến <code>destination</code>: <code>(row, column)</code>. Bob chỉ có thể đi <strong>sang phải</strong> và <strong>xuống dưới</strong>. Hãy cung cấp <strong>instructions</strong> để Bob đến <code>destination</code>.</p>

<p><strong>instructions</strong> được biểu diễn bằng một chuỗi, trong đó mỗi ký tự là một trong hai loại:</p>

<ul>
	<li><code>&#39;H&#39;</code>, nghĩa là di chuyển theo chiều ngang (đi <strong>sang phải</strong>), hoặc</li>
	<li><code>&#39;V&#39;</code>, nghĩa là di chuyển theo chiều dọc (đi <strong>xuống dưới</strong>).</li>
</ul>

<p>Có nhiều <strong>instructions</strong> có thể đưa Bob đến <code>destination</code>. Ví dụ, nếu <code>destination</code> là <code>(2, 3)</code>, cả <code>&quot;HHHVV&quot;</code> và <code>&quot;HVHVH&quot;</code> đều là <strong>instructions</strong> hợp lệ.</p>

<p>Tuy nhiên, Bob rất kén chọn. Bob có số may mắn <code>k</code> và muốn instructions <strong>nhỏ thứ <code>k<sup>th</sup></code> theo thứ tự từ điển</strong> đưa mình đến <code>destination</code>. <code>k</code> được <strong>đánh chỉ số từ 1</strong>.</p>

<p>Cho mảng số nguyên <code>destination</code> và số nguyên <code>k</code>, hãy trả về <em>instructions </em><em><strong>nhỏ thứ <code>k<sup>th</sup></code> theo thứ tự từ điển</strong> đưa Bob đến </em><code>destination</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1643.Kth%20Smallest%20Instructions/images/ex1.png" style="width: 300px; height: 229px;" /></p>

<pre>
<strong>Input:</strong> destination = [2,3], k = 1
<strong>Output:</strong> &quot;HHHVV&quot;
<strong>Explanation:</strong> Tất cả instructions đưa đến (2, 3) theo thứ tự từ điển là:
[&quot;HHHVV&quot;, &quot;HHVHV&quot;, &quot;HHVVH&quot;, &quot;HVHHV&quot;, &quot;HVHVH&quot;, &quot;HVVHH&quot;, &quot;VHHHV&quot;, &quot;VHHVH&quot;, &quot;VHVHH&quot;, &quot;VVHHH&quot;].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1643.Kth%20Smallest%20Instructions/images/ex2.png" style="width: 300px; height: 229px;" /></strong></p>

<pre>
<strong>Input:</strong> destination = [2,3], k = 2
<strong>Output:</strong> &quot;HHVHV&quot;
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1643.Kth%20Smallest%20Instructions/images/ex3.png" style="width: 300px; height: 229px;" /></strong></p>

<pre>
<strong>Input:</strong> destination = [2,3], k = 3
<strong>Output:</strong> &quot;HHVVH&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>destination.length == 2</code></li>
	<li><code>1 &lt;= row, column &lt;= 15</code></li>
	<li><code>1 &lt;= k &lt;= nCr(row + column, row)</code>, where <code>nCr(a, b)</code> denotes <code>a</code> choose <code>b</code>​​​​​.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi đường đi có $v$ ký tự $\texttt{V}$ và $h$ ký tự $\texttt{H}$. Không cần liệt kê toàn bộ $\binom{h+v}{h}$ đường đi để lấy đường thứ $k$.
>
> Nếu ký tự tiếp theo là $\texttt{H}$, có $C_{h+v-1}^{h-1}$ đường đi như vậy. Nếu $k$ lớn hơn số này, ký tự phải là $\texttt{V}$ và ta trừ số đường đó khỏi k; nếu không thì chọn $\texttt{H}$.
>
> Quyết định từng ký tự bằng hệ số nhị thức; khi $h$ bằng 0 thì phần còn lại đều là $\texttt{V}$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kthSmallestPath(self, destination: List[int], k: int) -> str:
        v, h = destination
        ans = []
        for _ in range(h + v):
            if h == 0:
                ans.append("V")
            else:
                x = comb(h + v - 1, h - 1)
                if k > x:
                    ans.append("V")
                    v -= 1
                    k -= x
                else:
                    ans.append("H")
                    h -= 1
        return "".join(ans)
```

#### Java

```java
class Solution {
    public String kthSmallestPath(int[] destination, int k) {
        int v = destination[0], h = destination[1];
        int n = v + h;
        int[][] c = new int[n + 1][h + 1];
        c[0][0] = 1;
        for (int i = 1; i <= n; ++i) {
            c[i][0] = 1;
            for (int j = 1; j <= h; ++j) {
                c[i][j] = c[i - 1][j] + c[i - 1][j - 1];
            }
        }
        StringBuilder ans = new StringBuilder();
        for (int i = n; i > 0; --i) {
            if (h == 0) {
                ans.append('V');
            } else {
                int x = c[v + h - 1][h - 1];
                if (k > x) {
                    ans.append('V');
                    k -= x;
                    --v;
                } else {
                    ans.append('H');
                    --h;
                }
            }
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string kthSmallestPath(vector<int>& destination, int k) {
        int v = destination[0], h = destination[1];
        int n = v + h;
        int c[n + 1][h + 1];
        memset(c, 0, sizeof(c));
        c[0][0] = 1;
        for (int i = 1; i <= n; ++i) {
            c[i][0] = 1;
            for (int j = 1; j <= h; ++j) {
                c[i][j] = c[i - 1][j] + c[i - 1][j - 1];
            }
        }
        string ans;
        for (int i = 0; i < n; ++i) {
            if (h == 0) {
                ans.push_back('V');
            } else {
                int x = c[v + h - 1][h - 1];
                if (k > x) {
                    ans.push_back('V');
                    --v;
                    k -= x;
                } else {
                    ans.push_back('H');
                    --h;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func kthSmallestPath(destination []int, k int) string {
	v, h := destination[0], destination[1]
	n := v + h
	c := make([][]int, n+1)
	for i := range c {
		c[i] = make([]int, h+1)
		c[i][0] = 1
	}
	for i := 1; i <= n; i++ {
		for j := 1; j <= h; j++ {
			c[i][j] = c[i-1][j] + c[i-1][j-1]
		}
	}
	ans := []byte{}
	for i := 0; i < n; i++ {
		if h == 0 {
			ans = append(ans, 'V')
		} else {
			x := c[v+h-1][h-1]
			if k > x {
				ans = append(ans, 'V')
				k -= x
				v--
			} else {
				ans = append(ans, 'H')
				h--
			}
		}
	}
	return string(ans)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
