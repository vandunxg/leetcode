---
comments: true
difficulty: Easy
rating: 1139
source: Weekly Contest 176 Q1
tags:
    - Array
    - Binary Search
    - Matrix
---

<!-- problem:start -->

# [1351. Count Negative Numbers in a Sorted Matrix](https://leetcode.com/problems/count-negative-numbers-in-a-sorted-matrix)

[中文文档](/solution/1300-1399/1351.Count%20Negative%20Numbers%20in%20a%20Sorted%20Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận <code>m x n</code> <code>grid</code> được sắp xếp theo thứ tự không tăng dần cả theo hàng lẫn theo cột. Hãy trả về <em>số lượng số <strong>âm</strong> trong</em> <code>grid</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> grid = [[4,3,2,-1],[3,2,1,-1],[1,1,-1,-2],[-1,-1,-2,-3]]
<strong>Output:</strong> 8
<strong>Giải thích:</strong> Ma trận có 8 số âm.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> grid = [[3,2],[1,0]]
<strong>Output:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 100</code></li>
	<li><code>-100 &lt;= grid[i][j] &lt;= 100</code></li>
</ul>

<p>&nbsp;</p>
<strong>Câu hỏi mở rộng:</strong> Bạn có thể tìm lời giải có độ phức tạp <code>O(n + m)</code> không?

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt từ góc dưới bên trái

<!-- thinking:start -->

> **Tư duy**
>
> Các hàng và cột đều không tăng; ta cần đếm các số âm. Duyệt toàn bộ ma trận tốn $O(mn)$. Ô dưới cùng bên trái giúp chia vùng theo tính chất “nhỏ nhất trong hàng / lớn nhất trong cột”: nếu giá trị không âm, dịch sang phải để bỏ qua phần còn lại của hàng ở bên trái; nếu là số âm, cộng $n-j$ vào kết quả rồi dịch lên trên. Mỗi chỉ số chỉ thay đổi một lần, nên độ phức tạp là $O(m+n)$.

<!-- thinking:end -->

Since the matrix is sorted in non-strictly decreasing order both row-wise and column-wise, we can start traversing from the bottom-left corner of the matrix. Let the current position be $(i, j)$.

Nếu phần tử ở vị trí hiện tại lớn hơn hoặc bằng $0$, thì mọi phần tử đứng trước nó trong hàng đó cũng lớn hơn hoặc bằng $0$. Vì vậy, ta tăng chỉ số cột $j$ thêm một, tức là $j = j + 1$.

Nếu phần tử ở vị trí hiện tại nhỏ hơn $0$, thì phần tử đó cùng mọi phần tử bên phải nó trong hàng đều âm. Vì vậy, ta cộng $n - j$ vào số lượng số âm (trong đó $n$ là số cột của ma trận), rồi giảm chỉ số hàng $i$ đi một, tức là $i = i - 1$.

Ta lặp lại các bước trên cho đến khi chỉ số hàng $i$ nhỏ hơn $0$ hoặc chỉ số cột $j$ lớn hơn hoặc bằng $n$. Cuối cùng, số lượng số âm chính là đáp án.

Độ phức tạp thời gian là $O(m + n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countNegatives(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        i, j = m - 1, 0
        ans = 0
        while i >= 0 and j < n:
            if grid[i][j] >= 0:
                j += 1
            else:
                ans += n - j
                i -= 1
        return ans
```

#### Java

```java
class Solution {
    public int countNegatives(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        int i = m - 1;
        int j = 0;
        int ans = 0;
        while (i >= 0 && j < n) {
            if (grid[i][j] >= 0) {
                j++;
            } else {
                ans += n - j;
                i--;
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
    int countNegatives(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        int i = m - 1;
        int j = 0;
        int ans = 0;
        while (i >= 0 && j < n) {
            if (grid[i][j] >= 0) {
                j++;
            } else {
                ans += n - j;
                i--;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countNegatives(grid [][]int) (ans int) {
	m := len(grid)
	n := len(grid[0])
	i := m - 1
	j := 0
	for i >= 0 && j < n {
		if grid[i][j] >= 0 {
			j++
		} else {
			ans += n - j
			i--
		}
	}
	return
}
```

#### TypeScript

```ts
function countNegatives(grid: number[][]): number {
    const m = grid.length;
    const n = grid[0].length;
    let i = m - 1;
    let j = 0;
    let ans = 0;
    while (i >= 0 && j < n) {
        if (grid[i][j] >= 0) {
            j++;
        } else {
            ans += n - j;
            i--;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_negatives(grid: Vec<Vec<i32>>) -> i32 {
        let m = grid.len();
        let n = grid[0].len();
        let mut i: i32 = m as i32 - 1;
        let mut j: usize = 0;
        let mut ans: i32 = 0;
        while i >= 0 && j < n {
            if grid[i as usize][j] >= 0 {
                j += 1;
            } else {
                ans += (n - j) as i32;
                i -= 1;
            }
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[][]} grid
 * @return {number}
 */
var countNegatives = function (grid) {
    const m = grid.length;
    const n = grid[0].length;
    let i = m - 1;
    let j = 0;
    let ans = 0;
    while (i >= 0 && j < n) {
        if (grid[i][j] >= 0) {
            j++;
        } else {
            ans += n - j;
            i--;
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
