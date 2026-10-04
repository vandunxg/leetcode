---
comments: true
difficulty: Easy
rating: 1334
source: Weekly Contest 409 Q1
tags:
    - Design
    - Array
    - Hash Table
    - Matrix
    - Simulation
---

<!-- problem:start -->

# [3242. Design Neighbor Sum Service](https://leetcode.com/problems/design-neighbor-sum-service)

[中文文档](/solution/3200-3299/3242.Design%20Neighbor%20Sum%20Service/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng 2 chiều <code>grid</code> kích thước <code>n x n</code>, chứa các phần tử <strong>phân biệt</strong> trong phạm vi <code>[0, n<sup>2</sup> - 1]</code>.</p>

<p>Cài đặt lớp <code>NeighborSum</code>:</p>

<ul>
    <li><code>NeighborSum(int [][]grid)</code> khởi tạo đối tượng.</li>
    <li><code>int adjacentSum(int value)</code> trả về <strong>tổng</strong> các phần tử là hàng xóm kề của <code>value</code>, tức là nằm phía trên, bên trái, bên phải hoặc bên dưới <code>value</code> trong <code>grid</code>.</li>
    <li><code>int diagonalSum(int value)</code> trả về <strong>tổng</strong> các phần tử là hàng xóm đường chéo của <code>value</code>, tức là nằm ở phía trên bên trái, phía trên bên phải, phía dưới bên trái hoặc phía dưới bên phải <code>value</code> trong <code>grid</code>.</li>
</ul>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3242.Design%20Neighbor%20Sum%20Service/images/design.png" style="width: 400px; height: 248px;" /></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>[&quot;NeighborSum&quot;, &quot;adjacentSum&quot;, &quot;adjacentSum&quot;, &quot;diagonalSum&quot;, &quot;diagonalSum&quot;]</p>

<p>[[[[0, 1, 2], [3, 4, 5], [6, 7, 8]]], [1], [4], [4], [8]]</p>

<p><strong>Đầu ra:</strong> [null, 6, 16, 16, 4]</p>

<p><strong>Giải thích:</strong></p>

<p><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3242.Design%20Neighbor%20Sum%20Service/images/designexample0.png" style="width: 250px; height: 249px;" /></strong></p>

<ul>
    <li>Các hàng xóm kề của 1 là 0, 2 và 4.</li>
    <li>Các hàng xóm kề của 4 là 1, 3, 5 và 7.</li>
    <li>Các hàng xóm đường chéo của 4 là 0, 2, 6 và 8.</li>
    <li>Hàng xóm đường chéo của 8 là 4.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>[&quot;NeighborSum&quot;, &quot;adjacentSum&quot;, &quot;diagonalSum&quot;]</p>

<p>[[[[1, 2, 0, 3], [4, 7, 15, 6], [8, 9, 10, 11], [12, 13, 14, 5]]], [15], [9]]</p>

<p><strong>Đầu ra:</strong> [null, 23, 45]</p>

<p><strong>Giải thích:</strong></p>

<p><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3242.Design%20Neighbor%20Sum%20Service/images/designexample2.png" style="width: 300px; height: 300px;" /></strong></p>

<ul>
    <li>Các hàng xóm kề của 15 là 0, 10, 7 và 6.</li>
    <li>Các hàng xóm đường chéo của 9 là 4, 12, 14 và 15.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>3 &lt;= n == grid.length == grid[0].length &lt;= 10</code></li>
    <li><code>0 &lt;= grid[i][j] &lt;= n<sup>2</sup> - 1</code></li>
    <li>Tất cả <code>grid[i][j]</code> đều phân biệt.</li>
    <li><code>value</code> trong <code>adjacentSum</code> và <code>diagonalSum</code> sẽ nằm trong phạm vi <code>[0, n<sup>2</sup> - 1]</code>.</li>
    <li>Số lần gọi <code>adjacentSum</code> và <code>diagonalSum</code> nhiều nhất là <code>2 * n<sup>2</sup></code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Vì grid có kích thước tối đa $10\times 10$ và số truy vấn ít, ta có thể duyệt bốn hàng xóm, nhưng việc tìm ô chứa $\textit{value}$ trong mỗi lần truy vấn vẫn mất thời gian tuyến tính.
>
> Xây dựng một map từ giá trị đến tọa độ khi khởi tạo. Tổng của các hàng xóm kề và hàng xóm đường chéo dùng chung một hàm phụ, chỉ khác nhau ở tập offset. Sau khi tra cứu, mỗi truy vấn có độ phức tạp $O(1)$ vì chỉ cần tính tổng nhiều nhất bốn hàng xóm.

<!-- thinking:end -->

Ta có thể dùng một hash table $\textit{d}$ để lưu tọa độ của mỗi phần tử. Sau đó, theo mô tả bài toán, ta lần lượt tính tổng các phần tử kề và các phần tử kề theo đường chéo.

Về độ phức tạp thời gian, việc khởi tạo hash table có độ phức tạp $O(m \times n)$, còn việc tính tổng các phần tử kề và các phần tử kề theo đường chéo có độ phức tạp $O(1)$. Độ phức tạp không gian là $O(m \times n)$.

<!-- tabs:start -->

#### Python3

```python
class NeighborSum:

    def __init__(self, grid: List[List[int]]):
        self.grid = grid
        self.d = {}
        self.dirs = ((-1, 0, 1, 0, -1), (-1, 1, 1, -1, -1))
        for i, row in enumerate(grid):
            for j, x in enumerate(row):
                self.d[x] = (i, j)

    def adjacentSum(self, value: int) -> int:
        return self.cal(value, 0)

    def cal(self, value: int, k: int):
        i, j = self.d[value]
        s = 0
        for a, b in pairwise(self.dirs[k]):
            x, y = i + a, j + b
            if 0 <= x < len(self.grid) and 0 <= y < len(self.grid[0]):
                s += self.grid[x][y]
        return s

    def diagonalSum(self, value: int) -> int:
        return self.cal(value, 1)


# Your NeighborSum object will be instantiated and called as such:
# obj = NeighborSum(grid)
# param_1 = obj.adjacentSum(value)
# param_2 = obj.diagonalSum(value)
```

#### Java

```java
class NeighborSum {
    private int[][] grid;
    private final Map<Integer, int[]> d = new HashMap<>();
    private final int[][] dirs = {{-1, 0, 1, 0, -1}, {-1, 1, 1, -1, -1}};

    public NeighborSum(int[][] grid) {
        this.grid = grid;
        int m = grid.length, n = grid[0].length;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                d.put(grid[i][j], new int[] {i, j});
            }
        }
    }

    public int adjacentSum(int value) {
        return cal(value, 0);
    }

    public int diagonalSum(int value) {
        return cal(value, 1);
    }

    private int cal(int value, int k) {
        int[] p = d.get(value);
        int s = 0;
        for (int q = 0; q < 4; ++q) {
            int x = p[0] + dirs[k][q], y = p[1] + dirs[k][q + 1];
            if (x >= 0 && x < grid.length && y >= 0 && y < grid[0].length) {
                s += grid[x][y];
            }
        }
        return s;
    }
}

/**
 * Your NeighborSum object will be instantiated and called as such:
 * NeighborSum obj = new NeighborSum(grid);
 * int param_1 = obj.adjacentSum(value);
 * int param_2 = obj.diagonalSum(value);
 */
```

#### C++

```cpp
class NeighborSum {
public:
    NeighborSum(vector<vector<int>>& grid) {
        this->grid = grid;
        int m = grid.size(), n = grid[0].size();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                d[grid[i][j]] = {i, j};
            }
        }
    }

    int adjacentSum(int value) {
        return cal(value, 0);
    }

    int diagonalSum(int value) {
        return cal(value, 1);
    }

private:
    vector<vector<int>> grid;
    unordered_map<int, pair<int, int>> d;
    int dirs[2][5] = {{-1, 0, 1, 0, -1}, {-1, 1, 1, -1, -1}};

    int cal(int value, int k) {
        auto [i, j] = d[value];
        int s = 0;
        for (int q = 0; q < 4; ++q) {
            int x = i + dirs[k][q], y = j + dirs[k][q + 1];
            if (x >= 0 && x < grid.size() && y >= 0 && y < grid[0].size()) {
                s += grid[x][y];
            }
        }
        return s;
    }
};

/**
 * Your NeighborSum object will be instantiated and called as such:
 * NeighborSum* obj = new NeighborSum(grid);
 * int param_1 = obj->adjacentSum(value);
 * int param_2 = obj->diagonalSum(value);
 */
```

#### Go

```go
type NeighborSum struct {
    grid [][]int
    d    map[int][2]int
    dirs [2][5]int
}

func Constructor(grid [][]int) NeighborSum {
    d := map[int][2]int{}
    for i, row := range grid {
        for j, x := range row {
            d[x] = [2]int{i, j}
        }
    }
    dirs := [2][5]int{{-1, 0, 1, 0, -1}, {-1, 1, 1, -1, -1}}
    return NeighborSum{grid, d, dirs}
}

func (this *NeighborSum) AdjacentSum(value int) int {
    return this.cal(value, 0)
}

func (this *NeighborSum) DiagonalSum(value int) int {
    return this.cal(value, 1)
}

func (this *NeighborSum) cal(value, k int) int {
    p := this.d[value]
    s := 0
    for q := 0; q < 4; q++ {
        x, y := p[0]+this.dirs[k][q], p[1]+this.dirs[k][q+1]
        if x >= 0 && x < len(this.grid) && y >= 0 && y < len(this.grid[0]) {
            s += this.grid[x][y]
        }
    }
    return s
}

/**
 * Your NeighborSum object will be instantiated and called as such:
 * obj := Constructor(grid);
 * param_1 := obj.AdjacentSum(value);
 * param_2 := obj.DiagonalSum(value);
 */
```

#### TypeScript

```ts
class NeighborSum {
    private grid: number[][];
    private d: Map<number, [number, number]> = new Map();
    private dirs: number[][] = [
        [-1, 0, 1, 0, -1],
        [-1, 1, 1, -1, -1],
    ];
    constructor(grid: number[][]) {
        for (let i = 0; i < grid.length; ++i) {
            for (let j = 0; j < grid[0].length; ++j) {
                this.d.set(grid[i][j], [i, j]);
            }
        }
        this.grid = grid;
    }

    adjacentSum(value: number): number {
        return this.cal(value, 0);
    }

    diagonalSum(value: number): number {
        return this.cal(value, 1);
    }

    cal(value: number, k: number): number {
        const [i, j] = this.d.get(value)!;
        let s = 0;
        for (let q = 0; q < 4; ++q) {
            const [x, y] = [i + this.dirs[k][q], j + this.dirs[k][q + 1]];
            if (x >= 0 && x < this.grid.length && y >= 0 && y < this.grid[0].length) {
                s += this.grid[x][y];
            }
        }
        return s;
    }
}

/**
 * Your NeighborSum object will be instantiated and called as such:
 * var obj = new NeighborSum(grid)
 * var param_1 = obj.adjacentSum(value)
 * var param_2 = obj.diagonalSum(value)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
