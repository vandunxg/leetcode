---
comments: true
difficulty: Medium
rating: 1663
source: Weekly Contest 157 Q3
tags:
    - Array
    - Backtracking
    - Matrix
---

<!-- problem:start -->

# [1219. Path with Maximum Gold](https://leetcode.com/problems/path-with-maximum-gold)

[中文文档](/solution/1200-1299/1219.Path%20with%20Maximum%20Gold/README.md)

## Mô tả

<!-- description:start -->

<p>Trong mỏ vàng <code>grid</code> có kích thước <code>m x n</code>, mỗi ô chứa một số nguyên biểu thị lượng vàng trong ô đó; nếu ô trống thì giá trị là <code>0</code>.</p>

<p>Hãy trả về lượng vàng lớn nhất có thể thu thập theo các điều kiện sau:</p>

<ul>
	<li>Mỗi khi đi vào một ô, bạn sẽ thu thập toàn bộ vàng trong ô đó.</li>
	<li>Từ vị trí hiện tại, bạn có thể đi một bước sang trái, phải, lên hoặc xuống.</li>
	<li>Không được đi vào cùng một ô nhiều hơn một lần.</li>
	<li>Không được đi vào ô có <code>0</code> vàng.</li>
	<li>Bạn có thể bắt đầu và dừng thu thập vàng tại <strong>bất kỳ </strong>ô nào trong grid có vàng.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[0,6,0],[5,8,7],[0,9,0]]
<strong>Đầu ra:</strong> 24
<strong>Giải thích:</strong>
[[0,6,0],
 [5,8,7],
 [0,9,0]]
Đường đi thu được nhiều vàng nhất: 9 -&gt; 8 -&gt; 7.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,0,7],[2,0,6],[3,4,5],[0,3,0],[9,0,20]]
<strong>Đầu ra:</strong> 28
<strong>Giải thích:</strong>
[[1,0,7],
 [2,0,6],
 [3,4,5],
 [0,3,0],
 [9,0,20]]
Đường đi thu được nhiều vàng nhất: 1 -&gt; 2 -&gt; 3 -&gt; 4 -&gt; 5 -&gt; 6 -&gt; 7.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 15</code></li>
	<li><code>0 &lt;= grid[i][j] &lt;= 100</code></li>
	<li>Có nhiều nhất <strong>25 </strong>ô chứa vàng.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Grid có kích thước tối đa $15\times 15$ và không quá $25$ ô có vàng, nên có thể duyệt mọi đường đi đơn. Đường đi không được ghé lại ô đã thăm; ta có thể bắt đầu từ bất kỳ ô nào có vàng và cần tìm tổng lớn nhất.
>
> Khi vào một ô, DFS gán ô đó thành 0 để đánh dấu đã thăm, đệ quy sang bốn ô lân cận rồi khôi phục giá trị khi backtrack. Bắt đầu DFS từ từng ô và lấy giá trị lớn nhất toàn cục giúp dùng chính grid làm dấu đã thăm.

<!-- thinking:end -->

Ta lần lượt chọn từng ô làm điểm bắt đầu và chạy depth-first search từ đó. Trong quá trình tìm kiếm, mỗi khi gặp ô khác 0, ta đổi giá trị ô đó thành 0 rồi tiếp tục tìm. Khi không thể đi tiếp, ta tính tổng lượng vàng trên đường đi hiện tại, sau đó khôi phục ô đang xét về giá trị khác 0 để backtrack.

Độ phức tạp thời gian là $O(m \times n \times 3^k)$, trong đó $k$ là độ dài tối đa của mỗi đường đi. Vì mỗi ô chỉ được thăm nhiều nhất một lần, độ phức tạp thời gian không vượt quá $O(m \times n \times 3^k)$. Độ phức tạp không gian là $O(m \times n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getMaximumGold(self, grid: List[List[int]]) -> int:
        def dfs(i: int, j: int) -> int:
            if not (0 <= i < m and 0 <= j < n and grid[i][j]):
                return 0
            v = grid[i][j]
            grid[i][j] = 0
            ans = max(dfs(i + a, j + b) for a, b in pairwise(dirs)) + v
            grid[i][j] = v
            return ans

        m, n = len(grid), len(grid[0])
        dirs = (-1, 0, 1, 0, -1)
        return max(dfs(i, j) for i in range(m) for j in range(n))
```

#### Java

```java
class Solution {
    private final int[] dirs = {-1, 0, 1, 0, -1};
    private int[][] grid;
    private int m;
    private int n;

    public int getMaximumGold(int[][] grid) {
        m = grid.length;
        n = grid[0].length;
        this.grid = grid;
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                ans = Math.max(ans, dfs(i, j));
            }
        }
        return ans;
    }

    private int dfs(int i, int j) {
        if (i < 0 || i >= m || j < 0 || j >= n || grid[i][j] == 0) {
            return 0;
        }
        int v = grid[i][j];
        grid[i][j] = 0;
        int ans = 0;
        for (int k = 0; k < 4; ++k) {
            ans = Math.max(ans, v + dfs(i + dirs[k], j + dirs[k + 1]));
        }
        grid[i][j] = v;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int getMaximumGold(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        function<int(int, int)> dfs = [&](int i, int j) {
            if (i < 0 || i >= m || j < 0 || j >= n || !grid[i][j]) {
                return 0;
            }
            int v = grid[i][j];
            grid[i][j] = 0;
            int ans = v + max({dfs(i - 1, j), dfs(i + 1, j), dfs(i, j - 1), dfs(i, j + 1)});
            grid[i][j] = v;
            return ans;
        };
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                ans = max(ans, dfs(i, j));
            }
        }
        return ans;
    }
};
```

#### Go

```go
func getMaximumGold(grid [][]int) (ans int) {
	m, n := len(grid), len(grid[0])
	var dfs func(i, j int) int
	dfs = func(i, j int) int {
		if i < 0 || i >= m || j < 0 || j >= n || grid[i][j] == 0 {
			return 0
		}
		v := grid[i][j]
		grid[i][j] = 0
		ans := 0
		dirs := []int{-1, 0, 1, 0, -1}
		for k := 0; k < 4; k++ {
			ans = max(ans, v+dfs(i+dirs[k], j+dirs[k+1]))
		}
		grid[i][j] = v
		return ans
	}
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			ans = max(ans, dfs(i, j))
		}
	}
	return
}
```

#### TypeScript

```ts
function getMaximumGold(grid: number[][]): number {
    const m = grid.length;
    const n = grid[0].length;
    const dfs = (i: number, j: number): number => {
        if (i < 0 || i >= m || j < 0 || j >= n || !grid[i][j]) {
            return 0;
        }
        const v = grid[i][j];
        grid[i][j] = 0;
        let ans = v + Math.max(dfs(i - 1, j), dfs(i + 1, j), dfs(i, j - 1), dfs(i, j + 1));
        grid[i][j] = v;
        return ans;
    };
    let ans = 0;
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            ans = Math.max(ans, dfs(i, j));
        }
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number[][]} grid
 * @return {number}
 */
var getMaximumGold = function (grid) {
    const m = grid.length;
    const n = grid[0].length;
    const dfs = (i, j) => {
        if (i < 0 || i >= m || j < 0 || j >= n || !grid[i][j]) {
            return 0;
        }
        const v = grid[i][j];
        grid[i][j] = 0;
        let ans = v + Math.max(dfs(i - 1, j), dfs(i + 1, j), dfs(i, j - 1), dfs(i, j + 1));
        grid[i][j] = v;
        return ans;
    };
    let ans = 0;
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            ans = Math.max(ans, dfs(i, j));
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
