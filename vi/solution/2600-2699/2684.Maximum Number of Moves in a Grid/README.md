---
comments: true
difficulty: Medium
rating: 1625
source: Weekly Contest 345 Q3
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [2684. Maximum Number of Moves in a Grid](https://leetcode.com/problems/maximum-number-of-moves-in-a-grid)

[中文文档](/solution/2600-2699/2684.Maximum%20Number%20of%20Moves%20in%20a%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một ma trận <code>m x n</code> <code>grid</code> được đánh chỉ số từ <strong>0</strong>, gồm các số nguyên <strong>dương</strong>.</p>

<p>Bạn có thể bắt đầu tại <strong>bất kỳ</strong> ô nào ở cột đầu tiên của ma trận và duyệt qua lưới theo cách sau:</p>

<ul>
	<li>Từ một ô <code>(row, col)</code>, bạn có thể di chuyển đến một trong các ô <code>(row - 1, col + 1)</code>, <code>(row, col + 1)</code> và <code>(row + 1, col + 1)</code>, với điều kiện giá trị của ô đích phải <strong>lớn hơn</strong> giá trị của ô hiện tại.</li>
</ul>

<p>Hãy trả về <em><strong>số lần di chuyển</strong> <strong>lớn nhất</strong> mà bạn có thể thực hiện.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2684.Maximum%20Number%20of%20Moves%20in%20a%20Grid/images/yetgriddrawio-10.png" style="width: 201px; height: 201px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[2,4,3,5],[5,4,9,3],[3,4,2,11],[10,9,13,15]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có thể bắt đầu tại ô (0, 0) và thực hiện các bước di chuyển sau:
- (0, 0) -&gt; (0, 1).
- (0, 1) -&gt; (1, 2).
- (1, 2) -&gt; (2, 3).
Có thể chứng minh rằng đây là số lần di chuyển lớn nhất có thể thực hiện.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2684.Maximum%20Number%20of%20Moves%20in%20a%20Grid/images/yetgrid4drawio.png" />
<strong>Đầu vào:</strong> grid = [[3,2,4],[2,1,9],[1,1,7]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Khi bắt đầu từ bất kỳ ô nào ở cột đầu tiên, ta không thể thực hiện bước di chuyển nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>2 &lt;= m, n &lt;= 1000</code></li>
	<li><code>4 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= grid[i][j] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước di chuyển đi đến một ô lớn hơn nghiêm ngặt ở cột kế tiếp, với hàng có thể tăng, giữ nguyên hoặc giảm một đơn vị. Nếu thực hiện DFS riêng từ mỗi điểm bắt đầu, ta sẽ duyệt lại các ô. Chỉ cần BFS theo từng cột: các hàng có thể đi đến ở cột $j$ sẽ sinh ra các ứng viên ở cột $j+1$.
>
> Một set lưu các hàng có thể đi đến; nếu set kế tiếp rỗng thì trả về số cột đã đi qua, còn nếu đến được cột cuối thì kết quả là $n-1$.

<!-- thinking:end -->

Ta định nghĩa một queue $q$ và ban đầu thêm tất cả tọa độ hàng của cột đầu tiên vào queue.

Tiếp theo, ta bắt đầu từ cột đầu tiên và duyệt lần lượt qua từng cột. Với mỗi cột, ta lấy từng tọa độ hàng trong queue ra. Với mỗi tọa độ hàng $i$, ta tìm tất cả tọa độ hàng $k$ có thể đi đến ở cột kế tiếp sao cho thỏa mãn $grid[i][j] < grid[k][j + 1]$, rồi thêm các tọa độ hàng đó vào một set mới $t$. Nếu $t$ rỗng, nghĩa là ta không thể tiếp tục di chuyển, nên trả về số thứ tự của cột hiện tại. Ngược lại, ta gán $t$ cho $q$ và tiếp tục duyệt cột kế tiếp.

Cuối cùng, nếu đã duyệt qua tất cả các cột, nghĩa là ta có thể đi đến cột cuối, nên trả về $n - 1$.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxMoves(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        q = set(range(m))
        for j in range(n - 1):
            t = set()
            for i in q:
                for k in range(i - 1, i + 2):
                    if 0 <= k < m and grid[i][j] < grid[k][j + 1]:
                        t.add(k)
            if not t:
                return j
            q = t
        return n - 1
```

#### Java

```java
class Solution {
    public int maxMoves(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        Set<Integer> q = IntStream.range(0, m).boxed().collect(Collectors.toSet());
        for (int j = 0; j < n - 1; ++j) {
            Set<Integer> t = new HashSet<>();
            for (int i : q) {
                for (int k = i - 1; k <= i + 1; ++k) {
                    if (k >= 0 && k < m && grid[i][j] < grid[k][j + 1]) {
                        t.add(k);
                    }
                }
            }
            if (t.isEmpty()) {
                return j;
            }
            q = t;
        }
        return n - 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxMoves(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        unordered_set<int> q, t;
        for (int i = 0; i < m; ++i) {
            q.insert(i);
        }
        for (int j = 0; j < n - 1; ++j) {
            t.clear();
            for (int i : q) {
                for (int k = i - 1; k <= i + 1; ++k) {
                    if (k >= 0 && k < m && grid[i][j] < grid[k][j + 1]) {
                        t.insert(k);
                    }
                }
            }
            if (t.empty()) {
                return j;
            }
            q.swap(t);
        }
        return n - 1;
    }
};
```

#### Go

```go
func maxMoves(grid [][]int) (ans int) {
	m, n := len(grid), len(grid[0])
	q := map[int]bool{}
	for i := range grid {
		q[i] = true
	}
	for j := 0; j < n-1; j++ {
		t := map[int]bool{}
		for i := range q {
			for k := i - 1; k <= i+1; k++ {
				if k >= 0 && k < m && grid[i][j] < grid[k][j+1] {
					t[k] = true
				}
			}
		}
		if len(t) == 0 {
			return j
		}
		q = t
	}
	return n - 1
}
```

#### TypeScript

```ts
function maxMoves(grid: number[][]): number {
    const m = grid.length;
    const n = grid[0].length;
    let q = new Set<number>(Array.from({ length: m }, (_, i) => i));
    for (let j = 0; j < n - 1; ++j) {
        const t = new Set<number>();
        for (const i of q) {
            for (let k = i - 1; k <= i + 1; ++k) {
                if (k >= 0 && k < m && grid[i][j] < grid[k][j + 1]) {
                    t.add(k);
                }
            }
        }
        if (t.size === 0) {
            return j;
        }
        q = t;
    }
    return n - 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
