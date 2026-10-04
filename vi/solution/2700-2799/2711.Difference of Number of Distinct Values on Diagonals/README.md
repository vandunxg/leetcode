---
comments: true
difficulty: Medium
rating: 1428
source: Weekly Contest 347 Q2
tags:
    - Array
    - Hash Table
    - Matrix
---

<!-- problem:start -->

# [2711. Difference of Number of Distinct Values on Diagonals](https://leetcode.com/problems/difference-of-number-of-distinct-values-on-diagonals)

[中文文档](/solution/2700-2799/2711.Difference%20of%20Number%20of%20Distinct%20Values%20on%20Diagonals/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một <code>grid</code> hai chiều có kích thước <code>m x n</code>, hãy tìm ma trận <code>answer</code> có kích thước <code>m x n</code>.</p>

<p>Ô <code>answer[r][c]</code> được tính dựa trên các giá trị trên đường chéo của ô <code>grid[r][c]</code>:</p>

<ul>
	<li>Gọi <code>leftAbove[r][c]</code> là số lượng giá trị <strong>phân biệt</strong> trên đường chéo nằm bên trái và phía trên ô <code>grid[r][c]</code>, không tính chính ô <code>grid[r][c]</code>.</li>
	<li>Gọi <code>rightBelow[r][c]</code> là số lượng giá trị <strong>phân biệt</strong> trên đường chéo nằm bên phải và phía dưới ô <code>grid[r][c]</code>, không tính chính ô <code>grid[r][c]</code>.</li>
	<li>Khi đó, <code>answer[r][c] = |leftAbove[r][c] - rightBelow[r][c]|</code>.</li>
</ul>

<p><strong>Đường chéo của ma trận</strong> là một đường chéo gồm các ô, bắt đầu từ một ô bất kỳ trên hàng đầu tiên hoặc cột đầu tiên, đi theo hướng xuống dưới và sang phải cho đến khi tới biên của ma trận.</p>

<ul>
	<li>Ví dụ, trong hình dưới đây, đường chéo chứa ô có chỉ số <code>(2, 3)</code> được tô màu xám:

    <ul>
    <li>Các ô màu đỏ nằm bên trái và phía trên ô đó.</li>
    <li>Các ô màu xanh dương nằm bên phải và phía dưới ô đó.</li>
    </ul>
    </li>

</ul>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2700-2799/2711.Difference%20of%20Number%20of%20Distinct%20Values%20on%20Diagonals/images/diagonal.png" style="width: 200px; height: 160px;" /></p>

<p>Trả về ma trận <code>answer</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,2,3],[3,1,5],[3,2,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">Output: [[1,1,0],[1,0,1],[0,1,1]]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Để tính các ô <code>answer</code>:</p>

<table>
	<thead>
		<tr>
			<th>answer</th>
			<th>các phần tử bên trái-phía trên</th>
			<th>leftAbove</th>
			<th>các phần tử bên phải-phía dưới</th>
			<th>rightBelow</th>
			<th>|leftAbove - rightBelow|</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>[0][0]</td>
			<td>[]</td>
			<td>0</td>
			<td>[grid[1][1], grid[2][2]]</td>
			<td>|{1, 1}| = 1</td>
			<td>1</td>
		</tr>
		<tr>
			<td>[0][1]</td>
			<td>[]</td>
			<td>0</td>
			<td>[grid[1][2]]</td>
			<td>|{5}| = 1</td>
			<td>1</td>
		</tr>
		<tr>
			<td>[0][2]</td>
			<td>[]</td>
			<td>0</td>
			<td>[]</td>
			<td>0</td>
			<td>0</td>
		</tr>
		<tr>
			<td>[1][0]</td>
			<td>[]</td>
			<td>0</td>
			<td>[grid[2][1]]</td>
			<td>|{2}| = 1</td>
			<td>1</td>
		</tr>
		<tr>
			<td>[1][1]</td>
			<td>[grid[0][0]]</td>
			<td>|{1}| = 1</td>
			<td>[grid[2][2]]</td>
			<td>|{1}| = 1</td>
			<td>0</td>
		</tr>
		<tr>
			<td>[1][2]</td>
			<td>[grid[0][1]]</td>
			<td>|{2}| = 1</td>
			<td>[]</td>
			<td>0</td>
			<td>1</td>
		</tr>
		<tr>
			<td>[2][0]</td>
			<td>[]</td>
			<td>0</td>
			<td>[]</td>
			<td>0</td>
			<td>0</td>
		</tr>
		<tr>
			<td>[2][1]</td>
			<td>[grid[1][0]]</td>
			<td>|{3}| = 1</td>
			<td>[]</td>
			<td>0</td>
			<td>1</td>
		</tr>
		<tr>
			<td>[2][2]</td>
			<td>[grid[0][0], grid[1][1]]</td>
			<td>|{1, 1}| = 1</td>
			<td>[]</td>
			<td>0</td>
			<td>1</td>
		</tr>
	</tbody>
</table>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">Output: [[0]]</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n, grid[i][j] &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Đáp án tại một ô là hiệu tuyệt đối giữa số lượng giá trị phân biệt trên đường chéo phía trên-bên trái và phía dưới-bên phải của ô đó. Vì mỗi chiều không vượt quá $50$, việc duyệt cả hai đường chéo từ mỗi ô và đưa các giá trị vào một set có độ phức tạp $O(mn\min(m,n))$, hoàn toàn phù hợp.
>
> Không cần tiền xử lý toàn bộ các đường chéo: từ $(i,j)$, ta duyệt về phía trên-bên trái và phía dưới-bên phải, lấy kích thước của các set rồi tính hiệu.

<!-- thinking:end -->

Ta có thể mô phỏng quá trình được mô tả trong đề bài: với mỗi ô, tính số lượng giá trị phân biệt trên đường chéo phía trên-bên trái $tl$ và đường chéo phía dưới-bên phải $br$, sau đó tính hiệu tuyệt đối $|tl - br|$.

Độ phức tạp thời gian là $O(m \times n \times \min(m, n))$, còn độ phức tạp không gian là $O(m \times n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def differenceOfDistinctValues(self, grid: List[List[int]]) -> List[List[int]]:
        m, n = len(grid), len(grid[0])
        ans = [[0] * n for _ in range(m)]
        for i in range(m):
            for j in range(n):
                x, y = i, j
                s = set()
                while x and y:
                    x, y = x - 1, y - 1
                    s.add(grid[x][y])
                tl = len(s)
                x, y = i, j
                s = set()
                while x + 1 < m and y + 1 < n:
                    x, y = x + 1, y + 1
                    s.add(grid[x][y])
                br = len(s)
                ans[i][j] = abs(tl - br)
        return ans
```

#### Java

```java
class Solution {
    public int[][] differenceOfDistinctValues(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[][] ans = new int[m][n];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int x = i, y = j;
                Set<Integer> s = new HashSet<>();
                while (x > 0 && y > 0) {
                    s.add(grid[--x][--y]);
                }
                int tl = s.size();
                x = i;
                y = j;
                s.clear();
                while (x < m - 1 && y < n - 1) {
                    s.add(grid[++x][++y]);
                }
                int br = s.size();
                ans[i][j] = Math.abs(tl - br);
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
    vector<vector<int>> differenceOfDistinctValues(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<vector<int>> ans(m, vector<int>(n));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int x = i, y = j;
                unordered_set<int> s;
                while (x > 0 && y > 0) {
                    s.insert(grid[--x][--y]);
                }
                int tl = s.size();
                x = i;
                y = j;
                s.clear();
                while (x < m - 1 && y < n - 1) {
                    s.insert(grid[++x][++y]);
                }
                int br = s.size();
                ans[i][j] = abs(tl - br);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func differenceOfDistinctValues(grid [][]int) [][]int {
	m, n := len(grid), len(grid[0])
	ans := make([][]int, m)
	for i := range grid {
		ans[i] = make([]int, n)
		for j := range grid[i] {
			x, y := i, j
			s := map[int]bool{}
			for x > 0 && y > 0 {
				x, y = x-1, y-1
				s[grid[x][y]] = true
			}
			tl := len(s)
			x, y = i, j
			s = map[int]bool{}
			for x+1 < m && y+1 < n {
				x, y = x+1, y+1
				s[grid[x][y]] = true
			}
			br := len(s)
			ans[i][j] = abs(tl - br)
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
function differenceOfDistinctValues(grid: number[][]): number[][] {
    const m = grid.length;
    const n = grid[0].length;
    const ans: number[][] = Array(m)
        .fill(0)
        .map(() => Array(n).fill(0));
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            let [x, y] = [i, j];
            const s = new Set<number>();
            while (x && y) {
                s.add(grid[--x][--y]);
            }
            const tl = s.size;
            [x, y] = [i, j];
            s.clear();
            while (x + 1 < m && y + 1 < n) {
                s.add(grid[++x][++y]);
            }
            const br = s.size;
            ans[i][j] = Math.abs(tl - br);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
