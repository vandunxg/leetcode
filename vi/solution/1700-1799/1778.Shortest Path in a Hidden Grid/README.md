---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Breadth-First Search
    - Array
    - Interactive
    - Matrix
---

<!-- problem:start -->

# [1778. Shortest Path in a Hidden Grid 🔒](https://leetcode.com/problems/shortest-path-in-a-hidden-grid)

[中文文档](/solution/1700-1799/1778.Shortest%20Path%20in%20a%20Hidden%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Đây là một <strong>bài toán tương tác</strong>.</p>

<p>Có một robot trong một lưới ẩn, và bạn cần đưa robot từ ô bắt đầu đến ô đích. Lưới có kích thước <code>m x n</code>, mỗi ô hoặc trống hoặc bị chặn. Đề bài <strong>đảm bảo</strong> ô bắt đầu và ô đích khác nhau, đồng thời cả hai đều không bị chặn.</p>

<p>Bạn muốn tìm khoảng cách nhỏ nhất đến ô đích. Tuy nhiên, bạn <strong>không biết</strong> kích thước lưới, ô bắt đầu hay ô đích. Bạn chỉ được phép truy vấn đối tượng <code>GridMaster</code>.</p>

<p>Lớp <code>GridMaster</code> có các hàm sau:</p>

<ul>
<li><code>boolean canMove(char direction)</code> Trả về <code>true</code> nếu robot có thể đi theo hướng đó, ngược lại trả về <code>false</code>.</li>
<li><code>void move(char direction)</code> Di chuyển robot theo hướng đó. Nếu robot sẽ đi vào ô bị chặn hoặc ra ngoài lưới, thao tác sẽ bị <strong>bỏ qua</strong> và robot vẫn ở vị trí cũ.</li>
<li><code>boolean isTarget()</code> Trả về <code>true</code> nếu robot đang ở ô đích, ngược lại trả về <code>false</code>.</li>
</ul>

<p>Lưu ý rằng <code>direction</code> trong các hàm trên phải là một ký tự thuộc <code>{&#39;U&#39;,&#39;D&#39;,&#39;L&#39;,&#39;R&#39;}</code>, lần lượt biểu diễn các hướng lên, xuống, trái và phải.</p>

<p>Trả về <em><strong>khoảng cách nhỏ nhất</strong> giữa ô bắt đầu ban đầu của robot và ô đích. Nếu không có đường đi hợp lệ giữa hai ô, trả về </em><code>-1</code>.</p>

<p><strong>Kiểm thử tùy chỉnh:</strong></p>

<p>Dữ liệu kiểm thử được đọc dưới dạng ma trận 2D <code>grid</code> kích thước <code>m x n</code>, trong đó:</p>

<ul>
<li><code>grid[i][j] == -1</code> cho biết robot ở ô <code>(i, j)</code> (ô bắt đầu).</li>
<li><code>grid[i][j] == 0</code> cho biết ô <code>(i, j)</code> bị chặn.</li>
<li><code>grid[i][j] == 1</code> cho biết ô <code>(i, j)</code> trống.</li>
<li><code>grid[i][j] == 2</code> cho biết ô <code>(i, j)</code> là ô đích.</li>
</ul>

<p>Có đúng một <code>-1</code> và một <code>2</code> trong <code>grid</code>. Hãy nhớ rằng mã của bạn sẽ <strong>không</strong> được cung cấp thông tin này.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> grid = [[1,2],[-1,0]]
<strong>Output:</strong> 2
<strong>Explanation:</strong> One possible interaction is described below:
The robot is initially standing on cell (1, 0), denoted by the -1.
- master.canMove(&#39;U&#39;) returns true.
- master.canMove(&#39;D&#39;) returns false.
- master.canMove(&#39;L&#39;) returns false.
- master.canMove(&#39;R&#39;) returns false.
- master.move(&#39;U&#39;) moves the robot to the cell (0, 0).
- master.isTarget() returns false.
- master.canMove(&#39;U&#39;) returns false.
- master.canMove(&#39;D&#39;) returns true.
- master.canMove(&#39;L&#39;) returns false.
- master.canMove(&#39;R&#39;) returns true.
- master.move(&#39;R&#39;) moves the robot to the cell (0, 1).
- master.isTarget() returns true.
We now know that the target is the cell (0, 1), and the shortest path to the target cell is 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> grid = [[0,0,-1],[1,1,1],[2,0,0]]
<strong>Output:</strong> 4
<strong>Explanation:</strong>&nbsp;The minimum distance between the robot and the target cell is 4.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> grid = [[-1,0],[0,2]]
<strong>Output:</strong> -1
<strong>Explanation:</strong>&nbsp;There is no path from the robot to the target cell.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n, m &lt;= 500</code></li>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>grid[i][j]</code> is either <code>-1</code>, <code>0</code>, <code>1</code>, or <code>2</code>.</li>
	<li>Có <strong>chính xác một</strong> <code>-1</code> trong <code>grid</code>.</li>
	<li>Có <strong>chính xác một</strong> <code>2</code> trong <code>grid</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS xây dựng đồ thị + BFS tìm đường đi ngắn nhất

<!-- thinking:start -->

> **Tư duy**
>
> Lưới bị ẩn; ta chỉ có $\textit{canMove}/\textit{move}/\textit{isTarget}$. Muốn tìm đường đi ngắn nhất, cần biết các ô có thể đến và vị trí đích.
>
> Coi ô bắt đầu là $(0,0)$. DFS thử bốn hướng và quay lui bằng $\textit{move}$ ngược lại, đồng thời ghi các ô có thể đến vào $\textit{vis}$ và ghi nhận ô đích.
>
> Nếu chưa từng thấy ô đích, trả về $-1$; ngược lại, chạy BFS trên các ô đã phát hiện để tìm khoảng cách không trọng số.

<!-- thinking:end -->

Ta có thể giả sử robot bắt đầu tại tọa độ $(0, 0)$. Sau đó dùng DFS để tìm mọi tọa độ có thể đến và ghi chúng vào hash table $vis$. Đồng thời, cần ghi lại tọa độ của điểm đích $target$.

Nếu không tìm thấy điểm đích, trả về ngay $-1$. Ngược lại, dùng BFS để tìm đường đi ngắn nhất.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của lưới.

Similar problems:

- [1810. Minimum Path Cost in a Hidden Grid](https://github.com/doocs/leetcode/blob/main/solution/1800-1899/1810.Minimum%20Path%20Cost%20in%20a%20Hidden%20Grid/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
# """
# This is GridMaster's API interface.
# You should not implement it, or speculate about its implementation
# """
# class GridMaster(object):
#    def canMove(self, direction: str) -> bool:
#
#
#    def move(self, direction: str) -> bool:
#
#
#    def isTarget(self) -> None:
#
#


class Solution(object):
    def findShortestPath(self, master: "GridMaster") -> int:
        def dfs(i: int, j: int):
            if master.isTarget():
                nonlocal target
                target = (i, j)
                return
            for k, c in enumerate(s):
                x, y = i + dirs[k], j + dirs[k + 1]
                if master.canMove(c) and (x, y) not in vis:
                    vis.add((x, y))
                    master.move(c)
                    dfs(x, y)
                    master.move(s[(k + 2) % 4])

        s = "URDL"
        dirs = (-1, 0, 1, 0, -1)
        target = None
        vis = set()
        dfs(0, 0)
        if target is None:
            return -1
        vis.discard((0, 0))
        q = deque([(0, 0)])
        ans = -1
        while q:
            ans += 1
            for _ in range(len(q)):
                i, j = q.popleft()
                if (i, j) == target:
                    return ans
                for a, b in pairwise(dirs):
                    x, y = i + a, j + b
                    if (x, y) in vis:
                        vis.remove((x, y))
                        q.append((x, y))
        return -1
```

#### Java

```java
/**
 * // This is the GridMaster's API interface.
 * // You should not implement it, or speculate about its implementation
 * class GridMaster {
 *     boolean canMove(char direction);
 *     void move(char direction);
 *     boolean isTarget();
 * }
 */

class Solution {
    private int[] target;
    private GridMaster master;
    private final int n = 2010;
    private final String s = "URDL";
    private final int[] dirs = {-1, 0, 1, 0, -1};
    private final Set<Integer> vis = new HashSet<>();

    public int findShortestPath(GridMaster master) {
        this.master = master;
        dfs(0, 0);
        if (target == null) {
            return -1;
        }
        vis.remove(0);
        Deque<int[]> q = new ArrayDeque<>();
        q.offer(new int[] {0, 0});
        for (int ans = 0; !q.isEmpty(); ++ans) {
            for (int m = q.size(); m > 0; --m) {
                var p = q.poll();
                if (p[0] == target[0] && p[1] == target[1]) {
                    return ans;
                }
                for (int k = 0; k < 4; ++k) {
                    int x = p[0] + dirs[k], y = p[1] + dirs[k + 1];
                    if (vis.remove(x * n + y)) {
                        q.offer(new int[] {x, y});
                    }
                }
            }
        }
        return -1;
    }

    private void dfs(int i, int j) {
        if (master.isTarget()) {
            target = new int[] {i, j};
            return;
        }
        for (int k = 0; k < 4; ++k) {
            int x = i + dirs[k], y = j + dirs[k + 1];
            if (master.canMove(s.charAt(k)) && vis.add(x * n + y)) {
                master.move(s.charAt(k));
                dfs(x, y);
                master.move(s.charAt((k + 2) % 4));
            }
        }
    }
}
```

#### C++

```cpp
/**
 * // This is the GridMaster's API interface.
 * // You should not implement it, or speculate about its implementation
 * class GridMaster {
 *   public:
 *     bool canMove(char direction);
 *     void move(char direction);
 *     boolean isTarget();
 * };
 */

class Solution {
private:
    const int n = 2010;
    int dirs[5] = {-1, 0, 1, 0, -1};
    string s = "URDL";
    int target;
    unordered_set<int> vis;

public:
    int findShortestPath(GridMaster& master) {
        target = n * n;
        vis.insert(0);
        dfs(0, 0, master);
        if (target == n * n) {
            return -1;
        }
        vis.erase(0);
        queue<pair<int, int>> q;
        q.emplace(0, 0);
        for (int ans = 0; q.size(); ++ans) {
            for (int m = q.size(); m; --m) {
                auto [i, j] = q.front();
                q.pop();
                if (i * n + j == target) {
                    return ans;
                }
                for (int k = 0; k < 4; ++k) {
                    int x = i + dirs[k], y = j + dirs[k + 1];
                    if (vis.count(x * n + y)) {
                        vis.erase(x * n + y);
                        q.emplace(x, y);
                    }
                }
            }
        }
        return -1;
    }

    void dfs(int i, int j, GridMaster& master) {
        if (master.isTarget()) {
            target = i * n + j;
        }
        for (int k = 0; k < 4; ++k) {
            int x = i + dirs[k], y = j + dirs[k + 1];
            if (master.canMove(s[k]) && !vis.count(x * n + y)) {
                vis.insert(x * n + y);
                master.move(s[k]);
                dfs(x, y, master);
                master.move(s[(k + 2) % 4]);
            }
        }
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
