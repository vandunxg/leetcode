---
comments: true
difficulty: Medium
rating: 1372
source: Weekly Contest 436 Q1
tags:
    - Array
    - Matrix
    - Sorting
---

<!-- problem:start -->

# [3446. Sort Matrix by Diagonals](https://leetcode.com/problems/sort-matrix-by-diagonals)

[中文文档](/solution/3400-3499/3446.Sort%20Matrix%20by%20Diagonals/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận vuông <code>n x n</code> gồm các số nguyên là <code>grid</code>. Hãy trả về ma trận sao cho:</p>

<ul>
	<li>Các đường chéo trong <strong>tam giác dưới-trái</strong> (bao gồm đường chéo giữa) được sắp xếp theo <strong>thứ tự không tăng</strong>.</li>
	<li>Các đường chéo trong <strong>tam giác trên-phải</strong> được sắp xếp theo <strong>thứ tự không giảm</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,7,3],[9,8,2],[4,5,6]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[8,2,3],[9,6,7],[4,5,1]]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3446.Sort%20Matrix%20by%20Diagonals/images/4052example1drawio.png" style="width: 461px; height: 181px;" /></p>

<p>Các đường chéo có mũi tên màu đen (tam giác dưới-trái) cần được sắp xếp theo thứ tự không tăng:</p>

<ul>
	<li><code>[1, 8, 6]</code> trở thành <code>[8, 6, 1]</code>.</li>
	<li><code>[9, 5]</code> và <code>[4]</code> giữ nguyên.</li>
</ul>

<p>Các đường chéo có mũi tên màu xanh dương (tam giác trên-phải) cần được sắp xếp theo thứ tự không giảm:</p>

<ul>
	<li><code>[7, 2]</code> trở thành <code>[2, 7]</code>.</li>
	<li><code>[3]</code> giữ nguyên.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[0,1],[1,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[2,1],[1,0]]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3446.Sort%20Matrix%20by%20Diagonals/images/4052example2adrawio.png" style="width: 383px; height: 141px;" /></p>

<p>Các đường chéo có mũi tên màu đen phải được sắp xếp theo thứ tự không tăng, nên <code>[0, 2]</code> được đổi thành <code>[2, 0]</code>. Các đường chéo còn lại đã ở đúng thứ tự.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[1]]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các đường chéo chỉ có một phần tử vốn đã được sắp xếp, nên không cần thay đổi.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>grid.length == grid[i].length == n</code></li>
	<li><code>1 &lt;= n &lt;= 10</code></li>
	<li><code>-10<sup>5</sup> &lt;= grid[i][j] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 10$ và mỗi đường chéo là độc lập. Tam giác phía dưới, bao gồm đường chéo chính, được sắp xếp theo thứ tự không tăng; tam giác phía trên được sắp xếp theo thứ tự không giảm.
>
> Ta thu thập, sắp xếp rồi ghi lại; không cần cấu trúc dữ liệu phức tạp hơn.
>
> Ta lấy từng đường chéo phía dưới, sắp xếp và lấy phần tử lớn nhất trước; các đường chéo phía trên cũng được xử lý tương tự. Vòng lặp đầu tiên đã bao gồm đường chéo chính.

<!-- thinking:end -->

Ta có thể mô phỏng quá trình sắp xếp các đường chéo như mô tả trong đề bài.

Trước tiên, ta sắp xếp các đường chéo của tam giác dưới-trái, bao gồm đường chéo chính, theo thứ tự không tăng. Sau đó, ta sắp xếp các đường chéo của tam giác trên-phải theo thứ tự không giảm. Cuối cùng, ta trả về ma trận đã sắp xếp.

Độ phức tạp thời gian là $O(n^2 \log n)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là kích thước của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sortMatrix(self, grid: List[List[int]]) -> List[List[int]]:
        n = len(grid)
        for k in range(n - 2, -1, -1):
            i, j = k, 0
            t = []
            while i < n and j < n:
                t.append(grid[i][j])
                i += 1
                j += 1
            t.sort()
            i, j = k, 0
            while i < n and j < n:
                grid[i][j] = t.pop()
                i += 1
                j += 1
        for k in range(n - 2, 0, -1):
            i, j = k, n - 1
            t = []
            while i >= 0 and j >= 0:
                t.append(grid[i][j])
                i -= 1
                j -= 1
            t.sort()
            i, j = k, n - 1
            while i >= 0 and j >= 0:
                grid[i][j] = t.pop()
                i -= 1
                j -= 1
        return grid
```

#### Java

```java
class Solution {
    public int[][] sortMatrix(int[][] grid) {
        int n = grid.length;
        for (int k = n - 2; k >= 0; --k) {
            int i = k, j = 0;
            List<Integer> t = new ArrayList<>();
            while (i < n && j < n) {
                t.add(grid[i++][j++]);
            }
            Collections.sort(t);
            for (int x : t) {
                grid[--i][--j] = x;
            }
        }
        for (int k = n - 2; k > 0; --k) {
            int i = k, j = n - 1;
            List<Integer> t = new ArrayList<>();
            while (i >= 0 && j >= 0) {
                t.add(grid[i--][j--]);
            }
            Collections.sort(t);
            for (int x : t) {
                grid[++i][++j] = x;
            }
        }
        return grid;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> sortMatrix(vector<vector<int>>& grid) {
        int n = grid.size();
        for (int k = n - 2; k >= 0; --k) {
            int i = k, j = 0;
            vector<int> t;
            while (i < n && j < n) {
                t.push_back(grid[i++][j++]);
            }
            ranges::sort(t);
            for (int x : t) {
                grid[--i][--j] = x;
            }
        }
        for (int k = n - 2; k > 0; --k) {
            int i = k, j = n - 1;
            vector<int> t;
            while (i >= 0 && j >= 0) {
                t.push_back(grid[i--][j--]);
            }
            ranges::sort(t);
            for (int x : t) {
                grid[++i][++j] = x;
            }
        }
        return grid;
    }
};
```

#### Go

```go
func sortMatrix(grid [][]int) [][]int {
	n := len(grid)
	for k := n - 2; k >= 0; k-- {
		i, j := k, 0
		t := []int{}
		for ; i < n && j < n; i, j = i+1, j+1 {
			t = append(t, grid[i][j])
		}
		sort.Ints(t)
		for _, x := range t {
			i, j = i-1, j-1
			grid[i][j] = x
		}
	}
	for k := n - 2; k > 0; k-- {
		i, j := k, n-1
		t := []int{}
		for ; i >= 0 && j >= 0; i, j = i-1, j-1 {
			t = append(t, grid[i][j])
		}
		sort.Ints(t)
		for _, x := range t {
			i, j = i+1, j+1
			grid[i][j] = x
		}
	}
	return grid
}
```

#### TypeScript

```ts
function sortMatrix(grid: number[][]): number[][] {
    const n = grid.length;
    for (let k = n - 2; k >= 0; --k) {
        let [i, j] = [k, 0];
        const t: number[] = [];
        while (i < n && j < n) {
            t.push(grid[i++][j++]);
        }
        t.sort((a, b) => a - b);
        for (const x of t) {
            grid[--i][--j] = x;
        }
    }
    for (let k = n - 2; k > 0; --k) {
        let [i, j] = [k, n - 1];
        const t: number[] = [];
        while (i >= 0 && j >= 0) {
            t.push(grid[i--][j--]);
        }
        t.sort((a, b) => a - b);
        for (const x of t) {
            grid[++i][++j] = x;
        }
    }
    return grid;
}
```

#### Rust

```rust
impl Solution {
    pub fn sort_matrix(mut grid: Vec<Vec<i32>>) -> Vec<Vec<i32>> {
        let n = grid.len();
        if n <= 1 {
            return grid;
        }
        for k in (0..=n - 2).rev() {
            let mut i = k;
            let mut j = 0;
            let mut t = Vec::new();
            while i < n && j < n {
                t.push(grid[i][j]);
                i += 1;
                j += 1;
            }
            t.sort();
            let mut i = k;
            let mut j = 0;
            while i < n && j < n {
                grid[i][j] = t.pop().unwrap();
                i += 1;
                j += 1;
            }
        }
        for k in (1..=n - 2).rev() {
            let mut i = k;
            let mut j = n - 1;
            let mut t = Vec::new();
            loop {
                t.push(grid[i][j]);
                if i == 0 { break; }
                i -= 1;
                j -= 1;
            }
            t.sort();
            let mut i = k;
            let mut j = n - 1;
            loop {
                grid[i][j] = t.pop().unwrap();
                if i == 0 { break; }
                i -= 1;
                j -= 1;
            }
        }
        grid
    }
}
```

#### JavaScript

```js
/**
 * @param {number[][]} grid
 * @return {number[][]}
 */
var sortMatrix = function (grid) {
    const n = grid.length;
    for (let k = n - 2; k >= 0; --k) {
        let i = k,
            j = 0;
        const t = [];
        while (i < n && j < n) {
            t.push(grid[i++][j++]);
        }
        t.sort((a, b) => a - b);
        for (const x of t) {
            grid[--i][--j] = x;
        }
    }
    for (let k = n - 2; k > 0; --k) {
        let i = k,
            j = n - 1;
        const t = [];
        while (i >= 0 && j >= 0) {
            t.push(grid[i--][j--]);
        }
        t.sort((a, b) => a - b);
        for (const x of t) {
            grid[++i][++j] = x;
        }
    }
    return grid;
};
```

#### C#

```cs
public class Solution {
    public int[][] SortMatrix(int[][] grid) {
        int n = grid.Length;
        for (int k = n - 2; k >= 0; --k) {
            int i = k, j = 0;
            List<int> t = new List<int>();
            while (i < n && j < n) {
                t.Add(grid[i++][j++]);
            }
            t.Sort();
            foreach (int x in t) {
                grid[--i][--j] = x;
            }
        }
        for (int k = n - 2; k > 0; --k) {
            int i = k, j = n - 1;
            List<int> t = new List<int>();
            while (i >= 0 && j >= 0) {
                t.Add(grid[i--][j--]);
            }
            t.Sort();
            foreach (int x in t) {
                grid[++i][++j] = x;
            }
        }
        return grid;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
