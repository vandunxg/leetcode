---
comments: true
difficulty: Medium
tags:
    - Array
    - Math
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [750. Number Of Corner Rectangles 🔒](https://leetcode.com/problems/number-of-corner-rectangles)

[中文文档](/solution/0700-0799/0750.Number%20Of%20Corner%20Rectangles/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận số nguyên <code>grid</code> kích thước <code>m x n</code>, trong đó mỗi phần tử chỉ có thể là <code>0</code> hoặc <code>1</code>. Hãy trả về <em>số lượng <strong>hình chữ nhật có bốn góc là 1</strong></em>.</p>

<p><strong>Hình chữ nhật có bốn góc là 1</strong> gồm bốn giá trị <code>1</code> khác nhau trên grid, tạo thành hình chữ nhật có các cạnh song song với trục. Chỉ các ô ở bốn góc cần có giá trị <code>1</code>. Cả bốn giá trị <code>1</code> phải nằm ở các ô khác nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0750.Number%20Of%20Corner%20Rectangles/images/cornerrec1-grid.jpg" style="width: 413px; height: 333px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,0,0,1,0],[0,0,1,0,1],[0,0,0,1,0],[1,0,1,0,1]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Chỉ có một hình chữ nhật, với các góc tại grid[1][2], grid[1][4], grid[3][2], grid[3][4].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0750.Number%20Of%20Corner%20Rectangles/images/cornerrec2-grid.jpg" style="width: 253px; height: 253px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,1,1],[1,1,1],[1,1,1]]
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Có bốn hình chữ nhật 2x2, bốn hình chữ nhật 2x3 và 3x2, cùng một hình chữ nhật 3x3.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0750.Number%20Of%20Corner%20Rectangles/images/cornerrec3-grid.jpg" style="width: 333px; height: 93px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,1,1,1]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Hình chữ nhật phải có bốn góc khác nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 200</code></li>
	<li><code>grid[i][j]</code> là <code>0</code> hoặc <code>1</code>.</li>
	<li>Số lượng giá trị <code>1</code> trong grid nằm trong khoảng <code>[1, 6000]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash table + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các hình chữ nhật có cạnh song song với trục và cả bốn góc là $1$. Liệt kê mọi bộ đỉnh quá chậm với kích thước $200\times 200$; vì có tối đa $6000$ số 1, ta xét các cặp số 1 trong từng hàng.
>
> Hai hàng tạo thành hình chữ nhật khi có hai cột cùng chứa số 1 ở cả hai hàng. Với mỗi cặp số 1 trong hàng hiện tại, số hàng trước đó có cùng cặp cột chính là số hình chữ nhật mới được tạo.
>
> Dùng counter đếm các cặp cột: cộng $\textit{cnt}[(i,j)]$ vào đáp án rồi tăng giá trị này lên. Độ phức tạp là $O(mn^2)$.

<!-- thinking:end -->

Ta lần lượt xem mỗi hàng là cạnh dưới của hình chữ nhật. Với hàng hiện tại, nếu cả cột $i$ và cột $j$ đều là $1$, ta dùng hash table để tìm số hàng trước đó cũng có số 1 ở cả hai cột này. Đây là số hình chữ nhật được tạo với cặp cột $(i, j)$ làm hai góc dưới; cộng số đó vào đáp án. Sau đó tăng bộ đếm của cặp $(i, j)$ trong hash table rồi tiếp tục xét cặp tiếp theo $(i, j)$.

Độ phức tạp thời gian là $O(m \times n^2)$ và độ phức tạp không gian là $O(n^2)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countCornerRectangles(self, grid: List[List[int]]) -> int:
        ans = 0
        cnt = Counter()
        n = len(grid[0])
        for row in grid:
            for i, c1 in enumerate(row):
                if c1:
                    for j in range(i + 1, n):
                        if row[j]:
                            ans += cnt[(i, j)]
                            cnt[(i, j)] += 1
        return ans
```

#### Java

```java
class Solution {
    public int countCornerRectangles(int[][] grid) {
        int n = grid[0].length;
        int ans = 0;
        Map<List<Integer>, Integer> cnt = new HashMap<>();
        for (var row : grid) {
            for (int i = 0; i < n; ++i) {
                if (row[i] == 1) {
                    for (int j = i + 1; j < n; ++j) {
                        if (row[j] == 1) {
                            List<Integer> t = List.of(i, j);
                            ans += cnt.getOrDefault(t, 0);
                            cnt.merge(t, 1, Integer::sum);
                        }
                    }
                }
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
    int countCornerRectangles(vector<vector<int>>& grid) {
        int n = grid[0].size();
        int ans = 0;
        map<pair<int, int>, int> cnt;
        for (auto& row : grid) {
            for (int i = 0; i < n; ++i) {
                if (row[i]) {
                    for (int j = i + 1; j < n; ++j) {
                        if (row[j]) {
                            ans += cnt[{i, j}];
                            ++cnt[{i, j}];
                        }
                    }
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countCornerRectangles(grid [][]int) (ans int) {
	n := len(grid[0])
	type pair struct{ x, y int }
	cnt := map[pair]int{}
	for _, row := range grid {
		for i, x := range row {
			if x == 1 {
				for j := i + 1; j < n; j++ {
					if row[j] == 1 {
						t := pair{i, j}
						ans += cnt[t]
						cnt[t]++
					}
				}
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function countCornerRectangles(grid: number[][]): number {
    const n = grid[0].length;
    let ans = 0;
    const cnt: Map<number, number> = new Map();
    for (const row of grid) {
        for (let i = 0; i < n; ++i) {
            if (row[i] === 1) {
                for (let j = i + 1; j < n; ++j) {
                    if (row[j] === 1) {
                        const t = i * 200 + j;
                        ans += cnt.get(t) ?? 0;
                        cnt.set(t, (cnt.get(t) ?? 0) + 1);
                    }
                }
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
