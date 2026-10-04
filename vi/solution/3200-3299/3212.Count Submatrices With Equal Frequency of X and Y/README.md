---
comments: true
difficulty: Medium
rating: 1672
source: Weekly Contest 405 Q3
tags:
    - Array
    - Matrix
    - Prefix Sum
---

<!-- problem:start -->

# [3212. Count Submatrices With Equal Frequency of X and Y](https://leetcode.com/problems/count-submatrices-with-equal-frequency-of-x-and-y)

[中文文档](/solution/3200-3299/3212.Count%20Submatrices%20With%20Equal%20Frequency%20of%20X%20and%20Y/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận ký tự 2D <code>grid</code>, trong đó <code>grid[i][j]</code> là một trong các ký tự <code>&#39;X&#39;</code>, <code>&#39;Y&#39;</code> hoặc <code>&#39;.&#39;</code>, hãy trả về số lượng <span data-keyword="submatrix">ma trận con</span> thỏa mãn:</p>

<ul>
	<li>chứa <code>grid[0][0]</code></li>
	<li>có tần suất <strong>bằng nhau</strong> của <code>&#39;X&#39;</code> và <code>&#39;Y&#39;</code>.</li>
	<li>có <strong>ít nhất</strong> một ký tự <code>&#39;X&#39;</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[&quot;X&quot;,&quot;Y&quot;,&quot;.&quot;],[&quot;Y&quot;,&quot;.&quot;,&quot;.&quot;]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3212.Count%20Submatrices%20With%20Equal%20Frequency%20of%20X%20and%20Y/images/examplems.png" style="padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; width: 175px; height: 350px;" /></strong></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[&quot;X&quot;,&quot;X&quot;],[&quot;X&quot;,&quot;Y&quot;]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có ma trận con nào có tần suất <code>&#39;X&#39;</code> và <code>&#39;Y&#39;</code> bằng nhau.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[&quot;.&quot;,&quot;.&quot;],[&quot;.&quot;,&quot;.&quot;]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có ma trận con nào chứa ít nhất một ký tự <code>&#39;X&#39;</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= grid.length, grid[i].length &lt;= 1000</code></li>
	<li><code>grid[i][j]</code> là một trong các ký tự <code>&#39;X&#39;</code>, <code>&#39;Y&#39;</code> hoặc <code>&#39;.&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố 2D

<!-- thinking:start -->

> **Tư duy**
>
> Mọi ma trận con được đếm đều phải chứa ô góc trên bên trái $(0,0)$, nên chỉ có $O(mn)$ ứng viên, với $m,n\le 10^3$. Nếu quét lại từng ma trận con thì độ phức tạp là bậc ba và quá chậm.
>
> Tổng tiền tố 2D cho phép lấy số lượng `X`/`Y` trong $[0,0]$– $(i,j)$ trong $O(1)$. Tại mỗi ô, cập nhật bằng nguyên lý bù trừ và đếm khi số lượng `X` dương và bằng số lượng `Y`. Chỉ cần một lượt duyệt $O(mn)$.

<!-- thinking:end -->

Theo mô tả bài toán, chỉ cần tính các tổng tiền tố $s[i][j][0]$ và $s[i][j][1]$ tại mỗi vị trí $(i, j)$, lần lượt biểu diễn số ký tự `X` và `Y` trong ma trận con từ $(0, 0)$ đến $(i, j)$. Nếu $s[i][j][0] > 0$ và $s[i][j][0] = s[i][j][1]$, điều kiện được thỏa mãn và ta tăng đáp án lên một.

Sau khi duyệt qua tất cả các vị trí, trả về đáp án.

Độ phức tạp thời gian là $O(m \times n)$, và độ phức tạp không gian là $O(m \times n)$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfSubmatrices(self, grid: List[List[str]]) -> int:
        m, n = len(grid), len(grid[0])
        s = [[[0] * 2 for _ in range(n + 1)] for _ in range(m + 1)]
        ans = 0
        for i, row in enumerate(grid, 1):
            for j, x in enumerate(row, 1):
                s[i][j][0] = s[i - 1][j][0] + s[i][j - 1][0] - s[i - 1][j - 1][0]
                s[i][j][1] = s[i - 1][j][1] + s[i][j - 1][1] - s[i - 1][j - 1][1]
                if x != ".":
                    s[i][j][ord(x) & 1] += 1
                if s[i][j][0] > 0 and s[i][j][0] == s[i][j][1]:
                    ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int numberOfSubmatrices(char[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[][][] s = new int[m + 1][n + 1][2];
        int ans = 0;
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                s[i][j][0] = s[i - 1][j][0] + s[i][j - 1][0] - s[i - 1][j - 1][0]
                    + (grid[i - 1][j - 1] == 'X' ? 1 : 0);
                s[i][j][1] = s[i - 1][j][1] + s[i][j - 1][1] - s[i - 1][j - 1][1]
                    + (grid[i - 1][j - 1] == 'Y' ? 1 : 0);
                if (s[i][j][0] > 0 && s[i][j][0] == s[i][j][1]) {
                    ++ans;
                }
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfSubmatrices(vector<vector<char>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<vector<vector<int>>> s(m + 1, vector<vector<int>>(n + 1, vector<int>(2)));
        int ans = 0;
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                s[i][j][0] = s[i - 1][j][0] + s[i][j - 1][0] - s[i - 1][j - 1][0]
                    + (grid[i - 1][j - 1] == 'X' ? 1 : 0);
                s[i][j][1] = s[i - 1][j][1] + s[i][j - 1][1] - s[i - 1][j - 1][1]
                    + (grid[i - 1][j - 1] == 'Y' ? 1 : 0);
                if (s[i][j][0] > 0 && s[i][j][0] == s[i][j][1]) {
                    ++ans;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfSubmatrices(grid [][]byte) (ans int) {
	m, n := len(grid), len(grid[0])
	s := make([][][]int, m+1)
	for i := range s {
		s[i] = make([][]int, n+1)
		for j := range s[i] {
			s[i][j] = make([]int, 2)
		}
	}

	for i := 1; i <= m; i++ {
		for j := 1; j <= n; j++ {
			s[i][j][0] = s[i-1][j][0] + s[i][j-1][0] - s[i-1][j-1][0]
			if grid[i-1][j-1] == 'X' {
				s[i][j][0]++
			}
			s[i][j][1] = s[i-1][j][1] + s[i][j-1][1] - s[i-1][j-1][1]
			if grid[i-1][j-1] == 'Y' {
				s[i][j][1]++
			}
			if s[i][j][0] > 0 && s[i][j][0] == s[i][j][1] {
				ans++
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function numberOfSubmatrices(grid: string[][]): number {
    const [m, n] = [grid.length, grid[0].length];
    const s = Array.from({ length: m + 1 }, () => Array.from({ length: n + 1 }, () => [0, 0]));
    let ans = 0;

    for (let i = 1; i <= m; ++i) {
        for (let j = 1; j <= n; ++j) {
            s[i][j][0] =
                s[i - 1][j][0] +
                s[i][j - 1][0] -
                s[i - 1][j - 1][0] +
                (grid[i - 1][j - 1] === 'X' ? 1 : 0);
            s[i][j][1] =
                s[i - 1][j][1] +
                s[i][j - 1][1] -
                s[i - 1][j - 1][1] +
                (grid[i - 1][j - 1] === 'Y' ? 1 : 0);
            if (s[i][j][0] > 0 && s[i][j][0] === s[i][j][1]) {
                ++ans;
            }
        }
    }

    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn number_of_submatrices(grid: Vec<Vec<char>>) -> i32 {
        let m = grid.len();
        let n = grid[0].len();
        let mut s = vec![vec![vec![0i32; 2]; n + 1]; m + 1];
        let mut ans = 0;

        for i in 1..=m {
            for j in 1..=n {
                s[i][j][0] = s[i - 1][j][0]
                    + s[i][j - 1][0]
                    - s[i - 1][j - 1][0]
                    + if grid[i - 1][j - 1] == 'X' { 1 } else { 0 };

                s[i][j][1] = s[i - 1][j][1]
                    + s[i][j - 1][1]
                    - s[i - 1][j - 1][1]
                    + if grid[i - 1][j - 1] == 'Y' { 1 } else { 0 };

                if s[i][j][0] > 0 && s[i][j][0] == s[i][j][1] {
                    ans += 1;
                }
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
