---
comments: true
difficulty: Medium
rating: 1692
source: Weekly Contest 415 Q2
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3290. Maximum Multiplication Score](https://leetcode.com/problems/maximum-multiplication-score)

[中文文档](/solution/3200-3299/3290.Maximum%20Multiplication%20Score/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>a</code> có kích thước 4 và một mảng số nguyên khác <code>b</code> có kích thước <strong>ít nhất</strong> 4.</p>

<p>Bạn cần chọn 4 chỉ số <code>i<sub>0</sub></code>, <code>i<sub>1</sub></code>, <code>i<sub>2</sub></code> và <code>i<sub>3</sub></code> từ mảng <code>b</code> sao cho <code>i<sub>0</sub> &lt; i<sub>1</sub> &lt; i<sub>2</sub> &lt; i<sub>3</sub></code>. Điểm số của bạn sẽ bằng giá trị <code>a[0] * b[i<sub>0</sub>] + a[1] * b[i<sub>1</sub>] + a[2] * b[i<sub>2</sub>] + a[3] * b[i<sub>3</sub>]</code>.</p>

<p>Trả về điểm số <strong>lớn nhất</strong> có thể đạt được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">a = [3,2,5,6], b = [2,-6,4,-5,-3,2,-7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">26</span></p>

<p><strong>Giải thích:</strong><br />
Ta có thể chọn các chỉ số 0, 1, 2 và 5. Điểm số khi đó là <code>3 * 2 + 2 * (-6) + 5 * 4 + 6 * 2 = 26</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">a = [-1,4,5,-2], b = [-5,-1,-3,-2,-4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong><br />
Ta có thể chọn các chỉ số 0, 1, 3 và 4. Điểm số khi đó là <code>(-1) * (-5) + 4 * (-1) + 5 * (-2) + (-2) * (-4) = -1</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>a.length == 4</code></li>
	<li><code>4 &lt;= b.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= a[i], b[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Memoization

<!-- thinking:start -->

> **Tư duy**
>
> $a$ có bốn phần tử; ta chọn bốn chỉ số tăng dần trong $b$ và tối đa hóa tích vô hướng. $|b|\le 10^5$ khiến việc duyệt $O(n^4)$ bộ chỉ số là không thể. Chỉ có bốn giai đoạn, vì vậy ta dùng quy hoạch động theo vị trí trong $b$.
>
> $\textit{dfs}(i,j)$: đã dùng $i$ phần tử của $a$, đang ở $b[j]$. Bỏ qua $b[j]$, hoặc chọn $a[i]\times b[j]$ rồi tăng cả hai chỉ số. Các trạng thái được ghi nhớ có kích thước $4\times n$.

<!-- thinking:end -->

Ta định nghĩa hàm $\textit{dfs}(i, j)$, biểu diễn điểm số lớn nhất có thể đạt được khi bắt đầu từ phần tử thứ $i$ của mảng $a$ và phần tử thứ $j$ của mảng $b$. Khi đó, đáp án là $\textit{dfs}(0, 0)$.

Hàm $\textit{dfs}(i, j)$ được tính như sau:

- Nếu $j \geq \text{len}(b)$, nghĩa là đã duyệt hết mảng $b$. Khi đó, nếu mảng $a$ cũng đã được duyệt hết thì trả về $0$; nếu không thì trả về âm vô cùng.
- Nếu $i \geq \text{len}(a)$, nghĩa là đã duyệt hết mảng $a$. Trả về $0$.
- Nếu không, ta có thể bỏ qua phần tử thứ $j$ của mảng $b$ và chuyển sang phần tử tiếp theo, tức là $\textit{dfs}(i, j + 1)$; hoặc chọn phần tử thứ $j$ của mảng $b$, khi đó điểm số là $a[i] \times b[j]$ cộng với $\textit{dfs}(i + 1, j + 1)$. Giá trị trả về của $\textit{dfs}(i, j)$ là giá trị lớn hơn trong hai giá trị này.

Ta có thể dùng memoization để tránh tính toán trùng lặp.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m \times n)$. Trong đó, $m$ và $n$ lần lượt là độ dài của các mảng $a$ và $b$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxScore(self, a: List[int], b: List[int]) -> int:
        @cache
        def dfs(i: int, j: int) -> int:
            if j >= len(b):
                return 0 if i >= len(a) else -inf
            if i >= len(a):
                return 0
            return max(dfs(i, j + 1), a[i] * b[j] + dfs(i + 1, j + 1))

        return dfs(0, 0)
```

#### Java

```java
class Solution {
    private Long[][] f;
    private int[] a;
    private int[] b;

    public long maxScore(int[] a, int[] b) {
        f = new Long[a.length][b.length];
        this.a = a;
        this.b = b;
        return dfs(0, 0);
    }

    private long dfs(int i, int j) {
        if (j >= b.length) {
            return i >= a.length ? 0 : Long.MIN_VALUE / 2;
        }
        if (i >= a.length) {
            return 0;
        }
        if (f[i][j] != null) {
            return f[i][j];
        }
        return f[i][j] = Math.max(dfs(i, j + 1), 1L * a[i] * b[j] + dfs(i + 1, j + 1));
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxScore(vector<int>& a, vector<int>& b) {
        int m = a.size(), n = b.size();
        long long f[m][n];
        memset(f, -1, sizeof(f));
        auto dfs = [&](this auto&& dfs, int i, int j) -> long long {
            if (j >= n) {
                return i >= m ? 0 : LLONG_MIN / 2;
            }
            if (i >= m) {
                return 0;
            }
            if (f[i][j] != -1) {
                return f[i][j];
            }
            return f[i][j] = max(dfs(i, j + 1), 1LL * a[i] * b[j] + dfs(i + 1, j + 1));
        };
        return dfs(0, 0);
    }
};
```

#### Go

```go
func maxScore(a []int, b []int) int64 {
	m, n := len(a), len(b)
	f := make([][]int64, m)
	for i := range f {
		f[i] = make([]int64, n)
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	var dfs func(i, j int) int64
	dfs = func(i, j int) int64 {
		if j >= n {
			if i >= m {
				return 0
			}
			return math.MinInt64 / 2
		}
		if i >= m {
			return 0
		}
		if f[i][j] != -1 {
			return f[i][j]
		}
		f[i][j] = max(dfs(i, j+1), int64(a[i])*int64(b[j])+dfs(i+1, j+1))
		return f[i][j]
	}
	return dfs(0, 0)
}
```

#### TypeScript

```ts
function maxScore(a: number[], b: number[]): number {
    const [m, n] = [a.length, b.length];
    const f: number[][] = Array.from({ length: m }, () => Array(n).fill(-1));
    const dfs = (i: number, j: number): number => {
        if (j >= n) {
            return i >= m ? 0 : -Infinity;
        }
        if (i >= m) {
            return 0;
        }
        if (f[i][j] !== -1) {
            return f[i][j];
        }
        return (f[i][j] = Math.max(dfs(i, j + 1), a[i] * b[j] + dfs(i + 1, j + 1)));
    };
    return dfs(0, 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
