---
comments: true
difficulty: Medium
rating: 1411
source: Weekly Contest 449 Q2
tags:
    - Array
    - Enumeration
    - Matrix
    - Prefix Sum
---

<!-- problem:start -->

# [3546. Equal Sum Grid Partition I](https://leetcode.com/problems/equal-sum-grid-partition-i)

[中文文档](/solution/3500-3599/3546.Equal%20Sum%20Grid%20Partition%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận <code>m x n</code> <code>grid</code> gồm các số nguyên dương. Nhiệm vụ của bạn là xác định xem có thể thực hiện <strong>một đường cắt ngang hoặc một đường cắt dọc</strong> trên ma trận sao cho:</p>

<ul>
    <li>Mỗi trong hai phần tạo thành sau khi cắt đều <strong>không rỗng</strong>.</li>
    <li>Tổng các phần tử trong hai phần <strong>bằng nhau</strong>.</li>
</ul>

<p>Trả về <code>true</code> nếu tồn tại cách phân hoạch như vậy; nếu không, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,4],[2,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3546.Equal%20Sum%20Grid%20Partition%20I/images/lc.png" style="width: 200px;" /><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3546.Equal%20Sum%20Grid%20Partition%20I/images/lc.jpeg" style="width: 200px; height: 200px;" /></p>

<p>Một đường cắt ngang giữa hàng 0 và hàng 1 tạo ra hai phần không rỗng, mỗi phần có tổng bằng 5. Do đó, đáp án là <code>true</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,3],[2,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có đường cắt ngang hoặc dọc nào tạo ra hai phần không rỗng có tổng bằng nhau. Do đó, đáp án là <code>false</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= m == grid.length &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= n == grid[i].length &lt;= 10<sup>5</sup></code></li>
    <li><code>2 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= grid[i][j] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê + Prefix Sum

<!-- thinking:start -->

> **Tư duy**
>
> Một đường cắt ngang hoặc dọc duy nhất phải chia ma trận thành hai khối có tổng bằng nhau. Nếu tổng toàn bộ là số lẻ thì điều này là không thể.
>
> Duyệt tổng tiền tố theo hàng; một đường cắt sau một hàng là hợp lệ khi tổng tiền tố bằng một nửa tổng toàn bộ và hàng đó không phải hàng cuối. Sau đó lặp lại với các cột. Chỉ cần một biến lưu tổng đang chạy, không cần ma trận tổng tiền tố đầy đủ.

<!-- thinking:end -->

Trước tiên, ta tính tổng của tất cả phần tử trong ma trận, ký hiệu là $s$. Nếu $s$ là số lẻ, không thể chia ma trận thành hai phần có tổng bằng nhau, nên ta trả về `false` ngay.

Nếu $s$ là số chẵn, ta có thể liệt kê tất cả các đường phân hoạch có thể để kiểm tra xem có đường nào chia ma trận thành hai phần có tổng bằng nhau hay không.

Ta duyệt từng hàng từ trên xuống dưới, tính tổng tất cả phần tử trong các hàng phía trên hàng hiện tại, ký hiệu là $\textit{pre}$. Nếu $\textit{pre} \times 2 = s$ và hàng hiện tại không phải hàng cuối, nghĩa là ta có thể thực hiện phân hoạch ngang giữa hàng hiện tại và hàng tiếp theo, nên trả về `true`.

Nếu không tìm thấy đường phân hoạch như vậy, ta duyệt từng cột từ trái sang phải, tính tổng tất cả phần tử trong các cột bên trái cột hiện tại, ký hiệu là $\textit{pre}$. Nếu $\textit{pre} \times 2 = s$ và cột hiện tại không phải cột cuối, nghĩa là ta có thể thực hiện phân hoạch dọc giữa cột hiện tại và cột tiếp theo, nên trả về `true`.

Nếu không tìm thấy đường phân hoạch nào, ta trả về `false`.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận. Độ phức tạp không gian là $O(1)$ vì chỉ sử dụng thêm không gian hằng số.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canPartitionGrid(self, grid: List[List[int]]) -> bool:
        s = sum(sum(row) for row in grid)
        if s % 2:
            return False
        pre = 0
        for i, row in enumerate(grid):
            pre += sum(row)
            if pre * 2 == s and i != len(grid) - 1:
                return True
        pre = 0
        for j, col in enumerate(zip(*grid)):
            pre += sum(col)
            if pre * 2 == s and j != len(grid[0]) - 1:
                return True
        return False
```

#### Java

```java
class Solution {
    public boolean canPartitionGrid(int[][] grid) {
        long s = 0;
        for (var row : grid) {
            for (int x : row) {
                s += x;
            }
        }
        if (s % 2 != 0) {
            return false;
        }
        int m = grid.length, n = grid[0].length;
        long pre = 0;
        for (int i = 0; i < m; ++i) {
            for (int x : grid[i]) {
                pre += x;
            }
            if (pre * 2 == s && i < m - 1) {
                return true;
            }
        }
        pre = 0;
        for (int j = 0; j < n; ++j) {
            for (int i = 0; i < m; ++i) {
                pre += grid[i][j];
            }
            if (pre * 2 == s && j < n - 1) {
                return true;
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
    bool canPartitionGrid(vector<vector<int>>& grid) {
        long long s = 0;
        for (const auto& row : grid) {
            for (int x : row) {
                s += x;
            }
        }
        if (s % 2 != 0) {
            return false;
        }
        int m = grid.size(), n = grid[0].size();
        long long pre = 0;
        for (int i = 0; i < m; ++i) {
            for (int x : grid[i]) {
                pre += x;
            }
            if (pre * 2 == s && i + 1 < m) {
                return true;
            }
        }
        pre = 0;
        for (int j = 0; j < n; ++j) {
            for (int i = 0; i < m; ++i) {
                pre += grid[i][j];
            }
            if (pre * 2 == s && j + 1 < n) {
                return true;
            }
        }
        return false;
    }
};
```

#### Go

```go
func canPartitionGrid(grid [][]int) bool {
    s := 0
    for _, row := range grid {
        for _, x := range row {
            s += x
        }
    }
    if s%2 != 0 {
        return false
    }
    m, n := len(grid), len(grid[0])
    pre := 0
    for i, row := range grid {
        for _, x := range row {
            pre += x
        }
        if pre*2 == s && i+1 < m {
            return true
        }
    }
    pre = 0
    for j := 0; j < n; j++ {
        for i := 0; i < m; i++ {
            pre += grid[i][j]
        }
        if pre*2 == s && j+1 < n {
            return true
        }
    }
    return false
}
```

#### TypeScript

```ts
function canPartitionGrid(grid: number[][]): boolean {
    let s = 0;
    for (const row of grid) {
        s += row.reduce((a, b) => a + b, 0);
    }
    if (s % 2 !== 0) {
        return false;
    }
    const [m, n] = [grid.length, grid[0].length];
    let pre = 0;
    for (let i = 0; i < m; ++i) {
        pre += grid[i].reduce((a, b) => a + b, 0);
        if (pre * 2 === s && i + 1 < m) {
            return true;
        }
    }
    pre = 0;
    for (let j = 0; j < n; ++j) {
        for (let i = 0; i < m; ++i) {
            pre += grid[i][j];
        }
        if (pre * 2 === s && j + 1 < n) {
            return true;
        }
    }
    return false;
}
```

#### Rust

```rust
impl Solution {
    pub fn can_partition_grid(grid: Vec<Vec<i32>>) -> bool {
        let mut s: i64 = 0;
        for row in &grid {
            for &x in row {
                s += x as i64;
            }
        }

        if s % 2 != 0 {
            return false;
        }

        let m = grid.len();
        let n = grid[0].len();

        let mut pre: i64 = 0;
        for i in 0..m {
            for &x in &grid[i] {
                pre += x as i64;
            }
            if pre * 2 == s && i + 1 < m {
                return true;
            }
        }

        pre = 0;
        for j in 0..n {
            for i in 0..m {
                pre += grid[i][j] as i64;
            }
            if pre * 2 == s && j + 1 < n {
                return true;
            }
        }

        false
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
