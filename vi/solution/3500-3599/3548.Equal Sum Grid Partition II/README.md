---
comments: true
difficulty: Hard
rating: 2245
source: Weekly Contest 449 Q4
tags:
    - Array
    - Hash Table
    - Enumeration
    - Matrix
    - Prefix Sum
---

<!-- problem:start -->

# [3548. Equal Sum Grid Partition II](https://leetcode.com/problems/equal-sum-grid-partition-ii)

[中文文档](/solution/3500-3599/3548.Equal%20Sum%20Grid%20Partition%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận <code>m x n</code> <code>grid</code> gồm các số nguyên dương. Nhiệm vụ của bạn là xác định xem có thể thực hiện <strong>một đường cắt ngang hoặc một đường cắt dọc</strong> trên ma trận sao cho:</p>

<ul>
    <li>Mỗi trong hai phần tạo thành sau khi cắt đều <strong>không rỗng</strong>.</li>
    <li>Tổng các phần tử trong hai phần <b>bằng nhau</b>, hoặc có thể làm cho bằng nhau bằng cách bỏ qua <strong>nhiều nhất một ô duy nhất</strong> trong toàn bộ hai phần (từ một trong hai phần).</li>
    <li>Nếu bỏ qua một ô, phần còn lại phải <strong>liên thông</strong>.</li>
</ul>

<p>Trả về <code>true</code> nếu tồn tại cách phân chia như vậy; nếu không, trả về <code>false</code>.</p>

<p><strong>Lưu ý:</strong> Một phần được gọi là <strong>liên thông</strong> nếu có thể đi từ mọi ô trong phần đến mọi ô khác bằng cách di chuyển lên, xuống, sang trái hoặc sang phải qua các ô khác trong phần.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,4],[2,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3548.Equal%20Sum%20Grid%20Partition%20II/images/lc.jpeg" style="height: 180px; width: 180px;" /></p>

<ul>
    <li>Một đường cắt ngang sau hàng đầu tiên cho tổng <code>1 + 4 = 5</code> và <code>2 + 3 = 5</code>, là hai tổng bằng nhau. Do đó, đáp án là <code>true</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,2],[3,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3548.Equal%20Sum%20Grid%20Partition%20II/images/chatgpt-image-apr-1-2025-at-05_28_12-pm.png" style="height: 180px; width: 180px;" /></p>

<ul>
    <li>Một đường cắt dọc sau cột đầu tiên cho tổng <code>1 + 3 = 4</code> và <code>2 + 4 = 6</code>.</li>
    <li>Bỏ qua 2 khỏi phần bên phải (<code>6 - 2 = 4</code>), hai phần có tổng bằng nhau và vẫn liên thông. Do đó, đáp án là <code>true</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,2,4],[2,3,5]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3548.Equal%20Sum%20Grid%20Partition%20II/images/chatgpt-image-apr-2-2025-at-02_50_29-am.png" style="height: 180px; width: 180px;" /></strong></p>

<ul>
    <li>Một đường cắt ngang sau hàng đầu tiên cho <code>1 + 2 + 4 = 7</code> và <code>2 + 3 + 5 = 10</code>.</li>
    <li>Bỏ qua 3 khỏi phần bên dưới (<code>10 - 3 = 7</code>), hai phần có tổng bằng nhau nhưng không còn liên thông vì phần bên dưới bị tách thành hai phần (<code>[2]</code> và <code>[5]</code>). Do đó, đáp án là <code>false</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[4,1,8],[3,2,6]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không tồn tại đường cắt hợp lệ, nên đáp án là <code>false</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= m == grid.length &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= n == grid[i].length &lt;= 10<sup>5</sup></code></li>
    <li><code>2 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= grid[i][j] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê các đường phân chia

<!-- thinking:start -->

> **Tư duy**
>
> Bài trước yêu cầu hai phần có tổng bằng nhau. Ở bài này, ta có thể loại bỏ một ô, nhưng phần tương ứng phải vẫn liên thông. Vì $mn \le 10^5$, ta duyệt các đường cắt thay vì mọi cặp ô.
>
> Duyệt các đường cắt ngang, đồng thời duy trì tổng và số lần xuất hiện của các giá trị trong mỗi phần. Nếu hai tổng bằng nhau thì trả về kết quả thành công; nếu không, kiểm tra xem hiệu có xuất hiện trong phần lớn hơn hay không và hình dạng của phần đó cùng vị trí của ô có bảo đảm tính liên thông sau khi loại bỏ ô hay không. Sau đó chuyển vị ma trận và lặp lại cho các đường cắt dọc.

<!-- thinking:end -->

Trước tiên, ta có thể duyệt các đường phân chia ngang, tính tổng phần tử của mỗi phần được tạo thành và dùng các hash map để ghi lại số lần xuất hiện của các phần tử trong mỗi phần.

Với mỗi đường phân chia, ta cần xác định xem tổng của hai phần có bằng nhau hay không, hoặc có thể làm cho bằng nhau bằng cách loại bỏ một ô hay không. Nếu hai tổng bằng nhau, ta trả về $\text{true}$ ngay. Nếu hai tổng không bằng nhau, ta tính hiệu của chúng là $\textit{diff}$. Nếu $\textit{diff}$ tồn tại trong hash map của phần lớn hơn và thỏa mãn điều kiện liên thông sau khi loại bỏ ô đó, ta cũng trả về $\text{true}$.

Điều kiện liên thông có thể được kiểm tra bằng các tiêu chí sau:

- Phần đó có nhiều hơn $1$ hàng và nhiều hơn $1$ cột.
- Phần đó có đúng $1$ hàng, và ô bị loại bỏ nằm ở biên của phần đó (tức là cột đầu tiên hoặc cột cuối cùng).
- Phần đó có đúng $1$ hàng, và ô bị loại bỏ nằm ở biên của phần đó (tức là hàng đầu tiên hoặc hàng cuối cùng).

Chỉ cần thỏa mãn một trong các điều kiện trên là đủ để bảo đảm phần đó vẫn liên thông sau khi loại bỏ ô.

Ta cũng cần duyệt các đường phân chia dọc, tương tự như trường hợp ngang. Để đơn giản hóa việc duyệt các đường phân chia dọc, trước tiên ta có thể chuyển vị ma trận rồi áp dụng cùng logic.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canPartitionGrid(self, grid: List[List[int]]) -> bool:
        def check(g: List[List[int]]) -> bool:
            m, n = len(g), len(g[0])
            s1 = s2 = 0
            cnt1 = defaultdict(int)
            cnt2 = defaultdict(int)
            for i, row in enumerate(g):
                for j, x in enumerate(row):
                    s2 += x
                    cnt2[x] += 1
            for i, row in enumerate(g[: m - 1]):
                for x in row:
                    s1 += x
                    s2 -= x
                    cnt1[x] += 1
                    cnt2[x] -= 1
                if s1 == s2:
                    return True
                if s1 < s2:
                    diff = s2 - s1
                    if cnt2[diff]:
                        if (
                            (m - i - 1 > 1 and n > 1)
                            or (
                                i == m - 2
                                and (g[i + 1][0] == diff or g[i + 1][-1] == diff)
                            )
                            or (n == 1 and (g[i + 1][0] == diff or g[-1][0] == diff))
                        ):
                            return True
                else:
                    diff = s1 - s2
                    if cnt1[diff]:
                        if (
                            (i + 1 > 1 and n > 1)
                            or (i == 0 and (g[0][0] == diff or g[0][-1] == diff))
                            or (n == 1 and (g[0][0] == diff or g[i][0] == diff))
                        ):
                            return True
            return False

        return check(grid) or check(list(zip(*grid)))
```

#### Java

```java
class Solution {
    public boolean canPartitionGrid(int[][] grid) {
        return check(grid) || check(rotate(grid));
    }

    private boolean check(int[][] g) {
        int m = g.length, n = g[0].length;
        long s1 = 0, s2 = 0;

        Map<Long, Integer> cnt1 = new HashMap<>();
        Map<Long, Integer> cnt2 = new HashMap<>();

        for (int[] row : g) {
            for (int x : row) {
                s2 += x;
                cnt2.merge((long) x, 1, Integer::sum);
            }
        }

        for (int i = 0; i < m - 1; i++) {
            for (int x : g[i]) {
                s1 += x;
                s2 -= x;

                cnt1.merge((long) x, 1, Integer::sum);
                cnt2.merge((long) x, -1, Integer::sum);
            }

            if (s1 == s2) {
                return true;
            }

            if (s1 < s2) {
                long diff = s2 - s1;
                if (cnt2.getOrDefault(diff, 0) > 0) {
                    if ((m - i - 1 > 1 && n > 1)
                        || (i == m - 2 && (g[i + 1][0] == diff || g[i + 1][n - 1] == diff))
                        || (n == 1 && (g[i + 1][0] == diff || g[m - 1][0] == diff))) {
                        return true;
                    }
                }
            } else {
                long diff = s1 - s2;
                if (cnt1.getOrDefault(diff, 0) > 0) {
                    if ((i + 1 > 1 && n > 1) || (i == 0 && (g[0][0] == diff || g[0][n - 1] == diff))
                        || (n == 1 && (g[0][0] == diff || g[i][0] == diff))) {
                        return true;
                    }
                }
            }
        }

        return false;
    }

    private int[][] rotate(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[][] t = new int[n][m];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                t[j][i] = grid[i][j];
            }
        }
        return t;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canPartitionGrid(vector<vector<int>>& grid) {
        return check(grid) || check(rotate(grid));
    }

private:
    bool check(const vector<vector<int>>& g) {
        int m = g.size(), n = g[0].size();
        long long s1 = 0, s2 = 0;

        unordered_map<long long, int> cnt1, cnt2;

        for (auto& row : g) {
            for (int x : row) {
                s2 += x;
                cnt2[x]++;
            }
        }

        for (int i = 0; i < m - 1; i++) {
            for (int x : g[i]) {
                s1 += x;
                s2 -= x;
                cnt1[x]++;
                cnt2[x]--;
            }

            if (s1 == s2) return true;

            if (s1 < s2) {
                long long diff = s2 - s1;
                if (cnt2[diff] > 0) {
                    if (
                        (m - i - 1 > 1 && n > 1) || (i == m - 2 && (g[i + 1][0] == diff || g[i + 1][n - 1] == diff)) || (n == 1 && (g[i + 1][0] == diff || g[m - 1][0] == diff))) return true;
                }
            } else {
                long long diff = s1 - s2;
                if (cnt1[diff] > 0) {
                    if (
                        (i + 1 > 1 && n > 1) || (i == 0 && (g[0][0] == diff || g[0][n - 1] == diff)) || (n == 1 && (g[0][0] == diff || g[i][0] == diff))) return true;
                }
            }
        }

        return false;
    }

    vector<vector<int>> rotate(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<vector<int>> t(n, vector<int>(m));
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                t[j][i] = grid[i][j];
            }
        }
        return t;
    }
};
```

#### Go

```go
func canPartitionGrid(grid [][]int) bool {
    return check(grid) || check(rotate(grid))
}

func check(g [][]int) bool {
    m, n := len(g), len(g[0])
    var s1, s2 int64

    cnt1 := map[int64]int{}
    cnt2 := map[int64]int{}

    for _, row := range g {
        for _, x := range row {
            v := int64(x)
            s2 += v
            cnt2[v]++
        }
    }

    for i := 0; i < m-1; i++ {
        for _, x := range g[i] {
            v := int64(x)
            s1 += v
            s2 -= v
            cnt1[v]++
            cnt2[v]--
        }

        if s1 == s2 {
            return true
        }

        if s1 < s2 {
            diff := s2 - s1
            if cnt2[diff] > 0 {
                if (m-i-1 > 1 && n > 1) ||
                    (i == m-2 && (int64(g[i+1][0]) == diff || int64(g[i+1][n-1]) == diff)) ||
                    (n == 1 && (int64(g[i+1][0]) == diff || int64(g[m-1][0]) == diff)) {
                    return true
                }
            }
        } else {
            diff := s1 - s2
            if cnt1[diff] > 0 {
                if (i+1 > 1 && n > 1) ||
                    (i == 0 && (int64(g[0][0]) == diff || int64(g[0][n-1]) == diff)) ||
                    (n == 1 && (int64(g[0][0]) == diff || int64(g[i][0]) == diff)) {
                    return true
                }
            }
        }
    }

    return false
}

func rotate(grid [][]int) [][]int {
    m, n := len(grid), len(grid[0])
    t := make([][]int, n)
    for i := range t {
        t[i] = make([]int, m)
    }
    for i := 0; i < m; i++ {
        for j := 0; j < n; j++ {
            t[j][i] = grid[i][j]
        }
    }
    return t
}
```

#### TypeScript

```ts
function canPartitionGrid(grid: number[][]): boolean {
    return check(grid) || check(rotate(grid));
}

function check(g: number[][]): boolean {
    const m = g.length,
        n = g[0].length;
    let s1 = 0,
        s2 = 0;

    const cnt1 = new Map<number, number>();
    const cnt2 = new Map<number, number>();

    for (const row of g) {
        for (const x of row) {
            s2 += x;
            cnt2.set(x, (cnt2.get(x) || 0) + 1);
        }
    }

    for (let i = 0; i < m - 1; i++) {
        for (const x of g[i]) {
            s1 += x;
            s2 -= x;

            cnt1.set(x, (cnt1.get(x) || 0) + 1);
            cnt2.set(x, (cnt2.get(x) || 0) - 1);
        }

        if (s1 === s2) return true;

        if (s1 < s2) {
            const diff = s2 - s1;
            if ((cnt2.get(diff) || 0) > 0) {
                if (
                    (m - i - 1 > 1 && n > 1) ||
                    (i === m - 2 && (g[i + 1][0] === diff || g[i + 1][n - 1] === diff)) ||
                    (n === 1 && (g[i + 1][0] === diff || g[m - 1][0] === diff))
                )
                    return true;
            }
        } else {
            const diff = s1 - s2;
            if ((cnt1.get(diff) || 0) > 0) {
                if (
                    (i + 1 > 1 && n > 1) ||
                    (i === 0 && (g[0][0] === diff || g[0][n - 1] === diff)) ||
                    (n === 1 && (g[0][0] === diff || g[i][0] === diff))
                )
                    return true;
            }
        }
    }

    return false;
}

function rotate(grid: number[][]): number[][] {
    const m = grid.length,
        n = grid[0].length;
    const t: number[][] = Array.from({ length: n }, () => Array(m).fill(0));

    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            t[j][i] = grid[i][j];
        }
    }

    return t;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
