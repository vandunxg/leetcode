---
comments: true
difficulty: Medium
rating: 1781
source: Biweekly Contest 54 Q3
tags:
    - Array
    - Matrix
    - Prefix Sum
---

<!-- problem:start -->

# [1895. Largest Magic Square](https://leetcode.com/problems/largest-magic-square)

[中文文档](/solution/1800-1899/1895.Largest%20Magic%20Square/README.md)

## Mô tả

<!-- description:start -->

<p>Một <strong>ma phương</strong> <code>k x k</code> là một lưới <code>k x k</code> được điền các số nguyên sao cho tổng mỗi hàng, tổng mỗi cột và tổng của cả hai đường chéo đều <strong>bằng nhau</strong>. Các số nguyên trong ma phương <strong>không nhất thiết phải khác nhau</strong>. Mọi lưới <code>1 x 1</code> hiển nhiên đều là một <strong>ma phương</strong>.</p>

<p>Cho một <code>grid</code> số nguyên kích thước <code>m x n</code>, hãy trả về <em><strong>kích thước</strong> (tức là độ dài cạnh </em><code>k</code><em>) của <strong>ma phương lớn nhất</strong> có thể tìm thấy trong lưới này</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1895.Largest%20Magic%20Square/images/magicsquare-grid.jpg" style="width: 413px; height: 335px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[7,1,4,5,6],[2,5,1,6,4],[1,5,4,3,2],[1,2,7,3,4]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ma phương lớn nhất có kích thước 3.
Tổng mỗi hàng, mỗi cột và mỗi đường chéo của ma phương này đều bằng 12.
- Tổng các hàng: 5+1+6 = 5+4+3 = 2+7+3 = 12
- Tổng các cột: 5+5+2 = 1+4+7 = 6+3+3 = 12
- Tổng các đường chéo: 5+4+3 = 6+4+2 = 12
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1895.Largest%20Magic%20Square/images/magicsquare2-grid.jpg" style="width: 333px; height: 255px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[5,1,3,1],[9,3,3,1],[1,3,3,8]]
<strong>Đầu ra:</strong> 2
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 50</code></li>
	<li><code>1 &lt;= grid[i][j] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Ma phương có tổng các hàng, cột và đường chéo bằng nhau. Lưới có kích thước nhiều nhất $50\times 50$, nên ta có thể thử độ dài cạnh từ lớn xuống nhỏ, nhưng tính lại tổng của từng hình vuông từ đầu sẽ lãng phí.
>
> Tổng tiền tố theo hàng và cột cho phép tính mỗi đường trong $O(k)$; hai đường chéo chỉ cần được duyệt một lần. Duyệt $k$ giảm dần giúp trả về ma phương đầu tiên tìm thấy.

<!-- thinking:end -->

Ta định nghĩa $\text{rowsum}[i][j]$ là tổng các phần tử trong hàng thứ $i$ từ đầu đến cột thứ $j$ của ma trận, và $\text{colsum}[i][j]$ là tổng các phần tử trong cột thứ $j$ từ đầu đến hàng thứ $i$. Vì vậy, với mọi ma trận con từ $(x_1, y_1)$ đến $(x_2, y_2)$, tổng hàng thứ $i$ của nó có thể biểu diễn là $\text{rowsum}[i+1][y_2+1] - \text{rowsum}[i+1][y_1]$, còn tổng cột thứ $j$ có thể biểu diễn là $\text{colsum}[x_2+1][j+1] - \text{colsum}[x_1][j+1]$.

Ta liệt kê tất cả ma trận con có thể có và kiểm tra xem chúng có phải ma phương hay không. Với mỗi ma trận con, ta tính tổng của từng hàng, từng cột và cả hai đường chéo để xác định xem tất cả có bằng nhau hay không.

Độ phức tạp thời gian là $O(m \times n \times \min(m, n)^2)$, còn độ phức tạp không gian là $O(m \times n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestMagicSquare(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        rowsum = [[0] * (n + 1) for _ in range(m + 1)]
        colsum = [[0] * (n + 1) for _ in range(m + 1)]
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                rowsum[i][j] = rowsum[i][j - 1] + grid[i - 1][j - 1]
                colsum[i][j] = colsum[i - 1][j] + grid[i - 1][j - 1]

        def check(x1, y1, x2, y2):
            val = rowsum[x1 + 1][y2 + 1] - rowsum[x1 + 1][y1]
            for i in range(x1 + 1, x2 + 1):
                if rowsum[i + 1][y2 + 1] - rowsum[i + 1][y1] != val:
                    return False
            for j in range(y1, y2 + 1):
                if colsum[x2 + 1][j + 1] - colsum[x1][j + 1] != val:
                    return False
            s, i, j = 0, x1, y1
            while i <= x2:
                s += grid[i][j]
                i += 1
                j += 1
            if s != val:
                return False
            s, i, j = 0, x1, y2
            while i <= x2:
                s += grid[i][j]
                i += 1
                j -= 1
            if s != val:
                return False
            return True

        for k in range(min(m, n), 1, -1):
            i = 0
            while i + k - 1 < m:
                j = 0
                while j + k - 1 < n:
                    i2, j2 = i + k - 1, j + k - 1
                    if check(i, j, i2, j2):
                        return k
                    j += 1
                i += 1
        return 1
```

#### Java

```java
class Solution {
    private int[][] rowsum;
    private int[][] colsum;

    public int largestMagicSquare(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        rowsum = new int[m + 1][n + 1];
        colsum = new int[m + 1][n + 1];
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                rowsum[i][j] = rowsum[i][j - 1] + grid[i - 1][j - 1];
                colsum[i][j] = colsum[i - 1][j] + grid[i - 1][j - 1];
            }
        }
        for (int k = Math.min(m, n); k > 1; --k) {
            for (int i = 0; i + k - 1 < m; ++i) {
                for (int j = 0; j + k - 1 < n; ++j) {
                    int i2 = i + k - 1, j2 = j + k - 1;
                    if (check(grid, i, j, i2, j2)) {
                        return k;
                    }
                }
            }
        }
        return 1;
    }

    private boolean check(int[][] grid, int x1, int y1, int x2, int y2) {
        int val = rowsum[x1 + 1][y2 + 1] - rowsum[x1 + 1][y1];
        for (int i = x1 + 1; i <= x2; ++i) {
            if (rowsum[i + 1][y2 + 1] - rowsum[i + 1][y1] != val) {
                return false;
            }
        }
        for (int j = y1; j <= y2; ++j) {
            if (colsum[x2 + 1][j + 1] - colsum[x1][j + 1] != val) {
                return false;
            }
        }
        int s = 0;
        for (int i = x1, j = y1; i <= x2; ++i, ++j) {
            s += grid[i][j];
        }
        if (s != val) {
            return false;
        }
        s = 0;
        for (int i = x1, j = y2; i <= x2; ++i, --j) {
            s += grid[i][j];
        }
        if (s != val) {
            return false;
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> rowsum;
    vector<vector<int>> colsum;

    int largestMagicSquare(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        rowsum.assign(m + 1, vector<int>(n + 1, 0));
        colsum.assign(m + 1, vector<int>(n + 1, 0));
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                rowsum[i][j] = rowsum[i][j - 1] + grid[i - 1][j - 1];
                colsum[i][j] = colsum[i - 1][j] + grid[i - 1][j - 1];
            }
        }
        for (int k = min(m, n); k > 1; --k) {
            for (int i = 0; i + k - 1 < m; ++i) {
                for (int j = 0; j + k - 1 < n; ++j) {
                    int i2 = i + k - 1, j2 = j + k - 1;
                    if (check(grid, i, j, i2, j2)) {
                        return k;
                    }
                }
            }
        }
        return 1;
    }

    bool check(vector<vector<int>>& grid, int x1, int y1, int x2, int y2) {
        int val = rowsum[x1 + 1][y2 + 1] - rowsum[x1 + 1][y1];
        for (int i = x1 + 1; i <= x2; ++i) {
            if (rowsum[i + 1][y2 + 1] - rowsum[i + 1][y1] != val) {
                return false;
            }
        }
        for (int j = y1; j <= y2; ++j) {
            if (colsum[x2 + 1][j + 1] - colsum[x1][j + 1] != val) {
                return false;
            }
        }
        int s = 0;
        for (int i = x1, j = y1; i <= x2; ++i, ++j) {
            s += grid[i][j];
        }
        if (s != val) {
            return false;
        }
        s = 0;
        for (int i = x1, j = y2; i <= x2; ++i, --j) {
            s += grid[i][j];
        }
        if (s != val) {
            return false;
        }
        return true;
    }
};
```

#### Go

```go
func largestMagicSquare(grid [][]int) int {
	m, n := len(grid), len(grid[0])
	rowsum := make([][]int, m+1)
	colsum := make([][]int, m+1)
	for i := 0; i <= m; i++ {
		rowsum[i] = make([]int, n+1)
		colsum[i] = make([]int, n+1)
	}
	for i := 1; i < m+1; i++ {
		for j := 1; j < n+1; j++ {
			rowsum[i][j] = rowsum[i][j-1] + grid[i-1][j-1]
			colsum[i][j] = colsum[i-1][j] + grid[i-1][j-1]
		}
	}
	for k := min(m, n); k > 1; k-- {
		for i := 0; i+k-1 < m; i++ {
			for j := 0; j+k-1 < n; j++ {
				i2, j2 := i+k-1, j+k-1
				if check(grid, rowsum, colsum, i, j, i2, j2) {
					return k
				}
			}
		}
	}
	return 1
}

func check(grid, rowsum, colsum [][]int, x1, y1, x2, y2 int) bool {
	val := rowsum[x1+1][y2+1] - rowsum[x1+1][y1]
	for i := x1 + 1; i < x2+1; i++ {
		if rowsum[i+1][y2+1]-rowsum[i+1][y1] != val {
			return false
		}
	}
	for j := y1; j < y2+1; j++ {
		if colsum[x2+1][j+1]-colsum[x1][j+1] != val {
			return false
		}
	}
	s := 0
	for i, j := x1, y1; i <= x2; i, j = i+1, j+1 {
		s += grid[i][j]
	}
	if s != val {
		return false
	}
	s = 0
	for i, j := x1, y2; i <= x2; i, j = i+1, j-1 {
		s += grid[i][j]
	}
	if s != val {
		return false
	}
	return true
}
```

#### TypeScript

```ts
function largestMagicSquare(grid: number[][]): number {
    const [m, n] = [grid.length, grid[0].length];
    const rowsum: number[][] = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));
    const colsum: number[][] = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));

    for (let i = 1; i <= m; ++i) {
        for (let j = 1; j <= n; ++j) {
            rowsum[i][j] = rowsum[i][j - 1] + grid[i - 1][j - 1];
            colsum[i][j] = colsum[i - 1][j] + grid[i - 1][j - 1];
        }
    }

    const check = (x1: number, y1: number, x2: number, y2: number): boolean => {
        const val = rowsum[x1 + 1][y2 + 1] - rowsum[x1 + 1][y1];
        for (let i = x1 + 1; i <= x2; ++i) {
            if (rowsum[i + 1][y2 + 1] - rowsum[i + 1][y1] !== val) {
                return false;
            }
        }
        for (let j = y1; j <= y2; ++j) {
            if (colsum[x2 + 1][j + 1] - colsum[x1][j + 1] !== val) {
                return false;
            }
        }
        let s = 0;
        for (let i = x1, j = y1; i <= x2; ++i, ++j) {
            s += grid[i][j];
        }
        if (s !== val) {
            return false;
        }
        s = 0;
        for (let i = x1, j = y2; i <= x2; ++i, --j) {
            s += grid[i][j];
        }
        if (s !== val) {
            return false;
        }
        return true;
    };

    for (let k = Math.min(m, n); k > 1; --k) {
        for (let i = 0; i + k - 1 < m; ++i) {
            for (let j = 0; j + k - 1 < n; ++j) {
                const i2 = i + k - 1,
                    j2 = j + k - 1;
                if (check(i, j, i2, j2)) {
                    return k;
                }
            }
        }
    }
    return 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
