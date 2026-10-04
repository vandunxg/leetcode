---
comments: true
difficulty: Medium
rating: 1448
source: Weekly Contest 337 Q2
tags:
    - Depth-First Search
    - Breadth-First Search
    - Array
    - Matrix
    - Simulation
---

<!-- problem:start -->

# [2596. Check Knight Tour Configuration](https://leetcode.com/problems/check-knight-tour-configuration)

[中文文档](/solution/2500-2599/2596.Check%20Knight%20Tour%20Configuration/README.md)

## Mô tả

<!-- description:start -->

<p>Có một quân mã trên bàn cờ <code>n x n</code>. Trong một cấu hình hợp lệ, quân mã bắt đầu <strong>tại ô góc trên bên trái</strong> của bàn cờ và đi qua mọi ô trên bàn cờ <strong>đúng một lần</strong>.</p>

<p>Bạn được cho một ma trận số nguyên <code>n x n</code> <code>grid</code> gồm các số nguyên phân biệt trong khoảng <code>[0, n * n - 1]</code>, trong đó <code>grid[row][col]</code> cho biết ô <code>(row, col)</code> là ô thứ <code>grid[row][col]<sup>th</sup></code> mà quân mã đã đi qua. Các nước đi được <strong>đánh chỉ số từ 0</strong>.</p>

<p>Trả về <code>true</code> <em>nếu</em> <code>grid</code> <em>biểu diễn một cấu hình hợp lệ cho các bước di chuyển của quân mã, hoặc</em> <code>false</code> <em>nếu ngược lại</em>.</p>

<p><strong>Lưu ý</strong> rằng một nước đi hợp lệ của quân mã gồm di chuyển hai ô theo chiều dọc và một ô theo chiều ngang, hoặc hai ô theo chiều ngang và một ô theo chiều dọc. Hình dưới đây minh họa cả tám nước đi có thể có của quân mã từ một ô bất kỳ.</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2596.Check%20Knight%20Tour%20Configuration/images/knight.png" style="width: 300px; height: 300px;" />
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2596.Check%20Knight%20Tour%20Configuration/images/yetgriddrawio-5.png" style="width: 251px; height: 251px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,11,16,5,20],[17,4,19,10,15],[12,1,8,21,6],[3,18,23,14,9],[24,13,2,7,22]]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Hình minh họa trên biểu diễn grid. Có thể chứng minh đây là một cấu hình hợp lệ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2596.Check%20Knight%20Tour%20Configuration/images/yetgriddrawio-6.png" style="width: 151px; height: 151px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,3,6],[5,8,1],[2,7,4]]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Hình minh họa trên biểu diễn grid. Nước đi 8<sup>th</sup> của quân mã không hợp lệ xét theo vị trí của nó sau nước đi 7<sup>th</sup>.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == grid.length == grid[i].length</code></li>
	<li><code>3 &lt;= n &lt;= 7</code></li>
	<li><code>0 &lt;= grid[row][col] &lt; n * n</code></li>
	<li>Tất cả các số nguyên trong <code>grid</code> đều <strong>duy nhất</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Xác định xem grid có biểu diễn đường đi của quân mã bắt đầu từ $(0,0)$ theo thứ tự $0,1,\ldots,n^2-1$ hay không. Vì $n\le 7$, ta lưu tọa độ của mỗi bước rồi kiểm tra rằng hai ô liên tiếp có hiệu tọa độ tương ứng với một bước nhảy $(1,2)$. Ô bắt đầu phải chứa giá trị $0$.

<!-- thinking:end -->

Trước tiên, ta dùng một mảng $\textit{pos}$ để lưu tọa độ của từng ô mà quân mã đi qua, sau đó duyệt mảng $\textit{pos}$ và kiểm tra xem độ chênh lệch tọa độ giữa hai ô kề nhau có phải là $(1, 2)$ hoặc $(2, 1)$ hay không. Nếu không, trả về $\textit{false}$.

Ngược lại, sau khi duyệt xong, trả về $\textit{true}$.

Độ phức tạp thời gian là $O(n^2)$, độ phức tạp không gian là $O(n^2)$. Ở đây, $n$ là độ dài cạnh của bàn cờ.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkValidGrid(self, grid: List[List[int]]) -> bool:
        if grid[0][0]:
            return False
        n = len(grid)
        pos = [None] * (n * n)
        for i in range(n):
            for j in range(n):
                pos[grid[i][j]] = (i, j)
        for (x1, y1), (x2, y2) in pairwise(pos):
            dx, dy = abs(x1 - x2), abs(y1 - y2)
            ok = (dx == 1 and dy == 2) or (dx == 2 and dy == 1)
            if not ok:
                return False
        return True
```

#### Java

```java
class Solution {
    public boolean checkValidGrid(int[][] grid) {
        if (grid[0][0] != 0) {
            return false;
        }
        int n = grid.length;
        int[][] pos = new int[n * n][2];
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                pos[grid[i][j]] = new int[] {i, j};
            }
        }
        for (int i = 1; i < n * n; ++i) {
            int[] p1 = pos[i - 1];
            int[] p2 = pos[i];
            int dx = Math.abs(p1[0] - p2[0]);
            int dy = Math.abs(p1[1] - p2[1]);
            boolean ok = (dx == 1 && dy == 2) || (dx == 2 && dy == 1);
            if (!ok) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool checkValidGrid(vector<vector<int>>& grid) {
        if (grid[0][0] != 0) {
            return false;
        }
        int n = grid.size();
        vector<pair<int, int>> pos(n * n);
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                pos[grid[i][j]] = {i, j};
            }
        }
        for (int i = 1; i < n * n; ++i) {
            auto [x1, y1] = pos[i - 1];
            auto [x2, y2] = pos[i];
            int dx = abs(x1 - x2);
            int dy = abs(y1 - y2);
            bool ok = (dx == 1 && dy == 2) || (dx == 2 && dy == 1);
            if (!ok) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func checkValidGrid(grid [][]int) bool {
	if grid[0][0] != 0 {
		return false
	}
	n := len(grid)
	type pair struct{ x, y int }
	pos := make([]pair, n*n)
	for i, row := range grid {
		for j, x := range row {
			pos[x] = pair{i, j}
		}
	}
	for i := 1; i < n*n; i++ {
		p1, p2 := pos[i-1], pos[i]
		dx := abs(p1.x - p2.x)
		dy := abs(p1.y - p2.y)
		ok := (dx == 2 && dy == 1) || (dx == 1 && dy == 2)
		if !ok {
			return false
		}
	}
	return true
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function checkValidGrid(grid: number[][]): boolean {
    if (grid[0][0] !== 0) {
        return false;
    }
    const n = grid.length;
    const pos = Array.from(new Array(n * n), () => new Array(2).fill(0));
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < n; ++j) {
            pos[grid[i][j]] = [i, j];
        }
    }
    for (let i = 1; i < n * n; ++i) {
        const p1 = pos[i - 1];
        const p2 = pos[i];
        const dx = Math.abs(p1[0] - p2[0]);
        const dy = Math.abs(p1[1] - p2[1]);
        const ok = (dx === 1 && dy === 2) || (dx === 2 && dy === 1);
        if (!ok) {
            return false;
        }
    }
    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
