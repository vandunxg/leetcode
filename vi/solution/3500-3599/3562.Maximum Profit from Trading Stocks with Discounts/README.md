---
comments: true
difficulty: Hard
rating: 2458
source: Weekly Contest 451 Q3
tags:
    - Tree
    - Depth-First Search
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3562. Maximum Profit from Trading Stocks with Discounts](https://leetcode.com/problems/maximum-profit-from-trading-stocks-with-discounts)

[中文文档](/solution/3500-3599/3562.Maximum%20Profit%20from%20Trading%20Stocks%20with%20Discounts/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>, biểu thị số nhân viên trong một công ty. Mỗi nhân viên được gán một ID duy nhất từ 1 đến <code>n</code>, trong đó nhân viên 1 là CEO, là cấp trên trực tiếp hoặc gián tiếp của mọi nhân viên. Cho hai mảng số nguyên <strong>được đánh số từ 1 </strong>, <code>present</code> và <code>future</code>, mỗi mảng có độ dài <code>n</code>, trong đó:</p>

<ul>
    <li><code>present[i]</code> biểu thị giá <strong>hiện tại</strong> mà nhân viên thứ <code>i<sup>th</sup></code> có thể mua một cổ phiếu hôm nay.</li>
    <li><code>future[i]</code> biểu thị giá <strong>dự kiến</strong> mà nhân viên thứ <code>i<sup>th</sup></code> có thể bán cổ phiếu vào ngày mai.</li>
</ul>

<p>Cấu trúc cấp bậc của công ty được biểu diễn bởi một mảng số nguyên 2 chiều <code>hierarchy</code>, trong đó <code>hierarchy[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> nghĩa là nhân viên <code>u<sub>i</sub></code> là cấp trên trực tiếp của nhân viên <code>v<sub>i</sub></code>.</p>

<p>Ngoài ra, cho số nguyên <code>budget</code> biểu thị tổng số vốn có thể dùng để đầu tư.</p>

<p>Tuy nhiên, công ty có chính sách giảm giá: nếu cấp trên trực tiếp của một nhân viên mua cổ phiếu của mình, nhân viên đó có thể mua cổ phiếu với giá bằng <strong>một nửa</strong> giá ban đầu (<code>floor(present[v] / 2)</code>).</p>

<p>Trả về lợi nhuận <strong>lớn nhất</strong> có thể đạt được mà không vượt quá ngân sách đã cho.</p>

<p><strong>Lưu ý:</strong></p>

<ul>
    <li>Bạn có thể mua mỗi cổ phiếu nhiều nhất <strong>một lần</strong>.</li>
    <li>Bạn <strong>không thể</strong> dùng lợi nhuận thu được từ giá cổ phiếu trong tương lai để tài trợ cho các khoản đầu tư bổ sung và chỉ được mua bằng <code>budget</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, present = [1,2], future = [4,3], hierarchy = [[1,2]], budget = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3562.Maximum%20Profit%20from%20Trading%20Stocks%20with%20Discounts/images/screenshot-2025-04-10-at-053641.png" style="width: 200px; height: 80px;" /></p>

<ul>
    <li>Nhân viên 1 mua cổ phiếu với giá 1 và thu được lợi nhuận <code>4 - 1 = 3</code>.</li>
    <li>Vì nhân viên 1 là cấp trên trực tiếp của nhân viên 2, nhân viên 2 được mua với giá ưu đãi <code>floor(2 / 2) = 1</code>.</li>
    <li>Nhân viên 2 mua cổ phiếu với giá 1 và thu được lợi nhuận <code>3 - 1 = 2</code>.</li>
    <li>Tổng chi phí mua là <code>1 + 1 = 2 &lt;= budget</code>. Do đó, tổng lợi nhuận lớn nhất đạt được là <code>3 + 2 = 5</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, present = [3,4], future = [5,8], hierarchy = [[1,2]], budget = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3562.Maximum%20Profit%20from%20Trading%20Stocks%20with%20Discounts/images/screenshot-2025-04-10-at-053641.png" style="width: 200px; height: 80px;" /></p>

<ul>
    <li>Nhân viên 2 mua cổ phiếu với giá 4 và thu được lợi nhuận <code>8 - 4 = 4</code>.</li>
    <li>Vì hai nhân viên không thể cùng mua, lợi nhuận lớn nhất là 4.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, present = [4,6,8], future = [7,9,11], hierarchy = [[1,2],[1,3]], budget = 10</span></p>

<p><strong>Đầu ra:</strong> 10</p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3562.Maximum%20Profit%20from%20Trading%20Stocks%20with%20Discounts/images/image.png" style="width: 180px; height: 153px;" /></p>

<ul>
    <li>Nhân viên 1 mua cổ phiếu với giá 4 và thu được lợi nhuận <code>7 - 4 = 3</code>.</li>
    <li>Nhân viên 3 được mua với giá ưu đãi <code>floor(8 / 2) = 4</code> và thu được lợi nhuận <code>11 - 4 = 7</code>.</li>
    <li>Nhân viên 1 và nhân viên 3 mua cổ phiếu với tổng chi phí <code>4 + 4 = 8 &lt;= budget</code>. Do đó, tổng lợi nhuận lớn nhất đạt được là <code>3 + 7 = 10</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, present = [5,2,3], future = [8,5,6], hierarchy = [[1,2],[2,3]], budget = 7</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3562.Maximum%20Profit%20from%20Trading%20Stocks%20with%20Discounts/images/screenshot-2025-04-10-at-054114.png" style="width: 300px; height: 85px;" /></p>

<ul>
    <li>Nhân viên 1 mua cổ phiếu với giá 5 và thu được lợi nhuận <code>8 - 5 = 3</code>.</li>
    <li>Nhân viên 2 được mua với giá ưu đãi <code>floor(2 / 2) = 1</code> và thu được lợi nhuận <code>5 - 1 = 4</code>.</li>
    <li>Nhân viên 3 được mua với giá ưu đãi <code>floor(3 / 2) = 1</code> và thu được lợi nhuận <code>6 - 1 = 5</code>.</li>
    <li>Tổng chi phí là <code>5 + 1 + 1 = 7&nbsp;&lt;= budget</code>. Do đó, tổng lợi nhuận lớn nhất đạt được là <code>3 + 4 + 5 = 12</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n &lt;= 160</code></li>
    <li><code>present.length, future.length == n</code></li>
    <li><code>1 &lt;= present[i], future[i] &lt;= 50</code></li>
    <li><code>hierarchy.length == n - 1</code></li>
    <li><code>hierarchy[i] == [u<sub>i</sub>, v<sub>i</sub>]</code></li>
    <li><code>1 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n</code></li>
    <li><code>u<sub>i</sub> != v<sub>i</sub></code></li>
    <li><code>1 &lt;= budget &lt;= 160</code></li>
    <li>Không có cạnh trùng lặp.</li>
    <li>Nhân viên 1 là cấp trên trực tiếp hoặc gián tiếp của mọi nhân viên.</li>
    <li>Đồ thị đầu vào <code>hierarchy </code>được <strong>đảm bảo</strong> không có chu trình.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động trên cây

<!-- thinking:start -->

> **Tư duy**
>
> Cấu trúc cấp bậc là một cây, việc giảm giá phụ thuộc vào việc cấp trên có mua hay không, còn ngân sách là sức chứa của bài toán knapsack. Tại $u$, ta lưu $f[j][\textit{pre}]$ — lợi nhuận lớn nhất trong cây con của $u$ với ngân sách $j$ và cờ cho biết cấp trên có mua hay không $\textit{pre}$.
>
> Ta gộp các knapsack của các nút con vào $\textit{nxt}$ theo ngân sách, sau đó quyết định có mua $u$ với chi phí $\lfloor \textit{present}/(\textit{pre}+1)\rfloor$ hay không. Gốc không có cấp trên, nên đáp án là $f_1[\textit{budget}][0]$.

<!-- thinking:end -->

Với mỗi nút $u$, ta duy trì một mảng 2 chiều $f_u[j][pre]$, biểu thị lợi nhuận lớn nhất có thể đạt được trong cây con gốc $u$ với ngân sách không vượt quá $j$, đồng thời cho biết quản lý của $u$ có mua cổ phiếu hay không (trong đó $pre=1$ nghĩa là đã mua, còn $pre=0$ nghĩa là chưa mua). Đáp án là $f_1[\text{budget}][0]$.

Với nút $u$, hàm $\text{dfs}(u)$ trả về một mảng 2 chiều $(\text{budget}+1) \times 2$ là $f$, biểu thị lợi nhuận lớn nhất có thể đạt được trong cây con gốc $u$ với ngân sách không vượt quá $j$, đồng thời cho biết quản lý của $u$ có mua cổ phiếu hay không.

Với $u$, ta cần xét hai yếu tố:

1. Nút $u$ có tự mua cổ phiếu hay không (việc này sẽ tiêu tốn một phần ngân sách $\text{cost}$, trong đó $\text{cost} = \lfloor \text{present}[u] / (pre + 1) \rfloor$), và làm lợi nhuận tăng thêm $\text{future}[u] - \text{cost}$.
2. Phân bổ ngân sách cho các nút con $v$ của $u$ như thế nào để tối đa hóa lợi nhuận. Ta coi kết quả $\text{dfs}(v)$ của mỗi nút con là một "item" và dùng phương pháp knapsack để gộp lợi nhuận của các cây con vào mảng $\text{nxt}$ hiện tại của $u$.

Trong phần cài đặt cụ thể, trước tiên ta khởi tạo một mảng 2 chiều $(\text{budget}+1) \times 2$ là $\text{nxt}$, biểu thị lợi nhuận đã gộp từ các nút con. Sau đó, với mỗi nút con $v$, ta gọi đệ quy $\text{dfs}(v)$ để nhận mảng lợi nhuận $\text{fv}$ của nút con, rồi dùng phương pháp knapsack để gộp $\text{fv}$ vào $\text{nxt}$.

Công thức gộp là:

$$
\text{nxt}[j][pre] = \max(\text{nxt}[j][pre], \text{nxt}[j - j_v][pre] + \text{fv}[j_v][pre])
$$

trong đó $j_v$ là ngân sách được phân bổ cho nút con $v$.

Sau khi gộp tất cả các nút con, $\text{nxt}[j][pre]$ biểu thị lợi nhuận lớn nhất có thể đạt được khi phân bổ toàn bộ ngân sách $j$ cho các nút con, trong trường hợp bản thân $u$ vẫn chưa quyết định có mua cổ phiếu hay không và trạng thái mua của quản lý $u$ là $pre$.

Cuối cùng, ta quyết định nút $u$ có mua cổ phiếu hay không.

- Nếu $j \lt \text{cost}$, $u$ không thể mua cổ phiếu, khi đó $f[j][pre] = \text{nxt}[j][0]$.
- Nếu $j \geq \text{cost}$, $u$ có thể chọn mua hoặc không mua cổ phiếu, khi đó $f[j][pre] = \max(\text{nxt}[j][0], \text{nxt}[j - \text{cost}][1] + (\text{future}[u] - \text{cost}))$.

Cuối cùng, trả về $f$.

Đáp án là $\text{dfs}(1)[\text{budget}][0]$.

Độ phức tạp thời gian là $O(n \times \text{budget}^2)$, và độ phức tạp không gian là $O(n \times \text{budget})$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxProfit(
        self,
        n: int,
        present: List[int],
        future: List[int],
        hierarchy: List[List[int]],
        budget: int,
    ) -> int:
        max = lambda a, b: a if a > b else b
        g = [[] for _ in range(n + 1)]
        for u, v in hierarchy:
            g[u].append(v)

        def dfs(u: int):
            nxt = [[0, 0] for _ in range(budget + 1)]
            for v in g[u]:
                fv = dfs(v)
                for j in range(budget, -1, -1):
                    for jv in range(j + 1):
                        for pre in (0, 1):
                            val = nxt[j - jv][pre] + fv[jv][pre]
                            if val > nxt[j][pre]:
                                nxt[j][pre] = val

            f = [[0, 0] for _ in range(budget + 1)]
            price = future[u - 1]

            for j in range(budget + 1):
                for pre in (0, 1):
                    cost = present[u - 1] // (pre + 1)
                    if j >= cost:
                        f[j][pre] = max(nxt[j][0], nxt[j - cost][1] + (price - cost))
                    else:
                        f[j][pre] = nxt[j][0]

            return f

        return dfs(1)[budget][0]
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private int[] present;
    private int[] future;
    private int budget;

    public int maxProfit(int n, int[] present, int[] future, int[][] hierarchy, int budget) {
        this.present = present;
        this.future = future;
        this.budget = budget;

        g = new ArrayList[n + 1];
        Arrays.setAll(g, k -> new ArrayList<>());

        for (int[] e : hierarchy) {
            g[e[0]].add(e[1]);
        }

        return dfs(1)[budget][0];
    }

    private int[][] dfs(int u) {
        int[][] nxt = new int[budget + 1][2];

        for (int v : g[u]) {
            int[][] fv = dfs(v);
            for (int j = budget; j >= 0; j--) {
                for (int jv = 0; jv <= j; jv++) {
                    for (int pre = 0; pre < 2; pre++) {
                        int val = nxt[j - jv][pre] + fv[jv][pre];
                        if (val > nxt[j][pre]) {
                            nxt[j][pre] = val;
                        }
                    }
                }
            }
        }

        int[][] f = new int[budget + 1][2];
        int price = future[u - 1];

        for (int j = 0; j <= budget; j++) {
            for (int pre = 0; pre < 2; pre++) {
                int cost = present[u - 1] / (pre + 1);
                if (j >= cost) {
                    f[j][pre] = Math.max(nxt[j][0], nxt[j - cost][1] + (price - cost));
                } else {
                    f[j][pre] = nxt[j][0];
                }
            }
        }

        return f;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxProfit(int n, vector<int>& present, vector<int>& future, vector<vector<int>>& hierarchy, int budget) {
        vector<vector<int>> g(n + 1);
        for (auto& e : hierarchy) {
            g[e[0]].push_back(e[1]);
        }

        auto dfs = [&](const auto& dfs, int u) -> vector<array<int, 2>> {
            vector<array<int, 2>> nxt(budget + 1);
            for (int j = 0; j <= budget; j++) nxt[j] = {0, 0};

            for (int v : g[u]) {
                auto fv = dfs(dfs, v);
                for (int j = budget; j >= 0; j--) {
                    for (int jv = 0; jv <= j; jv++) {
                        for (int pre = 0; pre < 2; pre++) {
                            int val = nxt[j - jv][pre] + fv[jv][pre];
                            if (val > nxt[j][pre]) {
                                nxt[j][pre] = val;
                            }
                        }
                    }
                }
            }

            vector<array<int, 2>> f(budget + 1);
            int price = future[u - 1];

            for (int j = 0; j <= budget; j++) {
                for (int pre = 0; pre < 2; pre++) {
                    int cost = present[u - 1] / (pre + 1);
                    if (j >= cost) {
                        f[j][pre] = max(nxt[j][0], nxt[j - cost][1] + (price - cost));
                    } else {
                        f[j][pre] = nxt[j][0];
                    }
                }
            }

            return f;
        };

        return dfs(dfs, 1)[budget][0];
    }
};
```

#### Go

```go
func maxProfit(n int, present []int, future []int, hierarchy [][]int, budget int) int {
    g := make([][]int, n+1)
    for _, e := range hierarchy {
        u, v := e[0], e[1]
        g[u] = append(g[u], v)
    }

    var dfs func(u int) [][2]int
    dfs = func(u int) [][2]int {
        nxt := make([][2]int, budget+1)

        for _, v := range g[u] {
            fv := dfs(v)
            for j := budget; j >= 0; j-- {
                for jv := 0; jv <= j; jv++ {
                    for pre := 0; pre < 2; pre++ {
                        nxt[j][pre] = max(nxt[j][pre], nxt[j-jv][pre]+fv[jv][pre])
                    }
                }
            }
        }

        f := make([][2]int, budget+1)
        price := future[u-1]

        for j := 0; j <= budget; j++ {
            for pre := 0; pre < 2; pre++ {
                cost := present[u-1] / (pre + 1)
                if j >= cost {
                    buyProfit := nxt[j-cost][1] + (price - cost)
                    f[j][pre] = max(nxt[j][0], buyProfit)
                } else {
                    f[j][pre] = nxt[j][0]
                }
            }
        }
        return f
    }

    return dfs(1)[budget][0]
}
```

#### TypeScript

```ts
function maxProfit(
    n: number,
    present: number[],
    future: number[],
    hierarchy: number[][],
    budget: number,
): number {
    const g: number[][] = Array.from({ length: n + 1 }, () => []);

    for (const [u, v] of hierarchy) {
        g[u].push(v);
    }

    const dfs = (u: number): number[][] => {
        const nxt: number[][] = Array.from({ length: budget + 1 }, () => [0, 0]);

        for (const v of g[u]) {
            const fv = dfs(v);
            for (let j = budget; j >= 0; j--) {
                for (let jv = 0; jv <= j; jv++) {
                    for (let pre = 0; pre < 2; pre++) {
                        nxt[j][pre] = Math.max(nxt[j][pre], nxt[j - jv][pre] + fv[jv][pre]);
                    }
                }
            }
        }

        const f: number[][] = Array.from({ length: budget + 1 }, () => [0, 0]);
        const price = future[u - 1];

        for (let j = 0; j <= budget; j++) {
            for (let pre = 0; pre < 2; pre++) {
                const cost = Math.floor(present[u - 1] / (pre + 1));
                if (j >= cost) {
                    const profitIfBuy = nxt[j - cost][1] + (price - cost);
                    f[j][pre] = Math.max(nxt[j][0], profitIfBuy);
                } else {
                    f[j][pre] = nxt[j][0];
                }
            }
        }

        return f;
    };

    return dfs(1)[budget][0];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
