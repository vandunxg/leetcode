---
comments: true
difficulty: Hard
tags:
    - Backtracking
    - Interactive
---

<!-- problem:start -->

# [489. Robot Room Cleaner 🔒](https://leetcode.com/problems/robot-room-cleaner)

[中文文档](/solution/0400-0499/0489.Robot%20Room%20Cleaner/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn điều khiển một robot đang ở một vị trí nào đó trong phòng. Phòng được mô hình hóa thành lưới nhị phân <code>m x n</code>, trong đó <code>0</code> biểu diễn tường và <code>1</code> biểu diễn ô trống.</p>

<p>Robot bắt đầu ở một vị trí chưa biết trong phòng, nhưng chắc chắn là ô trống. Bạn không thể truy cập lưới, nhưng có thể điều khiển robot bằng API <code>Robot</code> đã cho.</p>

<p>Hãy dùng robot để dọn sạch toàn bộ phòng (tức là dọn mọi ô trống). Với bốn API đã cho, robot có thể đi thẳng, rẽ trái hoặc rẽ phải. Mỗi lần rẽ là <code>90</code> độ.</p>

<p>Khi robot cố đi vào ô tường, cảm biến va chạm phát hiện vật cản và robot vẫn ở ô hiện tại.</p>

<p>Hãy thiết kế thuật toán dọn sạch toàn bộ phòng bằng các API sau:</p>

<pre>
interface Robot {
  // returns true if next cell is open and robot moves into the cell.
  // returns false if next cell is obstacle and robot stays on the current cell.
  boolean move();

  // Robot will stay on the same cell after calling turnLeft/turnRight.
  // Each turn will be 90 degrees.
  void turnLeft();
  void turnRight();

  // Clean the current cell.
  void clean();
}
</pre>

<p><strong>Lưu ý</strong>, ban đầu robot hướng lên trên. Có thể giả sử cả bốn cạnh của lưới đều được bao quanh bởi tường.</p>

<p>&nbsp;</p>

<p><strong>Kiểm thử tùy chỉnh:</strong></p>

<p>Đầu vào chỉ được dùng nội bộ để khởi tạo phòng và vị trí robot. Bạn phải giải bài toán như thể bị &quot;bịt mắt&quot;. Nói cách khác, bạn chỉ được điều khiển robot bằng bốn API đã nêu, mà không biết bố cục phòng hay vị trí ban đầu của robot.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0489.Robot%20Room%20Cleaner/images/lc-grid.jpg" style="width: 500px; height: 314px;" />
<pre>
<strong>Đầu vào:</strong> room = [[1,1,1,1,1,0,1,1],[1,1,1,1,1,0,1,1],[1,0,1,1,1,1,1,1],[0,0,0,1,0,0,0,0],[1,1,1,1,1,1,1,1]], row = 1, col = 3
<strong>Đầu ra:</strong> Robot cleaned all rooms.
<strong>Giải thích:</strong> Mỗi ô trong phòng được đánh dấu bằng 0 hoặc 1.
0 nghĩa là ô bị chặn, còn 1 nghĩa là ô có thể đi vào.
Ban đầu robot ở hàng=1, cột=3.
Tính từ góc trên bên trái, vị trí này nằm dưới một hàng và lệch sang phải ba cột.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> room = [[1]], row = 0, col = 0
<strong>Đầu ra:</strong> Robot cleaned all rooms.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == room.length</code></li>
	<li><code>n == room[i].length</code></li>
	<li><code>1 &lt;= m &lt;= 100</code></li>
	<li><code>1 &lt;= n &lt;= 200</code></li>
	<li><code>room[i][j]</code> bằng <code>0</code> hoặc <code>1</code>.</li>
	<li><code>0 &lt;= row &lt;&nbsp;m</code></li>
	<li><code>0 &lt;= col &lt; n</code></li>
	<li><code>room[row][col] == 1</code></li>
	<li>Có thể đi đến mọi ô trống từ vị trí bắt đầu.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Solution 1

<!-- thinking:start -->

> **Tư duy**
>
> Bản đồ phòng bị ẩn; ta chỉ có thể di chuyển, xoay hướng và dọn ô. Đi ngẫu nhiên không đảm bảo dọn hết phòng hoặc quay lại vị trí ban đầu.
>
> Xem vị trí bắt đầu là $(0,0)$ rồi DFS theo bốn hướng: đi vào ô nếu đó là ô mới và $\textit{move}$ thành công. Sau khi thử cả bốn hướng, quay ngược lại, lùi về ô cha rồi khôi phục hướng ban đầu.
>
> Dùng set các tọa độ tương đối đã thăm để tránh lặp lại. Khi quay lui, phải xoay $180^\circ$ trước khi gọi $\textit{move}$; nếu không, hướng robot sẽ không khớp với trạng thái đệ quy.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
# """
# This is the robot's control interface.
# You should not implement it, or speculate about its implementation
# """
# class Robot:
#    def move(self):
#        """
#        Returns true if the cell in front is open and robot moves into the cell.
#        Returns false if the cell in front is blocked and robot stays in the current cell.
#        :rtype bool
#        """
#
#    def turnLeft(self):
#        """
#        Robot will stay in the same cell after calling turnLeft/turnRight.
#        Each turn will be 90 degrees.
#        :rtype void
#        """
#
#    def turnRight(self):
#        """
#        Robot will stay in the same cell after calling turnLeft/turnRight.
#        Each turn will be 90 degrees.
#        :rtype void
#        """
#
#    def clean(self):
#        """
#        Clean the current cell.
#        :rtype void
#        """


class Solution:
    def cleanRoom(self, robot):
        """
        :type robot: Robot
        :rtype: None
        """

        def dfs(i, j, d):
            vis.add((i, j))
            robot.clean()
            for k in range(4):
                nd = (d + k) % 4
                x, y = i + dirs[nd], j + dirs[nd + 1]
                if (x, y) not in vis and robot.move():
                    dfs(x, y, nd)
                    robot.turnRight()
                    robot.turnRight()
                    robot.move()
                    robot.turnRight()
                    robot.turnRight()
                robot.turnRight()

        dirs = (-1, 0, 1, 0, -1)
        vis = set()
        dfs(0, 0, 0)
```

#### Java

```java
/**
 * // This is the robot's control interface.
 * // You should not implement it, or speculate about its implementation
 * interface Robot {
 *     // Returns true if the cell in front is open and robot moves into the cell.
 *     // Returns false if the cell in front is blocked and robot stays in the current cell.
 *     public boolean move();
 *
 *     // Robot will stay in the same cell after calling turnLeft/turnRight.
 *     // Each turn will be 90 degrees.
 *     public void turnLeft();
 *     public void turnRight();
 *
 *     // Clean the current cell.
 *     public void clean();
 * }
 */

class Solution {
    private int[] dirs = {-1, 0, 1, 0, -1};
    private Set<List<Integer>> vis = new HashSet<>();
    private Robot robot;

    public void cleanRoom(Robot robot) {
        this.robot = robot;
        dfs(0, 0, 0);
    }

    private void dfs(int i, int j, int d) {
        robot.clean();
        vis.add(List.of(i, j));
        for (int k = 0; k < 4; ++k) {
            int nd = (d + k) % 4;
            int x = i + dirs[nd], y = j + dirs[nd + 1];
            if (!vis.contains(List.of(x, y)) && robot.move()) {
                dfs(x, y, nd);
                robot.turnRight();
                robot.turnRight();
                robot.move();
                robot.turnRight();
                robot.turnRight();
            }
            robot.turnRight();
        }
    }
}
```

#### C++

```cpp
/**
 * // This is the robot's control interface.
 * // You should not implement it, or speculate about its implementation
 * class Robot {
 *   public:
 *     // Returns true if the cell in front is open and robot moves into the cell.
 *     // Returns false if the cell in front is blocked and robot stays in the current cell.
 *     bool move();
 *
 *     // Robot will stay in the same cell after calling turnLeft/turnRight.
 *     // Each turn will be 90 degrees.
 *     void turnLeft();
 *     void turnRight();
 *
 *     // Clean the current cell.
 *     void clean();
 * };
 */

class Solution {
public:
    void cleanRoom(Robot& robot) {
        int dirs[5] = {-1, 0, 1, 0, -1};
        set<pair<int, int>> vis;
        function<void(int, int, int)> dfs = [&](int i, int j, int d) {
            robot.clean();
            vis.insert({i, j});
            for (int k = 0; k < 4; ++k) {
                int nd = (d + k) % 4;
                int x = i + dirs[nd], y = j + dirs[nd + 1];
                if (!vis.count({x, y}) && robot.move()) {
                    dfs(x, y, nd);
                    robot.turnRight();
                    robot.turnRight();
                    robot.move();
                    robot.turnRight();
                    robot.turnRight();
                }
                robot.turnRight();
            }
        };
        dfs(0, 0, 0);
    }
};
```

#### Go

```go
/**
 * // This is the robot's control interface.
 * // You should not implement it, or speculate about its implementation
 * type Robot struct {
 * }
 *
 * // Returns true if the cell in front is open and robot moves into the cell.
 * // Returns false if the cell in front is blocked and robot stays in the current cell.
 * func (robot *Robot) Move() bool {}
 *
 * // Robot will stay in the same cell after calling TurnLeft/TurnRight.
 * // Each turn will be 90 degrees.
 * func (robot *Robot) TurnLeft() {}
 * func (robot *Robot) TurnRight() {}
 *
 * // Clean the current cell.
 * func (robot *Robot) Clean() {}
 */

func cleanRoom(robot *Robot) {
	vis := map[[2]int]bool{}
	dirs := [5]int{-1, 0, 1, 0, -1}
	var dfs func(int, int, int)
	dfs = func(i, j, d int) {
		vis[[2]int{i, j}] = true
		robot.Clean()
		for k := 0; k < 4; k++ {
			nd := (d + k) % 4
			x, y := i+dirs[nd], j+dirs[nd+1]
			if !vis[[2]int{x, y}] && robot.Move() {
				dfs(x, y, nd)
				robot.TurnRight()
				robot.TurnRight()
				robot.Move()
				robot.TurnRight()
				robot.TurnRight()
			}
			robot.TurnRight()
		}
	}
	dfs(0, 0, 0)
}
```

#### TypeScript

```ts
/**
 * class Robot {
 *      // Returns true if the cell in front is open and robot moves into the cell.
 *      // Returns false if the cell in front is blocked and robot stays in the current cell.
 * 		move(): boolean {}
 *
 *      // Robot will stay in the same cell after calling turnLeft/turnRight.
 *      // Each turn will be 90 degrees.
 * 		turnRight() {}
 *
 *      // Robot will stay in the same cell after calling turnLeft/turnRight.
 *      // Each turn will be 90 degrees.
 * 		turnLeft() {}
 *
 * 		// Clean the current cell.
 * 		clean(): {}
 * }
 */

function cleanRoom(robot: Robot) {
    const dirs = [-1, 0, 1, 0, -1];
    const vis = new Set<string>();
    const dfs = (i: number, j: number, d: number) => {
        vis.add(`${i}-${j}`);
        robot.clean();
        for (let k = 0; k < 4; ++k) {
            const nd = (d + k) % 4;
            const [x, y] = [i + dirs[nd], j + dirs[nd + 1]];
            if (!vis.has(`${x}-${y}`) && robot.move()) {
                dfs(x, y, nd);
                robot.turnRight();
                robot.turnRight();
                robot.move();
                robot.turnRight();
                robot.turnRight();
            }
            robot.turnRight();
        }
    };
    dfs(0, 0, 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
