---
comments: true
difficulty: Medium
rating: 1548
source: Biweekly Contest 18 Q3
tags:
    - Array
    - Matrix
    - Sorting
---

<!-- problem:start -->

# [1329. Sort the Matrix Diagonally](https://leetcode.com/problems/sort-the-matrix-diagonally)

[中文文档](/solution/1300-1399/1329.Sort%20the%20Matrix%20Diagonally/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Đường chéo của ma trận</strong> là đường chéo bắt đầu từ một ô bất kỳ ở hàng trên cùng hoặc cột ngoài cùng bên trái, rồi đi theo hướng xuống dưới và sang phải đến hết ma trận. Ví dụ, trong ma trận <code>6 x 3</code> <code>mat</code>, đường chéo bắt đầu từ <code>mat[2][0]</code> gồm các ô <code>mat[2][0]</code>, <code>mat[3][1]</code> và <code>mat[4][2]</code>.</p>

<p>Cho ma trận số nguyên <code>m x n</code> <code>mat</code>, hãy sắp xếp từng <strong>đường chéo của ma trận</strong> theo thứ tự tăng dần và trả về <em>ma trận sau khi sắp xếp</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1329.Sort%20the%20Matrix%20Diagonally/images/1482_example_1_2.png" style="width: 500px; height: 198px;" />
<pre>
<strong>Input:</strong> mat = [[3,3,1,1],[2,2,1,2],[1,1,1,2]]
<strong>Output:</strong> [[1,1,1,1],[1,2,2,2],[1,2,3,3]]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> mat = [[11,25,66,1,69,7],[23,55,17,45,15,52],[75,31,36,44,58,8],[22,27,33,25,68,4],[84,28,14,11,5,50]]
<strong>Output:</strong> [[5,17,4,1,52,7],[11,11,25,45,8,69],[14,23,25,44,58,15],[22,27,31,36,50,66],[84,28,75,33,55,68]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == mat.length</code></li>
	<li><code>n == mat[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 100</code></li>
	<li><code>1 &lt;= mat[i][j] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Các ô trên cùng một đường chéo có chung giá trị $j-i$. Ta lấy các ô trên từng đường chéo, sắp xếp rồi ghi lại vào ma trận. Ánh xạ theo $m-i+j$ tạo chỉ số bucket không âm; sắp xếp từng bucket theo thứ tự giảm dần rồi lấy phần tử từ cuối khi duyệt ma trận sẽ khôi phục thứ tự tăng dần từ trên trái xuống dưới phải.

<!-- thinking:end -->

Ta có thể xem mỗi đường chéo của ma trận là một mảng, sắp xếp các mảng này rồi ghi các phần tử đã sắp xếp trở lại ma trận ban đầu.

Cụ thể, gọi số hàng của ma trận là $m$ và số cột là $n$. Hai phần tử $(i_1, j_1)$ và $(i_2, j_2)$ nằm trên cùng một đường chéo khi $j_1 - i_1 = j_2 - i_2$, nên ta có thể xác định mỗi đường chéo dựa vào giá trị $j - i$. Để chỉ số này không âm, ta cộng thêm offset $m$, tức dùng $m - i + j$.

Cuối cùng, ta điền các phần tử đã sắp xếp của từng đường chéo trở lại ma trận ban đầu.

Độ phức tạp thời gian là $O(m \times n \times \log \min(m, n))$ và độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def diagonalSort(self, mat: List[List[int]]) -> List[List[int]]:
        m, n = len(mat), len(mat[0])
        g = [[] for _ in range(m + n)]
        for i, row in enumerate(mat):
            for j, x in enumerate(row):
                g[m - i + j].append(x)
        for e in g:
            e.sort(reverse=True)
        for i in range(m):
            for j in range(n):
                mat[i][j] = g[m - i + j].pop()
        return mat
```

#### Java

```java
class Solution {
    public int[][] diagonalSort(int[][] mat) {
        int m = mat.length, n = mat[0].length;
        List<Integer>[] g = new List[m + n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                g[m - i + j].add(mat[i][j]);
            }
        }
        for (var e : g) {
            Collections.sort(e, (a, b) -> b - a);
        }
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                mat[i][j] = g[m - i + j].removeLast();
            }
        }
        return mat;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> diagonalSort(vector<vector<int>>& mat) {
        int m = mat.size(), n = mat[0].size();
        vector<int> g[m + n];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                g[m - i + j].push_back(mat[i][j]);
            }
        }
        for (auto& e : g) {
            sort(e.rbegin(), e.rend());
        }
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                mat[i][j] = g[m - i + j].back();
                g[m - i + j].pop_back();
            }
        }
        return mat;
    }
};
```

#### Go

```go
func diagonalSort(mat [][]int) [][]int {
	m, n := len(mat), len(mat[0])
	g := make([][]int, m+n)
	for i, row := range mat {
		for j, x := range row {
			g[m-i+j] = append(g[m-i+j], x)
		}
	}
	for _, e := range g {
		sort.Sort(sort.Reverse(sort.IntSlice(e)))
	}
	for i, row := range mat {
		for j := range row {
			k := len(g[m-i+j])
			mat[i][j] = g[m-i+j][k-1]
			g[m-i+j] = g[m-i+j][:k-1]
		}
	}
	return mat
}
```

#### TypeScript

```ts
function diagonalSort(mat: number[][]): number[][] {
    const [m, n] = [mat.length, mat[0].length];
    const g: number[][] = Array.from({ length: m + n }, () => []);
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            g[m - i + j].push(mat[i][j]);
        }
    }
    for (const e of g) {
        e.sort((a, b) => b - a);
    }
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            mat[i][j] = g[m - i + j].pop()!;
        }
    }
    return mat;
}
```

#### Rust

```rust
impl Solution {
    pub fn diagonal_sort(mut mat: Vec<Vec<i32>>) -> Vec<Vec<i32>> {
        let m = mat.len();
        let n = mat[0].len();
        let mut g: Vec<Vec<i32>> = vec![vec![]; m + n];
        for i in 0..m {
            for j in 0..n {
                g[m - i + j].push(mat[i][j]);
            }
        }
        for e in &mut g {
            e.sort_by(|a, b| b.cmp(a));
        }
        for i in 0..m {
            for j in 0..n {
                mat[i][j] = g[m - i + j].pop().unwrap();
            }
        }
        mat
    }
}
```

#### C#

```cs
public class Solution {
    public int[][] DiagonalSort(int[][] mat) {
        int m = mat.Length;
        int n = mat[0].Length;
        List<List<int>> g = new List<List<int>>();
        for (int i = 0; i < m + n; i++) {
            g.Add(new List<int>());
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                g[m - i + j].Add(mat[i][j]);
            }
        }
        foreach (var e in g) {
            e.Sort((a, b) => b.CompareTo(a));
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                int val = g[m - i + j][g[m - i + j].Count - 1];
                g[m - i + j].RemoveAt(g[m - i + j].Count - 1);
                mat[i][j] = val;
            }
        }
        return mat;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
