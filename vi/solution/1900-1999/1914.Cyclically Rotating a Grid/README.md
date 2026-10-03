---
comments: true
difficulty: Medium
rating: 1766
source: Weekly Contest 247 Q2
tags:
    - Array
    - Matrix
    - Simulation
---

<!-- problem:start -->

# [1914. Cyclically Rotating a Grid](https://leetcode.com/problems/cyclically-rotating-a-grid)

[中文文档](/solution/1900-1999/1914.Cyclically%20Rotating%20a%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận số nguyên <code>m x n</code> <code>grid</code>​​​, trong đó <code>m</code> và <code>n</code> đều là số nguyên <strong>chẵn</strong>, cùng một số nguyên <code>k</code>.</p>

<p>Ma trận được tạo thành từ nhiều lớp, như hình bên dưới, trong đó mỗi màu là một lớp riêng:</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1914.Cyclically%20Rotating%20a%20Grid/images/ringofgrid.png" style="width: 231px; height: 258px;" /></p>

<p>Phép xoay tuần hoàn ma trận được thực hiện bằng cách xoay tuần hoàn <strong>từng lớp</strong> trong ma trận. Để xoay tuần hoàn một lớp một lần, mỗi phần tử sẽ nhận vị trí của phần tử kề bên theo hướng <strong>ngược chiều kim đồng hồ</strong>. Ví dụ về một lần xoay được minh họa bên dưới:</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1914.Cyclically%20Rotating%20a%20Grid/images/explanation_grid.jpg" style="width: 500px; height: 268px;" />
<p>Trả về <em>ma trận sau khi áp dụng </em><code>k</code> <em>lần xoay tuần hoàn</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1914.Cyclically%20Rotating%20a%20Grid/images/rod2.png" style="width: 421px; height: 191px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[40,10],[30,20]], k = 1
<strong>Đầu ra:</strong> [[10,20],[40,30]]
<strong>Giải thích:</strong> Các hình trên minh họa trạng thái của ma trận ở mỗi bước.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1914.Cyclically%20Rotating%20a%20Grid/images/ringofgrid5.png" style="width: 231px; height: 262px;" /></strong> <strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1914.Cyclically%20Rotating%20a%20Grid/images/ringofgrid6.png" style="width: 231px; height: 262px;" /></strong> <strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1914.Cyclically%20Rotating%20a%20Grid/images/ringofgrid7.png" style="width: 231px; height: 262px;" /></strong>

<pre>
<strong>Đầu vào:</strong> grid = [[1,2,3,4],[5,6,7,8],[9,10,11,12],[13,14,15,16]], k = 2
<strong>Đầu ra:</strong> [[3,4,8,12],[2,11,10,16],[1,7,6,15],[5,9,13,14]]
<strong>Giải thích:</strong> Các hình trên minh họa trạng thái của ma trận ở mỗi bước.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>2 &lt;= m, n &lt;= 50</code></li>
	<li><code>m</code> và <code>n</code> đều là số nguyên <strong>chẵn</strong>.</li>
	<li><code>1 &lt;= grid[i][j] &lt;=<sup> </sup>5000</code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng từng lớp

<!-- thinking:start -->

> **Tư duy**
>
> Các lớp là những chu kỳ rời nhau và $k$ có thể lớn hơn độ dài của một chu kỳ, vì vậy việc duyệt từng ô một sẽ gây lãng phí. Ta trải phẳng từng lớp theo chiều kim đồng hồ, lấy $k$ theo modulo độ dài của lớp, rồi ghi lại các phần tử.
>
> Ta thu thập các phần tử ở cạnh trên, phải, dưới và trái theo thứ tự đó, rồi khôi phục theo cùng thứ tự. Các lớp độc lập với nhau nên tổng thời gian là tuyến tính theo kích thước ma trận.

<!-- thinking:end -->

Đầu tiên, ta tính số lớp trong ma trận, ký hiệu là $p$, sau đó mô phỏng phép xoay tuần hoàn từng lớp từ ngoài vào trong.

Với mỗi lớp, ta duyệt theo chiều kim đồng hồ và lần lượt thêm các phần tử ở cạnh trên, phải, dưới và trái vào một mảng $nums$. Gọi độ dài của $nums$ là $l$. Tiếp theo, ta lấy $k \bmod l$. Sau đó, bắt đầu từ chỉ số $k$ trong mảng, ta ghi các phần tử trở lại ma trận theo thứ tự cạnh trên, phải, dưới và trái.

Độ phức tạp thời gian là $O(m \times n)$, còn độ phức tạp không gian là $O(m + n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def rotateGrid(self, grid: List[List[int]], k: int) -> List[List[int]]:
        def rotate(p: int, k: int):
            nums = []
            for j in range(p, n - p - 1):
                nums.append(grid[p][j])
            for i in range(p, m - p - 1):
                nums.append(grid[i][n - p - 1])
            for j in range(n - p - 1, p, -1):
                nums.append(grid[m - p - 1][j])
            for i in range(m - p - 1, p, -1):
                nums.append(grid[i][p])
            k %= len(nums)
            if k == 0:
                return
            nums = nums[k:] + nums[:k]
            k = 0
            for j in range(p, n - p - 1):
                grid[p][j] = nums[k]
                k += 1
            for i in range(p, m - p - 1):
                grid[i][n - p - 1] = nums[k]
                k += 1
            for j in range(n - p - 1, p, -1):
                grid[m - p - 1][j] = nums[k]
                k += 1
            for i in range(m - p - 1, p, -1):
                grid[i][p] = nums[k]
                k += 1

        m, n = len(grid), len(grid[0])
        for p in range(min(m, n) >> 1):
            rotate(p, k)
        return grid
```

#### Java

```java
class Solution {
    private int m;
    private int n;
    private int[][] grid;

    public int[][] rotateGrid(int[][] grid, int k) {
        m = grid.length;
        n = grid[0].length;
        this.grid = grid;
        for (int p = 0; p < Math.min(m, n) / 2; ++p) {
            rotate(p, k);
        }
        return grid;
    }

    private void rotate(int p, int k) {
        List<Integer> nums = new ArrayList<>();
        for (int j = p; j < n - p - 1; ++j) {
            nums.add(grid[p][j]);
        }
        for (int i = p; i < m - p - 1; ++i) {
            nums.add(grid[i][n - p - 1]);
        }
        for (int j = n - p - 1; j > p; --j) {
            nums.add(grid[m - p - 1][j]);
        }
        for (int i = m - p - 1; i > p; --i) {
            nums.add(grid[i][p]);
        }
        int l = nums.size();
        k %= l;
        if (k == 0) {
            return;
        }
        for (int j = p; j < n - p - 1; ++j) {
            grid[p][j] = nums.get(k++ % l);
        }
        for (int i = p; i < m - p - 1; ++i) {
            grid[i][n - p - 1] = nums.get(k++ % l);
        }
        for (int j = n - p - 1; j > p; --j) {
            grid[m - p - 1][j] = nums.get(k++ % l);
        }
        for (int i = m - p - 1; i > p; --i) {
            grid[i][p] = nums.get(k++ % l);
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> rotateGrid(vector<vector<int>>& grid, int k) {
        int m = grid.size(), n = grid[0].size();
        auto rotate = [&](int p, int k) {
            vector<int> nums;
            for (int j = p; j < n - p - 1; ++j) {
                nums.push_back(grid[p][j]);
            }
            for (int i = p; i < m - p - 1; ++i) {
                nums.push_back(grid[i][n - p - 1]);
            }
            for (int j = n - p - 1; j > p; --j) {
                nums.push_back(grid[m - p - 1][j]);
            }
            for (int i = m - p - 1; i > p; --i) {
                nums.push_back(grid[i][p]);
            }
            int l = nums.size();
            k %= l;
            if (k == 0) {
                return;
            }
            for (int j = p; j < n - p - 1; ++j) {
                grid[p][j] = nums[k++ % l];
            }
            for (int i = p; i < m - p - 1; ++i) {
                grid[i][n - p - 1] = nums[k++ % l];
            }
            for (int j = n - p - 1; j > p; --j) {
                grid[m - p - 1][j] = nums[k++ % l];
            }
            for (int i = m - p - 1; i > p; --i) {
                grid[i][p] = nums[k++ % l];
            }
        };
        for (int p = 0; p < min(m, n) / 2; ++p) {
            rotate(p, k);
        }
        return grid;
    }
};
```

#### Go

```go
func rotateGrid(grid [][]int, k int) [][]int {
	m, n := len(grid), len(grid[0])

	rotate := func(p, k int) {
		nums := []int{}
		for j := p; j < n-p-1; j++ {
			nums = append(nums, grid[p][j])
		}
		for i := p; i < m-p-1; i++ {
			nums = append(nums, grid[i][n-p-1])
		}
		for j := n - p - 1; j > p; j-- {
			nums = append(nums, grid[m-p-1][j])
		}
		for i := m - p - 1; i > p; i-- {
			nums = append(nums, grid[i][p])
		}
		l := len(nums)
		k %= l
		if k == 0 {
			return
		}
		for j := p; j < n-p-1; j++ {
			grid[p][j] = nums[k]
			k = (k + 1) % l
		}
		for i := p; i < m-p-1; i++ {
			grid[i][n-p-1] = nums[k]
			k = (k + 1) % l
		}
		for j := n - p - 1; j > p; j-- {
			grid[m-p-1][j] = nums[k]
			k = (k + 1) % l
		}
		for i := m - p - 1; i > p; i-- {
			grid[i][p] = nums[k]
			k = (k + 1) % l
		}
	}

	for i := 0; i < m/2 && i < n/2; i++ {
		rotate(i, k)
	}
	return grid
}
```

#### TypeScript

```ts
function rotateGrid(grid: number[][], k: number): number[][] {
    const m = grid.length;
    const n = grid[0].length;
    const rotate = (p: number, k: number) => {
        const nums: number[] = [];
        for (let j = p; j < n - p - 1; ++j) {
            nums.push(grid[p][j]);
        }
        for (let i = p; i < m - p - 1; ++i) {
            nums.push(grid[i][n - p - 1]);
        }
        for (let j = n - p - 1; j > p; --j) {
            nums.push(grid[m - p - 1][j]);
        }
        for (let i = m - p - 1; i > p; --i) {
            nums.push(grid[i][p]);
        }
        const l = nums.length;
        k %= l;
        if (k === 0) {
            return;
        }
        for (let j = p; j < n - p - 1; ++j) {
            grid[p][j] = nums[k++ % l];
        }
        for (let i = p; i < m - p - 1; ++i) {
            grid[i][n - p - 1] = nums[k++ % l];
        }
        for (let j = n - p - 1; j > p; --j) {
            grid[m - p - 1][j] = nums[k++ % l];
        }
        for (let i = m - p - 1; i > p; --i) {
            grid[i][p] = nums[k++ % l];
        }
    };
    for (let p = 0; p < Math.min(m, n) >> 1; ++p) {
        rotate(p, k);
    }
    return grid;
}
```

#### Rust

```rust
impl Solution {
    pub fn rotate_grid(grid: Vec<Vec<i32>>, k: i32) -> Vec<Vec<i32>> {
        let mut grid = grid;
        let m = grid.len();
        let n = grid[0].len();

        let mut rotate = |p: usize, mut k: usize| {
            let mut nums = Vec::new();
            for j in p..n - p - 1 {
                nums.push(grid[p][j]);
            }
            for i in p..m - p - 1 {
                nums.push(grid[i][n - p - 1]);
            }
            for j in (p + 1..n - p).rev() {
                nums.push(grid[m - p - 1][j]);
            }
            for i in (p + 1..m - p).rev() {
                nums.push(grid[i][p]);
            }
            let l = nums.len();
            if l == 0 {
                return;
            }
            k %= l;
            if k == 0 {
                return;
            }
            for j in p..n - p - 1 {
                grid[p][j] = nums[k];
                k = (k + 1) % l;
            }
            for i in p..m - p - 1 {
                grid[i][n - p - 1] = nums[k];
                k = (k + 1) % l;
            }
            for j in (p + 1..n - p).rev() {
                grid[m - p - 1][j] = nums[k];
                k = (k + 1) % l;
            }
            for i in (p + 1..m - p).rev() {
                grid[i][p] = nums[k];
                k = (k + 1) % l;
            }
        };

        let layers = std::cmp::min(m / 2, n / 2);
        for i in 0..layers {
            rotate(i, k as usize);
        }
        grid
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
