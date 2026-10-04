---
comments: true
difficulty: Medium
rating: 2071
source: Weekly Contest 364 Q3
tags:
    - Stack
    - Array
    - Monotonic Stack
---

<!-- problem:start -->

# [2866. Beautiful Towers II](https://leetcode.com/problems/beautiful-towers-ii)

[中文文档](/solution/2800-2899/2866.Beautiful%20Towers%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>maxHeights</code> có độ dài <code>n</code>, được <strong>đánh chỉ số từ 0</strong>.</p>

<p>Bạn cần xây dựng <code>n</code> tòa tháp trên trục tọa độ. Tòa tháp thứ <code>i<sup>th</sup></code> được xây tại tọa độ <code>i</code> và có chiều cao <code>heights[i]</code>.</p>

<p>Một cấu hình các tòa tháp là <strong>đẹp</strong> nếu thỏa mãn các điều kiện sau:</p>

<ol>
	<li><code>1 &lt;= heights[i] &lt;= maxHeights[i]</code></li>
	<li><code>heights</code> là một mảng <strong>dạng núi</strong>.</li>
</ol>

<p>Mảng <code>heights</code> là mảng <strong>dạng núi</strong> nếu tồn tại một chỉ số <code>i</code> sao cho:</p>

<ul>
	<li>Với mọi <code>0 &lt; j &lt;= i</code>, <code>heights[j - 1] &lt;= heights[j]</code></li>
	<li>Với mọi <code>i &lt;= k &lt; n - 1</code>, <code>heights[k + 1] &lt;= heights[k]</code></li>
</ul>

<p>Trả về <em><strong>tổng chiều cao lớn nhất có thể</strong> của một cấu hình các tòa tháp đẹp</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> maxHeights = [5,3,4,1,1]
<strong>Đầu ra:</strong> 13
<strong>Giải thích:</strong> Một cấu hình đẹp có tổng lớn nhất là heights = [5,3,3,1,1]. Cấu hình này đẹp vì:
- 1 &lt;= heights[i] &lt;= maxHeights[i]
- heights là một mảng dạng núi với đỉnh i = 0.
Có thể chứng minh rằng không tồn tại cấu hình đẹp nào khác có tổng chiều cao lớn hơn 13.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> maxHeights = [6,5,3,9,2,7]
<strong>Đầu ra:</strong> 22
<strong>Giải thích:</strong> Một cấu hình đẹp có tổng lớn nhất là heights = [3,3,3,9,2,2]. Cấu hình này đẹp vì:
- 1 &lt;= heights[i] &lt;= maxHeights[i]
- heights là một mảng dạng núi với đỉnh i = 3.
Có thể chứng minh rằng không tồn tại cấu hình đẹp nào khác có tổng chiều cao lớn hơn 22.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> maxHeights = [3,2,5,5,2,3]
<strong>Đầu ra:</strong> 18
<strong>Giải thích:</strong> Một cấu hình đẹp có tổng lớn nhất là heights = [2,2,5,5,2,2]. Cấu hình này đẹp vì:
- 1 &lt;= heights[i] &lt;= maxHeights[i]
- heights là một mảng dạng núi với đỉnh i = 2.
Lưu ý rằng với cấu hình này, i = 3 cũng có thể được xem là một đỉnh.
Có thể chứng minh rằng không tồn tại cấu hình đẹp nào khác có tổng chiều cao lớn hơn 18.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == maxHeights.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= maxHeights[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động + Stack đơn điệu

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n$ lớn, việc mở rộng tuyến tính từ mỗi đỉnh sẽ quá chậm. Quy hoạch động dùng stack đơn điệu tương tự bài toán tối ưu hóa các tòa tháp sẽ tính tổng chiều cao ở bên trái và bên phải; cộng hai giá trị tại $i$ rồi trừ $maxHeights[i]$ một lần để thu được đáp án.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là tổng chiều cao của cấu hình các tòa tháp đẹp trong đó tòa tháp cuối cùng là tòa tháp cao nhất trong $i+1$ tòa tháp đầu tiên. Ta có công thức chuyển trạng thái sau:

$$
f[i]=
\begin{cases}
f[i-1]+heights[i],&\textit{if } heights[i]\geq heights[i-1]\\
heights[i]\times(i-j)+f[j],&\textit{if } heights[i]<heights[i-1]
\end{cases}
$$

Trong đó $j$ là chỉ số của tòa tháp đầu tiên ở bên trái tòa tháp cuối cùng có chiều cao nhỏ hơn hoặc bằng $heights[i]$. Ta có thể dùng một stack đơn điệu để duy trì chỉ số này.

Ta có thể dùng phương pháp tương tự để tìm $g[i]$, biểu thị tổng chiều cao của cấu hình các tòa tháp đẹp khi xét từ phải sang trái và tòa tháp thứ $i$ là tòa tháp cao nhất. Đáp án cuối cùng là giá trị lớn nhất của $f[i]+g[i]-heights[i]$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $maxHeights$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumSumOfHeights(self, maxHeights: List[int]) -> int:
        n = len(maxHeights)
        stk = []
        left = [-1] * n
        for i, x in enumerate(maxHeights):
            while stk and maxHeights[stk[-1]] > x:
                stk.pop()
            if stk:
                left[i] = stk[-1]
            stk.append(i)
        stk = []
        right = [n] * n
        for i in range(n - 1, -1, -1):
            x = maxHeights[i]
            while stk and maxHeights[stk[-1]] >= x:
                stk.pop()
            if stk:
                right[i] = stk[-1]
            stk.append(i)
        f = [0] * n
        for i, x in enumerate(maxHeights):
            if i and x >= maxHeights[i - 1]:
                f[i] = f[i - 1] + x
            else:
                j = left[i]
                f[i] = x * (i - j) + (f[j] if j != -1 else 0)
        g = [0] * n
        for i in range(n - 1, -1, -1):
            if i < n - 1 and maxHeights[i] >= maxHeights[i + 1]:
                g[i] = g[i + 1] + maxHeights[i]
            else:
                j = right[i]
                g[i] = maxHeights[i] * (j - i) + (g[j] if j != n else 0)
        return max(a + b - c for a, b, c in zip(f, g, maxHeights))
```

#### Java

```java
class Solution {
    public long maximumSumOfHeights(List<Integer> maxHeights) {
        int n = maxHeights.size();
        Deque<Integer> stk = new ArrayDeque<>();
        int[] left = new int[n];
        int[] right = new int[n];
        Arrays.fill(left, -1);
        Arrays.fill(right, n);
        for (int i = 0; i < n; ++i) {
            int x = maxHeights.get(i);
            while (!stk.isEmpty() && maxHeights.get(stk.peek()) > x) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                left[i] = stk.peek();
            }
            stk.push(i);
        }
        stk.clear();
        for (int i = n - 1; i >= 0; --i) {
            int x = maxHeights.get(i);
            while (!stk.isEmpty() && maxHeights.get(stk.peek()) >= x) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                right[i] = stk.peek();
            }
            stk.push(i);
        }
        long[] f = new long[n];
        long[] g = new long[n];
        for (int i = 0; i < n; ++i) {
            int x = maxHeights.get(i);
            if (i > 0 && x >= maxHeights.get(i - 1)) {
                f[i] = f[i - 1] + x;
            } else {
                int j = left[i];
                f[i] = 1L * x * (i - j) + (j >= 0 ? f[j] : 0);
            }
        }
        for (int i = n - 1; i >= 0; --i) {
            int x = maxHeights.get(i);
            if (i < n - 1 && x >= maxHeights.get(i + 1)) {
                g[i] = g[i + 1] + x;
            } else {
                int j = right[i];
                g[i] = 1L * x * (j - i) + (j < n ? g[j] : 0);
            }
        }
        long ans = 0;
        for (int i = 0; i < n; ++i) {
            ans = Math.max(ans, f[i] + g[i] - maxHeights.get(i));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maximumSumOfHeights(vector<int>& maxHeights) {
        int n = maxHeights.size();
        stack<int> stk;
        vector<int> left(n, -1);
        vector<int> right(n, n);
        for (int i = 0; i < n; ++i) {
            int x = maxHeights[i];
            while (!stk.empty() && maxHeights[stk.top()] > x) {
                stk.pop();
            }
            if (!stk.empty()) {
                left[i] = stk.top();
            }
            stk.push(i);
        }
        stk = stack<int>();
        for (int i = n - 1; ~i; --i) {
            int x = maxHeights[i];
            while (!stk.empty() && maxHeights[stk.top()] >= x) {
                stk.pop();
            }
            if (!stk.empty()) {
                right[i] = stk.top();
            }
            stk.push(i);
        }
        long long f[n], g[n];
        for (int i = 0; i < n; ++i) {
            int x = maxHeights[i];
            if (i && x >= maxHeights[i - 1]) {
                f[i] = f[i - 1] + x;
            } else {
                int j = left[i];
                f[i] = 1LL * x * (i - j) + (j != -1 ? f[j] : 0);
            }
        }
        for (int i = n - 1; ~i; --i) {
            int x = maxHeights[i];
            if (i < n - 1 && x >= maxHeights[i + 1]) {
                g[i] = g[i + 1] + x;
            } else {
                int j = right[i];
                g[i] = 1LL * x * (j - i) + (j != n ? g[j] : 0);
            }
        }
        long long ans = 0;
        for (int i = 0; i < n; ++i) {
            ans = max(ans, f[i] + g[i] - maxHeights[i]);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumSumOfHeights(maxHeights []int) (ans int64) {
	n := len(maxHeights)
	stk := []int{}
	left := make([]int, n)
	right := make([]int, n)
	for i := range left {
		left[i] = -1
		right[i] = n
	}
	for i, x := range maxHeights {
		for len(stk) > 0 && maxHeights[stk[len(stk)-1]] > x {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			left[i] = stk[len(stk)-1]
		}
		stk = append(stk, i)
	}
	stk = []int{}
	for i := n - 1; i >= 0; i-- {
		x := maxHeights[i]
		for len(stk) > 0 && maxHeights[stk[len(stk)-1]] >= x {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			right[i] = stk[len(stk)-1]
		}
		stk = append(stk, i)
	}
	f := make([]int64, n)
	g := make([]int64, n)
	for i, x := range maxHeights {
		if i > 0 && x >= maxHeights[i-1] {
			f[i] = f[i-1] + int64(x)
		} else {
			j := left[i]
			f[i] = int64(x) * int64(i-j)
			if j != -1 {
				f[i] += f[j]
			}
		}
	}
	for i := n - 1; i >= 0; i-- {
		x := maxHeights[i]
		if i < n-1 && x >= maxHeights[i+1] {
			g[i] = g[i+1] + int64(x)
		} else {
			j := right[i]
			g[i] = int64(x) * int64(j-i)
			if j != n {
				g[i] += g[j]
			}
		}
	}
	for i, x := range maxHeights {
		ans = max(ans, f[i]+g[i]-int64(x))
	}
	return
}
```

#### TypeScript

```ts
function maximumSumOfHeights(maxHeights: number[]): number {
    const n = maxHeights.length;
    const stk: number[] = [];
    const left: number[] = Array(n).fill(-1);
    const right: number[] = Array(n).fill(n);
    for (let i = 0; i < n; ++i) {
        const x = maxHeights[i];
        while (stk.length && maxHeights[stk.at(-1)] > x) {
            stk.pop();
        }
        if (stk.length) {
            left[i] = stk.at(-1);
        }
        stk.push(i);
    }
    stk.length = 0;
    for (let i = n - 1; ~i; --i) {
        const x = maxHeights[i];
        while (stk.length && maxHeights[stk.at(-1)] >= x) {
            stk.pop();
        }
        if (stk.length) {
            right[i] = stk.at(-1);
        }
        stk.push(i);
    }
    const f: number[] = Array(n).fill(0);
    const g: number[] = Array(n).fill(0);
    for (let i = 0; i < n; ++i) {
        const x = maxHeights[i];
        if (i && x >= maxHeights[i - 1]) {
            f[i] = f[i - 1] + x;
        } else {
            const j = left[i];
            f[i] = x * (i - j) + (j >= 0 ? f[j] : 0);
        }
    }
    for (let i = n - 1; ~i; --i) {
        const x = maxHeights[i];
        if (i + 1 < n && x >= maxHeights[i + 1]) {
            g[i] = g[i + 1] + x;
        } else {
            const j = right[i];
            g[i] = x * (j - i) + (j < n ? g[j] : 0);
        }
    }
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        ans = Math.max(ans, f[i] + g[i] - maxHeights[i]);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
