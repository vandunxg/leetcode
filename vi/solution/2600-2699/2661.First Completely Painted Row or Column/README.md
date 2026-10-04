---
comments: true
difficulty: Medium
rating: 1502
source: Weekly Contest 343 Q2
tags:
    - Array
    - Hash Table
    - Matrix
---

<!-- problem:start -->

# [2661. First Completely Painted Row or Column](https://leetcode.com/problems/first-completely-painted-row-or-column)

[中文文档](/solution/2600-2699/2661.First%20Completely%20Painted%20Row%20or%20Column/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>arr</code> được đánh chỉ số từ <strong>0</strong> và một <strong>ma trận</strong> số nguyên <code>m x n</code> <code>mat</code>. Cả <code>arr</code> và <code>mat</code> đều chứa <strong>tất cả</strong> các số nguyên trong khoảng <code>[1, m * n]</code>.</p>

<p>Duyệt qua từng chỉ số <code>i</code> trong <code>arr</code>, bắt đầu từ chỉ số <code>0</code>, và tô màu ô trong <code>mat</code> chứa số nguyên <code>arr[i]</code>.</p>

<p>Trả về <em>chỉ số nhỏ nhất</em> <code>i</code> <em>mà tại đó một hàng hoặc một cột được tô màu hoàn toàn trong</em> <code>mat</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2661.First%20Completely%20Painted%20Row%20or%20Column/images/image explanation for example 1" /><img alt="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2661.First%20Completely%20Painted%20Row%20or%20Column/images/image explanation for example 1" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2661.First%20Completely%20Painted%20Row%20or%20Column/images/grid1.jpg" style="width: 321px; height: 81px;" />
<pre>
<strong>Đầu vào:</strong> arr = [1,3,4,2], mat = [[1,4],[2,3]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Các bước được thực hiện theo thứ tự hiển thị, và cả hàng đầu tiên lẫn cột thứ hai của ma trận đều được tô màu hoàn toàn tại arr[2].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="image explanation for example 2" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2661.First%20Completely%20Painted%20Row%20or%20Column/images/grid2.jpg" style="width: 601px; height: 121px;" />
<pre>
<strong>Đầu vào:</strong> arr = [2,8,7,4,1,3,5,6,9], mat = [[3,2,5],[1,4,6],[8,7,9]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Cột thứ hai được tô màu hoàn toàn tại arr[3].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == mat.length</code></li>
	<li><code>n = mat[i].length</code></li>
	<li><code>arr.length == m * n</code></li>
	<li><code>1 &lt;= m, n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= arr[i], mat[r][c] &lt;= m * n</code></li>
	<li>Tất cả các số nguyên trong <code>arr</code> là <strong>khác nhau</strong>.</li>
	<li>Tất cả các số nguyên trong <code>mat</code> là <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm + Đếm bằng mảng

<!-- thinking:start -->

> **Tư duy**
>
> Các ô được tô theo thứ tự trong $arr$; ta cần tìm hàng hoặc cột đầu tiên được tô kín. Việc duyệt lại một hàng hoặc cột sau mỗi lần tô sẽ quá chậm khi $mn \le 10^5$.
>
> Ta ánh xạ mỗi giá trị tới tọa độ của nó và tăng bộ đếm của hàng, cột tương ứng. Khi một bộ đếm đạt $n$ hoặc $m$, hàng hoặc cột đó đã hoàn tất; chỉ số đầu tiên như vậy chính là đáp án.

<!-- thinking:end -->

Ta sử dụng một bảng băm $idx$ để lưu vị trí của mỗi phần tử trong ma trận $mat$, tức là $idx[mat[i][j]] = (i, j)$, đồng thời định nghĩa hai mảng $row$ và $col$ để lần lượt lưu số phần tử đã được tô trong mỗi hàng và mỗi cột.

Duyệt qua mảng $arr$. Với mỗi phần tử $arr[k]$, ta tìm vị trí $(i, j)$ của nó trong ma trận $mat$, sau đó tăng $row[i]$ và $col[j]$ lên một. Nếu $row[i] = n$ hoặc $col[j] = m$, điều đó có nghĩa là hàng thứ $i$ hoặc cột thứ $j$ đã được tô kín, nên $arr[k]$ là phần tử cần tìm và ta trả về $k$.

Độ phức tạp thời gian là $O(m \times n)$, độ phức tạp không gian là $O(m \times n)$. Ở đây, $m$ và $n$ lần lượt là số hàng và số cột của ma trận $mat$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def firstCompleteIndex(self, arr: List[int], mat: List[List[int]]) -> int:
        m, n = len(mat), len(mat[0])
        idx = {}
        for i in range(m):
            for j in range(n):
                idx[mat[i][j]] = (i, j)
        row = [0] * m
        col = [0] * n
        for k in range(len(arr)):
            i, j = idx[arr[k]]
            row[i] += 1
            col[j] += 1
            if row[i] == n or col[j] == m:
                return k
```

#### Java

```java
class Solution {
    public int firstCompleteIndex(int[] arr, int[][] mat) {
        int m = mat.length, n = mat[0].length;
        Map<Integer, int[]> idx = new HashMap<>();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                idx.put(mat[i][j], new int[] {i, j});
            }
        }
        int[] row = new int[m];
        int[] col = new int[n];
        for (int k = 0;; ++k) {
            var x = idx.get(arr[k]);
            int i = x[0], j = x[1];
            ++row[i];
            ++col[j];
            if (row[i] == n || col[j] == m) {
                return k;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int firstCompleteIndex(vector<int>& arr, vector<vector<int>>& mat) {
        int m = mat.size(), n = mat[0].size();
        unordered_map<int, pair<int, int>> idx;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                idx[mat[i][j]] = {i, j};
            }
        }
        vector<int> row(m), col(n);
        for (int k = 0;; ++k) {
            auto [i, j] = idx[arr[k]];
            ++row[i];
            ++col[j];
            if (row[i] == n || col[j] == m) {
                return k;
            }
        }
    }
};
```

#### Go

```go
func firstCompleteIndex(arr []int, mat [][]int) int {
	m, n := len(mat), len(mat[0])
	idx := map[int][2]int{}
	for i := range mat {
		for j := range mat[i] {
			idx[mat[i][j]] = [2]int{i, j}
		}
	}
	row := make([]int, m)
	col := make([]int, n)
	for k := 0; ; k++ {
		x := idx[arr[k]]
		i, j := x[0], x[1]
		row[i]++
		col[j]++
		if row[i] == n || col[j] == m {
			return k
		}
	}
}
```

#### TypeScript

```ts
function firstCompleteIndex(arr: number[], mat: number[][]): number {
    const m = mat.length;
    const n = mat[0].length;
    const idx: Map<number, number[]> = new Map();
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            idx.set(mat[i][j], [i, j]);
        }
    }
    const row: number[] = Array(m).fill(0);
    const col: number[] = Array(n).fill(0);
    for (let k = 0; ; ++k) {
        const [i, j] = idx.get(arr[k])!;
        ++row[i];
        ++col[j];
        if (row[i] === n || col[j] === m) {
            return k;
        }
    }
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn first_complete_index(arr: Vec<i32>, mat: Vec<Vec<i32>>) -> i32 {
        let m = mat.len();
        let n = mat[0].len();
        let mut idx = HashMap::new();
        for i in 0..m {
            for j in 0..n {
                idx.insert(mat[i][j], [i, j]);
            }
        }

        let mut row = vec![0; m];
        let mut col = vec![0; n];
        for k in 0..arr.len() {
            let x = idx.get(&arr[k]).unwrap();
            let i = x[0];
            let j = x[1];
            row[i] += 1;
            col[j] += 1;
            if row[i] == n || col[j] == m {
                return k as i32;
            }
        }

        -1
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
