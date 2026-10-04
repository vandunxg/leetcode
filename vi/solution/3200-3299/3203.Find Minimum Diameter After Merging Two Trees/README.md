---
comments: true
difficulty: Hard
rating: 2266
source: Weekly Contest 404 Q4
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Graph
---

<!-- problem:start -->

# [3203. Find Minimum Diameter After Merging Two Trees](https://leetcode.com/problems/find-minimum-diameter-after-merging-two-trees)

[中文文档](/solution/3200-3299/3203.Find%20Minimum%20Diameter%20After%20Merging%20Two%20Trees/README.md)

## Mô tả

<!-- description:start -->

<p>Có hai cây <strong>vô hướng </strong> với lần lượt <code>n</code> và <code>m</code> nút, được đánh số từ <code>0</code> đến <code>n - 1</code> và từ <code>0</code> đến <code>m - 1</code>. Bạn được cho hai mảng số nguyên 2 chiều <code>edges1</code> và <code>edges2</code> có độ dài lần lượt là <code>n - 1</code> và <code>m - 1</code>, trong đó <code>edges1[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> biểu thị có một cạnh nối các nút <code>a<sub>i</sub></code> và <code>b<sub>i</sub></code> trong cây thứ nhất, còn <code>edges2[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> biểu thị có một cạnh nối các nút <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code> trong cây thứ hai.</p>

<p>Bạn phải nối một nút của cây thứ nhất với một nút khác của cây thứ hai bằng một cạnh.</p>

<p>Hãy trả về <strong>đường kính </strong><strong>nhỏ nhất </strong>có thể có của cây kết quả.</p>

<p><strong>Đường kính</strong> của một cây là độ dài của đường đi <em>dài nhất</em> giữa hai nút bất kỳ trong cây.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3203.Find%20Minimum%20Diameter%20After%20Merging%20Two%20Trees/images/example11-transformed.png" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges1 = [[0,1],[0,2],[0,3]], edges2 = [[0,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể thu được một cây có đường kính bằng 3 bằng cách nối nút 0 của cây thứ nhất với bất kỳ nút nào của cây thứ hai.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3203.Find%20Minimum%20Diameter%20After%20Merging%20Two%20Trees/images/example211.png" />
<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges1 = [[0,1],[0,2],[0,3],[2,4],[2,5],[3,6],[2,7]], edges2 = [[0,1],[0,2],[0,3],[2,4],[2,5],[3,6],[2,7]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể thu được một cây có đường kính bằng 5 bằng cách nối nút 0 của cây thứ nhất với nút 0 của cây thứ hai.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n, m &lt;= 10<sup>5</sup></code></li>
    <li><code>edges1.length == n - 1</code></li>
    <li><code>edges2.length == m - 1</code></li>
    <li><code>edges1[i].length == edges2[i].length == 2</code></li>
    <li><code>edges1[i] = [a<sub>i</sub>, b<sub>i</sub>]</code></li>
    <li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
    <li><code>edges2[i] = [u<sub>i</sub>, v<sub>i</sub>]</code></li>
    <li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt; m</code></li>
    <li>Dữ liệu đầu vào được tạo sao cho <code>edges1</code> và <code>edges2</code> biểu diễn các cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai lượt DFS

<!-- thinking:start -->

> **Tư duy**
>
> Ta phải thêm một cạnh nối hai cây để tối thiểu hóa đường kính mới. Với $n,m\le 10^5$, thử mọi cặp đầu mút rồi tính lại đường kính sẽ tốn $nm$, không khả thi.
>
> Đường kính mới chỉ có thể thuộc một trong hai loại: nằm hoàn toàn trong một cây ban đầu, nên bằng $\max(d_1,d_2)$; hoặc đi qua cạnh mới, khi đó phương án tối ưu nối các điểm gần hai tâm và có độ dài bằng tổng hai bán kính cộng một. Vì vậy, việc còn lại là tính đường kính của mỗi cây. Từ một nút bất kỳ, đi đến nút xa nhất $a$, rồi từ $a$ đi đến nút xa nhất $b$; đường đi $a$–$b$ là một đường kính. Hai lượt DFS trên mỗi cây có độ phức tạp tuyến tính.

<!-- thinking:end -->

Ta ký hiệu $d_1$ và $d_2$ lần lượt là đường kính của hai cây. Khi đó, đường kính của cây sau khi gộp có thể thuộc một trong hai trường hợp sau:

1. Đường kính của cây sau khi gộp chính là đường kính của một trong hai cây ban đầu, tức là $\max(d_1, d_2)$;
2. Đường kính của cây sau khi gộp đi qua cả hai cây ban đầu. Ta tính bán kính của hai cây ban đầu lần lượt là $r_1 = \lceil \frac{d_1}{2} \rceil$ và $r_2 = \lceil \frac{d_2}{2} \rceil$. Khi đó, đường kính của cây sau khi gộp là $r_1 + r_2 + 1$.

Ta lấy giá trị lớn hơn trong hai trường hợp này.

Khi tính đường kính của một cây, ta có thể dùng hai lượt DFS. Đầu tiên, chọn ngẫu nhiên một nút và bắt đầu DFS từ nút này để tìm nút xa nhất, ký hiệu là nút $a$. Sau đó, bắt đầu một DFS khác từ nút $a$ để tìm nút xa nhất tính từ nút $a$, ký hiệu là nút $b$. Có thể chứng minh rằng đường đi giữa nút $a$ và nút $b$ là đường kính của cây.

Độ phức tạp thời gian là $O(n + m)$, và độ phức tạp không gian là $O(n + m)$, trong đó $n$ và $m$ lần lượt là số nút trong hai cây.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumDiameterAfterMerge(
        self, edges1: List[List[int]], edges2: List[List[int]]
    ) -> int:
        d1 = self.treeDiameter(edges1)
        d2 = self.treeDiameter(edges2)
        return max(d1, d2, (d1 + 1) // 2 + (d2 + 1) // 2 + 1)

    def treeDiameter(self, edges: List[List[int]]) -> int:
        def dfs(i: int, fa: int, t: int):
            for j in g[i]:
                if j != fa:
                    dfs(j, i, t + 1)
            nonlocal ans, a
            if ans < t:
                ans = t
                a = i

        g = defaultdict(list)
        for a, b in edges:
            g[a].append(b)
            g[b].append(a)
        ans = a = 0
        dfs(0, -1, 0)
        dfs(a, -1, 0)
        return ans
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private int ans;
    private int a;

    public int minimumDiameterAfterMerge(int[][] edges1, int[][] edges2) {
        int d1 = treeDiameter(edges1);
        int d2 = treeDiameter(edges2);
        return Math.max(Math.max(d1, d2), (d1 + 1) / 2 + (d2 + 1) / 2 + 1);
    }

    public int treeDiameter(int[][] edges) {
        int n = edges.length + 1;
        g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        ans = 0;
        a = 0;
        for (var e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        dfs(0, -1, 0);
        dfs(a, -1, 0);
        return ans;
    }

    private void dfs(int i, int fa, int t) {
        for (int j : g[i]) {
            if (j != fa) {
                dfs(j, i, t + 1);
            }
        }
        if (ans < t) {
            ans = t;
            a = i;
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumDiameterAfterMerge(vector<vector<int>>& edges1, vector<vector<int>>& edges2) {
        int d1 = treeDiameter(edges1);
        int d2 = treeDiameter(edges2);
        return max({d1, d2, (d1 + 1) / 2 + (d2 + 1) / 2 + 1});
    }

    int treeDiameter(vector<vector<int>>& edges) {
        int n = edges.size() + 1;
        vector<int> g[n];
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].push_back(b);
            g[b].push_back(a);
        }
        int ans = 0, a = 0;
        auto dfs = [&](this auto&& dfs, int i, int fa, int t) -> void {
            for (int j : g[i]) {
                if (j != fa) {
                    dfs(j, i, t + 1);
                }
            }
            if (ans < t) {
                ans = t;
                a = i;
            }
        };
        dfs(0, -1, 0);
        dfs(a, -1, 0);
        return ans;
    }
};
```

#### Go

```go
func minimumDiameterAfterMerge(edges1 [][]int, edges2 [][]int) int {
    d1 := treeDiameter(edges1)
    d2 := treeDiameter(edges2)
    return max(d1, d2, (d1+1)/2+(d2+1)/2+1)
}

func treeDiameter(edges [][]int) (ans int) {
    n := len(edges) + 1
    g := make([][]int, n)
    for _, e := range edges {
        a, b := e[0], e[1]
        g[a] = append(g[a], b)
        g[b] = append(g[b], a)
    }
    a := 0
    var dfs func(i, fa, t int)
    dfs = func(i, fa, t int) {
        for _, j := range g[i] {
            if j != fa {
                dfs(j, i, t+1)
            }
        }
        if ans < t {
            ans = t
            a = i
        }
    }
    dfs(0, -1, 0)
    dfs(a, -1, 0)
    return
}
```

#### TypeScript

```ts
function minimumDiameterAfterMerge(edges1: number[][], edges2: number[][]): number {
    const d1 = treeDiameter(edges1);
    const d2 = treeDiameter(edges2);
    return Math.max(d1, d2, Math.ceil(d1 / 2) + Math.ceil(d2 / 2) + 1);
}

function treeDiameter(edges: number[][]): number {
    const n = edges.length + 1;
    const g: number[][] = Array.from({ length: n }, () => []);
    for (const [a, b] of edges) {
        g[a].push(b);
        g[b].push(a);
    }
    let [ans, a] = [0, 0];
    const dfs = (i: number, fa: number, t: number): void => {
        for (const j of g[i]) {
            if (j !== fa) {
                dfs(j, i, t + 1);
            }
        }
        if (ans < t) {
            ans = t;
            a = i;
        }
    };
    dfs(0, -1, 0);
    dfs(a, -1, 0);
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_diameter_after_merge(edges1: Vec<Vec<i32>>, edges2: Vec<Vec<i32>>) -> i32 {
        let d1 = Self::tree_diameter(&edges1);
        let d2 = Self::tree_diameter(&edges2);
        d1.max(d2).max((d1 + 1) / 2 + (d2 + 1) / 2 + 1)
    }

    fn tree_diameter(edges: &Vec<Vec<i32>>) -> i32 {
        let n = edges.len() + 1;
        let mut g = vec![vec![]; n];
        for e in edges {
            let a = e[0] as usize;
            let b = e[1] as usize;
            g[a].push(b);
            g[b].push(a);
        }
        let mut ans = 0;
        let mut a = 0;
        fn dfs(g: &Vec<Vec<usize>>, i: usize, fa: isize, t: i32, ans: &mut i32, a: &mut usize) {
            for &j in &g[i] {
                if j as isize != fa {
                    dfs(g, j, i as isize, t + 1, ans, a);
                }
            }
            if *ans < t {
                *ans = t;
                *a = i;
            }
        }
        dfs(&g, 0, -1, 0, &mut ans, &mut a);
        dfs(&g, a, -1, 0, &mut ans, &mut a);
        ans
    }
}
```

#### C#

```cs
public class Solution {
    private List<int>[] g;
    private int ans;
    private int a;

    public int MinimumDiameterAfterMerge(int[][] edges1, int[][] edges2) {
        int d1 = TreeDiameter(edges1);
        int d2 = TreeDiameter(edges2);
        return Math.Max(Math.Max(d1, d2), (d1 + 1) / 2 + (d2 + 1) / 2 + 1);
    }

    public int TreeDiameter(int[][] edges) {
        int n = edges.Length + 1;
        g = new List<int>[n];
        for (int k = 0; k < n; ++k) {
            g[k] = new List<int>();
        }
        ans = 0;
        a = 0;
        foreach (var e in edges) {
            int a = e[0], b = e[1];
            g[a].Add(b);
            g[b].Add(a);
        }
        Dfs(0, -1, 0);
        Dfs(a, -1, 0);
        return ans;
    }

    private void Dfs(int i, int fa, int t) {
        foreach (int j in g[i]) {
            if (j != fa) {
                Dfs(j, i, t + 1);
            }
        }
        if (ans < t) {
            ans = t;
            a = i;
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
