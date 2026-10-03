---
comments: true
difficulty: Hard
rating: 2084
source: Weekly Contest 292 Q4
tags:
    - Array
    - Dynamic Programming
    - Matrix
    - Parentheses
---

<!-- problem:start -->

# [2267. Check if There Is a Valid Parentheses String Path](https://leetcode.com/problems/check-if-there-is-a-valid-parentheses-string-path)

[中文文档](/solution/2200-2299/2267.Check%20if%20There%20Is%20a%20Valid%20Parentheses%20String%20Path/README.md)

## Mô tả

<!-- description:start -->

<p>Chuỗi ngoặc là một chuỗi <strong>không rỗng</strong> chỉ gồm <code>&#39;(&#39;</code> và <code>&#39;)&#39;</code>. Chuỗi này <strong>hợp lệ</strong> nếu <strong>ít nhất một</strong> trong các điều kiện sau là <strong>đúng</strong>:</p>

<ul>
	<li>Chuỗi là <code>()</code>.</li>
	<li>Chuỗi có thể được viết dưới dạng <code>AB</code> (<code>A</code> nối với <code>B</code>), trong đó <code>A</code> và <code>B</code> là các chuỗi ngoặc hợp lệ.</li>
	<li>Chuỗi có thể được viết dưới dạng <code>(A)</code>, trong đó <code>A</code> là một chuỗi ngoặc hợp lệ.</li>
</ul>

<p>Cho một ma trận ngoặc <code>m x n</code> <code>grid</code>. <strong>Đường đi tạo thành chuỗi ngoặc hợp lệ</strong> trong ma trận là một đường đi thỏa mãn <strong>tất cả</strong> các điều kiện sau:</p>

<ul>
	<li>Đường đi bắt đầu từ ô góc trên bên trái <code>(0, 0)</code>.</li>
	<li>Đường đi kết thúc tại ô góc dưới bên phải <code>(m - 1, n - 1)</code>.</li>
	<li>Đường đi chỉ được di chuyển <strong>xuống dưới</strong> hoặc <strong>sang phải</strong>.</li>
	<li>Chuỗi ngoặc tạo thành từ đường đi là <strong>hợp lệ</strong>.</li>
</ul>

<p>Trả về <code>true</code> <em>nếu tồn tại <strong>đường đi tạo thành chuỗi ngoặc hợp lệ</strong> trong ma trận.</em> Nếu không, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2267.Check%20if%20There%20Is%20a%20Valid%20Parentheses%20String%20Path/images/example1drawio.png" style="width: 521px; height: 300px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[&quot;(&quot;,&quot;(&quot;,&quot;(&quot;],[&quot;)&quot;,&quot;(&quot;,&quot;)&quot;],[&quot;(&quot;,&quot;(&quot;,&quot;)&quot;],[&quot;(&quot;,&quot;(&quot;,&quot;)&quot;]]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Hình trên minh họa hai đường đi có thể tạo thành chuỗi ngoặc hợp lệ.
Đường đi đầu tiên tạo thành chuỗi ngoặc hợp lệ &quot;()(())&quot;.
Đường đi thứ hai tạo thành chuỗi ngoặc hợp lệ &quot;((()))&quot;.
Lưu ý rằng có thể còn những đường đi tạo thành chuỗi ngoặc hợp lệ khác.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2267.Check%20if%20There%20Is%20a%20Valid%20Parentheses%20String%20Path/images/example2drawio.png" style="width: 165px; height: 165px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[&quot;)&quot;,&quot;)&quot;],[&quot;(&quot;,&quot;(&quot;]]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Hai đường đi có thể tạo thành các chuỗi ngoặc &quot;))(&quot; và &quot;)((&quot;. Vì cả hai đều không phải là chuỗi ngoặc hợp lệ, ta trả về false.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 100</code></li>
	<li><code>grid[i][j]</code> là <code>&#39;(&#39;</code> hoặc <code>&#39;)&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS + Cắt tỉa

<!-- thinking:start -->

> **Tư duy**
>
> Ta chỉ được di chuyển sang phải hoặc xuống dưới, và đường đi phải tạo thành một chuỗi ngoặc hợp lệ. Độ dài đường đi là $m+n-1$. Đường đi có độ dài lẻ, bắt đầu bằng $')'$, hoặc kết thúc bằng $'('$ đều là bất khả thi. Duyệt toàn bộ sẽ có số trường hợp tăng theo cấp số mũ, nhưng balance $k$ của một prefix hợp lệ không thể lớn hơn số ô còn lại.
>
> DFS có ghi nhớ $\textit{dfs}(i,j,k)$ cập nhật $k$ tại ô hiện tại, cắt tỉa khi $k<0$ hoặc $k$ vượt quá số bước còn lại, đồng thời yêu cầu $k=0$ tại ô cuối.

<!-- thinking:end -->

Gọi $m$ là số hàng và $n$ là số cột của ma trận.

Nếu $m + n - 1$ là số lẻ, hoặc ngoặc tại góc trên bên trái và góc dưới bên phải không phù hợp, thì không tồn tại đường đi hợp lệ, và ta trả về $\text{false}$ ngay.

Nếu không, ta xây dựng hàm $\textit{dfs}(i, j, k)$, biểu diễn việc có tồn tại đường đi hợp lệ bắt đầu từ $(i, j)$ với balance ngoặc hiện tại bằng $k$ hay không. Balance $k$ được định nghĩa là số ngoặc trái trừ đi số ngoặc phải trong đường đi từ $(0, 0)$ đến $(i, j)$.

Nếu balance $k$ nhỏ hơn $0$ hoặc lớn hơn $m + n - i - j$, thì không tồn tại đường đi hợp lệ, và ta trả về $\text{false}$ ngay. Nếu $(i, j)$ là ô góc dưới bên phải, đường đi chỉ hợp lệ khi $k = 0$. Nếu không, ta liệt kê ô tiếp theo $(x, y)$ của $(i, j)$. Nếu $(x, y)$ là một ô hợp lệ và $\textit{dfs}(x, y, k)$ là $\text{true}$, thì tồn tại một đường đi hợp lệ.

Độ phức tạp thời gian là $O(m \times n \times (m + n))$, và độ phức tạp không gian là $O(m \times n \times (m + n))$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def hasValidPath(self, grid: List[List[str]]) -> bool:
        @cache
        def dfs(i: int, j: int, k: int) -> bool:
            d = 1 if grid[i][j] == "(" else -1
            k += d
            if k < 0 or k > m - i + n - j:
                return False
            if i == m - 1 and j == n - 1:
                return k == 0
            for a, b in pairwise((0, 1, 0)):
                x, y = i + a, j + b
                if 0 <= x < m and 0 <= y < n and dfs(x, y, k):
                    return True
            return False

        m, n = len(grid), len(grid[0])
        if (m + n - 1) % 2 or grid[0][0] == ")" or grid[m - 1][n - 1] == "(":
            return False
        return dfs(0, 0, 0)
```

#### Java

```java
class Solution {
    private int m, n;
    private char[][] grid;
    private boolean[][][] vis;

    public boolean hasValidPath(char[][] grid) {
        m = grid.length;
        n = grid[0].length;
        if ((m + n - 1) % 2 == 1 || grid[0][0] == ')' || grid[m - 1][n - 1] == '(') {
            return false;
        }
        this.grid = grid;
        vis = new boolean[m][n][m + n];
        return dfs(0, 0, 0);
    }

    private boolean dfs(int i, int j, int k) {
        if (vis[i][j][k]) {
            return false;
        }
        vis[i][j][k] = true;
        k += grid[i][j] == '(' ? 1 : -1;
        if (k < 0 || k > m - i + n - j) {
            return false;
        }
        if (i == m - 1 && j == n - 1) {
            return k == 0;
        }
        final int[] dirs = {1, 0, 1};
        for (int d = 0; d < 2; ++d) {
            int x = i + dirs[d], y = j + dirs[d + 1];
            if (x >= 0 && x < m && y >= 0 && y < n && dfs(x, y, k)) {
                return true;
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool hasValidPath(vector<vector<char>>& grid) {
        int m = grid.size(), n = grid[0].size();
        if ((m + n - 1) % 2 || grid[0][0] == ')' || grid[m - 1][n - 1] == '(') {
            return false;
        }
        bool vis[m][n][m + n];
        memset(vis, false, sizeof(vis));
        int dirs[3] = {1, 0, 1};
        auto dfs = [&](this auto&& dfs, int i, int j, int k) -> bool {
            if (vis[i][j][k]) {
                return false;
            }
            vis[i][j][k] = true;
            k += grid[i][j] == '(' ? 1 : -1;
            if (k < 0 || k > m - i + n - j) {
                return false;
            }
            if (i == m - 1 && j == n - 1) {
                return k == 0;
            }
            for (int d = 0; d < 2; ++d) {
                int x = i + dirs[d], y = j + dirs[d + 1];
                if (x >= 0 && x < m && y >= 0 && y < n && dfs(x, y, k)) {
                    return true;
                }
            }
            return false;
        };
        return dfs(0, 0, 0);
    }
};
```

#### Go

```go
func hasValidPath(grid [][]byte) bool {
	m, n := len(grid), len(grid[0])
	if (m+n-1)%2 == 1 || grid[0][0] == ')' || grid[m-1][n-1] == '(' {
		return false
	}
	vis := make([][][]bool, m)
	for i := range vis {
		vis[i] = make([][]bool, n)
		for j := range vis[i] {
			vis[i][j] = make([]bool, m+n)
		}
	}
	dirs := [3]int{1, 0, 1}
	var dfs func(i, j, k int) bool
	dfs = func(i, j, k int) bool {
		if vis[i][j][k] {
			return false
		}
		vis[i][j][k] = true
		if grid[i][j] == '(' {
			k++
		} else {
			k--
		}
		if k < 0 || k > m-i+n-j {
			return false
		}
		if i == m-1 && j == n-1 {
			return k == 0
		}
		for d := 0; d < 2; d++ {
			x, y := i+dirs[d], j+dirs[d+1]
			if x >= 0 && x < m && y >= 0 && y < n && dfs(x, y, k) {
				return true
			}
		}
		return false
	}
	return dfs(0, 0, 0)
}
```

#### TypeScript

```ts
function hasValidPath(grid: string[][]): boolean {
    const m = grid.length,
        n = grid[0].length;

    if ((m + n - 1) % 2 || grid[0][0] === ')' || grid[m - 1][n - 1] === '(') {
        return false;
    }

    const vis: boolean[][][] = Array.from({ length: m }, () =>
        Array.from({ length: n }, () => Array(m + n).fill(false)),
    );
    const dirs = [1, 0, 1];

    const dfs = (i: number, j: number, k: number): boolean => {
        if (vis[i][j][k]) {
            return false;
        }

        vis[i][j][k] = true;
        k += grid[i][j] === '(' ? 1 : -1;

        if (k < 0 || k > m - i + n - j) {
            return false;
        }

        if (i === m - 1 && j === n - 1) {
            return k === 0;
        }

        for (let d = 0; d < 2; ++d) {
            const x = i + dirs[d],
                y = j + dirs[d + 1];
            if (x >= 0 && x < m && y >= 0 && y < n && dfs(x, y, k)) {
                return true;
            }
        }

        return false;
    };

    return dfs(0, 0, 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
