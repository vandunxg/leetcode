---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [08.02. Robot in a Grid](https://leetcode.cn/problems/robot-in-a-grid-lcci)

[中文文档](/lcci/08.02.Robot%20in%20a%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy hình dung một robot đang ở góc trên bên trái của grid gồm r hàng và c cột. Robot chỉ có thể di chuyển theo hai hướng là sang phải và xuống dưới, nhưng một số ô là &quot;không được đi vào&quot;, nên robot không thể bước lên đó. Hãy thiết kế một thuật toán để tìm đường đi cho robot từ góc trên bên trái đến góc dưới bên phải.</p>

![](https://fastly.jsdelivr.net/gh/doocs/leetcode@main/lcci/08.02.Robot%20in%20a%20Grid/images/robot_maze.png)

<p>Các ô &quot;không được đi vào&quot; và grid trống lần lượt được biểu diễn bằng&nbsp;<code>1</code> và&nbsp;<code>0</code>.</p>
<p>Trả về một đường đi hợp lệ, gồm số hàng và số cột của các ô trên đường đi.</p>
<p><strong>Ví dụ&nbsp;1:</strong></p>
<pre>

<strong>Đầu vào:

</strong>[

&nbsp; [<strong>0</strong>,<strong>0</strong>,<strong>0</strong>],

&nbsp; [0,1,<strong>0</strong>],

&nbsp; [0,0,<strong>0</strong>]

]

<strong>Đầu ra:</strong> [[0,0],[0,1],[0,2],[1,2],[2,2]]</pre>

<p><strong>Lưu ý: </strong></p>
<ul>
	<li><code>r,&nbsp;c &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS (Depth-First Search)

<!-- thinking:start -->

> **Tư duy**
>
> Một đường đi từ góc trên bên trái đến góc dưới bên phải chỉ có thể đi sang phải hoặc xuống dưới và phải tránh chướng ngại vật. Grid đủ nhỏ để có thể tìm kiếm, nhưng việc đi lại một ô đã thăm sẽ gây lãng phí.
>
> Chỉ cần tìm được bất kỳ đường đi khả thi nào, nên DFS thử đi xuống trước rồi sang phải, và backtrack khi thất bại.
>
> Khi vào một ô, ta thêm ô đó vào đường đi và đánh dấu nó là chướng ngại vật để ngăn việc đi lại; nếu thành công thì giữ đường đi, nếu thất bại thì xóa ô khỏi đường đi. Việc đánh dấu trực tiếp thay thế cho một $vis$ riêng.

<!-- thinking:end -->

Ta có thể sử dụng tìm kiếm theo chiều sâu để giải bài toán này. Ta bắt đầu từ góc trên bên trái và di chuyển sang phải hoặc xuống dưới cho đến khi đến góc dưới bên phải. Nếu tại một bước nào đó, ta nhận thấy vị trí hiện tại là chướng ngại vật hoặc đã nằm trong đường đi, thì dừng lại. Nếu không, ta thêm vị trí hiện tại vào đường đi và đánh dấu vị trí đó đã được thăm, sau đó tiếp tục di chuyển sang phải hoặc xuống dưới.

Nếu cuối cùng ta đến được góc dưới bên phải, nghĩa là đã tìm thấy một đường đi khả thi; ngược lại, nghĩa là không có đường đi khả thi.

Độ phức tạp thời gian là $O(m \times n)$, còn độ phức tạp không gian là $O(m \times n)$. Ở đây, $m$ và $n$ lần lượt là số hàng và số cột của grid.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def pathWithObstacles(self, obstacleGrid: List[List[int]]) -> List[List[int]]:
        def dfs(i, j):
            if i >= m or j >= n or obstacleGrid[i][j] == 1:
                return False
            ans.append([i, j])
            obstacleGrid[i][j] = 1
            if (i == m - 1 and j == n - 1) or dfs(i + 1, j) or dfs(i, j + 1):
                return True
            ans.pop()
            return False

        m, n = len(obstacleGrid), len(obstacleGrid[0])
        ans = []
        return ans if dfs(0, 0) else []
```

#### Java

```java
class Solution {
    private List<List<Integer>> ans = new ArrayList<>();
    private int[][] g;
    private int m;
    private int n;

    public List<List<Integer>> pathWithObstacles(int[][] obstacleGrid) {
        g = obstacleGrid;
        m = g.length;
        n = g[0].length;
        return dfs(0, 0) ? ans : Collections.emptyList();
    }

    private boolean dfs(int i, int j) {
        if (i >= m || j >= n || g[i][j] == 1) {
            return false;
        }
        ans.add(List.of(i, j));
        g[i][j] = 1;
        if ((i == m - 1 && j == n - 1) || dfs(i + 1, j) || dfs(i, j + 1)) {
            return true;
        }
        ans.remove(ans.size() - 1);
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> pathWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();
        vector<vector<int>> ans;
        auto dfs = [&](this auto&& dfs, int i, int j) -> bool {
            if (i >= m || j >= n || obstacleGrid[i][j] == 1) {
                return false;
            }
            ans.push_back({i, j});
            obstacleGrid[i][j] = 1;
            if ((i == m - 1 && j == n - 1) || dfs(i + 1, j) || dfs(i, j + 1)) {
                return true;
            }
            ans.pop_back();
            return false;
        };
        return dfs(0, 0) ? ans : vector<vector<int>>();
    }
};
```

#### Go

```go
func pathWithObstacles(obstacleGrid [][]int) [][]int {
	m, n := len(obstacleGrid), len(obstacleGrid[0])
	ans := [][]int{}
	var dfs func(i, j int) bool
	dfs = func(i, j int) bool {
		if i >= m || j >= n || obstacleGrid[i][j] == 1 {
			return false
		}
		ans = append(ans, []int{i, j})
		obstacleGrid[i][j] = 1
		if (i == m-1 && j == n-1) || dfs(i+1, j) || dfs(i, j+1) {
			return true
		}
		ans = ans[:len(ans)-1]
		return false
	}
	if dfs(0, 0) {
		return ans
	}
	return [][]int{}
}
```

#### TypeScript

```ts
function pathWithObstacles(obstacleGrid: number[][]): number[][] {
    const m = obstacleGrid.length;
    const n = obstacleGrid[0].length;
    const res = [];
    const dfs = (i: number, j: number): boolean => {
        if (i === m || j === n || obstacleGrid[i][j] === 1) {
            return false;
        }
        res.push([i, j]);
        obstacleGrid[i][j] = 1;
        if ((i + 1 === m && j + 1 === n) || dfs(i + 1, j) || dfs(i, j + 1)) {
            return true;
        }
        res.pop();
        return false;
    };
    if (dfs(0, 0)) {
        return res;
    }
    return [];
}
```

#### Rust

```rust
impl Solution {
    fn dfs(grid: &mut Vec<Vec<i32>>, path: &mut Vec<Vec<i32>>, i: usize, j: usize) -> bool {
        if i == grid.len() || j == grid[0].len() || grid[i][j] == 1 {
            return false;
        }
        path.push(vec![i as i32, j as i32]);
        grid[i as usize][j as usize] = 1;
        if (i + 1 == grid.len() && j + 1 == grid[0].len())
            || Self::dfs(grid, path, i + 1, j)
            || Self::dfs(grid, path, i, j + 1)
        {
            return true;
        }
        path.pop();
        false
    }

    pub fn path_with_obstacles(mut obstacle_grid: Vec<Vec<i32>>) -> Vec<Vec<i32>> {
        let mut res = vec![];
        if Self::dfs(&mut obstacle_grid, &mut res, 0, 0) {
            return res;
        }
        vec![]
    }
}
```

#### Swift

```swift
class Solution {
    private var ans = [[Int]]()
    private var g: [[Int]] = []
    private var m: Int = 0
    private var n: Int = 0

    func pathWithObstacles(_ obstacleGrid: [[Int]]) -> [[Int]] {
        g = obstacleGrid
        m = g.count
        n = g[0].count
        return dfs(0, 0) ? ans : []
    }

    private func dfs(_ i: Int, _ j: Int) -> Bool {
        if i >= m || j >= n || g[i][j] == 1 {
            return false
        }
        ans.append([i, j])
        g[i][j] = 1
        if (i == m - 1 && j == n - 1) || dfs(i + 1, j) || dfs(i, j + 1) {
            return true
        }
        ans.removeLast()
        return false
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
