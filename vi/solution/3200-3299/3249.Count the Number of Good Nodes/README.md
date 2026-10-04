---
comments: true
difficulty: Medium
rating: 1565
source: Weekly Contest 410 Q2
tags:
    - Tree
    - Depth-First Search
---

<!-- problem:start -->

# [3249. Count the Number of Good Nodes](https://leetcode.com/problems/count-the-number-of-good-nodes)

[中文文档](/solution/3200-3299/3249.Count%20the%20Number%20of%20Good%20Nodes/README.md)

## Mô tả

<!-- description:start -->

<p>Có một cây <strong>vô hướng</strong> gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code>, với node <code>0</code> là gốc. Cho một mảng số nguyên 2 chiều <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> biểu thị có một cạnh nối node <code>a<sub>i</sub></code> và node <code>b<sub>i</sub></code> trong cây.</p>

<p>Một node được gọi là <strong>tốt</strong> nếu tất cả <span data-keyword="subtree">cây con</span> có gốc là các node con của nó đều có cùng kích thước.</p>

<p>Hãy trả về số lượng node <strong>tốt</strong> trong cây đã cho.</p>

<p>Một <strong>cây con</strong> của <code>treeName</code> là một cây gồm một node trong <code>treeName</code> và tất cả các hậu duệ của node đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges = [[0,1],[0,2],[1,3],[1,4],[2,5],[2,6]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3249.Count%20the%20Number%20of%20Good%20Nodes/images/tree1.png" style="width: 360px; height: 158px;" />
<p>Tất cả node của cây đã cho đều là node tốt.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges = [[0,1],[1,2],[2,3],[3,4],[0,5],[1,6],[2,7],[3,8]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3249.Count%20the%20Number%20of%20Good%20Nodes/images/screenshot-2024-06-03-193552.png" style="width: 360px; height: 303px;" />
<p>Cây đã cho có 6 node tốt. Chúng được tô màu trong hình trên.</p>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges = [[0,1],[1,2],[1,3],[1,4],[0,5],[5,6],[6,7],[7,8],[0,9],[9,10],[9,12],[10,11]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3249.Count%20the%20Number%20of%20Good%20Nodes/images/rob.jpg" style="width: 450px; height: 277px;" />
<p>Tất cả node ngoại trừ node 9 đều là node tốt.</p>
</div>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
    <li><code>edges.length == n - 1</code></li>
    <li><code>edges[i].length == 2</code></li>
    <li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
    <li>Dữ liệu đầu vào được tạo sao cho <code>edges</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Một node tốt khi tất cả cây con của nó có cùng kích thước. Vì $n\le 10^5$, không thể tính lại cây con tại mỗi node. Một lần DFS có thể so sánh kích thước các node con khi quay lui và đồng thời đếm node hiện tại.
>
> Hàm $\textit{dfs}(a,\textit{fa})$ trả về kích thước cây con; $a$ là node tốt nếu mọi node con đều trả về cùng một giá trị. Ta coi node $0$ là gốc của cây vô hướng. Độ phức tạp thời gian là tuyến tính.

<!-- thinking:end -->

Trước tiên, ta xây dựng danh sách kề $\textit{g}$ của cây dựa trên các cạnh đã cho $\textit{edges}$, trong đó $\textit{g}[a]$ biểu diễn tất cả node kề với node $a$.

Tiếp theo, ta thiết kế hàm $\textit{dfs}(a, \textit{fa})$ để tính số node trong cây con có gốc là node $a$ và cộng dồn số node tốt. Ở đây, $\textit{fa}$ là node cha của node $a$.

Quy trình thực thi của hàm $\textit{dfs}(a, \textit{fa})$ như sau:

1. Khởi tạo các biến $\textit{pre} = -1$, $\textit{cnt} = 1$, $\textit{ok} = 1$, lần lượt biểu diễn kích thước cây con của node $a$, tổng số node trong tất cả cây con của node $a$, và việc node $a$ có phải là node tốt hay không.
2. Duyệt qua tất cả node kề $b$ của node $a$. Nếu $b$ khác $\textit{fa}$, đệ quy gọi $\textit{dfs}(b, a)$, với giá trị trả về là $\textit{cur}$, rồi cộng $\textit{cur}$ vào $\textit{cnt}$. Nếu $\textit{pre} < 0$, gán $\textit{cur}$ cho $\textit{pre}$; ngược lại, nếu $\textit{pre}$ khác $\textit{cur}$, điều đó có nghĩa là số node trong các cây con khác nhau của node $a$ không giống nhau, nên gán $\textit{ok}$ bằng $0$.
3. Cuối cùng, cộng $\textit{ok}$ vào đáp án và trả về $\textit{cnt}$.

Trong hàm chính, ta gọi $\textit{dfs}(0, -1)$ và trả về đáp án cuối cùng.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số lượng node.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countGoodNodes(self, edges: List[List[int]]) -> int:
        def dfs(a: int, fa: int) -> int:
            pre = -1
            cnt = ok = 1
            for b in g[a]:
                if b != fa:
                    cur = dfs(b, a)
                    cnt += cur
                    if pre < 0:
                        pre = cur
                    elif pre != cur:
                        ok = 0
            nonlocal ans
            ans += ok
            return cnt

        g = defaultdict(list)
        for a, b in edges:
            g[a].append(b)
            g[b].append(a)
        ans = 0
        dfs(0, -1)
        return ans
```

#### Java

```java
class Solution {
    private int ans;
    private List<Integer>[] g;

    public int countGoodNodes(int[][] edges) {
        int n = edges.length + 1;
        g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        dfs(0, -1);
        return ans;
    }

    private int dfs(int a, int fa) {
        int pre = -1, cnt = 1, ok = 1;
        for (int b : g[a]) {
            if (b != fa) {
                int cur = dfs(b, a);
                cnt += cur;
                if (pre < 0) {
                    pre = cur;
                } else if (pre != cur) {
                    ok = 0;
                }
            }
        }
        ans += ok;
        return cnt;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countGoodNodes(vector<vector<int>>& edges) {
        int n = edges.size() + 1;
        vector<int> g[n];
        for (const auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].push_back(b);
            g[b].push_back(a);
        }
        int ans = 0;
        auto dfs = [&](this auto&& dfs, int a, int fa) -> int {
            int pre = -1, cnt = 1, ok = 1;
            for (int b : g[a]) {
                if (b != fa) {
                    int cur = dfs(b, a);
                    cnt += cur;
                    if (pre < 0) {
                        pre = cur;
                    } else if (pre != cur) {
                        ok = 0;
                    }
                }
            }
            ans += ok;
            return cnt;
        };
        dfs(0, -1);
        return ans;
    }
};
```

#### Go

```go
func countGoodNodes(edges [][]int) (ans int) {
    n := len(edges) + 1
    g := make([][]int, n)
    for _, e := range edges {
        a, b := e[0], e[1]
        g[a] = append(g[a], b)
        g[b] = append(g[b], a)
    }
    var dfs func(int, int) int
    dfs = func(a, fa int) int {
        pre, cnt, ok := -1, 1, 1
        for _, b := range g[a] {
            if b != fa {
                cur := dfs(b, a)
                cnt += cur
                if pre < 0 {
                    pre = cur
                } else if pre != cur {
                    ok = 0
                }
            }
        }
        ans += ok
        return cnt
    }
    dfs(0, -1)
    return
}
```

#### TypeScript

```ts
function countGoodNodes(edges: number[][]): number {
    const n = edges.length + 1;
    const g: number[][] = Array.from({ length: n }, () => []);
    for (const [a, b] of edges) {
        g[a].push(b);
        g[b].push(a);
    }
    let ans = 0;
    const dfs = (a: number, fa: number): number => {
        let [pre, cnt, ok] = [-1, 1, 1];
        for (const b of g[a]) {
            if (b !== fa) {
                const cur = dfs(b, a);
                cnt += cur;
                if (pre < 0) {
                    pre = cur;
                } else if (pre !== cur) {
                    ok = 0;
                }
            }
        }
        ans += ok;
        return cnt;
    };
    dfs(0, -1);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
