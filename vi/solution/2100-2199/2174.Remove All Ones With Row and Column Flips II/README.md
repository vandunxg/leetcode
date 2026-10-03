---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Breadth-First Search
    - Array
    - Matrix
---

<!-- problem:start -->

# [2174. Remove All Ones With Row and Column Flips II 🔒](https://leetcode.com/problems/remove-all-ones-with-row-and-column-flips-ii)

[中文文档](/solution/2100-2199/2174.Remove%20All%20Ones%20With%20Row%20and%20Column%20Flips%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận <strong>nhị phân</strong> <strong>0-indexed</strong> có kích thước <code>m x n</code> là <code>grid</code>.</p>

<p>Trong một thao tác, bạn có thể chọn bất kỳ <code>i</code> và <code>j</code> nào thỏa mãn các điều kiện sau:</p>

<ul>
	<li><code>0 &lt;= i &lt; m</code></li>
	<li><code>0 &lt;= j &lt; n</code></li>
	<li><code>grid[i][j] == 1</code></li>
</ul>

<p>sau đó đặt giá trị của <strong>tất cả</strong> các ô trên hàng <code>i</code> và cột <code>j</code> thành số 0.</p>

<p>Hãy trả về <em>số thao tác <strong>ít nhất</strong> cần thực hiện để xóa tất cả </em><code>1</code><em> khỏi </em><code>grid</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2174.Remove%20All%20Ones%20With%20Row%20and%20Column%20Flips%20II/images/image-20220213162716-1.png" style="width: 709px; height: 200px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,1,1],[1,1,1],[0,1,0]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Trong thao tác đầu tiên, đặt tất cả giá trị ô trên hàng 1 và cột 1 thành 0.
Trong thao tác thứ hai, đặt tất cả giá trị ô trên hàng 0 và cột 0 thành 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2174.Remove%20All%20Ones%20With%20Row%20and%20Column%20Flips%20II/images/image-20220213162737-2.png" style="width: 734px; height: 200px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,1,0],[1,0,1],[0,1,0]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Trong thao tác đầu tiên, đặt tất cả giá trị ô trên hàng 1 và cột 0 thành 0.
Trong thao tác thứ hai, đặt tất cả giá trị ô trên hàng 2 và cột 1 thành 0.
Lưu ý rằng ta không thể thực hiện thao tác với hàng 1 và cột 1 vì grid[1][1] != 1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2174.Remove%20All%20Ones%20With%20Row%20and%20Column%20Flips%20II/images/image-20220213162752-3.png" style="width: 156px; height: 150px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,0],[0,0]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Không có số 1 nào cần xóa nên trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 15</code></li>
	<li><code>1 &lt;= m * n &lt;= 15</code></li>
	<li><code>grid[i][j]</code> là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác chọn một ô chứa $1$ rồi xóa toàn bộ hàng và cột chứa ô đó. Ma trận có kích thước nhiều nhất $8\times 8$, nên mỗi trạng thái có thể được biểu diễn bằng một số nguyên. Thứ tự của một tập hợp thao tác không ảnh hưởng đến kết quả; BFS tìm được chuỗi thao tác ngắn nhất.
>
> Mã hóa ma trận vào $\textit{state}$. Từ một ô chứa $1$, xóa mọi bit trên hàng và cột của ô đó. Khoảng cách trong đồ thị này chính là số thao tác.
>
> Bắt đầu từ mask ban đầu và dừng khi đạt đến $0$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def removeOnes(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        state = sum(1 << (i * n + j) for i in range(m) for j in range(n) if grid[i][j])
        q = deque([state])
        vis = {state}
        ans = 0
        while q:
            for _ in range(len(q)):
                state = q.popleft()
                if state == 0:
                    return ans
                for i in range(m):
                    for j in range(n):
                        if grid[i][j] == 0:
                            continue
                        nxt = state
                        for r in range(m):
                            nxt &= ~(1 << (r * n + j))
                        for c in range(n):
                            nxt &= ~(1 << (i * n + c))
                        if nxt not in vis:
                            vis.add(nxt)
                            q.append(nxt)
            ans += 1
        return -1
```

#### Java

```java
class Solution {
    public int removeOnes(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int state = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 1) {
                    state |= 1 << (i * n + j);
                }
            }
        }
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(state);
        Set<Integer> vis = new HashSet<>();
        vis.add(state);
        int ans = 0;
        while (!q.isEmpty()) {
            for (int k = q.size(); k > 0; --k) {
                state = q.poll();
                if (state == 0) {
                    return ans;
                }
                for (int i = 0; i < m; ++i) {
                    for (int j = 0; j < n; ++j) {
                        if (grid[i][j] == 0) {
                            continue;
                        }
                        int nxt = state;
                        for (int r = 0; r < m; ++r) {
                            nxt &= ~(1 << (r * n + j));
                        }
                        for (int c = 0; c < n; ++c) {
                            nxt &= ~(1 << (i * n + c));
                        }
                        if (!vis.contains(nxt)) {
                            vis.add(nxt);
                            q.offer(nxt);
                        }
                    }
                }
            }
            ++ans;
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int removeOnes(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        int state = 0;
        for (int i = 0; i < m; ++i)
            for (int j = 0; j < n; ++j)
                if (grid[i][j])
                    state |= (1 << (i * n + j));
        queue<int> q{{state}};
        unordered_set<int> vis{{state}};
        int ans = 0;
        while (!q.empty()) {
            for (int k = q.size(); k > 0; --k) {
                state = q.front();
                q.pop();
                if (state == 0) return ans;
                for (int i = 0; i < m; ++i) {
                    for (int j = 0; j < n; ++j) {
                        if (grid[i][j] == 0) continue;
                        int nxt = state;
                        for (int r = 0; r < m; ++r) nxt &= ~(1 << (r * n + j));
                        for (int c = 0; c < n; ++c) nxt &= ~(1 << (i * n + c));
                        if (!vis.count(nxt)) {
                            vis.insert(nxt);
                            q.push(nxt);
                        }
                    }
                }
            }
            ++ans;
        }
        return -1;
    }
};
```

#### Go

```go
func removeOnes(grid [][]int) int {
	m, n := len(grid), len(grid[0])
	state := 0
	for i, row := range grid {
		for j, v := range row {
			if v == 1 {
				state |= 1 << (i*n + j)
			}
		}
	}
	q := []int{state}
	vis := map[int]bool{state: true}
	ans := 0
	for len(q) > 0 {
		for k := len(q); k > 0; k-- {
			state = q[0]
			if state == 0 {
				return ans
			}
			q = q[1:]
			for i, row := range grid {
				for j, v := range row {
					if v == 0 {
						continue
					}
					nxt := state
					for r := 0; r < m; r++ {
						nxt &= ^(1 << (r*n + j))
					}
					for c := 0; c < n; c++ {
						nxt &= ^(1 << (i*n + c))
					}
					if !vis[nxt] {
						vis[nxt] = true
						q = append(q, nxt)
					}
				}
			}
		}
		ans++
	}
	return -1
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
