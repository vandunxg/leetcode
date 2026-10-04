---
comments: true
difficulty: Medium
rating: 2074
source: Weekly Contest 367 Q4
tags:
    - Array
    - Matrix
    - Prefix Sum
---

<!-- problem:start -->

# [2906. Construct Product Matrix](https://leetcode.com/problems/construct-product-matrix)

[中文文档](/solution/2900-2999/2906.Construct%20Product%20Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận số nguyên 2 chiều <code><font face="monospace">grid</font></code><font face="monospace"> </font><strong>được đánh chỉ số từ 0</strong>, có kích thước <code>n * m</code>, ta định nghĩa một ma trận 2 chiều <code>p</code> <strong>được đánh chỉ số từ 0</strong>, cũng có kích thước <code>n * m</code>, là <strong>ma trận tích</strong> của <code>grid</code> nếu thỏa mãn điều kiện sau:</p>

<ul>
	<li>Mỗi phần tử <code>p[i][j]</code> được tính bằng tích của tất cả phần tử trong <code>grid</code>, ngoại trừ phần tử <code>grid[i][j]</code>. Sau đó lấy tích này modulo <code><font face="monospace">12345</font></code>.</li>
</ul>

<p>Trả về <em>ma trận tích của</em> <code><font face="monospace">grid</font></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,2],[3,4]]
<strong>Đầu ra:</strong> [[24,12],[8,6]]
<strong>Giải thích:</strong> p[0][0] = grid[0][1] * grid[1][0] * grid[1][1] = 2 * 3 * 4 = 24
p[0][1] = grid[0][0] * grid[1][0] * grid[1][1] = 1 * 3 * 4 = 12
p[1][0] = grid[0][0] * grid[0][1] * grid[1][1] = 1 * 2 * 4 = 8
p[1][1] = grid[0][0] * grid[0][1] * grid[1][0] = 1 * 2 * 3 = 6
Vì vậy, đáp án là [[24,12],[8,6]].</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[12345],[2],[1]]
<strong>Đầu ra:</strong> [[2],[0],[0]]
<strong>Giải thích:</strong> p[0][0] = grid[0][1] * grid[0][2] = 2 * 1 = 2.
p[0][1] = grid[0][0] * grid[0][2] = 12345 * 1 = 12345. 12345 % 12345 = 0. Vì vậy p[0][1] = 0.
p[0][2] = grid[0][0] * grid[0][1] = 12345 * 2 = 24690. 24690 % 12345 = 0. Vì vậy p[0][2] = 0.
Vì vậy, đáp án là [[2],[0],[0]].</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == grid.length&nbsp;&lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= m == grid[i].length&nbsp;&lt;= 10<sup>5</sup></code></li>
	<li><code>2 &lt;= n * m &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= grid[i][j] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân rã tích prefix và suffix

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi ô cần lưu tích của tất cả phần tử khác, lấy modulo $12345$. Vì modulo không phải số nguyên tố nên không thể dùng nghịch đảo của $grid[i][j]$, đồng thời một tích là bội của $12345$ sẽ khiến phép chia không thể thực hiện được. Với $n \cdot m \le 10^5$, ta có thể trải phẳng ma trận để đưa bài toán về dạng product-except-self.
>
> Duyệt từ góc dưới bên phải, ghi tích suffix $suf$ (không bao gồm chính nó) vào $p$, sau đó duyệt từ góc trên bên trái và nhân với tích prefix $pre$. Cả hai bước cập nhật đều thực hiện trong modulo.

<!-- thinking:end -->

Ta có thể tiền xử lý tích suffix (không bao gồm chính nó) của mỗi phần tử, sau đó duyệt qua ma trận để tính tích prefix (không bao gồm chính nó) của mỗi phần tử. Tích của hai giá trị này cho ta kết quả tại mỗi vị trí.

Cụ thể, ta dùng $p[i][j]$ để biểu diễn kết quả của phần tử tại hàng thứ $i$ và cột thứ $j$ trong ma trận. Ta định nghĩa biến $suf$ là tích của tất cả phần tử nằm bên dưới và bên phải vị trí hiện tại. Ban đầu, $suf$ được đặt bằng $1$. Ta bắt đầu duyệt từ góc dưới bên phải của ma trận. Với mỗi vị trí $(i, j)$, ta gán $suf$ cho $p[i][j]$, sau đó cập nhật $suf$ thành $suf \times grid[i][j] \bmod 12345$. Nhờ đó, ta thu được tích suffix của mỗi vị trí.

Tiếp theo, ta bắt đầu duyệt từ góc trên bên trái của ma trận. Với mỗi vị trí $(i, j)$, ta nhân $p[i][j]$ với $pre$, lấy kết quả modulo $12345$, sau đó cập nhật $pre$ thành $pre \times grid[i][j] \bmod 12345$. Nhờ đó, ta thu được tích prefix của mỗi vị trí.

Sau khi duyệt xong, ta trả về ma trận kết quả $p$.

Độ phức tạp thời gian là $O(n \times m)$, trong đó $n$ và $m$ lần lượt là số hàng và số cột của ma trận. Không tính phần không gian được sử dụng bởi ma trận kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def constructProductMatrix(self, grid: List[List[int]]) -> List[List[int]]:
        n, m = len(grid), len(grid[0])
        p = [[0] * m for _ in range(n)]
        mod = 12345
        suf = 1
        for i in range(n - 1, -1, -1):
            for j in range(m - 1, -1, -1):
                p[i][j] = suf
                suf = suf * grid[i][j] % mod
        pre = 1
        for i in range(n):
            for j in range(m):
                p[i][j] = p[i][j] * pre % mod
                pre = pre * grid[i][j] % mod
        return p
```

#### Java

```java
class Solution {
    public int[][] constructProductMatrix(int[][] grid) {
        final int mod = 12345;
        int n = grid.length, m = grid[0].length;
        int[][] p = new int[n][m];
        long suf = 1;
        for (int i = n - 1; i >= 0; --i) {
            for (int j = m - 1; j >= 0; --j) {
                p[i][j] = (int) suf;
                suf = suf * grid[i][j] % mod;
            }
        }
        long pre = 1;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                p[i][j] = (int) (p[i][j] * pre % mod);
                pre = pre * grid[i][j] % mod;
            }
        }
        return p;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> constructProductMatrix(vector<vector<int>>& grid) {
        const int mod = 12345;
        int n = grid.size(), m = grid[0].size();
        vector<vector<int>> p(n, vector<int>(m));
        long long suf = 1;
        for (int i = n - 1; i >= 0; --i) {
            for (int j = m - 1; j >= 0; --j) {
                p[i][j] = suf;
                suf = suf * grid[i][j] % mod;
            }
        }
        long long pre = 1;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                p[i][j] = p[i][j] * pre % mod;
                pre = pre * grid[i][j] % mod;
            }
        }
        return p;
    }
};
```

#### Go

```go
func constructProductMatrix(grid [][]int) [][]int {
	const mod int = 12345
	n, m := len(grid), len(grid[0])
	p := make([][]int, n)
	for i := range p {
		p[i] = make([]int, m)
	}
	suf := 1
	for i := n - 1; i >= 0; i-- {
		for j := m - 1; j >= 0; j-- {
			p[i][j] = suf
			suf = suf * grid[i][j] % mod
		}
	}
	pre := 1
	for i := 0; i < n; i++ {
		for j := 0; j < m; j++ {
			p[i][j] = p[i][j] * pre % mod
			pre = pre * grid[i][j] % mod
		}
	}
	return p
}
```

#### TypeScript

```ts
function constructProductMatrix(grid: number[][]): number[][] {
    const mod = 12345;
    const [n, m] = [grid.length, grid[0].length];
    const p: number[][] = Array.from({ length: n }, () => Array.from({ length: m }, () => 0));
    let suf = 1;
    for (let i = n - 1; ~i; --i) {
        for (let j = m - 1; ~j; --j) {
            p[i][j] = suf;
            suf = (suf * grid[i][j]) % mod;
        }
    }
    let pre = 1;
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < m; ++j) {
            p[i][j] = (p[i][j] * pre) % mod;
            pre = (pre * grid[i][j]) % mod;
        }
    }
    return p;
}
```

#### Rust

```rust
impl Solution {
    pub fn construct_product_matrix(grid: Vec<Vec<i32>>) -> Vec<Vec<i32>> {
        let modulo: i32 = 12345;
        let n = grid.len();
        let m = grid[0].len();
        let mut p: Vec<Vec<i32>> = vec![vec![0; m]; n];
        let mut suf = 1;

        for i in (0..n).rev() {
            for j in (0..m).rev() {
                p[i][j] = suf;
                suf = (((suf as i64) * (grid[i][j] as i64)) % (modulo as i64)) as i32;
            }
        }

        let mut pre = 1;

        for i in 0..n {
            for j in 0..m {
                p[i][j] = (((p[i][j] as i64) * (pre as i64)) % (modulo as i64)) as i32;
                pre = (((pre as i64) * (grid[i][j] as i64)) % (modulo as i64)) as i32;
            }
        }

        p
    }
}
```

#### JavaScript

```js
/**
 * @param {number[][]} grid
 * @return {number[][]}
 */
var constructProductMatrix = function (grid) {
    const mod = 12345;
    const [n, m] = [grid.length, grid[0].length];
    const p = Array.from({ length: n }, () => Array.from({ length: m }, () => 0));
    let suf = 1;
    for (let i = n - 1; ~i; --i) {
        for (let j = m - 1; ~j; --j) {
            p[i][j] = suf;
            suf = (suf * grid[i][j]) % mod;
        }
    }
    let pre = 1;
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < m; ++j) {
            p[i][j] = (p[i][j] * pre) % mod;
            pre = (pre * grid[i][j]) % mod;
        }
    }
    return p;
};
```

#### C#

```cs
public class Solution {
    public int[][] ConstructProductMatrix(int[][] grid) {
        const int mod = 12345;
        int n = grid.Length, m = grid[0].Length;
        int[][] p = new int[n][];
        for (int i = 0; i < n; ++i) {
            p[i] = new int[m];
        }

        long suf = 1;
        for (int i = n - 1; i >= 0; --i) {
            for (int j = m - 1; j >= 0; --j) {
                p[i][j] = (int)suf;
                suf = suf * grid[i][j] % mod;
            }
        }

        long pre = 1;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                p[i][j] = (int)(p[i][j] * pre % mod);
                pre = pre * grid[i][j] % mod;
            }
        }

        return p;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
