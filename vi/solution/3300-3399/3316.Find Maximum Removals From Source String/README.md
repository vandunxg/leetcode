---
comments: true
difficulty: Medium
rating: 2062
source: Biweekly Contest 141 Q3
tags:
    - Array
    - Hash Table
    - Two Pointers
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [3316. Find Maximum Removals From Source String](https://leetcode.com/problems/find-maximum-removals-from-source-string)

[中文文档](/solution/3300-3399/3316.Find%20Maximum%20Removals%20From%20Source%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>source</code> có độ dài <code>n</code>, một chuỗi <code>pattern</code> là một <span data-keyword="subsequence-string">dãy con</span> của <code>source</code>, và một mảng số nguyên <code>targetIndices</code> <strong>đã được sắp xếp</strong> chứa các số <strong>khác nhau</strong> trong phạm vi <code>[0, n - 1]</code>.</p>

<p>Ta định nghĩa một <strong>thao tác</strong> là xóa một ký tự tại chỉ số <code>idx</code> khỏi <code>source</code> sao cho:</p>

<ul>
	<li><code>idx</code> là một phần tử của <code>targetIndices</code>.</li>
	<li><code>pattern</code> vẫn là một <span data-keyword="subsequence-string">dãy con</span> của <code>source</code> sau khi xóa ký tự.</li>
</ul>

<p>Việc thực hiện một thao tác <strong>không</strong> làm thay đổi chỉ số của các ký tự khác trong <code>source</code>. Ví dụ, nếu xóa <code>&#39;c&#39;</code> khỏi <code>&quot;acb&quot;</code>, ký tự tại chỉ số 2 vẫn sẽ là <code>&#39;b&#39;</code>.</p>

<p>Hãy trả về số <strong>thao tác</strong> <em>lớn nhất</em> có thể thực hiện.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">source = &quot;abbaa&quot;, pattern = &quot;aba&quot;, </span>targetIndices<span class="example-io"> = [0,1,2]</span></p>

<p><strong>Đầu ra:</strong> 1</p>

<p><strong>Giải thích:</strong></p>

<p>Ta không thể xóa <code>source[0]</code>, nhưng có thể thực hiện một trong hai thao tác sau:</p>

<ul>
	<li>Xóa <code>source[1]</code>, khi đó <code>source</code> trở thành <code>&quot;a_baa&quot;</code>.</li>
	<li>Xóa <code>source[2]</code>, khi đó <code>source</code> trở thành <code>&quot;ab_aa&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">source = &quot;bcda&quot;, pattern = &quot;d&quot;, </span>targetIndices<span class="example-io"> = [0,3]</span></p>

<p><strong>Đầu ra:</strong> 2</p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể xóa <code>source[0]</code> và <code>source[3]</code> trong hai thao tác.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">source = &quot;dda&quot;, pattern = &quot;dda&quot;, </span>targetIndices<span class="example-io"> = [0,1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta không thể xóa bất kỳ ký tự nào khỏi <code>source</code>.</p>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">source = </span>&quot;yeyeykyded&quot;<span class="example-io">, pattern = </span>&quot;yeyyd&quot;<span class="example-io">, </span>targetIndices<span class="example-io"> = </span>[0,2,3,4]</p>

<p><strong>Đầu ra:</strong> 2</p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể xóa <code>source[2]</code> và <code>source[3]</code> trong hai thao tác.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == source.length &lt;= 3 * 10<sup>3</sup></code></li>
	<li><code>1 &lt;= pattern.length &lt;= n</code></li>
	<li><code>1 &lt;= targetIndices.length &lt;= n</code></li>
	<li><code>targetIndices</code> được sắp xếp theo thứ tự tăng dần.</li>
	<li>Dữ liệu đầu vào được tạo sao cho <code>targetIndices</code> chứa các phần tử khác nhau trong phạm vi <code>[0, n - 1]</code>.</li>
	<li><code>source</code> và <code>pattern</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
	<li>Dữ liệu đầu vào được tạo sao cho <code>pattern</code> xuất hiện như một dãy con trong <code>source</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Ta chỉ có thể xóa các chỉ số trong $\textit{targetIndices}$ khi $\textit{pattern}$ vẫn là một dãy con. Với $n \le 3 \times 10^3$, DP đối sánh với độ phức tạp $O(mn)$ là phù hợp.
>
> Trạng thái cần theo dõi cả tiến độ đối sánh và số lần xóa đã thực hiện. Các trạng thái không thể đạt được được giữ ở $-\infty$ để một đối sánh thất bại không bị xem là hợp lệ.
>
> $f[i][j]$ chọn phương án tốt hơn giữa bỏ qua $\textit{source}[i-1]$ (cộng $1$ nếu chỉ số đó có thể xóa) và đối sánh các ký tự hiện tại. $f[m][n]$ là đáp án.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là số lần xóa lớn nhất trong $i$ ký tự đầu tiên của $\textit{source}$, sao cho chúng đối sánh với $j$ ký tự đầu tiên của $\textit{pattern}$. Ban đầu, $f[0][0] = 0$, còn các giá trị $f[i][j] = -\infty$ khác.

Với $f[i][j]$, ta có hai lựa chọn:

- Bỏ qua ký tự thứ $i$ của $\textit{source}$, khi đó $f[i][j] = f[i-1][j] + \text{int}(i-1 \in \textit{targetIndices})$;
- Nếu $\textit{source}[i-1] = \textit{pattern}[j-1]$, ta có thể đối sánh ký tự thứ $i$ của $\textit{source}$, khi đó $f[i][j] = \max(f[i][j], f[i-1][j-1])$.

Đáp án cuối cùng là $f[m][n]$.

Độ phức tạp thời gian là $O(m \times n)$, và độ phức tạp không gian là $O(m \times n)$. Trong đó, $m$ và $n$ lần lượt là độ dài của $\textit{source}$ và $\textit{pattern}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxRemovals(self, source: str, pattern: str, targetIndices: List[int]) -> int:
        m, n = len(source), len(pattern)
        f = [[-inf] * (n + 1) for _ in range(m + 1)]
        f[0][0] = 0
        s = set(targetIndices)
        for i, c in enumerate(source, 1):
            for j in range(n + 1):
                f[i][j] = f[i - 1][j] + int((i - 1) in s)
                if j and c == pattern[j - 1]:
                    f[i][j] = max(f[i][j], f[i - 1][j - 1])
        return f[m][n]
```

#### Java

```java
class Solution {
    public int maxRemovals(String source, String pattern, int[] targetIndices) {
        int m = source.length(), n = pattern.length();
        int[][] f = new int[m + 1][n + 1];
        final int inf = Integer.MAX_VALUE / 2;
        for (var g : f) {
            Arrays.fill(g, -inf);
        }
        f[0][0] = 0;
        int[] s = new int[m];
        for (int i : targetIndices) {
            s[i] = 1;
        }
        for (int i = 1; i <= m; ++i) {
            for (int j = 0; j <= n; ++j) {
                f[i][j] = f[i - 1][j] + s[i - 1];
                if (j > 0 && source.charAt(i - 1) == pattern.charAt(j - 1)) {
                    f[i][j] = Math.max(f[i][j], f[i - 1][j - 1]);
                }
            }
        }
        return f[m][n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxRemovals(string source, string pattern, vector<int>& targetIndices) {
        int m = source.length(), n = pattern.length();
        vector<vector<int>> f(m + 1, vector<int>(n + 1, INT_MIN / 2));
        f[0][0] = 0;

        vector<int> s(m);
        for (int i : targetIndices) {
            s[i] = 1;
        }

        for (int i = 1; i <= m; ++i) {
            for (int j = 0; j <= n; ++j) {
                f[i][j] = f[i - 1][j] + s[i - 1];
                if (j > 0 && source[i - 1] == pattern[j - 1]) {
                    f[i][j] = max(f[i][j], f[i - 1][j - 1]);
                }
            }
        }

        return f[m][n];
    }
};
```

#### Go

```go
func maxRemovals(source string, pattern string, targetIndices []int) int {
	m, n := len(source), len(pattern)
	f := make([][]int, m+1)
	for i := range f {
		f[i] = make([]int, n+1)
		for j := range f[i] {
			f[i][j] = -math.MaxInt32 / 2
		}
	}
	f[0][0] = 0

	s := make([]int, m)
	for _, i := range targetIndices {
		s[i] = 1
	}

	for i := 1; i <= m; i++ {
		for j := 0; j <= n; j++ {
			f[i][j] = f[i-1][j] + s[i-1]
			if j > 0 && source[i-1] == pattern[j-1] {
				f[i][j] = max(f[i][j], f[i-1][j-1])
			}
		}
	}

	return f[m][n]
}
```

#### TypeScript

```ts
function maxRemovals(source: string, pattern: string, targetIndices: number[]): number {
    const m = source.length;
    const n = pattern.length;
    const f: number[][] = Array.from({ length: m + 1 }, () => Array(n + 1).fill(-Infinity));
    f[0][0] = 0;

    const s = Array(m).fill(0);
    for (const i of targetIndices) {
        s[i] = 1;
    }

    for (let i = 1; i <= m; i++) {
        for (let j = 0; j <= n; j++) {
            f[i][j] = f[i - 1][j] + s[i - 1];
            if (j > 0 && source[i - 1] === pattern[j - 1]) {
                f[i][j] = Math.max(f[i][j], f[i - 1][j - 1]);
            }
        }
    }

    return f[m][n];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
