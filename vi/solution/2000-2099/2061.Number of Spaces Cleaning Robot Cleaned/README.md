---
comments: true
difficulty: Medium
tags:
    - Array
    - Matrix
    - Simulation
---

<!-- problem:start -->

# [2061. Number of Spaces Cleaning Robot Cleaned 🔒](https://leetcode.com/problems/number-of-spaces-cleaning-robot-cleaned)

[中文文档](/solution/2000-2099/2061.Number%20of%20Spaces%20Cleaning%20Robot%20Cleaned/README.md)

## Mô tả

<!-- description:start -->

<p>Một căn phòng được biểu diễn bằng ma trận nhị phân 2D <code>room</code> <strong>đánh chỉ số từ 0</strong>, trong đó <code>0</code> biểu thị một ô <strong>trống</strong> và <code>1</code> biểu thị một ô có <strong>vật thể</strong>. Góc trên bên trái của căn phòng sẽ luôn trống trong mọi test case.</p>

<p>Một robot dọn dẹp bắt đầu ở góc trên bên trái của căn phòng và hướng sang phải. Robot sẽ tiếp tục đi thẳng cho đến khi chạm biên căn phòng hoặc gặp vật thể, sau đó quay 90 độ theo <strong>chiều kim đồng hồ</strong> và lặp lại quá trình này. Ô xuất phát và mọi ô robot đi qua đều được robot <strong>dọn sạch</strong>.</p>

<p>Hãy trả về <em>số ô <strong>sạch</strong> trong căn phòng nếu robot chạy vô hạn.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2061.Number%20of%20Spaces%20Cleaning%20Robot%20Cleaned/images/image-20211101204703-1.png" style="width: 250px; height: 242px;" />
<p>&nbsp;</p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">room = [[0,0,0],[1,1,0],[0,0,0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<ol>
	<li>​​​​​​​Robot dọn sạch các ô (0, 0), (0, 1) và (0, 2).</li>
	<li>Robot đang ở biên căn phòng, nên quay 90 độ theo chiều kim đồng hồ và hướng xuống.</li>
	<li>Robot dọn sạch các ô (1, 2) và (2, 2).</li>
	<li>Robot đang ở biên căn phòng, nên quay 90 độ theo chiều kim đồng hồ và hướng sang trái.</li>
	<li>Robot dọn sạch các ô (2, 1) và (2, 0).</li>
	<li>Robot đã dọn sạch cả 7 ô trống, nên trả về 7.</li>
</ol>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2061.Number%20of%20Spaces%20Cleaning%20Robot%20Cleaned/images/image-20211101204736-2.png" style="width: 250px; height: 245px;" />
<p>&nbsp;</p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">room = [[0,1,0],[1,0,0],[0,0,0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ol>
	<li>Robot dọn sạch ô (0, 0).</li>
	<li>Robot gặp một vật thể, nên quay 90 độ theo chiều kim đồng hồ và hướng xuống.</li>
	<li>Robot gặp một vật thể, nên quay 90 độ theo chiều kim đồng hồ và hướng sang trái.</li>
	<li>Robot đang ở biên căn phòng, nên quay 90 độ theo chiều kim đồng hồ và hướng lên.</li>
	<li>Robot đang ở biên căn phòng, nên quay 90 độ theo chiều kim đồng hồ và hướng sang phải.</li>
	<li>Robot quay lại vị trí xuất phát.</li>
	<li>Robot đã dọn sạch 1 ô, nên trả về 1.</li>
</ol>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">room = [[0,0,0],[0,0,0],[0,0,0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span>​​​​​​​</p>

<p>&nbsp;</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == room.length</code></li>
	<li><code>n == room[r].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 300</code></li>
	<li><code>room[r][c]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
	<li><code>room[0][0] == 0</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Robot rẽ phải khi gặp vật cản; căn phòng có kích thước tối đa $300 \times 300$. Robot sẽ lặp lại, vì vậy ta dừng khi một trạng thái (ô, hướng) xuất hiện lần thứ hai. Các ô trống đã dọn được đánh dấu và chỉ đếm một lần.
>
> Lưu các trạng thái $(i,j,k)$. DFS tiến lên nếu ô phía trước trống, ngược lại quay tại chỗ. Lần đầu ghé một ô trống thì tăng đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfCleanRooms(self, room: List[List[int]]) -> int:
        def dfs(i, j, k):
            if (i, j, k) in vis:
                return
            nonlocal ans
            ans += room[i][j] == 0
            room[i][j] = -1
            vis.add((i, j, k))
            x, y = i + dirs[k], j + dirs[k + 1]
            if 0 <= x < len(room) and 0 <= y < len(room[0]) and room[x][y] != 1:
                dfs(x, y, k)
            else:
                dfs(i, j, (k + 1) % 4)

        vis = set()
        dirs = (0, 1, 0, -1, 0)
        ans = 0
        dfs(0, 0, 0)
        return ans
```

#### Java

```java
class Solution {
    private boolean[][][] vis;
    private int[][] room;
    private int ans;

    public int numberOfCleanRooms(int[][] room) {
        vis = new boolean[room.length][room[0].length][4];
        this.room = room;
        dfs(0, 0, 0);
        return ans;
    }

    private void dfs(int i, int j, int k) {
        if (vis[i][j][k]) {
            return;
        }
        int[] dirs = {0, 1, 0, -1, 0};
        ans += room[i][j] == 0 ? 1 : 0;
        room[i][j] = -1;
        vis[i][j][k] = true;
        int x = i + dirs[k], y = j + dirs[k + 1];
        if (x >= 0 && x < room.length && y >= 0 && y < room[0].length && room[x][y] != 1) {
            dfs(x, y, k);
        } else {
            dfs(i, j, (k + 1) % 4);
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfCleanRooms(vector<vector<int>>& room) {
        int m = room.size(), n = room[0].size();
        bool vis[m][n][4];
        memset(vis, false, sizeof(vis));
        int dirs[5] = {0, 1, 0, -1, 0};
        int ans = 0;
        function<void(int, int, int)> dfs = [&](int i, int j, int k) {
            if (vis[i][j][k]) {
                return;
            }
            ans += room[i][j] == 0;
            room[i][j] = -1;
            vis[i][j][k] = true;
            int x = i + dirs[k], y = j + dirs[k + 1];
            if (x >= 0 && x < m && y >= 0 && y < n && room[x][y] != 1) {
                dfs(x, y, k);
            } else {
                dfs(i, j, (k + 1) % 4);
            }
        };
        dfs(0, 0, 0);
        return ans;
    }
};
```

#### Go

```go
func numberOfCleanRooms(room [][]int) (ans int) {
	m, n := len(room), len(room[0])
	vis := make([][][4]bool, m)
	for i := range vis {
		vis[i] = make([][4]bool, n)
	}
	dirs := [5]int{0, 1, 0, -1, 0}
	var dfs func(i, j, k int)
	dfs = func(i, j, k int) {
		if vis[i][j][k] {
			return
		}
		if room[i][j] == 0 {
			ans++
			room[i][j] = -1
		}
		vis[i][j][k] = true
		x, y := i+dirs[k], j+dirs[k+1]
		if x >= 0 && x < m && y >= 0 && y < n && room[x][y] != 1 {
			dfs(x, y, k)
		} else {
			dfs(i, j, (k+1)%4)
		}
	}
	dfs(0, 0, 0)
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 đệ quy đến độ sâu $O(mn)$. Cùng một chuyển trạng thái có thể được viết thành vòng lặp: ghi lại bộ ba, di chuyển hoặc quay, cho đến khi một trạng thái lặp lại.
>
> Quy tắc đếm không thay đổi.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfCleanRooms(self, room: List[List[int]]) -> int:
        dirs = (0, 1, 0, -1, 0)
        i = j = k = 0
        ans = 0
        vis = set()
        while (i, j, k) not in vis:
            vis.add((i, j, k))
            ans += room[i][j] == 0
            room[i][j] = -1
            x, y = i + dirs[k], j + dirs[k + 1]
            if 0 <= x < len(room) and 0 <= y < len(room[0]) and room[x][y] != 1:
                i, j = x, y
            else:
                k = (k + 1) % 4
        return ans
```

#### Java

```java
class Solution {
    public int numberOfCleanRooms(int[][] room) {
        int[] dirs = {0, 1, 0, -1, 0};
        int i = 0, j = 0, k = 0;
        int m = room.length, n = room[0].length;
        boolean[][][] vis = new boolean[m][n][4];
        int ans = 0;
        while (!vis[i][j][k]) {
            vis[i][j][k] = true;
            ans += room[i][j] == 0 ? 1 : 0;
            room[i][j] = -1;
            int x = i + dirs[k], y = j + dirs[k + 1];
            if (x >= 0 && x < m && y >= 0 && y < n && room[x][y] != 1) {
                i = x;
                j = y;
            } else {
                k = (k + 1) % 4;
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
    int numberOfCleanRooms(vector<vector<int>>& room) {
        int dirs[5] = {0, 1, 0, -1, 0};
        int i = 0, j = 0, k = 0;
        int m = room.size(), n = room[0].size();
        bool vis[m][n][4];
        memset(vis, false, sizeof(vis));
        int ans = 0;
        while (!vis[i][j][k]) {
            vis[i][j][k] = true;
            ans += room[i][j] == 0 ? 1 : 0;
            room[i][j] = -1;
            int x = i + dirs[k], y = j + dirs[k + 1];
            if (x >= 0 && x < m && y >= 0 && y < n && room[x][y] != 1) {
                i = x;
                j = y;
            } else {
                k = (k + 1) % 4;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfCleanRooms(room [][]int) (ans int) {
	m, n := len(room), len(room[0])
	vis := make([][][4]bool, m)
	for i := range vis {
		vis[i] = make([][4]bool, n)
	}
	dirs := [5]int{0, 1, 0, -1, 0}
	var i, j, k int
	for !vis[i][j][k] {
		vis[i][j][k] = true
		if room[i][j] == 0 {
			ans++
			room[i][j] = -1
		}
		x, y := i+dirs[k], j+dirs[k+1]
		if x >= 0 && x < m && y >= 0 && y < n && room[x][y] != 1 {
			i, j = x, y
		} else {
			k = (k + 1) % 4
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
