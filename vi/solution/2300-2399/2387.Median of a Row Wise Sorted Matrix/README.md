---
comments: true
difficulty: Medium
tags:
    - Array
    - Binary Search
    - Matrix
---

<!-- problem:start -->

# [2387. Median of a Row Wise Sorted Matrix 🔒](https://leetcode.com/problems/median-of-a-row-wise-sorted-matrix)

[中文文档](/solution/2300-2399/2387.Median%20of%20a%20Row%20Wise%20Sorted%20Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận <code>m x n</code> <code>grid</code> chứa một số <strong>lẻ</strong> các số nguyên, trong đó mỗi hàng được sắp xếp theo thứ tự <strong>không giảm</strong>, hãy trả về <em><strong>trung vị</strong> của ma trận</em>.</p>

<p>Bạn phải giải bài toán với độ phức tạp thời gian nhỏ hơn <code>O(m * n)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,1,2],[2,3,3],[1,3,4]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Các phần tử của ma trận theo thứ tự tăng dần là 1,1,1,2,<u>2</u>,3,3,3,4. Trung vị là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,1,3,3,4]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Các phần tử của ma trận theo thứ tự tăng dần là 1,1,<u>3</u>,3,4. Trung vị là 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 500</code></li>
	<li><code>m</code> và <code>n</code> đều là số lẻ.</li>
	<li><code>1 &lt;= grid[i][j] &lt;= 10<sup>6</sup></code></li>
	<li><code>grid[i]</code> được sắp xếp theo thứ tự không giảm.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai lần tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi hàng đều đã được sắp xếp; ta cần tìm trung vị của toàn bộ ma trận mà không làm phẳng ma trận. Đây là giá trị thứ $\lceil mn/2 \rceil$ trong thứ tự sắp xếp, vì vậy ta tìm kiếm nhị phân trên miền giá trị.
>
> Để kiểm tra $x$, ta dùng $bisect$ trên từng hàng để đếm số phần tử $\le x$. Nếu số lượng này đạt target, trung vị không lớn hơn $x$. Một lần tìm kiếm bên ngoài trên miền giá trị sẽ hoàn tất việc tìm kiếm.

<!-- thinking:end -->

Trung vị thực chất là số thứ $target = \left \lceil \frac{m \times n}{2} \right \rceil$ sau khi sắp xếp.

Ta thực hiện tìm kiếm nhị phân trên các phần tử của ma trận $x$, đếm số phần tử trong grid lớn hơn $x$, ký hiệu là $cnt$. Nếu $cnt \ge target$, điều đó có nghĩa là trung vị nằm ở phía bên trái của $x$ (bao gồm cả $x$); nếu không, nó nằm ở phía bên phải.

Độ phức tạp thời gian là $O(m \times \log n \times \log M)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của grid, còn $M$ là phần tử lớn nhất trong grid. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def matrixMedian(self, grid: List[List[int]]) -> int:
        def count(x):
            return sum(bisect_right(row, x) for row in grid)

        m, n = len(grid), len(grid[0])
        target = (m * n + 1) >> 1
        return bisect_left(range(10**6 + 1), target, key=count)
```

#### Java

```java
class Solution {
    private int[][] grid;

    public int matrixMedian(int[][] grid) {
        this.grid = grid;
        int m = grid.length, n = grid[0].length;
        int target = (m * n + 1) >> 1;
        int left = 0, right = 1000010;
        while (left < right) {
            int mid = (left + right) >> 1;
            if (count(mid) >= target) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }

    private int count(int x) {
        int cnt = 0;
        for (var row : grid) {
            int left = 0, right = row.length;
            while (left < right) {
                int mid = (left + right) >> 1;
                if (row[mid] > x) {
                    right = mid;
                } else {
                    left = mid + 1;
                }
            }
            cnt += left;
        }
        return cnt;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int matrixMedian(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        int left = 0, right = 1e6 + 1;
        int target = (m * n + 1) >> 1;
        auto count = [&](int x) {
            int cnt = 0;
            for (auto& row : grid) {
                cnt += (upper_bound(row.begin(), row.end(), x) - row.begin());
            }
            return cnt;
        };
        while (left < right) {
            int mid = (left + right) >> 1;
            if (count(mid) >= target) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
};
```

#### Go

```go
func matrixMedian(grid [][]int) int {
	m, n := len(grid), len(grid[0])

	count := func(x int) int {
		cnt := 0
		for _, row := range grid {
			left, right := 0, n
			for left < right {
				mid := (left + right) >> 1
				if row[mid] > x {
					right = mid
				} else {
					left = mid + 1
				}
			}
			cnt += left
		}
		return cnt
	}
	left, right := 0, 1000010
	target := (m*n + 1) >> 1
	for left < right {
		mid := (left + right) >> 1
		if count(mid) >= target {
			right = mid
		} else {
			left = mid + 1
		}
	}
	return left
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
