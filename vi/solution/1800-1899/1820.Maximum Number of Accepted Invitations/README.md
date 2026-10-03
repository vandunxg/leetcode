---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Graph
    - Array
    - Matrix
    - Bipartite Graph
    - Max Flow
    - Graph Matching
    - Maximum Matching
    - Edmonds–Karp
    - Dinic
    - MPM
    - Push-Relabel
    - Network Flow
---

<!-- problem:start -->

# [1820. Maximum Number of Accepted Invitations 🔒](https://leetcode.com/problems/maximum-number-of-accepted-invitations)

[中文文档](/solution/1800-1899/1820.Maximum%20Number%20of%20Accepted%20Invitations/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>m</code> bạn nam và <code>n</code> bạn nữ trong một lớp sẽ tham dự một bữa tiệc sắp tới.</p>

<p>Cho ma trận số nguyên <code>m x n</code> <code>grid</code>, trong đó <code>grid[i][j]</code> bằng <code>0</code> hoặc <code>1</code>. Nếu <code>grid[i][j] == 1</code>, điều đó có nghĩa là bạn nam thứ <code>i<sup>th</sup></code> có thể mời bạn nữ thứ <code>j<sup>th</sup></code> đến bữa tiệc. Mỗi bạn nam có thể mời nhiều nhất <strong>một bạn nữ</strong>, và mỗi bạn nữ có thể nhận nhiều nhất <strong>một lời mời</strong> từ một bạn nam.</p>

<p>Trả về <em><strong>số lượng lớn nhất</strong> lời mời được chấp nhận có thể có</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,1,1],
               [1,0,1],
               [0,0,1]]
<strong>Đầu ra:</strong> 3<strong>
Giải thích:</strong> Các lời mời được gửi như sau:
- Bạn nam thứ <sup>1st</sup> mời bạn nữ thứ <sup>2nd</sup>.
- Bạn nam thứ <sup>2nd</sup> mời bạn nữ thứ <sup>1st</sup>.
- Bạn nam thứ <sup>3rd</sup> mời bạn nữ thứ <sup>3rd</sup>.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,0,1,0],
               [1,0,0,0],
               [0,0,1,0],
               [1,1,1,0]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Các lời mời được gửi như sau:
- Bạn nam thứ <sup>1st</sup> mời bạn nữ thứ <sup>3rd</sup>.
- Bạn nam thứ <sup>2nd</sup> mời bạn nữ thứ <sup>1st</sup>.
- Bạn nam thứ <sup>3rd</sup> không mời ai.
- Bạn nam thứ <sup>4th</sup> mời bạn nữ thứ <sup>2nd</sup>.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>grid.length == m</code></li>
	<li><code>grid[i].length == n</code></li>
	<li><code>1 &lt;= m, n &lt;= 200</code></li>
	<li><code>grid[i][j]</code> là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán Hungarian

<!-- thinking:start -->

> **Tư duy**
>
> Các bạn nam và bạn nữ tạo thành một đồ thị lời mời hai phía; mỗi người chỉ được ghép nhiều nhất một lần. Liệt kê mọi cách ghép có độ phức tạp hàm mũ, không phù hợp với $m,n\le 200$.
>
> Đây là bài toán ghép cực đại trên đồ thị hai phía. Thuật toán Hungarian tìm đường tăng từ mỗi đỉnh chưa ghép ở phía trái: DFS qua các đỉnh phải chưa dùng và ghép lại đối tác cũ khi cần. Mỗi đỉnh trái được tìm kiếm một lần, đủ nhanh với giới hạn này.

<!-- thinking:end -->

Bài toán này thuộc dạng ghép cực đại trên đồ thị hai phía, phù hợp để giải bằng thuật toán Hungarian.

Ý tưởng cốt lõi của thuật toán Hungarian là liên tục bắt đầu từ các đỉnh chưa ghép, tìm đường tăng và dừng lại khi không còn đường tăng nào. Khi đó ta thu được phép ghép cực đại.

Độ phức tạp thời gian là $O(m \times n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumInvitations(self, grid: List[List[int]]) -> int:
        def find(i):
            for j, v in enumerate(grid[i]):
                if v and j not in vis:
                    vis.add(j)
                    if match[j] == -1 or find(match[j]):
                        match[j] = i
                        return True
            return False

        m, n = len(grid), len(grid[0])
        match = [-1] * n
        ans = 0
        for i in range(m):
            vis = set()
            ans += find(i)
        return ans
```

#### Java

```java
class Solution {
    private int[][] grid;
    private boolean[] vis;
    private int[] match;
    private int n;

    public int maximumInvitations(int[][] grid) {
        int m = grid.length;
        n = grid[0].length;
        this.grid = grid;
        vis = new boolean[n];
        match = new int[n];
        Arrays.fill(match, -1);
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            Arrays.fill(vis, false);
            if (find(i)) {
                ++ans;
            }
        }
        return ans;
    }

    private boolean find(int i) {
        for (int j = 0; j < n; ++j) {
            if (grid[i][j] == 1 && !vis[j]) {
                vis[j] = true;
                if (match[j] == -1 || find(match[j])) {
                    match[j] = i;
                    return true;
                }
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
    int maximumInvitations(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        bool vis[210];
        int match[210];
        memset(match, -1, sizeof match);
        int ans = 0;
        function<bool(int)> find = [&](int i) -> bool {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] && !vis[j]) {
                    vis[j] = true;
                    if (match[j] == -1 || find(match[j])) {
                        match[j] = i;
                        return true;
                    }
                }
            }
            return false;
        };
        for (int i = 0; i < m; ++i) {
            memset(vis, 0, sizeof vis);
            ans += find(i);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumInvitations(grid [][]int) int {
	m, n := len(grid), len(grid[0])
	var vis map[int]bool
	match := make([]int, n)
	for i := range match {
		match[i] = -1
	}
	var find func(i int) bool
	find = func(i int) bool {
		for j, v := range grid[i] {
			if v == 1 && !vis[j] {
				vis[j] = true
				if match[j] == -1 || find(match[j]) {
					match[j] = i
					return true
				}
			}
		}
		return false
	}
	ans := 0
	for i := 0; i < m; i++ {
		vis = map[int]bool{}
		if find(i) {
			ans++
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
