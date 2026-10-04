---
comments: true
difficulty: Easy
rating: 1235
source: Weekly Contest 370 Q1
tags:
    - Array
    - Matrix
---

<!-- problem:start -->

# [2923. Find Champion I](https://leetcode.com/problems/find-champion-i)

[中文文档](/solution/2900-2999/2923.Find%20Champion%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> đội được đánh số từ <code>0</code> đến <code>n - 1</code> trong một giải đấu.</p>

<p>Cho một ma trận boolean 2D <strong>đánh chỉ số từ 0</strong> <code>grid</code> có kích thước <code>n * n</code>. Với mọi <code>i, j</code> thỏa mãn <code>0 &lt;= i, j &lt;= n - 1</code> và <code>i != j</code>, đội <code>i</code> <strong>mạnh hơn</strong> đội <code>j</code> nếu <code>grid[i][j] == 1</code>; ngược lại, đội <code>j</code> <strong>mạnh hơn</strong> đội <code>i</code>.</p>

<p>Đội <code>a</code> sẽ là <strong>nhà vô địch</strong> của giải đấu nếu không có đội <code>b</code> nào mạnh hơn đội <code>a</code>.</p>

<p>Trả về <em>đội sẽ là nhà vô địch của giải đấu.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[0,1],[0,0]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Có hai đội trong giải đấu này.
grid[0][1] == 1 có nghĩa là đội 0 mạnh hơn đội 1. Vì vậy, đội 0 sẽ là nhà vô địch.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[0,0,1],[1,0,1],[0,0,0]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Có ba đội trong giải đấu này.
grid[1][0] == 1 có nghĩa là đội 1 mạnh hơn đội 0.
grid[1][2] == 1 có nghĩa là đội 1 mạnh hơn đội 2.
Vì vậy, đội 1 sẽ là nhà vô địch.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>2 &lt;= n &lt;= 100</code></li>
	<li><code>grid[i][j]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
	<li>Với mọi <code>i grid[i][i]</code> là <code>0.</code></li>
	<li>Với mọi <code>i, j</code> thỏa mãn <code>i != j</code>, <code>grid[i][j] != grid[j][i]</code>.</li>
	<li>Dữ liệu đầu vào được tạo sao cho nếu đội <code>a</code> mạnh hơn đội <code>b</code> và đội <code>b</code> mạnh hơn đội <code>c</code>, thì đội <code>a</code> mạnh hơn đội <code>c</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> $grid$ là một phép so sánh đầy đủ và có tính bắc cầu; nhà vô địch là đội đánh bại mọi đội khác. Vì $n \le 100$, chỉ cần kiểm tra xem mỗi hàng có toàn số 1 ở các vị trí ngoài đường chéo hay không.
>
> Đầu vào luôn có duy nhất một nhà vô địch, nên có thể trả về đội đầu tiên thỏa mãn điều kiện. Không cần đồ thị hay mảng in-degree.

<!-- thinking:end -->

Ta có thể duyệt từng đội $i$. Nếu đội $i$ đã thắng mọi trận đấu, thì đội $i$ là nhà vô địch và ta có thể trả về trực tiếp $i$.

Độ phức tạp thời gian là $O(n^2)$, trong đó $n$ là số đội. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findChampion(self, grid: List[List[int]]) -> int:
        for i, row in enumerate(grid):
            if all(x == 1 for j, x in enumerate(row) if i != j):
                return i
```

#### Java

```java
class Solution {
    public int findChampion(int[][] grid) {
        int n = grid.length;
        for (int i = 0;; ++i) {
            int cnt = 0;
            for (int j = 0; j < n; ++j) {
                if (i != j && grid[i][j] == 1) {
                    ++cnt;
                }
            }
            if (cnt == n - 1) {
                return i;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findChampion(vector<vector<int>>& grid) {
        int n = grid.size();
        for (int i = 0;; ++i) {
            int cnt = 0;
            for (int j = 0; j < n; ++j) {
                if (i != j && grid[i][j] == 1) {
                    ++cnt;
                }
            }
            if (cnt == n - 1) {
                return i;
            }
        }
    }
};
```

#### Go

```go
func findChampion(grid [][]int) int {
	n := len(grid)
	for i := 0; ; i++ {
		cnt := 0
		for j, x := range grid[i] {
			if i != j && x == 1 {
				cnt++
			}
		}
		if cnt == n-1 {
			return i
		}
	}
}
```

#### TypeScript

```ts
function findChampion(grid: number[][]): number {
    for (let i = 0, n = grid.length; ; ++i) {
        let cnt = 0;
        for (let j = 0; j < n; ++j) {
            if (i !== j && grid[i][j] === 1) {
                ++cnt;
            }
        }
        if (cnt === n - 1) {
            return i;
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
