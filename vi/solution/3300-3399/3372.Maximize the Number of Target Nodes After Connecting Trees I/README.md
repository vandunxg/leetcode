---
comments: true
difficulty: Medium
rating: 1926
source: Weekly Contest 426 Q3
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
---

<!-- problem:start -->

# [3372. Maximize the Number of Target Nodes After Connecting Trees I](https://leetcode.com/problems/maximize-the-number-of-target-nodes-after-connecting-trees-i)

[中文文档](/solution/3300-3399/3372.Maximize%20the%20Number%20of%20Target%20Nodes%20After%20Connecting%20Trees%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Có hai cây <strong>vô hướng</strong> gồm lần lượt <code>n</code> và <code>m</code> node, với các nhãn <strong>khác nhau</strong> thuộc các khoảng <code>[0, n - 1]</code> và <code>[0, m - 1]</code>.</p>

<p>Bạn được cho hai mảng số nguyên 2 chiều <code>edges1</code> và <code>edges2</code> có độ dài lần lượt là <code>n - 1</code> và <code>m - 1</code>, trong đó <code>edges1[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> biểu thị có một cạnh nối node <code>a<sub>i</sub></code> và node <code>b<sub>i</sub></code> trong cây thứ nhất, còn <code>edges2[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> biểu thị có một cạnh nối node <code>u<sub>i</sub></code> và node <code>v<sub>i</sub></code> trong cây thứ hai. Bạn cũng được cho một số nguyên <code>k</code>.</p>

<p>Node <code>u</code> là <strong>node mục tiêu</strong> của node <code>v</code> nếu số cạnh trên đường đi từ <code>u</code> đến <code>v</code> nhỏ hơn hoặc bằng <code>k</code>. <strong>Lưu ý</strong> rằng một node <em>luôn</em> là <strong>node mục tiêu</strong> của chính nó.</p>

<p>Hãy trả về một mảng gồm <code>n</code> số nguyên <code>answer</code>, trong đó <code>answer[i]</code> là số lượng <strong>lớn nhất</strong> các node <strong>mục tiêu</strong> của node <code>i</code> trong cây thứ nhất nếu bạn phải nối một node của cây thứ nhất với một node khác trong cây thứ hai.</p>

<p><strong>Lưu ý</strong> rằng các truy vấn độc lập với nhau. Nghĩa là sau mỗi truy vấn, bạn sẽ xóa cạnh vừa thêm trước khi thực hiện truy vấn tiếp theo.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges1 = [[0,1],[0,2],[2,3],[2,4]], edges2 = [[0,1],[0,2],[0,3],[2,7],[1,4],[4,5],[4,6]], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[9,7,9,8,8]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với <code>i = 0</code>, nối node 0 của cây thứ nhất với node 0 của cây thứ hai.</li>
	<li>Với <code>i = 1</code>, nối node 1 của cây thứ nhất với node 0 của cây thứ hai.</li>
	<li>Với <code>i = 2</code>, nối node 2 của cây thứ nhất với node 4 của cây thứ hai.</li>
	<li>Với <code>i = 3</code>, nối node 3 của cây thứ nhất với node 4 của cây thứ hai.</li>
	<li>Với <code>i = 4</code>, nối node 4 của cây thứ nhất với node 4 của cây thứ hai.</li>
</ul>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3372.Maximize%20the%20Number%20of%20Target%20Nodes%20After%20Connecting%20Trees%20I/images/3982-1.png" style="width: 600px; height: 169px;" /></div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges1 = [[0,1],[0,2],[0,3],[0,4]], edges2 = [[0,1],[1,2],[2,3]], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[6,3,3,3,3]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Với mọi <code>i</code>, nối node <code>i</code> của cây thứ nhất với bất kỳ node nào của cây thứ hai.</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3372.Maximize%20the%20Number%20of%20Target%20Nodes%20After%20Connecting%20Trees%20I/images/3928-2.png" style="height: 281px; width: 500px;" /></div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n, m &lt;= 1000</code></li>
	<li><code>edges1.length == n - 1</code></li>
	<li><code>edges2.length == m - 1</code></li>
	<li><code>edges1[i].length == edges2[i].length == 2</code></li>
	<li><code>edges1[i] = [a<sub>i</sub>, b<sub>i</sub>]</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
	<li><code>edges2[i] = [u<sub>i</sub>, v<sub>i</sub>]</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt; m</code></li>
	<li>Dữ liệu đầu vào được tạo sao cho <code>edges1</code> và <code>edges2</code> biểu diễn các cây hợp lệ.</li>
	<li><code>0 &lt;= k &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt tất cả trường hợp + DFS

<!-- thinking:start -->

> **Tư duy**
>
> Ta nối node $i$ trong cây thứ nhất với một node $j$ nào đó trong cây thứ hai; các node mục tiêu là những node có khoảng cách không quá $k$. Vì $n,m \le 1000$, ta có thể chạy DFS từ mọi node bắt đầu.
>
> Sau khi thêm cạnh mới, node $i$ đóng góp các node có độ sâu $\le k$, còn node $j$ có ngân sách $k-1$. Đại lượng liên quan đến cây thứ hai không phụ thuộc vào $i$, nên ta lấy giá trị lớn nhất trên toàn cây.
>
> Tính $t$, số node tốt nhất ở độ sâu $(k-1)$ trong cây thứ hai, sau đó cộng số node ở độ sâu $k$ của mỗi node $i$ trong cây thứ nhất.

<!-- thinking:end -->

Theo mô tả bài toán, để tối đa hóa số lượng node mục tiêu của node $i$, ta phải nối node $i$ với một trong các node $j$ của cây thứ hai. Vì vậy, số lượng node mục tiêu của node $i$ có thể được chia thành hai phần:

- Trong cây thứ nhất, số node có thể đi tới từ node $i$ với độ sâu không quá $k$.
- Trong cây thứ hai, số node lớn nhất có thể đi tới từ một node $j$ bất kỳ với độ sâu không quá $k - 1$.

Do đó, trước tiên ta có thể tính số node có thể đi tới với độ sâu không quá $k - 1$ cho mỗi node trong cây thứ hai. Sau đó, ta duyệt qua từng node $i$ trong cây thứ nhất, tính tổng của hai phần trên và lấy giá trị lớn nhất.

Độ phức tạp thời gian là $O(n^2 + m^2)$, còn độ phức tạp không gian là $O(n + m)$. Ở đây, $n$ và $m$ lần lượt là số node trong hai cây.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxTargetNodes(
        self, edges1: List[List[int]], edges2: List[List[int]], k: int
    ) -> List[int]:
        def build(edges: List[List[int]]) -> List[List[int]]:
            n = len(edges) + 1
            g = [[] for _ in range(n)]
            for a, b in edges:
                g[a].append(b)
                g[b].append(a)
            return g

        def dfs(g: List[List[int]], a: int, fa: int, d: int) -> int:
            if d < 0:
                return 0
            cnt = 1
            for b in g[a]:
                if b != fa:
                    cnt += dfs(g, b, a, d - 1)
            return cnt

        g2 = build(edges2)
        m = len(edges2) + 1
        t = max(dfs(g2, i, -1, k - 1) for i in range(m))
        g1 = build(edges1)
        n = len(edges1) + 1
        return [dfs(g1, i, -1, k) + t for i in range(n)]
```

#### Java

```java
class Solution {
    public int[] maxTargetNodes(int[][] edges1, int[][] edges2, int k) {
        var g2 = build(edges2);
        int m = edges2.length + 1;
        int t = 0;
        for (int i = 0; i < m; ++i) {
            t = Math.max(t, dfs(g2, i, -1, k - 1));
        }
        var g1 = build(edges1);
        int n = edges1.length + 1;
        int[] ans = new int[n];
        Arrays.fill(ans, t);
        for (int i = 0; i < n; ++i) {
            ans[i] += dfs(g1, i, -1, k);
        }
        return ans;
    }

    private List<Integer>[] build(int[][] edges) {
        int n = edges.length + 1;
        List<Integer>[] g = new List[n];
        Arrays.setAll(g, i -> new ArrayList<>());
        for (var e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        return g;
    }

    private int dfs(List<Integer>[] g, int a, int fa, int d) {
        if (d < 0) {
            return 0;
        }
        int cnt = 1;
        for (int b : g[a]) {
            if (b != fa) {
                cnt += dfs(g, b, a, d - 1);
            }
        }
        return cnt;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> maxTargetNodes(vector<vector<int>>& edges1, vector<vector<int>>& edges2, int k) {
        auto g2 = build(edges2);
        int m = edges2.size() + 1;
        int t = 0;
        for (int i = 0; i < m; ++i) {
            t = max(t, dfs(g2, i, -1, k - 1));
        }

        auto g1 = build(edges1);
        int n = edges1.size() + 1;

        vector<int> ans(n, t);
        for (int i = 0; i < n; ++i) {
            ans[i] += dfs(g1, i, -1, k);
        }

        return ans;
    }

private:
    vector<vector<int>> build(const vector<vector<int>>& edges) {
        int n = edges.size() + 1;
        vector<vector<int>> g(n);
        for (const auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].push_back(b);
            g[b].push_back(a);
        }
        return g;
    }

    int dfs(const vector<vector<int>>& g, int a, int fa, int d) {
        if (d < 0) {
            return 0;
        }
        int cnt = 1;
        for (int b : g[a]) {
            if (b != fa) {
                cnt += dfs(g, b, a, d - 1);
            }
        }
        return cnt;
    }
};
```

#### Go

```go
func maxTargetNodes(edges1 [][]int, edges2 [][]int, k int) []int {
	g2 := build(edges2)
	m := len(edges2) + 1
	t := 0
	for i := 0; i < m; i++ {
		t = max(t, dfs(g2, i, -1, k-1))
	}

	g1 := build(edges1)
	n := len(edges1) + 1
	ans := make([]int, n)
	for i := 0; i < n; i++ {
		ans[i] = t + dfs(g1, i, -1, k)
	}
	return ans
}

func build(edges [][]int) [][]int {
	n := len(edges) + 1
	g := make([][]int, n)
	for _, e := range edges {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	return g
}

func dfs(g [][]int, a, fa, d int) int {
	if d < 0 {
		return 0
	}
	cnt := 1
	for _, b := range g[a] {
		if b != fa {
			cnt += dfs(g, b, a, d-1)
		}
	}
	return cnt
}
```

#### TypeScript

```ts
function maxTargetNodes(edges1: number[][], edges2: number[][], k: number): number[] {
    const g2 = build(edges2);
    const m = edges2.length + 1;
    let t = 0;
    for (let i = 0; i < m; i++) {
        t = Math.max(t, dfs(g2, i, -1, k - 1));
    }

    const g1 = build(edges1);
    const n = edges1.length + 1;
    const ans = Array(n).fill(t);

    for (let i = 0; i < n; i++) {
        ans[i] += dfs(g1, i, -1, k);
    }

    return ans;
}

function build(edges: number[][]): number[][] {
    const n = edges.length + 1;
    const g: number[][] = Array.from({ length: n }, () => []);
    for (const [a, b] of edges) {
        g[a].push(b);
        g[b].push(a);
    }
    return g;
}

function dfs(g: number[][], a: number, fa: number, d: number): number {
    if (d < 0) {
        return 0;
    }
    let cnt = 1;
    for (const b of g[a]) {
        if (b !== fa) {
            cnt += dfs(g, b, a, d - 1);
        }
    }
    return cnt;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_target_nodes(edges1: Vec<Vec<i32>>, edges2: Vec<Vec<i32>>, k: i32) -> Vec<i32> {
        fn build(edges: &Vec<Vec<i32>>) -> Vec<Vec<i32>> {
            let n = edges.len() + 1;
            let mut g = vec![vec![]; n];
            for e in edges {
                let a = e[0] as usize;
                let b = e[1] as usize;
                g[a].push(b as i32);
                g[b].push(a as i32);
            }
            g
        }

        fn dfs(g: &Vec<Vec<i32>>, a: usize, fa: i32, d: i32) -> i32 {
            if d < 0 {
                return 0;
            }
            let mut cnt = 1;
            for &b in &g[a] {
                if b != fa {
                    cnt += dfs(g, b as usize, a as i32, d - 1);
                }
            }
            cnt
        }

        let g2 = build(&edges2);
        let m = edges2.len() + 1;
        let mut t = 0;
        for i in 0..m {
            t = t.max(dfs(&g2, i, -1, k - 1));
        }

        let g1 = build(&edges1);
        let n = edges1.len() + 1;
        let mut ans = vec![t; n];
        for i in 0..n {
            ans[i] += dfs(&g1, i, -1, k);
        }

        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int[] MaxTargetNodes(int[][] edges1, int[][] edges2, int k) {
        var g2 = Build(edges2);
        int m = edges2.Length + 1;
        int t = 0;

        for (int i = 0; i < m; i++) {
            t = Math.Max(t, Dfs(g2, i, -1, k - 1));
        }

        var g1 = Build(edges1);
        int n = edges1.Length + 1;
        var ans = new int[n];
        Array.Fill(ans, t);

        for (int i = 0; i < n; i++) {
            ans[i] += Dfs(g1, i, -1, k);
        }

        return ans;
    }

    private List<int>[] Build(int[][] edges) {
        int n = edges.Length + 1;
        var g = new List<int>[n];
        for (int i = 0; i < n; i++) {
            g[i] = new List<int>();
        }
        foreach (var e in edges) {
            int a = e[0], b = e[1];
            g[a].Add(b);
            g[b].Add(a);
        }
        return g;
    }

    private int Dfs(List<int>[] g, int a, int fa, int d) {
        if (d < 0) {
            return 0;
        }
        int cnt = 1;
        foreach (var b in g[a]) {
            if (b != fa) {
                cnt += Dfs(g, b, a, d - 1);
            }
        }
        return cnt;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
