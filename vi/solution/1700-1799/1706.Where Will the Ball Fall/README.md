---
comments: true
difficulty: Medium
rating: 1764
source: Weekly Contest 221 Q3
tags:
    - Array
    - Matrix
    - Simulation
---

<!-- problem:start -->

# [1706. Where Will the Ball Fall](https://leetcode.com/problems/where-will-the-ball-fall)

[中文文档](/solution/1700-1799/1706.Where%20Will%20the%20Ball%20Fall/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>grid</code> hai chiều kích thước <code>m x n</code> biểu diễn một chiếc hộp, cùng <code>n</code> quả bóng. Hộp mở ở phía trên và phía dưới.</p>

<p>Mỗi ô trong hộp có một tấm ván chéo nối hai góc ô, có thể hướng quả bóng sang phải hoặc sang trái.</p>

<ul>
	<li>Tấm ván hướng bóng sang phải nối góc trên trái với góc dưới phải và được biểu diễn bởi <code>1</code>.</li>
	<li>Tấm ván hướng bóng sang trái nối góc trên phải với góc dưới trái và được biểu diễn bởi <code>-1</code>.</li>
</ul>

<p>Ta thả một quả bóng ở phía trên mỗi cột của hộp. Mỗi quả bóng có thể bị kẹt trong hộp hoặc rơi ra phía dưới. Bóng bị kẹt nếu gặp hình chữ &quot;V&quot; tạo bởi hai tấm ván hoặc nếu một tấm ván hướng bóng vào một trong hai vách hộp.</p>

<p>Trả về <em>mảng </em><code>answer</code><em> có kích thước </em><code>n</code><em>, trong đó </em><code>answer[i]</code><em> là cột mà quả bóng rơi ra ở phía dưới sau khi được thả từ cột thứ </em><code>i<sup>th</sup></code><em> ở phía trên, hoặc là <code>-1</code><em> nếu bóng bị kẹt trong hộp</em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1700-1799/1706.Where%20Will%20the%20Ball%20Fall/images/ball.jpg" style="width: 500px; height: 385px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,1,1,-1,-1],[1,1,1,-1,-1],[-1,-1,-1,1,1],[1,1,1,1,-1],[-1,-1,-1,-1,-1]]
<strong>Đầu ra:</strong> [1,-1,-1,-1,-1]
<strong>Giải thích:</strong> Ví dụ này được minh họa trong hình.
Quả bóng b0 được thả ở cột 0 và rơi ra khỏi hộp tại cột 1.
Quả bóng b1 được thả ở cột 1 và bị kẹt giữa cột 2 và 3, hàng 1.
Quả bóng b2 được thả ở cột 2 và bị kẹt giữa cột 2 và 3, hàng 0.
Quả bóng b3 được thả ở cột 3 và bị kẹt giữa cột 2 và 3, hàng 0.
Quả bóng b4 được thả ở cột 4 và bị kẹt giữa cột 2 và 3, hàng 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[-1]]
<strong>Đầu ra:</strong> [-1]
<strong>Giải thích:</strong> Quả bóng bị kẹt ở vách trái.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,1,1,1,1,1],[-1,-1,-1,-1,-1,-1],[1,1,1,1,1,1],[-1,-1,-1,-1,-1,-1]]
<strong>Đầu ra:</strong> [0,1,2,3,4,-1]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 100</code></li>
	<li><code>grid[i][j]</code> là <code>1</code> hoặc <code>-1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích trường hợp + DFS

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi ô trên cùng được thả một quả bóng và các tấm ván hướng bóng sang trái hoặc phải. Kích thước grid đủ nhỏ để mô phỏng từng quả bóng độc lập.
>
> Bóng bị kẹt trong bốn trường hợp: bóng ở biên và tấm ván đẩy nó ra ngoài, hoặc hai tấm ván kề nhau tạo thành $V$. Nếu không, bóng trượt chéo xuống hàng tiếp theo.
>
> Gọi $\textit{dfs}(i,j)$ là cột thoát khi bóng ở $(i,j)$: trả về $-1$ nếu bị kẹt, ngược lại đệ quy đến $(i+1,j\pm 1)$. Khi đến hàng $m$, trả về cột hiện tại.

<!-- thinking:end -->

Ta có thể dùng DFS để mô phỏng chuyển động của bóng. Xây dựng hàm $\textit{dfs}(i, j)$ biểu diễn cột mà bóng rơi ra khi bắt đầu từ hàng $i$, cột $j$. Bóng sẽ bị kẹt trong các trường hợp sau:

1. Bóng ở cột ngoài cùng bên trái và đường chéo của ô hướng bóng sang trái.
2. Bóng ở cột ngoài cùng bên phải và đường chéo của ô hướng bóng sang phải.
3. Đường chéo của ô hướng bóng sang phải, còn ô kề bên phải hướng bóng sang trái.
4. Đường chéo của ô hướng bóng sang trái, còn ô kề bên trái hướng bóng sang phải.

Nếu một trong các điều kiện trên đúng, bóng bị kẹt và ta trả về $-1$. Ngược lại, ta tiếp tục đệ quy để tìm vị trí tiếp theo của bóng. Cuối cùng, nếu bóng đến hàng cuối, ta trả về chỉ số cột hiện tại.

Độ phức tạp thời gian là $O(m \times n)$, còn độ phức tạp không gian là $O(m)$. Ở đây, $m$ và $n$ lần lượt là số hàng và số cột của grid.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findBall(self, grid: List[List[int]]) -> List[int]:
        def dfs(i: int, j: int) -> int:
            if i == m:
                return j
            if j == 0 and grid[i][j] == -1:
                return -1
            if j == n - 1 and grid[i][j] == 1:
                return -1
            if grid[i][j] == 1 and grid[i][j + 1] == -1:
                return -1
            if grid[i][j] == -1 and grid[i][j - 1] == 1:
                return -1
            return dfs(i + 1, j + 1) if grid[i][j] == 1 else dfs(i + 1, j - 1)

        m, n = len(grid), len(grid[0])
        return [dfs(0, j) for j in range(n)]
```

#### Java

```java
class Solution {
    private int m;
    private int n;
    private int[][] grid;

    public int[] findBall(int[][] grid) {
        m = grid.length;
        n = grid[0].length;
        this.grid = grid;
        int[] ans = new int[n];
        for (int j = 0; j < n; ++j) {
            ans[j] = dfs(0, j);
        }
        return ans;
    }

    private int dfs(int i, int j) {
        if (i == m) {
            return j;
        }
        if (j == 0 && grid[i][j] == -1) {
            return -1;
        }
        if (j == n - 1 && grid[i][j] == 1) {
            return -1;
        }
        if (grid[i][j] == 1 && grid[i][j + 1] == -1) {
            return -1;
        }
        if (grid[i][j] == -1 && grid[i][j - 1] == 1) {
            return -1;
        }
        return grid[i][j] == 1 ? dfs(i + 1, j + 1) : dfs(i + 1, j - 1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findBall(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<int> ans(n);
        function<int(int, int)> dfs = [&](int i, int j) {
            if (i == m) {
                return j;
            }
            if (j == 0 && grid[i][j] == -1) {
                return -1;
            }
            if (j == n - 1 && grid[i][j] == 1) {
                return -1;
            }
            if (grid[i][j] == 1 && grid[i][j + 1] == -1) {
                return -1;
            }
            if (grid[i][j] == -1 && grid[i][j - 1] == 1) {
                return -1;
            }
            return grid[i][j] == 1 ? dfs(i + 1, j + 1) : dfs(i + 1, j - 1);
        };
        for (int j = 0; j < n; ++j) {
            ans[j] = dfs(0, j);
        }
        return ans;
    }
};
```

#### Go

```go
func findBall(grid [][]int) (ans []int) {
	m, n := len(grid), len(grid[0])
	var dfs func(i, j int) int
	dfs = func(i, j int) int {
		if i == m {
			return j
		}
		if j == 0 && grid[i][j] == -1 {
			return -1
		}
		if j == n-1 && grid[i][j] == 1 {
			return -1
		}
		if grid[i][j] == 1 && grid[i][j+1] == -1 {
			return -1
		}
		if grid[i][j] == -1 && grid[i][j-1] == 1 {
			return -1
		}
		if grid[i][j] == 1 {
			return dfs(i+1, j+1)
		}
		return dfs(i+1, j-1)
	}
	for j := 0; j < n; j++ {
		ans = append(ans, dfs(0, j))
	}
	return
}
```

#### TypeScript

```ts
function findBall(grid: number[][]): number[] {
    const m = grid.length;
    const n = grid[0].length;
    const dfs = (i: number, j: number) => {
        if (i === m) {
            return j;
        }
        if (grid[i][j] === 1) {
            if (j === n - 1 || grid[i][j + 1] === -1) {
                return -1;
            }
            return dfs(i + 1, j + 1);
        } else {
            if (j === 0 || grid[i][j - 1] === 1) {
                return -1;
            }
            return dfs(i + 1, j - 1);
        }
    };
    return Array.from({ length: n }, (_, j) => dfs(0, j));
}
```

#### Rust

```rust
impl Solution {
    fn dfs(grid: &Vec<Vec<i32>>, i: usize, j: usize) -> i32 {
        if i == grid.len() {
            return j as i32;
        }
        if grid[i][j] == 1 {
            if j == grid[0].len() - 1 || grid[i][j + 1] == -1 {
                return -1;
            }
            Self::dfs(grid, i + 1, j + 1)
        } else {
            if j == 0 || grid[i][j - 1] == 1 {
                return -1;
            }
            Self::dfs(grid, i + 1, j - 1)
        }
    }

    pub fn find_ball(grid: Vec<Vec<i32>>) -> Vec<i32> {
        let m = grid.len();
        let n = grid[0].len();
        let mut ans = vec![0; n];
        for i in 0..n {
            ans[i] = Self::dfs(&grid, 0, i);
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[][]} grid
 * @return {number[]}
 */
var findBall = function (grid) {
    const m = grid.length;
    const n = grid[0].length;
    const dfs = (i, j) => {
        if (i === m) {
            return j;
        }
        if (grid[i][j] === 1) {
            if (j === n - 1 || grid[i][j + 1] === -1) {
                return -1;
            }
            return dfs(i + 1, j + 1);
        } else {
            if (j === 0 || grid[i][j - 1] === 1) {
                return -1;
            }
            return dfs(i + 1, j - 1);
        }
    };
    return Array.from({ length: n }, (_, j) => dfs(0, j));
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
