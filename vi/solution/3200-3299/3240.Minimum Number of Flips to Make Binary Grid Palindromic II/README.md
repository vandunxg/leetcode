---
comments: true
difficulty: Medium
rating: 2080
source: Biweekly Contest 136 Q3
tags:
    - Array
    - Two Pointers
    - Matrix
---

<!-- problem:start -->

# [3240. Minimum Number of Flips to Make Binary Grid Palindromic II](https://leetcode.com/problems/minimum-number-of-flips-to-make-binary-grid-palindromic-ii)

[中文文档](/solution/3200-3299/3240.Minimum%20Number%20of%20Flips%20to%20Make%20Binary%20Grid%20Palindromic%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận nhị phân <code>m x n</code> là <code>grid</code>.</p>

<p>Một hàng hoặc cột được gọi là <strong>đối xứng</strong> nếu các giá trị của nó khi đọc từ đầu đến cuối và từ cuối về đầu giống nhau.</p>

<p>Ta có thể <strong>đảo</strong> giá trị của bất kỳ số ô nào trong <code>grid</code> từ <code>0</code> thành <code>1</code>, hoặc từ <code>1</code> thành <code>0</code>.</p>

<p>Hãy trả về số ô <strong>ít nhất</strong> cần đảo để <strong>tất cả</strong> các hàng và cột đều <strong>đối xứng</strong>, đồng thời tổng số <code>1</code> trong <code>grid</code> <strong>chia hết</strong> cho <code>4</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,0,0],[0,1,0],[0,0,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3240.Minimum%20Number%20of%20Flips%20to%20Make%20Binary%20Grid%20Palindromic%20II/images/image.png" style="width: 400px; height: 105px;" /></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[0,1],[0,1],[0,0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3240.Minimum%20Number%20of%20Flips%20to%20Make%20Binary%20Grid%20Palindromic%20II/images/screenshot-from-2024-07-09-01-37-48.png" style="width: 300px; height: 104px;" /></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1],[1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3240.Minimum%20Number%20of%20Flips%20to%20Make%20Binary%20Grid%20Palindromic%20II/images/screenshot-from-2024-08-01-23-05-26.png" style="width: 200px; height: 70px;" /></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>m == grid.length</code></li>
    <li><code>n == grid[i].length</code></li>
    <li><code>1 &lt;= m * n &lt;= 2 * 10<sup>5</sup></code></li>
    <li><code>0 &lt;= grid[i][j] &lt;= 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Tất cả các hàng và cột đều phải đối xứng, đồng thời số lượng $1$ phải là bội số của $4$. Mỗi chu kỳ gồm bốn ô đối xứng phải được biến thành toàn $0$ hoặc toàn $1$. Các cặp ô trên hàng hoặc cột giữa ảnh hưởng đến số lượng theo modulo $4$.
>
> Trước tiên, xử lý các quỹ đạo $2\times 2$. Nếu cả hai kích thước đều lẻ, ô trung tâm phải trở thành $0$. Trên trục giữa, các cặp khác nhau đóng góp vào $\textit{diff}$ còn các cặp bằng nhau chứa $1$ đóng góp vào $\textit{cnt1}$. Nếu $\textit{cnt1}\equiv 0\pmod 4$ hoặc tồn tại $\textit{diff}$, chỉ cần $\textit{diff}$ lần đảo để hoàn tất điều kiện modulo; nếu không, cần thêm hai lần đảo.

<!-- thinking:end -->

Nếu tất cả các hàng và cột đều đối xứng, thì với mọi $i \in [0, m / 2)$ và $j \in [0, n / 2)$, ta phải có $\text{grid}[i][j] = \text{grid}[m - i - 1][j] = \text{grid}[i][n - j - 1] = \text{grid}[m - i - 1][n - j - 1]$. Tất cả các ô này hoặc phải trở thành $0$, hoặc phải trở thành $1$. Số lần thay đổi về $0$ là $c_0 = \text{grid}[i][j] + \text{grid}[m - i - 1][j] + \text{grid}[i][n - j - 1] + \text{grid}[m - i - 1][n - j - 1]$, còn số lần thay đổi về $1$ là $c_1 = 4 - c_0$. Ta lấy giá trị nhỏ hơn trong hai số này và cộng vào đáp án.

Tiếp theo, ta xét tính chẵn lẻ của $m$ và $n$:

- Nếu cả $m$ và $n$ đều chẵn, ta trả về ngay đáp án.
- Nếu cả $m$ và $n$ đều lẻ, ô trung tâm phải là $0$ vì số lượng $1$ phải chia hết cho $4$.
- Nếu $m$ lẻ và $n$ chẵn, ta cần xét hàng giữa.
- Nếu $m$ chẵn và $n$ lẻ, ta cần xét cột giữa.

Trong hai trường hợp sau, ta cần đếm số cặp ô khác nhau $\text{diff}$ trên hàng hoặc cột giữa, cùng số ô bằng nhau và đều bằng $1$ $\text{cnt1}$. Khi đó, ta xét các trường hợp sau:

- Nếu $\text{cnt1} \bmod 4 = 0$, ta chỉ cần đổi các ô $\text{diff}$ khác nhau thành $0$, nên số phép toán là $\text{diff }$.
- Ngược lại, nếu $\text{cnt1} = 2$ và $\text{diff} \gt 0$, ta có thể đổi một trong các ô thành $1$ để $\text{cnt1} = 4$, sau đó đổi $\text{diff} - 1$ ô còn lại thành $0$. Tổng số phép toán là $\text{diff}$.
- Ngược lại, nếu $\text{diff} = 0$, ta đổi $2$ ô thành $0$ để $\text{cnt1} \bmod 4 = 0$, và số phép toán là $2$.

Ta cộng số phép toán vào đáp án rồi trả về đáp án cuối cùng.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minFlips(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        ans = 0
        for i in range(m // 2):
            for j in range(n // 2):
                x, y = m - i - 1, n - j - 1
                cnt1 = grid[i][j] + grid[x][j] + grid[i][y] + grid[x][y]
                ans += min(cnt1, 4 - cnt1)
        if m % 2 and n % 2:
            ans += grid[m // 2][n // 2]
        diff = cnt1 = 0
        if m % 2:
            for j in range(n // 2):
                if grid[m // 2][j] == grid[m // 2][n - j - 1]:
                    cnt1 += grid[m // 2][j] * 2
                else:
                    diff += 1
        if n % 2:
            for i in range(m // 2):
                if grid[i][n // 2] == grid[m - i - 1][n // 2]:
                    cnt1 += grid[i][n // 2] * 2
                else:
                    diff += 1
        ans += diff if cnt1 % 4 == 0 or diff else 2
        return ans
```

#### Java

```java
class Solution {
    public int minFlips(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int ans = 0;
        for (int i = 0; i < m / 2; ++i) {
            for (int j = 0; j < n / 2; ++j) {
                int x = m - i - 1, y = n - j - 1;
                int cnt1 = grid[i][j] + grid[x][j] + grid[i][y] + grid[x][y];
                ans += Math.min(cnt1, 4 - cnt1);
            }
        }
        if (m % 2 == 1 && n % 2 == 1) {
            ans += grid[m / 2][n / 2];
        }

        int diff = 0, cnt1 = 0;
        if (m % 2 == 1) {
            for (int j = 0; j < n / 2; ++j) {
                if (grid[m / 2][j] == grid[m / 2][n - j - 1]) {
                    cnt1 += grid[m / 2][j] * 2;
                } else {
                    diff += 1;
                }
            }
        }
        if (n % 2 == 1) {
            for (int i = 0; i < m / 2; ++i) {
                if (grid[i][n / 2] == grid[m - i - 1][n / 2]) {
                    cnt1 += grid[i][n / 2] * 2;
                } else {
                    diff += 1;
                }
            }
        }
        ans += cnt1 % 4 == 0 || diff > 0 ? diff : 2;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minFlips(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        int ans = 0;
        for (int i = 0; i < m / 2; ++i) {
            for (int j = 0; j < n / 2; ++j) {
                int x = m - i - 1, y = n - j - 1;
                int cnt1 = grid[i][j] + grid[x][j] + grid[i][y] + grid[x][y];
                ans += min(cnt1, 4 - cnt1);
            }
        }
        if (m % 2 == 1 && n % 2 == 1) {
            ans += grid[m / 2][n / 2];
        }

        int diff = 0, cnt1 = 0;
        if (m % 2 == 1) {
            for (int j = 0; j < n / 2; ++j) {
                if (grid[m / 2][j] == grid[m / 2][n - j - 1]) {
                    cnt1 += grid[m / 2][j] * 2;
                } else {
                    diff += 1;
                }
            }
        }
        if (n % 2 == 1) {
            for (int i = 0; i < m / 2; ++i) {
                if (grid[i][n / 2] == grid[m - i - 1][n / 2]) {
                    cnt1 += grid[i][n / 2] * 2;
                } else {
                    diff += 1;
                }
            }
        }
        ans += cnt1 % 4 == 0 || diff > 0 ? diff : 2;
        return ans;
    }
};
```

#### Go

```go
func minFlips(grid [][]int) int {
    m, n := len(grid), len(grid[0])
    ans := 0

    for i := 0; i < m/2; i++ {
        for j := 0; j < n/2; j++ {
            x, y := m-i-1, n-j-1
            cnt1 := grid[i][j] + grid[x][j] + grid[i][y] + grid[x][y]
            ans += min(cnt1, 4-cnt1)
        }
    }

    if m%2 == 1 && n%2 == 1 {
        ans += grid[m/2][n/2]
    }

    diff, cnt1 := 0, 0

    if m%2 == 1 {
        for j := 0; j < n/2; j++ {
            if grid[m/2][j] == grid[m/2][n-j-1] {
                cnt1 += grid[m/2][j] * 2
            } else {
                diff += 1
            }
        }
    }

    if n%2 == 1 {
        for i := 0; i < m/2; i++ {
            if grid[i][n/2] == grid[m-i-1][n/2] {
                cnt1 += grid[i][n/2] * 2
            } else {
                diff += 1
            }
        }
    }

    if cnt1%4 == 0 || diff > 0 {
        ans += diff
    } else {
        ans += 2
    }

    return ans
}
```

#### TypeScript

```ts
function minFlips(grid: number[][]): number {
    const m = grid.length;
    const n = grid[0].length;
    let ans = 0;

    for (let i = 0; i < Math.floor(m / 2); i++) {
        for (let j = 0; j < Math.floor(n / 2); j++) {
            const x = m - i - 1;
            const y = n - j - 1;
            const cnt1 = grid[i][j] + grid[x][j] + grid[i][y] + grid[x][y];
            ans += Math.min(cnt1, 4 - cnt1);
        }
    }

    if (m % 2 === 1 && n % 2 === 1) {
        ans += grid[Math.floor(m / 2)][Math.floor(n / 2)];
    }

    let diff = 0,
        cnt1 = 0;

    if (m % 2 === 1) {
        for (let j = 0; j < Math.floor(n / 2); j++) {
            if (grid[Math.floor(m / 2)][j] === grid[Math.floor(m / 2)][n - j - 1]) {
                cnt1 += grid[Math.floor(m / 2)][j] * 2;
            } else {
                diff += 1;
            }
        }
    }

    if (n % 2 === 1) {
        for (let i = 0; i < Math.floor(m / 2); i++) {
            if (grid[i][Math.floor(n / 2)] === grid[m - i - 1][Math.floor(n / 2)]) {
                cnt1 += grid[i][Math.floor(n / 2)] * 2;
            } else {
                diff += 1;
            }
        }
    }

    ans += cnt1 % 4 === 0 || diff > 0 ? diff : 2;
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
