---
comments: true
difficulty: Medium
rating: 1286
source: Weekly Contest 303 Q2
tags:
    - Array
    - Hash Table
    - Matrix
    - Simulation
---

<!-- problem:start -->

# [2352. Equal Row and Column Pairs](https://leetcode.com/problems/equal-row-and-column-pairs)

[中文文档](/solution/2300-2399/2352.Equal%20Row%20and%20Column%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận số nguyên <code>n x n</code> <code>grid</code> được đánh chỉ số từ <strong>0</strong>, hãy <em>trả về số cặp </em><code>(r<sub>i</sub>, c<sub>j</sub>)</code><em> sao cho hàng </em><code>r<sub>i</sub></code><em> và cột </em><code>c<sub>j</sub></code><em> bằng nhau</em>.</p>

<p>Một cặp hàng và cột được xem là bằng nhau nếu chúng chứa cùng các phần tử theo cùng thứ tự (tức là hai mảng bằng nhau).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2352.Equal%20Row%20and%20Column%20Pairs/images/ex1.jpg" style="width: 150px; height: 153px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[3,2,1],[1,7,6],[2,7,7]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Có 1 cặp hàng và cột bằng nhau:
- (Hàng 2, Cột 1): [2,7,7]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2352.Equal%20Row%20and%20Column%20Pairs/images/ex2.jpg" style="width: 200px; height: 209px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[3,1,2,2],[1,4,4,5],[2,4,2,2],[2,4,2,2]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có 3 cặp hàng và cột bằng nhau:
- (Hàng 0, Cột 0): [3,1,2,2]
- (Hàng 2, Cột 2): [2,4,2,2]
- (Hàng 3, Cột 2): [2,4,2,2]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == grid.length == grid[i].length</code></li>
	<li><code>1 &lt;= n &lt;= 200</code></li>
	<li><code>1 &lt;= grid[i][j] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các cặp hàng và cột bằng nhau. Vì $n \le 200$, việc so sánh $n$ vị trí với độ phức tạp $O(n^3)$ là chấp nhận được.
>
> Với mỗi hàng $i$ và cột $j$, kiểm tra $grid[i][k]=grid[k][j]$ với mọi $k$. Không cần băm toàn bộ các hàng.

<!-- thinking:end -->

Ta so sánh trực tiếp từng hàng và cột của ma trận $grid$. Nếu chúng bằng nhau, đó là một cặp hàng và cột bằng nhau, khi đó tăng đáp án lên một.

Độ phức tạp thời gian là $O(n^3)$, trong đó $n$ là số hàng hoặc số cột của ma trận $grid$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def equalPairs(self, grid: List[List[int]]) -> int:
        n = len(grid)
        ans = 0
        for i in range(n):
            for j in range(n):
                ans += all(grid[i][k] == grid[k][j] for k in range(n))
        return ans
```

#### Java

```java
class Solution {
    public int equalPairs(int[][] grid) {
        int n = grid.length;
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                int ok = 1;
                for (int k = 0; k < n; ++k) {
                    if (grid[i][k] != grid[k][j]) {
                        ok = 0;
                        break;
                    }
                }
                ans += ok;
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
    int equalPairs(vector<vector<int>>& grid) {
        int n = grid.size();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                int ok = 1;
                for (int k = 0; k < n; ++k) {
                    if (grid[i][k] != grid[k][j]) {
                        ok = 0;
                        break;
                    }
                }
                ans += ok;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func equalPairs(grid [][]int) (ans int) {
	for i := range grid {
		for j := range grid {
			ok := 1
			for k := range grid {
				if grid[i][k] != grid[k][j] {
					ok = 0
					break
				}
			}
			ans += ok
		}
	}
	return
}
```

#### TypeScript

```ts
function equalPairs(grid: number[][]): number {
    const n = grid.length;
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < n; ++j) {
            let ok = 1;
            for (let k = 0; k < n; ++k) {
                if (grid[i][k] !== grid[k][j]) {
                    ok = 0;
                    break;
                }
            }
            ans += ok;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
