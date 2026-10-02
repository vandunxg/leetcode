---
comments: true
difficulty: Medium
rating: 1374
source: Weekly Contest 164 Q2
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Array
    - Counting
    - Matrix
---

<!-- problem:start -->

# [1267. Count Servers that Communicate](https://leetcode.com/problems/count-servers-that-communicate)

[中文文档](/solution/1200-1299/1267.Count%20Servers%20that%20Communicate/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho sơ đồ một trung tâm máy chủ, biểu diễn bằng ma trận số nguyên <code>m * n</code> <code>grid</code>, trong đó giá trị 1 cho biết ô đó có máy chủ, còn 0 nghĩa là không có máy chủ. Hai máy chủ được xem là có thể liên lạc nếu chúng nằm cùng hàng hoặc cùng cột.<br />
<br />
Hãy trả về số máy chủ có thể liên lạc với ít nhất một máy chủ khác.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1267.Count%20Servers%20that%20Communicate/images/untitled-diagram-6.jpg" style="width: 202px; height: 203px;" /></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,0],[0,1]]
<strong>Đầu ra:</strong> 0
<b>Giải thích:</b>&nbsp;Không có máy chủ nào có thể liên lạc với máy chủ khác.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1267.Count%20Servers%20that%20Communicate/images/untitled-diagram-4.jpg" style="width: 203px; height: 203px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,0],[1,1]]
<strong>Đầu ra:</strong> 3
<b>Giải thích:</b>&nbsp;Cả ba máy chủ đều có thể liên lạc với ít nhất một máy chủ khác.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1267.Count%20Servers%20that%20Communicate/images/untitled-diagram-1-3.jpg" style="width: 443px; height: 443px;" /></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,1,0,0],[0,0,1,0],[0,0,1,0],[0,0,0,1]]
<strong>Đầu ra:</strong> 4
<b>Giải thích:</b>&nbsp;Hai máy chủ ở hàng đầu tiên có thể liên lạc với nhau. Hai máy chủ ở cột thứ ba cũng có thể liên lạc với nhau. Máy chủ ở góc dưới bên phải không thể liên lạc với máy chủ nào khác.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m &lt;= 250</code></li>
	<li><code>1 &lt;= n &lt;= 250</code></li>
	<li><code>grid[i][j] == 0 or 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Một máy chủ liên lạc được khi có máy chủ khác cùng hàng hoặc cùng cột. Vì $m,n \le 250$, ta đếm số máy chủ trên từng hàng và cột, rồi duyệt mỗi máy chủ và tính nó nếu số máy chủ trong hàng hoặc cột đó lớn hơn $1$. Cần hai lượt duyệt và thêm $O(m+n)$ bộ nhớ.

<!-- thinking:end -->

Ta đếm số máy chủ trong từng hàng và từng cột, sau đó duyệt từng máy chủ. Nếu số máy chủ trong hàng hoặc cột của máy chủ hiện tại lớn hơn $1$, máy chủ đó thỏa mãn điều kiện và ta tăng kết quả thêm $1$.

Sau khi duyệt xong, ta trả về kết quả.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m + n)$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countServers(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        row = [0] * m
        col = [0] * n
        for i in range(m):
            for j in range(n):
                row[i] += grid[i][j]
                col[j] += grid[i][j]
        return sum(
            grid[i][j] and (row[i] > 1 or col[j] > 1)
            for i in range(m)
            for j in range(n)
        )
```

#### Java

```java
class Solution {
    public int countServers(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[] row = new int[m];
        int[] col = new int[n];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                row[i] += grid[i][j];
                col[j] += grid[i][j];
            }
        }
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 1 && (row[i] > 1 || col[j] > 1)) {
                    ++ans;
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
    int countServers(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<int> row(m), col(n);
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                row[i] += grid[i][j];
                col[j] += grid[i][j];
            }
        }
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                ans += grid[i][j] && (row[i] > 1 || col[j] > 1);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countServers(grid [][]int) (ans int) {
	m, n := len(grid), len(grid[0])
	row, col := make([]int, m), make([]int, n)
	for i := range grid {
		for j, x := range grid[i] {
			row[i] += x
			col[j] += x
		}
	}
	for i := range grid {
		for j, x := range grid[i] {
			if x == 1 && (row[i] > 1 || col[j] > 1) {
				ans++
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function countServers(grid: number[][]): number {
    const m = grid.length;
    const n = grid[0].length;
    const row = new Array(m).fill(0);
    const col = new Array(n).fill(0);
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            row[i] += grid[i][j];
            col[j] += grid[i][j];
        }
    }
    let ans = 0;
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (grid[i][j] === 1 && (row[i] > 1 || col[j] > 1)) {
                ans++;
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_servers(grid: Vec<Vec<i32>>) -> i32 {
        let m = grid.len();
        let n = grid[0].len();
        let mut row = vec![0; m];
        let mut col = vec![0; n];
        for i in 0..m {
            for j in 0..n {
                row[i] += grid[i][j];
                col[j] += grid[i][j];
            }
        }
        let mut ans = 0;
        for i in 0..m {
            for j in 0..n {
                if grid[i][j] == 1 && (row[i] > 1 || col[j] > 1) {
                    ans += 1;
                }
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
