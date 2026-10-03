---
comments: true
difficulty: Medium
tags:
    - Breadth-First Search
    - Array
    - Matrix
---

<!-- problem:start -->

# [1730. Shortest Path to Get Food 🔒](https://leetcode.com/problems/shortest-path-to-get-food)

[中文文档](/solution/1700-1799/1730.Shortest%20Path%20to%20Get%20Food/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đang đói và muốn ăn thức ăn nhanh nhất có thể. Hãy tìm đường đi ngắn nhất để đến một ô thức ăn bất kỳ.</p>

<p>Bạn được cho một ma trận ký tự <code>m x n</code> là <code>grid</code>, gồm các loại ô sau:</p>

<ul>
	<li><code>&#39;*&#39;</code> là vị trí của bạn. Có <strong>chính xác một</strong> ô <code>&#39;*&#39;</code>.</li>
	<li><code>&#39;#&#39;</code> là ô thức ăn. Có thể có <strong>nhiều</strong> ô thức ăn.</li>
	<li><code>&#39;O&#39;</code> là ô trống, bạn có thể đi qua các ô này.</li>
	<li><code>&#39;X&#39;</code> là chướng ngại vật, bạn không thể đi qua các ô này.</li>
</ul>

<p>Nếu không có chướng ngại vật, bạn có thể đi đến ô kề phía bắc, đông, nam hoặc tây so với vị trí hiện tại.</p>

<p>Trả về <em><strong>độ dài</strong> đường đi ngắn nhất để bạn đến <strong>bất kỳ</strong> ô thức ăn nào</em>. Nếu không có đường đi đến thức ăn, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1700-1799/1730.Shortest%20Path%20to%20Get%20Food/images/img1.jpg" style="width: 300px; height: 201px;" />
<pre>
<strong>Input:</strong> grid = [[&quot;X&quot;,&quot;X&quot;,&quot;X&quot;,&quot;X&quot;,&quot;X&quot;,&quot;X&quot;],[&quot;X&quot;,&quot;*&quot;,&quot;O&quot;,&quot;O&quot;,&quot;O&quot;,&quot;X&quot;],[&quot;X&quot;,&quot;O&quot;,&quot;O&quot;,&quot;#&quot;,&quot;O&quot;,&quot;X&quot;],[&quot;X&quot;,&quot;X&quot;,&quot;X&quot;,&quot;X&quot;,&quot;X&quot;,&quot;X&quot;]]
<strong>Output:</strong> 3
<strong>Giải thích:</strong> Cần 3 bước để đến thức ăn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1700-1799/1730.Shortest%20Path%20to%20Get%20Food/images/img2.jpg" style="width: 300px; height: 241px;" />
<pre>
<strong>Input:</strong> grid = [[&quot;X&quot;,&quot;X&quot;,&quot;X&quot;,&quot;X&quot;,&quot;X&quot;],[&quot;X&quot;,&quot;*&quot;,&quot;X&quot;,&quot;O&quot;,&quot;X&quot;],[&quot;X&quot;,&quot;O&quot;,&quot;X&quot;,&quot;#&quot;,&quot;X&quot;],[&quot;X&quot;,&quot;X&quot;,&quot;X&quot;,&quot;X&quot;,&quot;X&quot;]]
<strong>Output:</strong> -1
<strong>Giải thích:</strong> Không thể đến được thức ăn.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1700-1799/1730.Shortest%20Path%20to%20Get%20Food/images/img3.jpg" style="width: 300px; height: 188px;" />
<pre>
<strong>Input:</strong> grid = [[&quot;X&quot;,&quot;X&quot;,&quot;X&quot;,&quot;X&quot;,&quot;X&quot;,&quot;X&quot;,&quot;X&quot;,&quot;X&quot;],[&quot;X&quot;,&quot;*&quot;,&quot;O&quot;,&quot;X&quot;,&quot;O&quot;,&quot;#&quot;,&quot;O&quot;,&quot;X&quot;],[&quot;X&quot;,&quot;O&quot;,&quot;O&quot;,&quot;X&quot;,&quot;O&quot;,&quot;O&quot;,&quot;X&quot;,&quot;X&quot;],[&quot;X&quot;,&quot;O&quot;,&quot;O&quot;,&quot;O&quot;,&quot;O&quot;,&quot;#&quot;,&quot;O&quot;,&quot;X&quot;],[&quot;X&quot;,&quot;X&quot;,&quot;X&quot;,&quot;X&quot;,&quot;X&quot;,&quot;X&quot;,&quot;X&quot;,&quot;X&quot;]]
<strong>Output:</strong> 6
<strong>Giải thích:</strong> Có thể có nhiều ô thức ăn. Chỉ cần 6 bước để đến ô thức ăn phía dưới.</pre>

<p><strong class="example">Ví dụ 4:</strong></p>

<pre>
<strong>Input:</strong> grid = [[&quot;X&quot;,&quot;X&quot;,&quot;X&quot;,&quot;X&quot;,&quot;X&quot;,&quot;X&quot;,&quot;X&quot;,&quot;X&quot;],[&quot;X&quot;,&quot;*&quot;,&quot;O&quot;,&quot;X&quot;,&quot;O&quot;,&quot;#&quot;,&quot;O&quot;,&quot;X&quot;],[&quot;X&quot;,&quot;O&quot;,&quot;O&quot;,&quot;X&quot;,&quot;O&quot;,&quot;O&quot;,&quot;X&quot;,&quot;X&quot;],[&quot;X&quot;,&quot;O&quot;,&quot;O&quot;,&quot;O&quot;,&quot;O&quot;,&quot;#&quot;,&quot;O&quot;,&quot;X&quot;],[&quot;O&quot;,&quot;O&quot;,&quot;O&quot;,&quot;O&quot;,&quot;O&quot;,&quot;O&quot;,&quot;O&quot;,&quot;O&quot;]]
<strong>Output:</strong> 6</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 200</code></li>
	<li><code>grid[row][col]</code> là <code>&#39;*&#39;</code>, <code>&#39;X&#39;</code>, <code>&#39;O&#39;</code> hoặc <code>&#39;#&#39;</code>.</li>
	<li><code>grid</code> chứa <strong>chính xác một</strong> <code>&#39;*&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS (Breadth-First Search)

<!-- thinking:start -->

> **Tư duy**
>
> Từ ô $*$ duy nhất, ta cần tìm ô $\#$ gần nhất và tránh chướng ngại vật. Đường đi ngắn nhất trên đồ thị không trọng số chính là BFS.
>
> Đưa $*$ vào queue và mở rộng bốn ô kề theo từng tầng: trả về độ sâu khi gặp $\#$, đồng thời đánh dấu ô trống là đã bị chặn ngay sau khi đưa vào queue để tránh thăm lại. Queue rỗng nghĩa là không thể đến được thức ăn.

<!-- thinking:end -->

Theo đề bài, ta cần bắt đầu từ `*`, tìm `#` gần nhất và trả về độ dài đường đi ngắn nhất.

Trước hết, ta duyệt toàn bộ mảng hai chiều để tìm vị trí của `*`, dùng vị trí đó làm điểm bắt đầu BFS và đưa vào queue.

Sau đó, ta bắt đầu BFS và duyệt các phần tử trong queue. Mỗi khi duyệt một phần tử, ta đưa các ô theo bốn hướng (lên, xuống, trái và phải) vào queue cho đến khi gặp `#`, rồi trả về số tầng hiện tại.

Độ phức tạp thời gian là $O(m \times n)$, còn độ phức tạp không gian là $O(1)$. Ở đây, $m$ và $n$ lần lượt là số hàng và số cột của mảng hai chiều.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getFood(self, grid: List[List[str]]) -> int:
        m, n = len(grid), len(grid[0])
        i, j = next((i, j) for i in range(m) for j in range(n) if grid[i][j] == '*')
        q = deque([(i, j)])
        dirs = (-1, 0, 1, 0, -1)
        ans = 0
        while q:
            ans += 1
            for _ in range(len(q)):
                i, j = q.popleft()
                for a, b in pairwise(dirs):
                    x, y = i + a, j + b
                    if 0 <= x < m and 0 <= y < n:
                        if grid[x][y] == '#':
                            return ans
                        if grid[x][y] == 'O':
                            grid[x][y] = 'X'
                            q.append((x, y))
        return -1
```

#### Java

```java
class Solution {
    private int[] dirs = {-1, 0, 1, 0, -1};

    public int getFood(char[][] grid) {
        int m = grid.length, n = grid[0].length;
        Deque<int[]> q = new ArrayDeque<>();
        for (int i = 0, x = 1; i < m && x == 1; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == '*') {
                    q.offer(new int[] {i, j});
                    x = 0;
                    break;
                }
            }
        }
        int ans = 0;
        while (!q.isEmpty()) {
            ++ans;
            for (int t = q.size(); t > 0; --t) {
                var p = q.poll();
                for (int k = 0; k < 4; ++k) {
                    int x = p[0] + dirs[k];
                    int y = p[1] + dirs[k + 1];
                    if (x >= 0 && x < m && y >= 0 && y < n) {
                        if (grid[x][y] == '#') {
                            return ans;
                        }
                        if (grid[x][y] == 'O') {
                            grid[x][y] = 'X';
                            q.offer(new int[] {x, y});
                        }
                    }
                }
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    const static inline vector<int> dirs = {-1, 0, 1, 0, -1};

    int getFood(vector<vector<char>>& grid) {
        int m = grid.size(), n = grid[0].size();
        queue<pair<int, int>> q;
        for (int i = 0, x = 1; i < m && x == 1; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == '*') {
                    q.emplace(i, j);
                    x = 0;
                    break;
                }
            }
        }
        int ans = 0;
        while (!q.empty()) {
            ++ans;
            for (int t = q.size(); t; --t) {
                auto [i, j] = q.front();
                q.pop();
                for (int k = 0; k < 4; ++k) {
                    int x = i + dirs[k], y = j + dirs[k + 1];
                    if (x >= 0 && x < m && y >= 0 && y < n) {
                        if (grid[x][y] == '#') return ans;
                        if (grid[x][y] == 'O') {
                            grid[x][y] = 'X';
                            q.emplace(x, y);
                        }
                    }
                }
            }
        }
        return -1;
    }
};
```

#### Go

```go
func getFood(grid [][]byte) (ans int) {
	m, n := len(grid), len(grid[0])
	dirs := []int{-1, 0, 1, 0, -1}
	type pair struct{ i, j int }
	q := []pair{}
	for i, x := 0, 1; i < m && x == 1; i++ {
		for j := 0; j < n; j++ {
			if grid[i][j] == '*' {
				q = append(q, pair{i, j})
				x = 0
				break
			}
		}
	}
	for len(q) > 0 {
		ans++
		for t := len(q); t > 0; t-- {
			p := q[0]
			q = q[1:]
			for k := 0; k < 4; k++ {
				x, y := p.i+dirs[k], p.j+dirs[k+1]
				if x >= 0 && x < m && y >= 0 && y < n {
					if grid[x][y] == '#' {
						return ans
					}
					if grid[x][y] == 'O' {
						grid[x][y] = 'X'
						q = append(q, pair{x, y})
					}
				}
			}
		}
	}
	return -1
}
```

#### JavaScript

```js
/**
 * @param {character[][]} grid
 * @return {number}
 */
var getFood = function (grid) {
    const m = grid.length;
    const n = grid[0].length;
    const dirs = [-1, 0, 1, 0, -1];
    const q = [];
    for (let i = 0, x = 1; i < m && x == 1; ++i) {
        for (let j = 0; j < n; ++j) {
            if (grid[i][j] == '*') {
                q.push([i, j]);
                x = 0;
                break;
            }
        }
    }
    let ans = 0;
    while (q.length) {
        ++ans;
        for (let t = q.length; t > 0; --t) {
            const [i, j] = q.shift();
            for (let k = 0; k < 4; ++k) {
                const x = i + dirs[k];
                const y = j + dirs[k + 1];
                if (x >= 0 && x < m && y >= 0 && y < n) {
                    if (grid[x][y] == '#') {
                        return ans;
                    }
                    if (grid[x][y] == 'O') {
                        grid[x][y] = 'X';
                        q.push([x, y]);
                    }
                }
            }
        }
    }
    return -1;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
