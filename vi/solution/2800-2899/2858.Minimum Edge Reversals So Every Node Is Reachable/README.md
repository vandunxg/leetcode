---
comments: true
difficulty: Hard
rating: 2294
source: Biweekly Contest 113 Q4
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Dynamic Programming
---

<!-- problem:start -->

# [2858. Minimum Edge Reversals So Every Node Is Reachable](https://leetcode.com/problems/minimum-edge-reversals-so-every-node-is-reachable)

[中文文档](/solution/2800-2899/2858.Minimum%20Edge%20Reversals%20So%20Every%20Node%20Is%20Reachable/README.md)

## Mô tả

<!-- description:start -->

<p>Có một <strong>đồ thị có hướng đơn</strong> gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code>. Nếu các cạnh của đồ thị là hai chiều, đồ thị sẽ tạo thành một <strong>cây</strong>.</p>

<p>Bạn được cho một số nguyên <code>n</code> và một mảng số nguyên <strong>2 chiều</strong> <code>edges</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> biểu diễn một <strong>cạnh có hướng</strong> đi từ node <code>u<sub>i</sub></code> đến node <code>v<sub>i</sub></code>.</p>

<p><strong>Đảo một cạnh</strong> sẽ thay đổi hướng của cạnh đó, tức là cạnh có hướng đi từ node <code>u<sub>i</sub></code> đến node <code>v<sub>i</sub></code> sẽ trở thành cạnh có hướng đi từ node <code>v<sub>i</sub></code> đến node <code>u<sub>i</sub></code>.</p>

<p>Với mỗi node <code>i</code> trong khoảng <code>[0, n - 1]</code>, hãy <strong>độc lập</strong> tính số <strong>lần đảo cạnh</strong> <strong>nhỏ nhất</strong> cần thực hiện để từ node <code>i</code> có thể đi đến mọi node khác thông qua một <strong>dãy</strong> các <strong>cạnh có hướng</strong>.</p>

<p>Trả về <em>một mảng số nguyên </em><code>answer</code><em>, trong đó </em><code>answer[i]</code><em> là </em><em> </em> <em><strong>lần đảo cạnh</strong> <strong>nhỏ nhất</strong> cần thực hiện để từ node </em><code>i</code><em> có thể đi đến mọi node khác thông qua một <strong>dãy</strong> các <strong>cạnh có hướng</strong>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img height="246" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2800-2899/2858.Minimum%20Edge%20Reversals%20So%20Every%20Node%20Is%20Reachable/images/image-20230826221104-3.png" width="312" /></p>

<pre>
<strong>Đầu vào:</strong> n = 4, edges = [[2,0],[2,1],[1,3]]
<strong>Đầu ra:</strong> [1,1,0,2]
<strong>Giải thích:</strong> Hình trên minh họa đồ thị được tạo bởi các cạnh.
Với node 0: sau khi đảo cạnh [2,0], ta có thể đi đến mọi node khác bắt đầu từ node 0.
Do đó, answer[0] = 1.
Với node 1: sau khi đảo cạnh [2,1], ta có thể đi đến mọi node khác bắt đầu từ node 1.
Do đó, answer[1] = 1.
Với node 2: từ node 2 đã có thể đi đến mọi node khác.
Do đó, answer[2] = 0.
Với node 3: sau khi đảo các cạnh [1,3] và [2,1], ta có thể đi đến mọi node khác bắt đầu từ node 3.
Do đó, answer[3] = 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img height="217" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2800-2899/2858.Minimum%20Edge%20Reversals%20So%20Every%20Node%20Is%20Reachable/images/image-20230826225541-2.png" width="322" /></p>

<pre>
<strong>Đầu vào:</strong> n = 3, edges = [[1,2],[2,0]]
<strong>Đầu ra:</strong> [2,0,1]
<strong>Giải thích:</strong> Hình trên minh họa đồ thị được tạo bởi các cạnh.
Với node 0: sau khi đảo các cạnh [2,0] và [1,2], ta có thể đi đến mọi node khác bắt đầu từ node 0.
Do đó, answer[0] = 2.
Với node 1: từ node 1 đã có thể đi đến mọi node khác.
Do đó, answer[1] = 0.
Với node 2: sau khi đảo cạnh [1, 2], ta có thể đi đến mọi node khác bắt đầu từ node 2.
Do đó, answer[2] = 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>0 &lt;= u<sub>i</sub> == edges[i][0] &lt; n</code></li>
	<li><code>0 &lt;= v<sub>i</sub> == edges[i][1] &lt; n</code></li>
	<li><code>u<sub>i</sub> != v<sub>i</sub></code></li>
	<li>Dữ liệu đầu vào được tạo sao cho nếu các cạnh là hai chiều, đồ thị sẽ tạo thành một cây.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi node được chọn làm gốc, ta cần số lần đảo cạnh ít nhất để toàn bộ cây có thể đi đến được. DFS đầu tiên từ node $0$ đếm các cạnh ngược chiều để tính $ans[0]$. Khi đổi gốc qua một cạnh, đáp án thay đổi $\pm 1$, vì vậy DFS thứ hai có thể tính đáp án cho mọi node.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minEdgeReversals(self, n: int, edges: List[List[int]]) -> List[int]:
        ans = [0] * n
        g = [[] for _ in range(n)]
        for x, y in edges:
            g[x].append((y, 1))
            g[y].append((x, -1))

        def dfs(i: int, fa: int):
            for j, k in g[i]:
                if j != fa:
                    ans[0] += int(k < 0)
                    dfs(j, i)

        dfs(0, -1)

        def dfs2(i: int, fa: int):
            for j, k in g[i]:
                if j != fa:
                    ans[j] = ans[i] + k
                    dfs2(j, i)

        dfs2(0, -1)
        return ans
```

#### Java

```java
class Solution {
    private List<int[]>[] g;
    private int[] ans;

    public int[] minEdgeReversals(int n, int[][] edges) {
        ans = new int[n];
        g = new List[n];
        Arrays.setAll(g, i -> new ArrayList<>());
        for (var e : edges) {
            int x = e[0], y = e[1];
            g[x].add(new int[] {y, 1});
            g[y].add(new int[] {x, -1});
        }
        dfs(0, -1);
        dfs2(0, -1);
        return ans;
    }

    private void dfs(int i, int fa) {
        for (var ne : g[i]) {
            int j = ne[0], k = ne[1];
            if (j != fa) {
                ans[0] += k < 0 ? 1 : 0;
                dfs(j, i);
            }
        }
    }

    private void dfs2(int i, int fa) {
        for (var ne : g[i]) {
            int j = ne[0], k = ne[1];
            if (j != fa) {
                ans[j] = ans[i] + k;
                dfs2(j, i);
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> minEdgeReversals(int n, vector<vector<int>>& edges) {
        vector<pair<int, int>> g[n];
        vector<int> ans(n);
        for (auto& e : edges) {
            int x = e[0], y = e[1];
            g[x].emplace_back(y, 1);
            g[y].emplace_back(x, -1);
        }
        function<void(int, int)> dfs = [&](int i, int fa) {
            for (auto& [j, k] : g[i]) {
                if (j != fa) {
                    ans[0] += k < 0;
                    dfs(j, i);
                }
            }
        };
        function<void(int, int)> dfs2 = [&](int i, int fa) {
            for (auto& [j, k] : g[i]) {
                if (j != fa) {
                    ans[j] = ans[i] + k;
                    dfs2(j, i);
                }
            }
        };
        dfs(0, -1);
        dfs2(0, -1);
        return ans;
    }
};
```

#### Go

```go
func minEdgeReversals(n int, edges [][]int) []int {
	g := make([][][2]int, n)
	for _, e := range edges {
		x, y := e[0], e[1]
		g[x] = append(g[x], [2]int{y, 1})
		g[y] = append(g[y], [2]int{x, -1})
	}
	ans := make([]int, n)
	var dfs func(int, int)
	var dfs2 func(int, int)
	dfs = func(i, fa int) {
		for _, ne := range g[i] {
			j, k := ne[0], ne[1]
			if j != fa {
				if k < 0 {
					ans[0]++
				}
				dfs(j, i)
			}
		}
	}
	dfs2 = func(i, fa int) {
		for _, ne := range g[i] {
			j, k := ne[0], ne[1]
			if j != fa {
				ans[j] = ans[i] + k
				dfs2(j, i)
			}
		}
	}
	dfs(0, -1)
	dfs2(0, -1)
	return ans
}
```

#### TypeScript

```ts
function minEdgeReversals(n: number, edges: number[][]): number[] {
    const g: number[][][] = Array.from({ length: n }, () => []);
    for (const [x, y] of edges) {
        g[x].push([y, 1]);
        g[y].push([x, -1]);
    }
    const ans: number[] = Array(n).fill(0);
    const dfs = (i: number, fa: number) => {
        for (const [j, k] of g[i]) {
            if (j !== fa) {
                ans[0] += k < 0 ? 1 : 0;
                dfs(j, i);
            }
        }
    };
    const dfs2 = (i: number, fa: number) => {
        for (const [j, k] of g[i]) {
            if (j !== fa) {
                ans[j] = ans[i] + k;
                dfs2(j, i);
            }
        }
    };
    dfs(0, -1);
    dfs2(0, -1);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
