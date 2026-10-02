---
comments: true
difficulty: Medium
rating: 1666
source: Weekly Contest 150 Q3
tags:
    - Breadth-First Search
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [1162. As Far from Land as Possible](https://leetcode.com/problems/as-far-from-land-as-possible)

[中文文档](/solution/1100-1199/1162.As%20Far%20from%20Land%20as%20Possible/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một <code>grid</code> kích thước <code>n x n</code>, chỉ chứa các giá trị <code>0</code> và <code>1</code>, trong đó <code>0</code> biểu thị nước và <code>1</code> biểu thị đất liền. Hãy tìm ô nước có khoảng cách đến ô đất liền gần nhất lớn nhất và trả về khoảng cách đó. Nếu lưới không có đất liền hoặc không có nước, trả về <code>-1</code>.</p>

<p>Khoảng cách dùng trong bài toán này là khoảng cách Manhattan: khoảng cách giữa hai ô <code>(x0, y0)</code> và <code>(x1, y1)</code> bằng <code>|x0 - x1| + |y0 - y1|</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1100-1199/1162.As%20Far%20from%20Land%20as%20Possible/images/1336_ex1.jpg" style="width: 185px; height: 87px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,0,1],[0,0,0],[1,0,1]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ô (1, 1) cách xa tất cả ô đất liền nhất, với khoảng cách bằng 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1100-1199/1162.As%20Far%20from%20Land%20as%20Possible/images/1336_ex2.jpg" style="width: 184px; height: 87px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,0,0],[0,0,0],[0,0,0]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Ô (2, 2) cách xa tất cả ô đất liền nhất, với khoảng cách bằng 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= n&nbsp;&lt;= 100</code></li>
	<li><code>grid[i][j]</code>&nbsp;là <code>0</code> hoặc <code>1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm giá trị lớn nhất trong các khoảng cách từ ô nước đến ô đất liền gần nhất. Chạy BFS từ từng ô nước sẽ lặp lại nhiều lần. BFS đa nguồn bắt đầu từ tất cả ô đất liền và mở rộng thêm một lớp mỗi bước; ô nước được đánh dấu cuối cùng cho biết khoảng cách lớn nhất. Nếu lưới toàn đất hoặc toàn nước thì không có đáp án, nên trả về $-1$.

<!-- thinking:end -->

Ta đưa tất cả ô đất liền vào queue $q$. Nếu queue rỗng hoặc số phần tử trong queue bằng tổng số ô của lưới, nghĩa là lưới chỉ có đất liền hoặc chỉ có nước, khi đó trả về $-1$.

Nếu không, ta bắt đầu BFS từ các ô đất liền. Đặt số bước ban đầu $ans=-1$.

Trong mỗi lượt tìm kiếm, ta mở rộng từ tất cả ô trong queue theo bốn hướng. Nếu ô kế tiếp là ô nước, ta đánh dấu nó thành ô đất liền rồi thêm vào queue. Sau mỗi lượt mở rộng, tăng số bước thêm $1$. Lặp lại cho đến khi queue rỗng.

Cuối cùng, trả về số bước $ans$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là độ dài cạnh của lưới.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxDistance(self, grid: List[List[int]]) -> int:
        n = len(grid)
        q = deque((i, j) for i in range(n) for j in range(n) if grid[i][j])
        ans = -1
        if len(q) in (0, n * n):
            return ans
        dirs = (-1, 0, 1, 0, -1)
        while q:
            for _ in range(len(q)):
                i, j = q.popleft()
                for a, b in pairwise(dirs):
                    x, y = i + a, j + b
                    if 0 <= x < n and 0 <= y < n and grid[x][y] == 0:
                        grid[x][y] = 1
                        q.append((x, y))
            ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int maxDistance(int[][] grid) {
        int n = grid.length;
        Deque<int[]> q = new ArrayDeque<>();
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 1) {
                    q.offer(new int[] {i, j});
                }
            }
        }
        int ans = -1;
        if (q.isEmpty() || q.size() == n * n) {
            return ans;
        }
        int[] dirs = {-1, 0, 1, 0, -1};
        while (!q.isEmpty()) {
            for (int i = q.size(); i > 0; --i) {
                int[] p = q.poll();
                for (int k = 0; k < 4; ++k) {
                    int x = p[0] + dirs[k], y = p[1] + dirs[k + 1];
                    if (x >= 0 && x < n && y >= 0 && y < n && grid[x][y] == 0) {
                        grid[x][y] = 1;
                        q.offer(new int[] {x, y});
                    }
                }
            }
            ++ans;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxDistance(vector<vector<int>>& grid) {
        int n = grid.size();
        queue<pair<int, int>> q;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j]) {
                    q.emplace(i, j);
                }
            }
        }
        int ans = -1;
        if (q.empty() || q.size() == n * n) {
            return ans;
        }
        int dirs[5] = {-1, 0, 1, 0, -1};
        while (!q.empty()) {
            for (int m = q.size(); m; --m) {
                auto [i, j] = q.front();
                q.pop();
                for (int k = 0; k < 4; ++k) {
                    int x = i + dirs[k], y = j + dirs[k + 1];
                    if (x >= 0 && x < n && y >= 0 && y < n && !grid[x][y]) {
                        grid[x][y] = 1;
                        q.emplace(x, y);
                    }
                }
            }
            ++ans;
        }
        return ans;
    }
};
```

#### Go

```go
func maxDistance(grid [][]int) int {
	n := len(grid)
	q := [][2]int{}
	for i, row := range grid {
		for j, v := range row {
			if v == 1 {
				q = append(q, [2]int{i, j})
			}
		}
	}
	ans := -1
	if len(q) == 0 || len(q) == n*n {
		return ans
	}
	dirs := [5]int{-1, 0, 1, 0, -1}
	for len(q) > 0 {
		for i := len(q); i > 0; i-- {
			p := q[0]
			q = q[1:]
			for k := 0; k < 4; k++ {
				x, y := p[0]+dirs[k], p[1]+dirs[k+1]
				if x >= 0 && x < n && y >= 0 && y < n && grid[x][y] == 0 {
					grid[x][y] = 1
					q = append(q, [2]int{x, y})
				}
			}
		}
		ans++
	}
	return ans
}
```

#### TypeScript

```ts
function maxDistance(grid: number[][]): number {
    const n = grid.length;
    const q: [number, number][] = [];
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < n; ++j) {
            if (grid[i][j] === 1) {
                q.push([i, j]);
            }
        }
    }
    let ans = -1;
    if (q.length === 0 || q.length === n * n) {
        return ans;
    }
    const dirs: number[] = [-1, 0, 1, 0, -1];
    while (q.length > 0) {
        for (let m = q.length; m; --m) {
            const [i, j] = q.shift()!;
            for (let k = 0; k < 4; ++k) {
                const x = i + dirs[k];
                const y = j + dirs[k + 1];
                if (x >= 0 && x < n && y >= 0 && y < n && grid[x][y] === 0) {
                    grid[x][y] = 1;
                    q.push([x, y]);
                }
            }
        }
        ++ans;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
