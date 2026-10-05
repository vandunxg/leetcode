---
comments: true
difficulty: Hard
rating: 1987
source: Weekly Contest 488 Q4
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3836. Maximum Score Using Exactly K Pairs](https://leetcode.com/problems/maximum-score-using-exactly-k-pairs)

[中文文档](/solution/3800-3899/3836.Maximum%20Score%20Using%20Exactly%20K%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên <code>nums1</code> và <code>nums2</code> có độ dài lần lượt là <code>n</code> và <code>m</code>, cùng một số nguyên <code>k</code>.</p>

<p>Bạn phải chọn <strong>đúng</strong> <code>k</code> cặp chỉ số <code>(i<sub>1</sub>, j<sub>1</sub>), (i<sub>2</sub>, j<sub>2</sub>), ..., (i<sub>k</sub>, j<sub>k</sub>)</code> sao cho:</p>

<ul>
	<li><code>0 &lt;= i<sub>1</sub> &lt; i<sub>2</sub> &lt; ... &lt; i<sub>k</sub> &lt; n</code></li>
	<li><code>0 &lt;= j<sub>1</sub> &lt; j<sub>2</sub> &lt; ... &lt; j<sub>k</sub> &lt; m</code></li>
</ul>

<p>Với mỗi cặp đã chọn <code>(i, j)</code>, bạn nhận được điểm <code>nums1[i] * nums2[j]</code>.</p>

<p><strong>Điểm</strong> tổng là <strong>tổng</strong> các tích của tất cả cặp đã chọn.</p>

<p>Trả về một số nguyên biểu thị <strong>tổng điểm lớn nhất</strong> có thể đạt được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [1,3,2], nums2 = [4,5,1], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">22</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một lựa chọn tối ưu cho các cặp chỉ số là:</p>

<ul>
	<li><code>(i<sub>1</sub>, j<sub>1</sub>) = (1, 0)</code>, có điểm <code>3 * 4 = 12</code></li>
	<li><code>(i<sub>2</sub>, j<sub>2</sub>) = (2, 1)</code>, có điểm <code>2 * 5 = 10</code></li>
</ul>

<p>Tổng điểm là <code>12 + 10 = 22</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [-2,0,5], nums2 = [-3,4,-1,2], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">26</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một lựa chọn tối ưu cho các cặp chỉ số là:</p>

<ul>
	<li><code>(i<sub>1</sub>, j<sub>1</sub>) = (0, 0)</code>, có điểm <code>-2 * -3 = 6</code></li>
	<li><code>(i<sub>2</sub>, j<sub>2</sub>) = (2, 1)</code>, có điểm <code>5 * 4 = 20</code></li>
</ul>

<p>Tổng điểm là <code>6 + 20 = 26</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [-3,-2], nums2 = [1,2], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-7</span></p>

<p><strong>Giải thích:</strong></p>

<p>Lựa chọn tối ưu cho các cặp chỉ số là:</p>

<ul>
	<li><code>(i<sub>1</sub>, j<sub>1</sub>) = (0, 0)</code>, có điểm <code>-3 * 1 = -3</code></li>
	<li><code>(i<sub>2</sub>, j<sub>2</sub>) = (1, 1)</code>, có điểm <code>-2 * 2 = -4</code></li>
</ul>

<p>Tổng điểm là <code>-3 + (-4) = -7</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums1.length &lt;= 100</code></li>
	<li><code>1 &lt;= m == nums2.length &lt;= 100</code></li>
	<li><code>-10<sup>6</sup> &lt;= nums1[i], nums2[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= k &lt;= min(n, m)</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Chọn $k$ cặp chỉ số tăng dần từ hai mảng và tối đa hóa tổng các tích. Vì $n,m \le 100$ và $k \le \min(n,m)$, ta có thể dùng DP 3 chiều.
>
> Mỗi cặp sử dụng một phần tử ở mỗi phía và không thể quay lui. Với mỗi phần tử, ta có thể bỏ qua $nums1$, bỏ qua $nums2$, hoặc ghép cả hai thành một cặp.
>
> Gọi $f[i][j][k]$ là điểm tốt nhất khi dùng hai tiền tố và chọn đúng $k$ cặp, với ba chuyển trạng thái tương ứng.
>
> Hai tiền tố rỗng với không cặp có điểm $0$; các trạng thái khác bắt đầu bằng $-\infty$. Đáp án là $f[n][m][K]$.

<!-- thinking:end -->

Ta ký hiệu độ dài của hai mảng $\textit{nums1}$ và $\textit{nums2}$ lần lượt là $n$ và $m$, đồng thời ký hiệu $k$ trong đề bài là $K$.

Ta định nghĩa một mảng ba chiều $f$, trong đó $f[i][j][k]$ biểu thị điểm lớn nhất khi chọn đúng $k$ cặp chỉ số từ $i$ phần tử đầu tiên của $\textit{nums1}$ và $j$ phần tử đầu tiên của $\textit{nums2}$. Ban đầu, $f[0][0][0] = 0$, còn tất cả giá trị khác của $f[i][j][k]$ là âm vô cùng.

Ta có thể tính $f[i][j][k]$ theo công thức chuyển trạng thái sau:

$$
f[i][j][k] = \max\begin{cases}
f[i-1][j][k], \\
f[i][j-1][k], \\
f[i-1][j-1][k-1] + nums1[i-1] * nums2[j-1]
\end{cases}
$$

Trường hợp đầu tiên biểu thị việc không chọn phần tử thứ $i$ của $\textit{nums1}$, trường hợp thứ hai biểu thị việc không chọn phần tử thứ $j$ của $\textit{nums2}$, còn trường hợp thứ ba biểu thị việc chọn phần tử thứ $i$ của $\textit{nums1}$ và phần tử thứ $j$ của $\textit{nums2}$ làm một cặp chỉ số.

Cuối cùng, ta trả về $f[n][m][K]$.

Độ phức tạp thời gian là $O(m \times n \times K)$ và độ phức tạp không gian là $O(m \times n \times K)$, trong đó $n$ và $m$ lần lượt là độ dài của hai mảng $\textit{nums1}$ và $\textit{nums2}$, còn $K$ là $k$ trong đề bài.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxScore(self, nums1: List[int], nums2: List[int], K: int) -> int:
        n, m = len(nums1), len(nums2)
        f = [[[-inf] * (K + 1) for _ in range(m + 1)] for _ in range(n + 1)]
        f[0][0][0] = 0
        for i in range(n + 1):
            for j in range(m + 1):
                for k in range(K + 1):
                    if i > 0:
                        f[i][j][k] = max(f[i][j][k], f[i - 1][j][k])
                    if j > 0:
                        f[i][j][k] = max(f[i][j][k], f[i][j - 1][k])
                    if i > 0 and j > 0 and k > 0:
                        f[i][j][k] = max(f[i][j][k], f[i - 1][j - 1][k - 1] + nums1[i - 1] * nums2[j - 1])
        return f[n][m][K]
```

#### Java

```java
class Solution {
    public long maxScore(int[] nums1, int[] nums2, int K) {
        int n = nums1.length, m = nums2.length;
        long NEG = Long.MIN_VALUE / 4;
        long[][][] f = new long[n + 1][m + 1][K + 1];
        for (int i = 0; i <= n; i++) {
            for (int j = 0; j <= m; j++) {
                Arrays.fill(f[i][j], NEG);
            }
        }
        f[0][0][0] = 0;
        for (int i = 0; i <= n; i++) {
            for (int j = 0; j <= m; j++) {
                for (int k = 0; k <= K; k++) {
                    if (i > 0) {
                        f[i][j][k] = Math.max(f[i][j][k], f[i - 1][j][k]);
                    }
                    if (j > 0) {
                        f[i][j][k] = Math.max(f[i][j][k], f[i][j - 1][k]);
                    }
                    if (i > 0 && j > 0 && k > 0) {
                        f[i][j][k] = Math.max(f[i][j][k],
                            f[i - 1][j - 1][k - 1] + (long) nums1[i - 1] * nums2[j - 1]);
                    }
                }
            }
        }
        return f[n][m][K];
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxScore(vector<int>& nums1, vector<int>& nums2, int K) {
        int n = nums1.size(), m = nums2.size();
        long long NEG = LLONG_MIN / 4;
        vector f(n + 1, vector(m + 1, vector<long long>(K + 1, NEG)));
        f[0][0][0] = 0;
        for (int i = 0; i <= n; i++) {
            for (int j = 0; j <= m; j++) {
                for (int k = 0; k <= K; k++) {
                    if (i > 0) {
                        f[i][j][k] = max(f[i][j][k], f[i - 1][j][k]);
                    }
                    if (j > 0) {
                        f[i][j][k] = max(f[i][j][k], f[i][j - 1][k]);
                    }
                    if (i > 0 && j > 0 && k > 0) {
                        f[i][j][k] = max(
                            f[i][j][k],
                            f[i - 1][j - 1][k - 1] + 1LL * nums1[i - 1] * nums2[j - 1]);
                    }
                }
            }
        }
        return f[n][m][K];
    }
};
```

#### Go

```go
func maxScore(nums1 []int, nums2 []int, K int) int64 {
	n, m := len(nums1), len(nums2)
	NEG := int64(math.MinInt64 / 4)
	f := make([][][]int64, n+1)
	for i := 0; i <= n; i++ {
		f[i] = make([][]int64, m+1)
		for j := 0; j <= m; j++ {
			f[i][j] = make([]int64, K+1)
			for k := 0; k <= K; k++ {
				f[i][j][k] = NEG
			}
		}
	}
	f[0][0][0] = 0
	for i := 0; i <= n; i++ {
		for j := 0; j <= m; j++ {
			for k := 0; k <= K; k++ {
				if i > 0 {
					f[i][j][k] = max(f[i][j][k], f[i-1][j][k])
				}
				if j > 0 {
					f[i][j][k] = max(f[i][j][k], f[i][j-1][k])
				}
				if i > 0 && j > 0 && k > 0 {
					f[i][j][k] = max(
						f[i][j][k],
						f[i-1][j-1][k-1]+int64(nums1[i-1])*int64(nums2[j-1]),
					)
				}
			}
		}
	}
	return f[n][m][K]
}
```

#### TypeScript

```ts
function maxScore(nums1: number[], nums2: number[], K: number): number {
    const n = nums1.length,
        m = nums2.length;
    const NEG = -1e18;
    const f = Array.from({ length: n + 1 }, () =>
        Array.from({ length: m + 1 }, () => Array(K + 1).fill(NEG)),
    );
    f[0][0][0] = 0;
    for (let i = 0; i <= n; i++) {
        for (let j = 0; j <= m; j++) {
            for (let k = 0; k <= K; k++) {
                if (i > 0) {
                    f[i][j][k] = Math.max(f[i][j][k], f[i - 1][j][k]);
                }
                if (j > 0) {
                    f[i][j][k] = Math.max(f[i][j][k], f[i][j - 1][k]);
                }
                if (i > 0 && j > 0 && k > 0) {
                    f[i][j][k] = Math.max(
                        f[i][j][k],
                        f[i - 1][j - 1][k - 1] + nums1[i - 1] * nums2[j - 1],
                    );
                }
            }
        }
    }
    return f[n][m][K];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
