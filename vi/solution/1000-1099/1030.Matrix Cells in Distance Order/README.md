---
comments: true
difficulty: Easy
rating: 1585
source: Weekly Contest 133 Q2
tags:
    - Geometry
    - Array
    - Math
    - Matrix
    - Sorting
---

<!-- problem:start -->

# [1030. Matrix Cells in Distance Order](https://leetcode.com/problems/matrix-cells-in-distance-order)

[中文文档](/solution/1000-1099/1030.Matrix%20Cells%20in%20Distance%20Order/README.md)

## Mô tả

<!-- description:start -->

<p>Cho bốn số nguyên <code>row</code>, <code>cols</code>, <code>rCenter</code> và <code>cCenter</code>. Có một ma trận kích thước <code>rows x cols</code>, và bạn đang ở ô có tọa độ <code>(rCenter, cCenter)</code>.</p>

<p>Hãy trả về <em>tọa độ của tất cả các ô trong ma trận, được sắp xếp theo <strong>khoảng cách</strong> đến </em><code>(rCenter, cCenter)</code><em> từ nhỏ đến lớn</em>. Bạn có thể trả về kết quả theo <strong>bất kỳ thứ tự nào</strong> thỏa mãn điều kiện này.</p>

<p><strong>Khoảng cách</strong> giữa hai ô <code>(r<sub>1</sub>, c<sub>1</sub>)</code> và <code>(r<sub>2</sub>, c<sub>2</sub>)</code> là <code>|r<sub>1</sub> - r<sub>2</sub>| + |c<sub>1</sub> - c<sub>2</sub>|</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> rows = 1, cols = 2, rCenter = 0, cCenter = 0
<strong>Đầu ra:</strong> [[0,0],[0,1]]
<strong>Giải thích:</strong> Khoảng cách từ (0, 0) đến các ô khác là: [0,1]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> rows = 2, cols = 2, rCenter = 0, cCenter = 1
<strong>Đầu ra:</strong> [[0,1],[0,0],[1,1],[1,0]]
<strong>Giải thích:</strong> Khoảng cách từ (0, 1) đến các ô khác là: [0,1,1,2]
Đáp án [[0,1],[1,1],[0,0],[1,0]] cũng được chấp nhận.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> rows = 2, cols = 3, rCenter = 1, cCenter = 2
<strong>Đầu ra:</strong> [[1,2],[0,2],[1,1],[0,1],[1,0],[0,0]]
<strong>Giải thích:</strong> Khoảng cách từ (1, 2) đến các ô khác là: [0,1,1,2,2,3]
Cũng có những đáp án khác được chấp nhận, chẳng hạn [[1,2],[1,1],[0,2],[1,0],[0,1],[0,0]].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= rows, cols &lt;= 100</code></li>
	<li><code>0 &lt;= rCenter &lt; rows</code></li>
	<li><code>0 &lt;= cCenter &lt; cols</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể sắp xếp tất cả $rows\times cols$ ô theo khoảng cách Manhattan vì $r,c\le 100$. Các ô cách nhau $d$ tạo thành hình thoi, nên duyệt lần lượt từng lớp sẽ cho đúng thứ tự.
>
> BFS bắt đầu từ $(rCenter,cCenter)$ chỉ thăm các ô cách $d+1$ sau khi đã thăm các ô cách $d$, đúng với thứ tự cần tìm.
>
> Queue và ma trận đánh dấu đã thăm giúp tránh trùng lặp; thứ tự dequeue chính là đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def allCellsDistOrder(
        self, rows: int, cols: int, rCenter: int, cCenter: int
    ) -> List[List[int]]:
        q = deque([[rCenter, cCenter]])
        vis = [[False] * cols for _ in range(rows)]
        vis[rCenter][cCenter] = True
        ans = []
        while q:
            for _ in range(len(q)):
                p = q.popleft()
                ans.append(p)
                for a, b in pairwise((-1, 0, 1, 0, -1)):
                    x, y = p[0] + a, p[1] + b
                    if 0 <= x < rows and 0 <= y < cols and not vis[x][y]:
                        vis[x][y] = True
                        q.append([x, y])
        return ans
```

#### Java

```java
class Solution {
    public int[][] allCellsDistOrder(int rows, int cols, int rCenter, int cCenter) {
        Deque<int[]> q = new ArrayDeque<>();
        q.offer(new int[] {rCenter, cCenter});
        boolean[][] vis = new boolean[rows][cols];
        vis[rCenter][cCenter] = true;
        int[][] ans = new int[rows * cols][2];
        int[] dirs = {-1, 0, 1, 0, -1};
        int idx = 0;
        while (!q.isEmpty()) {
            for (int n = q.size(); n > 0; --n) {
                var p = q.poll();
                ans[idx++] = p;
                for (int k = 0; k < 4; ++k) {
                    int x = p[0] + dirs[k], y = p[1] + dirs[k + 1];
                    if (x >= 0 && x < rows && y >= 0 && y < cols && !vis[x][y]) {
                        vis[x][y] = true;
                        q.offer(new int[] {x, y});
                    }
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
    vector<vector<int>> allCellsDistOrder(int rows, int cols, int rCenter, int cCenter) {
        queue<pair<int, int>> q;
        q.emplace(rCenter, cCenter);
        vector<vector<int>> ans;
        bool vis[rows][cols];
        memset(vis, false, sizeof(vis));
        vis[rCenter][cCenter] = true;
        int dirs[5] = {-1, 0, 1, 0, -1};
        while (!q.empty()) {
            for (int n = q.size(); n; --n) {
                auto [i, j] = q.front();
                q.pop();
                ans.push_back({i, j});
                for (int k = 0; k < 4; ++k) {
                    int x = i + dirs[k];
                    int y = j + dirs[k + 1];
                    if (x >= 0 && x < rows && y >= 0 && y < cols && !vis[x][y]) {
                        vis[x][y] = true;
                        q.emplace(x, y);
                    }
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func allCellsDistOrder(rows int, cols int, rCenter int, cCenter int) (ans [][]int) {
	q := [][]int{{rCenter, cCenter}}
	vis := make([][]bool, rows)
	for i := range vis {
		vis[i] = make([]bool, cols)
	}
	vis[rCenter][cCenter] = true
	dirs := [5]int{-1, 0, 1, 0, -1}
	for len(q) > 0 {
		for n := len(q); n > 0; n-- {
			p := q[0]
			q = q[1:]
			ans = append(ans, p)
			for k := 0; k < 4; k++ {
				x, y := p[0]+dirs[k], p[1]+dirs[k+1]
				if x >= 0 && x < rows && y >= 0 && y < cols && !vis[x][y] {
					vis[x][y] = true
					q = append(q, []int{x, y})
				}
			}
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
