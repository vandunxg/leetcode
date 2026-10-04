---
comments: true
difficulty: Medium
tags:
    - String
    - Binary Search
    - Dynamic Programming
    - Sliding Window
    - Hash Function
---

<!-- problem:start -->

# [3135. Equalize Strings by Adding or Removing Characters at Ends 🔒](https://leetcode.com/problems/equalize-strings-by-adding-or-removing-characters-at-ends)

[中文文档](/solution/3100-3199/3135.Equalize%20Strings%20by%20Adding%20or%20Removing%20Characters%20at%20Ends/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>initial</code> và <code>target</code>, nhiệm vụ của bạn là biến đổi <code>initial</code> bằng cách thực hiện một loạt thao tác để nó trở thành <code>target</code>.</p>

<p>Trong một thao tác, bạn chỉ có thể thêm hoặc xóa <strong>một ký tự</strong> ở <em>đầu</em> hoặc <em>cuối</em> chuỗi <code>initial</code>.</p>

<p>Hãy trả về số thao tác <strong>ít nhất</strong> cần thiết để <em>biến đổi</em> <code>initial</code> thành <code>target</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">initial = &quot;abcde&quot;, target = &quot;cdef&quot;</span></p>

<p><strong>Đầu ra:</strong> 3</p>

<p><strong>Giải thích:</strong></p>

<p>Xóa <code>&#39;a&#39;</code> và <code>&#39;b&#39;</code> ở đầu <code>initial</code>, sau đó thêm <code>&#39;f&#39;</code> vào cuối.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">initial = &quot;axxy&quot;, target = &quot;yabx&quot;</span></p>

<p><strong>Đầu ra:</strong> 6</p>

<p><strong>Giải thích:</strong></p>

<table border="1">
	<tbody>
		<tr>
			<th>Thao tác</th>
			<th>Chuỗi sau thao tác</th>
		</tr>
		<tr>
			<td>Thêm <code>&#39;y&#39;</code> vào đầu</td>
			<td><code>&quot;yaxxy&quot;</code></td>
		</tr>
		<tr>
			<td>Xóa ở cuối</td>
			<td><code>&quot;yaxx&quot;</code></td>
		</tr>
		<tr>
			<td>Xóa ở cuối</td>
			<td><code>&quot;yax&quot;</code></td>
		</tr>
		<tr>
			<td>Xóa ở cuối</td>
			<td><code>&quot;ya&quot;</code></td>
		</tr>
		<tr>
			<td>Thêm <code>&#39;b&#39;</code> vào cuối</td>
			<td><code>&quot;yab&quot;</code></td>
		</tr>
		<tr>
			<td>Thêm <code>&#39;x&#39;</code> vào cuối</td>
			<td><code>&quot;yabx&quot;</code></td>
		</tr>
	</tbody>
</table>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">initial = &quot;xyz&quot;, target = &quot;xyz&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không cần thực hiện thao tác nào vì hai chuỗi đã bằng nhau.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= initial.length, target.length &lt;= 1000</code></li>
	<li><code>initial</code> và <code>target</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Lập trình động

<!-- thinking:start -->

> **Tư duy**
>
> Vì chỉ có thể xóa hoặc thêm các ký tự ở đầu và cuối, nên ta phải giữ lại một chuỗi con liên tiếp chung. Nếu thử mọi chuỗi con của $initial$ trong $target$, độ phức tạp sẽ là $O(m^2n)$.
>
> Số thao tác bằng số ký tự ở tiền tố/hậu tố bị loại khỏi $initial$ cộng với số ký tự còn thiếu ở hai phía của $target$, tức là $m+n-2\cdot mx$ với độ dài chuỗi con chung dài nhất là $mx$.
>
> Dùng DP chuẩn: $f[i][j]$ là độ dài chuỗi con chung kết thúc tại $initial[i-1]$ và $target[j-1]$, được kéo dài khi hai ký tự bằng nhau. Đáp án sử dụng giá trị lớn nhất toàn cục $mx$.

<!-- thinking:end -->

Giả sử độ dài của hai chuỗi `initial` và `target` lần lượt là $m$ và $n$.

Theo mô tả bài toán, ta chỉ cần tìm độ dài $mx$ của chuỗi con chung dài nhất của `initial` và `target`. Sau đó, ta có thể xóa $m - mx$ ký tự khỏi `initial` và thêm $n - mx$ ký tự để biến `initial` thành `target`. Vì vậy, đáp án là $m + n - 2 \times mx$.

Ta có thể sử dụng quy hoạch động để tìm độ dài $mx$ của chuỗi con chung dài nhất của `initial` và `target`. Ta định nghĩa một mảng hai chiều $f$, trong đó $f[i][j]$ là độ dài của chuỗi con chung dài nhất kết thúc tại `initial[i - 1]` và `target[j - 1]`. Khi đó, ta có công thức chuyển trạng thái:

$$
f[i][j] = \begin{cases}
f[i - 1][j - 1] + 1, & \textit{if } \textit{initial}[i - 1] = \textit{target}[j - 1], \\
0, & \textit{otherwise}.
\end{cases}
$$

Khi đó $mx = \max f[i][j]$, và đáp án cuối cùng là $m + n - 2 \times mx$.

Độ phức tạp thời gian là $O(m \times n)$, còn độ phức tạp không gian là $O(m \times n)$. Trong đó, $m$ và $n$ lần lượt là độ dài của `initial` và `target`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, initial: str, target: str) -> int:
        m, n = len(initial), len(target)
        f = [[0] * (n + 1) for _ in range(m + 1)]
        mx = 0
        for i, a in enumerate(initial, 1):
            for j, b in enumerate(target, 1):
                if a == b:
                    f[i][j] = f[i - 1][j - 1] + 1
                    mx = max(mx, f[i][j])
        return m + n - mx * 2
```

#### Java

```java
class Solution {
    public int minOperations(String initial, String target) {
        int m = initial.length(), n = target.length();
        int[][] f = new int[m + 1][n + 1];
        int mx = 0;
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (initial.charAt(i - 1) == target.charAt(j - 1)) {
                    f[i][j] = f[i - 1][j - 1] + 1;
                    mx = Math.max(mx, f[i][j]);
                }
            }
        }
        return m + n - 2 * mx;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(string initial, string target) {
        int m = initial.size(), n = target.size();
        int f[m + 1][n + 1];
        memset(f, 0, sizeof(f));
        int mx = 0;
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (initial[i - 1] == target[j - 1]) {
                    f[i][j] = f[i - 1][j - 1] + 1;
                    mx = max(mx, f[i][j]);
                }
            }
        }
        return m + n - 2 * mx;
    }
};
```

#### Go

```go
func minOperations(initial string, target string) int {
	m, n := len(initial), len(target)
	f := make([][]int, m+1)
	for i := range f {
		f[i] = make([]int, n+1)
	}
	mx := 0
	for i, a := range initial {
		for j, b := range target {
			if a == b {
				f[i+1][j+1] = f[i][j] + 1
				mx = max(mx, f[i+1][j+1])
			}
		}
	}
	return m + n - 2*mx
}
```

#### TypeScript

```ts
function minOperations(initial: string, target: string): number {
    const m = initial.length;
    const n = target.length;
    const f: number[][] = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));
    let mx: number = 0;
    for (let i = 1; i <= m; ++i) {
        for (let j = 1; j <= n; ++j) {
            if (initial[i - 1] === target[j - 1]) {
                f[i][j] = f[i - 1][j - 1] + 1;
                mx = Math.max(mx, f[i][j]);
            }
        }
    }
    return m + n - 2 * mx;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
