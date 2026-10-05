---
comments: true
difficulty: Easy
rating: 1246
source: Weekly Contest 519 Q1
---

<!-- problem:start -->

# [4052. Cyclically Shift Rows and Columns](https://leetcode.com/problems/cyclically-shift-rows-and-columns)

[中文文档](/solution/4000-4099/4052.Cyclically%20Shift%20Rows%20and%20Columns/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code>, một mảng số nguyên 2D <code>grid</code> có kích thước <code>n x n</code>, và hai mảng số nguyên <code>rowShift</code> và <code>colShift</code>, mỗi mảng có độ dài <code>n</code>, trong đó:</p>

<ul>
	<li><code>rowShift[i]</code> biểu thị số vị trí cần <strong>dịch vòng</strong> hàng thứ <code>i<sup>th</sup></code> của <code>grid</code> sang <strong>trái</strong>.</li>
	<li><code>colShift[j]</code> biểu thị số vị trí cần <strong>dịch vòng</strong> cột thứ <code>j<sup>th</sup></code> của <code>grid</code> lên <strong>trên</strong>.</li>
</ul>

<p>Trước tiên, dịch vòng từng hàng theo <code>rowShift</code>, sau đó dịch vòng từng cột của grid thu được theo <code>colShift</code>.</p>

<p>Trả về grid thu được sau khi thực hiện tất cả các phép dịch.</p>

<p>Phép <strong>dịch vòng sang trái</strong> một hàng <code>k</code> vị trí sẽ chuyển phần tử ở cột <code>j</code> đến cột <code>(j - k + n) % n</code>. Các hàng khác không thay đổi.</p>

<p>Phép <strong>dịch vòng lên trên</strong> một cột <code>k</code> vị trí sẽ chuyển phần tử ở hàng <code>i</code> đến hàng <code>(i - k + n) % n</code>. Các cột khác không thay đổi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, <code>grid</code> = [[1,2],[3,4]], rowShift = [1,0], colShift = [0,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[2,4],[3,1]]</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>grid</code> thay đổi như sau:</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/4000-4099/4052.Cyclically%20Shift%20Rows%20and%20Columns/images/4743-1.png" style="width: 760px; height: 92px;" /></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, <code>grid</code> = [[1,2,3],[4,5,6],[7,8,9]], rowShift = [1,2,0], colShift = [2,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[7,8,5],[2,3,9],[6,4,1]]</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>grid</code> thay đổi như sau:</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/4000-4099/4052.Cyclically%20Shift%20Rows%20and%20Columns/images/4743-2.png" style="width: 750px; height: 349px;" /></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == grid.length == grid[i].length &lt;= 10</code></li>
	<li><code>1 &lt;= grid[i][j] &lt;= 100</code></li>
	<li><code>rowShift.length == colShift.length == n</code></li>
	<li><code>0 &lt;= rowShift[i], colShift[i] &lt; n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> $n \le 10$, vì vậy chỉ cần thực hiện chính xác hai phép dịch như đề bài mô tả. Không cần gộp ánh xạ thành một công thức chỉ số duy nhất ngay từ đầu.
>
> Các hàng phải được dịch sang trái trước khi các cột được dịch lên trên, và phép dịch lên trên sử dụng chỉ số cột **mới**. Không thể áp dụng $\textit{colShift}$ với chỉ số $j$ ban đầu.
>
> Do đó, ta giữ một grid trung gian cho các phép dịch hàng, sau đó ghi các phép dịch cột vào đáp án.

<!-- thinking:end -->

Bài toán yêu cầu ta dịch vòng từng hàng sang trái theo $\textit{rowShift}$, sau đó dịch vòng từng cột lên trên theo $\textit{colShift}$.

Tạo một ma trận trung gian $t$. Sau khi dịch vòng sang trái $\textit{rowShift}[i]$, phần tử $\textit{grid}[i][j]$ sẽ được chuyển đến

$$
t[i][(j - \textit{rowShift}[i] + n) \bmod n]
$$

Sau đó tạo ma trận đáp án $\textit{ans}$. Sau khi dịch vòng lên trên $\textit{colShift}[j]$, phần tử $t[i][j]$ sẽ được chuyển đến

$$
\textit{ans}[(i - \textit{colShift}[j] + n) \bmod n][j]
$$

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là độ dài cạnh của grid.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def cyclicShift(
        self, n: int, grid: list[list[int]], rowShift: list[int], colShift: list[int]
    ) -> list[list[int]]:
        t = [[0] * n for _ in range(n)]
        for i in range(n):
            for j in range(n):
                t[i][(j - rowShift[i] + n) % n] = grid[i][j]
        ans = [[0] * n for _ in range(n)]
        for j in range(n):
            for i in range(n):
                ans[(i - colShift[j] + n) % n][j] = t[i][j]
        return ans
```

#### Java

```java
class Solution {
    public int[][] cyclicShift(int n, int[][] grid, int[] rowShift, int[] colShift) {
        int[][] t = new int[n][n];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                t[i][(j - rowShift[i] + n) % n] = grid[i][j];
            }
        }
        int[][] ans = new int[n][n];
        for (int j = 0; j < n; j++) {
            for (int i = 0; i < n; i++) {
                ans[(i - colShift[j] + n) % n][j] = t[i][j];
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
    vector<vector<int>> cyclicShift(int n, vector<vector<int>>& grid, vector<int>& rowShift, vector<int>& colShift) {
        vector<vector<int>> t(n, vector<int>(n));
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                t[i][(j - rowShift[i] + n) % n] = grid[i][j];
            }
        }
        vector<vector<int>> ans(n, vector<int>(n));
        for (int j = 0; j < n; j++) {
            for (int i = 0; i < n; i++) {
                ans[(i - colShift[j] + n) % n][j] = t[i][j];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func cyclicShift(n int, grid [][]int, rowShift []int, colShift []int) [][]int {
	t := make([][]int, n)
	for i := range t {
		t[i] = make([]int, n)
	}
	for i := 0; i < n; i++ {
		for j := 0; j < n; j++ {
			t[i][(j-rowShift[i]+n)%n] = grid[i][j]
		}
	}
	ans := make([][]int, n)
	for i := range ans {
		ans[i] = make([]int, n)
	}
	for j := 0; j < n; j++ {
		for i := 0; i < n; i++ {
			ans[(i-colShift[j]+n)%n][j] = t[i][j]
		}
	}
	return ans
}
```

#### TypeScript

```ts
function cyclicShift(
    n: number,
    grid: number[][],
    rowShift: number[],
    colShift: number[],
): number[][] {
    const t = Array.from({ length: n }, () => Array(n).fill(0));
    for (let i = 0; i < n; i++) {
        for (let j = 0; j < n; j++) {
            t[i][(j - rowShift[i] + n) % n] = grid[i][j];
        }
    }
    const ans = Array.from({ length: n }, () => Array(n).fill(0));
    for (let j = 0; j < n; j++) {
        for (let i = 0; i < n; i++) {
            ans[(i - colShift[j] + n) % n][j] = t[i][j];
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
