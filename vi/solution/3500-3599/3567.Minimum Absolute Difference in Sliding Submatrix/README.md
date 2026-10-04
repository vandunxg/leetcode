---
comments: true
difficulty: Medium
rating: 1568
source: Weekly Contest 452 Q2
tags:
    - Array
    - Matrix
    - Sorting
---

<!-- problem:start -->

# [3567. Minimum Absolute Difference in Sliding Submatrix](https://leetcode.com/problems/minimum-absolute-difference-in-sliding-submatrix)

[中文文档](/solution/3500-3599/3567.Minimum%20Absolute%20Difference%20in%20Sliding%20Submatrix/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một ma trận số nguyên kích thước <code>m x n</code> tên là <code>grid</code> và một số nguyên <code>k</code>.</p>

<p>Với mọi <strong>ma trận con</strong> <code>k x k</code> liên tiếp của <code>grid</code>, hãy tính <strong>hiệu tuyệt đối nhỏ nhất</strong> giữa hai giá trị <strong>phân biệt</strong> bất kỳ trong <strong>ma trận con</strong> đó.</p>

<p>Trả về một mảng 2D <code>ans</code> có kích thước <code>(m - k + 1) x (n - k + 1)</code>, trong đó <code>ans[i][j]</code> là hiệu tuyệt đối nhỏ nhất trong ma trận con có góc trên bên trái là <code>(i, j)</code> trong <code>grid</code>.</p>

<p><strong>Lưu ý</strong>: Nếu tất cả phần tử trong ma trận con có cùng giá trị, đáp án sẽ là 0.</p>
Một ma trận con <code>(x1, y1, x2, y2)</code> là ma trận được tạo bằng cách chọn tất cả các ô <code>matrix[x][y]</code> sao cho <code>x1 &lt;= x &lt;= x2</code> và <code>y1 &lt;= y &lt;= y2</code>.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,8],[3,-2]], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[2]]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Chỉ có một ma trận con <code>k x k</code> có thể có: <code><span class="example-io">[[1, 8], [3, -2]]</span></code><span class="example-io">.</span></li>
    <li>Các giá trị phân biệt trong ma trận con là<span class="example-io"> <code>[1, 8, 3, -2]</code>.</span></li>
    <li>Hiệu tuyệt đối nhỏ nhất trong ma trận con là <code>|1 - 3| = 2</code>. Vì vậy, đáp án là <code>[[2]]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[3,-1]], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[0,0]]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Cả hai ma trận con <code>k x k</code> chỉ chứa một giá trị duy nhất.</li>
    <li>Vì vậy, đáp án là <code>[[0, 0]]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,-2,3],[2,3,5]], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[1,2]]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Có hai ma trận con <code>k &times; k</code> có thể có:

    <ul>
        <li>Bắt đầu tại <code>(0, 0)</code>: <code>[[1, -2], [2, 3]]</code>.

        <ul>
            <li>Các giá trị phân biệt trong ma trận con là <code>[1, -2, 2, 3]</code>.</li>
            <li>Hiệu tuyệt đối nhỏ nhất trong ma trận con là <code>|1 - 2| = 1</code>.</li>
        </ul>
        </li>
        <li>Bắt đầu tại <code>(0, 1)</code>: <code>[[-2, 3], [3, 5]]</code>.
    <ul>
            <li>Các giá trị phân biệt trong ma trận con là <code>[-2, 3, 5]</code>.</li>
            <li>Hiệu tuyệt đối nhỏ nhất trong ma trận con là <code>|3 - 5| = 2</code>.</li>
        </ul>
        </li>
    </ul>
    </li>
    <li>Vì vậy, đáp án là <code>[[1, 2]]</code>.</li>

</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= m == grid.length &lt;= 30</code></li>
    <li><code>1 &lt;= n == grid[i].length &lt;= 30</code></li>
    <li><code>-10<sup>5</sup> &lt;= grid[i][j] &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= k &lt;= min(m, n)</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi cửa sổ $k \times k$, ta cần hiệu tuyệt đối nhỏ nhất giữa các giá trị phân biệt. Sau khi sắp xếp $k^2$ phần tử, hiệu nhỏ nhất chính là hiệu giữa hai giá trị khác nhau liên tiếp.
>
> Ta liệt kê góc trên bên trái, thu thập các phần tử, sắp xếp rồi duyệt các cặp kề nhau. Nếu mọi phần tử trong cửa sổ giống nhau thì hiệu là $0$.

<!-- thinking:end -->

Ta có thể liệt kê tất cả các ma trận con $k \times k$ có thể có dựa trên tọa độ góc trên bên trái $(i, j)$. Với mỗi ma trận con, ta lấy tất cả phần tử của nó vào một danh sách $\textit{nums}$. Sau đó, ta sắp xếp $\textit{nums}$ và tính hiệu tuyệt đối giữa các phần tử phân biệt kề nhau để tìm hiệu tuyệt đối nhỏ nhất. Cuối cùng, ta lưu kết quả vào một mảng 2D.

Độ phức tạp thời gian là $O((m - k + 1) \times (n - k + 1) \times k^2 \log(k))$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận, còn $k$ là kích thước của ma trận con. Độ phức tạp không gian là $O(k^2)$, dùng để lưu các phần tử của mỗi ma trận con.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minAbsDiff(self, grid: List[List[int]], k: int) -> List[List[int]]:
        m, n = len(grid), len(grid[0])
        ans = [[0] * (n - k + 1) for _ in range(m - k + 1)]
        for i in range(m - k + 1):
            for j in range(n - k + 1):
                nums = []
                for x in range(i, i + k):
                    for y in range(j, j + k):
                        nums.append(grid[x][y])
                nums.sort()
                d = min((abs(a - b) for a, b in pairwise(nums) if a != b), default=0)
                ans[i][j] = d
        return ans
```

#### Java

```java
class Solution {
    public int[][] minAbsDiff(int[][] grid, int k) {
        int m = grid.length, n = grid[0].length;
        int[][] ans = new int[m - k + 1][n - k + 1];
        for (int i = 0; i <= m - k; i++) {
            for (int j = 0; j <= n - k; j++) {
                List<Integer> nums = new ArrayList<>();
                for (int x = i; x < i + k; x++) {
                    for (int y = j; y < j + k; y++) {
                        nums.add(grid[x][y]);
                    }
                }
                Collections.sort(nums);
                int d = Integer.MAX_VALUE;
                for (int t = 1; t < nums.size(); t++) {
                    int a = nums.get(t - 1);
                    int b = nums.get(t);
                    if (a != b) {
                        d = Math.min(d, Math.abs(a - b));
                    }
                }
                ans[i][j] = (d == Integer.MAX_VALUE) ? 0 : d;
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
    vector<vector<int>> minAbsDiff(vector<vector<int>>& grid, int k) {
        int m = grid.size(), n = grid[0].size();
        vector<vector<int>> ans(m - k + 1, vector<int>(n - k + 1, 0));
        for (int i = 0; i <= m - k; ++i) {
            for (int j = 0; j <= n - k; ++j) {
                vector<int> nums;
                for (int x = i; x < i + k; ++x) {
                    for (int y = j; y < j + k; ++y) {
                        nums.push_back(grid[x][y]);
                    }
                }
                ranges::sort(nums);
                int d = INT_MAX;
                for (int t = 1; t < nums.size(); ++t) {
                    if (nums[t] != nums[t - 1]) {
                        d = min(d, abs(nums[t] - nums[t - 1]));
                    }
                }
                ans[i][j] = (d == INT_MAX) ? 0 : d;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minAbsDiff(grid [][]int, k int) [][]int {
    m, n := len(grid), len(grid[0])
    ans := make([][]int, m-k+1)
    for i := range ans {
        ans[i] = make([]int, n-k+1)
    }
    for i := 0; i <= m-k; i++ {
        for j := 0; j <= n-k; j++ {
            var nums []int
            for x := i; x < i+k; x++ {
                for y := j; y < j+k; y++ {
                    nums = append(nums, grid[x][y])
                }
            }
            sort.Ints(nums)
            d := math.MaxInt
            for t := 1; t < len(nums); t++ {
                if nums[t] != nums[t-1] {
                    diff := abs(nums[t] - nums[t-1])
                    if diff < d {
                        d = diff
                    }
                }
            }
            if d != math.MaxInt {
                ans[i][j] = d
            }
        }
    }
    return ans
}

func abs(x int) int {
    if x < 0 {
        return -x
    }
    return x
}
```

#### TypeScript

```ts
function minAbsDiff(grid: number[][], k: number): number[][] {
    const m = grid.length;
    const n = grid[0].length;
    const ans: number[][] = Array.from({ length: m - k + 1 }, () => Array(n - k + 1).fill(0));
    for (let i = 0; i <= m - k; i++) {
        for (let j = 0; j <= n - k; j++) {
            const nums: number[] = [];
            for (let x = i; x < i + k; x++) {
                for (let y = j; y < j + k; y++) {
                    nums.push(grid[x][y]);
                }
            }
            nums.sort((a, b) => a - b);
            let d = Number.MAX_SAFE_INTEGER;
            for (let t = 1; t < nums.length; t++) {
                if (nums[t] !== nums[t - 1]) {
                    d = Math.min(d, Math.abs(nums[t] - nums[t - 1]));
                }
            }
            ans[i][j] = d === Number.MAX_SAFE_INTEGER ? 0 : d;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_abs_diff(grid: Vec<Vec<i32>>, k: i32) -> Vec<Vec<i32>> {
        let m = grid.len();
        let n = grid[0].len();
        let k = k as usize;

        let mut ans = vec![vec![0; n - k + 1]; m - k + 1];

        for i in 0..=m - k {
            for j in 0..=n - k {
                let mut nums = Vec::with_capacity(k * k);
                for x in i..i + k {
                    for y in j..j + k {
                        nums.push(grid[x][y]);
                    }
                }

                nums.sort_unstable();

                let mut d = i32::MAX;
                for t in 1..nums.len() {
                    if nums[t] != nums[t - 1] {
                        d = d.min((nums[t] - nums[t - 1]).abs());
                    }
                }

                ans[i][j] = if d == i32::MAX { 0 } else { d };
            }
        }

        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int[][] MinAbsDiff(int[][] grid, int k) {
        int m = grid.Length, n = grid[0].Length;
        int[][] ans = new int[m - k + 1][];
        for (int i = 0; i <= m - k; ++i) {
            ans[i] = new int[n - k + 1];
            for (int j = 0; j <= n - k; ++j) {
                List<int> nums = new List<int>(k * k);
                for (int x = i; x < i + k; ++x) {
                    for (int y = j; y < j + k; ++y) {
                        nums.Add(grid[x][y]);
                    }
                }

                nums.Sort();

                int d = int.MaxValue;
                for (int t = 1; t < nums.Count; ++t) {
                    if (nums[t] != nums[t - 1]) {
                        d = Math.Min(d, Math.Abs(nums[t] - nums[t - 1]));
                    }
                }

                ans[i][j] = d == int.MaxValue ? 0 : d;
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
