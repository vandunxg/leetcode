---
comments: true
difficulty: Medium
rating: 1679
source: Weekly Contest 322 Q3
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Graph
---

<!-- problem:start -->

# [2492. Minimum Score of a Path Between Two Cities](https://leetcode.com/problems/minimum-score-of-a-path-between-two-cities)

[中文文档](/solution/2400-2499/2492.Minimum%20Score%20of%20a%20Path%20Between%20Two%20Cities/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên dương <code>n</code> biểu thị <code>n</code> thành phố được đánh số từ <code>1</code> đến <code>n</code>. Bạn cũng được cung cấp một mảng <strong>2 chiều</strong> <code>roads</code>, trong đó <code>roads[i] = [a<sub>i</sub>, b<sub>i</sub>, distance<sub>i</sub>]</code> biểu thị có một con đường <strong>hai chiều</strong> nối thành phố <code>a<sub>i</sub></code> và thành phố <code>b<sub>i</sub></code>, với khoảng cách bằng <code>distance<sub>i</sub></code>. Đồ thị các thành phố không nhất thiết phải liên thông.</p>

<p><strong>Điểm số</strong> của một đường đi giữa hai thành phố được định nghĩa là khoảng cách <strong>nhỏ nhất</strong> của một con đường trên đường đi đó.</p>

<p>Hãy trả về điểm số <strong>nhỏ nhất</strong> có thể có của một đường đi giữa thành phố 1 và thành phố <code>n</code>.</p>

<p><strong>Lưu ý</strong>:</p>

<ul>
	<li>Một đường đi là một dãy các con đường nối giữa hai thành phố.</li>
	<li>Một đường đi có thể đi qua cùng một con đường <strong>nhiều lần</strong>, và bạn có thể ghé thăm thành phố 1 và thành phố <code>n</code> nhiều lần trên đường đi.</li>
	<li>Các test case được tạo sao cho có <strong>ít nhất</strong> một đường đi giữa 1 và <code>n</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2492.Minimum%20Score%20of%20a%20Path%20Between%20Two%20Cities/images/graph11.png" style="width: 190px; height: 231px;" />
<pre>
<strong>Đầu vào:</strong> n = 4, roads = [[1,2,9],[2,3,6],[2,4,5],[1,4,7]]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Đường đi từ thành phố 1 đến thành phố 4 có điểm số nhỏ nhất là: 1 -&gt; 2 -&gt; 4. Điểm số của đường đi này là min(9,5) = 5.
Có thể chứng minh rằng không có đường đi nào khác có điểm số nhỏ hơn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2492.Minimum%20Score%20of%20a%20Path%20Between%20Two%20Cities/images/graph22.png" style="width: 190px; height: 231px;" />
<pre>
<strong>Đầu vào:</strong> n = 4, roads = [[1,2,2],[1,3,4],[3,4,7]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Đường đi từ thành phố 1 đến thành phố 4 có điểm số nhỏ nhất là: 1 -&gt; 2 -&gt; 1 -&gt; 3 -&gt; 4. Điểm số của đường đi này là min(2,2,4,7) = 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= roads.length &lt;= 10<sup>5</sup></code></li>
	<li><code>roads[i].length == 3</code></li>
	<li><code>1 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt;= n</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li><code>1 &lt;= distance<sub>i</sub> &lt;= 10<sup>4</sup></code></li>
	<li>Không có cạnh nào bị lặp.</li>
	<li>Có ít nhất một đường đi giữa <code>1</code> và <code>n</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Các cạnh có thể được đi qua lại và $1$ liên thông với $n$. Điểm số của một đường đi là cạnh nhẹ nhất của nó, đồng thời mọi walk từ $1$ đến $n$ đều có thể đi qua mọi cạnh trong thành phần đó, nên đáp án là trọng số nhỏ nhất trong thành phần chứa $1$.
>
> Thực hiện DFS từ $1$, cập nhật đáp án khi duyệt qua mỗi cạnh.

<!-- thinking:end -->

Theo mô tả bài toán, mỗi cạnh có thể được đi qua nhiều lần, và node $1$ và node $n$ được đảm bảo nằm trong cùng một thành phần liên thông. Vì vậy, thực chất bài toán yêu cầu tìm trọng số cạnh nhỏ nhất trong thành phần liên thông chứa node $1$.

Trước tiên, ta xây dựng đồ thị vô hướng $g$ từ $\textit{roads}$, sau đó thực hiện DFS bắt đầu từ node $1$. Trong quá trình duyệt qua thành phần liên thông, ta cập nhật đáp án bằng $\textit{ans} = \min(\textit{ans}, w)$ với mỗi cạnh được duyệt.

Độ phức tạp thời gian là $O(n + m)$, và độ phức tạp không gian là $O(n + m)$, trong đó $n$ và $m$ lần lượt là số node và số cạnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minScore(self, n: int, roads: List[List[int]]) -> int:
        def dfs(a: int):
            vis[a] = True
            nonlocal ans
            for b, w in g[a]:
                ans = min(ans, w)
                if not vis[b]:
                    dfs(b)

        g = [[] for _ in range(n + 1)]
        for a, b, w in roads:
            g[a].append((b, w))
            g[b].append((a, w))
        ans = inf
        vis = [False] * (n + 1)
        dfs(1)
        return ans
```

#### Java

```java
class Solution {
    private int ans;
    private boolean[] vis;
    private List<int[]>[] g;

    public int minScore(int n, int[][] roads) {
        g = new ArrayList[n + 1];
        Arrays.setAll(g, k -> new ArrayList<>());

        for (int[] e : roads) {
            int a = e[0], b = e[1], w = e[2];
            g[a].add(new int[]{b, w});
            g[b].add(new int[]{a, w});
        }

        ans = Integer.MAX_VALUE;
        vis = new boolean[n + 1];

        dfs(1);
        return ans;
    }

    private void dfs(int a) {
        vis[a] = true;
        for (int[] nb : g[a]) {
            int b = nb[0], w = nb[1];
            ans = Math.min(ans, w);
            if (!vis[b]) {
                dfs(b);
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minScore(int n, vector<vector<int>>& roads) {
        vector<vector<pair<int,int>>> g(n + 1);
        for (auto &e : roads) {
            int a = e[0], b = e[1], w = e[2];
            g[a].push_back({b, w});
            g[b].push_back({a, w});
        }

        vector<bool> vis(n + 1, false);
        int ans = INT_MAX;

        auto dfs = [&](this auto&& dfs, int a) -> void {
            vis[a] = true;
            for (auto &[b, w] : g[a]) {
                ans = min(ans, w);
                if (!vis[b]) {
                    dfs(b);
                }
            }
        };

        dfs(1);
        return ans;
    }
};
```

#### Go

```go
func minScore(n int, roads [][]int) int {
	g := make([][][2]int, n+1)
	for _, e := range roads {
		a, b, w := e[0], e[1], e[2]
		g[a] = append(g[a], [2]int{b, w})
		g[b] = append(g[b], [2]int{a, w})
	}

	vis := make([]bool, n+1)
	ans := int(1e9)

	var dfs func(int)
	dfs = func(a int) {
		vis[a] = true
		for _, nb := range g[a] {
			b, w := nb[0], nb[1]
			ans = min(ans, w)
			if !vis[b] {
				dfs(b)
			}
		}
	}

	dfs(1)
	return ans
}
```

#### TypeScript

```ts
function minScore(n: number, roads: number[][]): number {
    const g: [number, number][][] = Array.from({ length: n + 1 }, () => []);
    for (const [a, b, w] of roads) {
        g[a].push([b, w]);
        g[b].push([a, w]);
    }

    const vis = new Array(n + 1).fill(false);
    let ans = Infinity;

    const dfs = (a: number): void => {
        vis[a] = true;
        for (const [b, w] of g[a]) {
            ans = Math.min(ans, w);
            if (!vis[b]) {
                dfs(b);
            }
        }
    };

    dfs(1);
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_score(n: i32, roads: Vec<Vec<i32>>) -> i32 {
        let n = n as usize;
        let mut g: Vec<Vec<(usize, i32)>> = vec![vec![]; n + 1];

        for e in roads {
            let a = e[0] as usize;
            let b = e[1] as usize;
            let w = e[2];
            g[a].push((b, w));
            g[b].push((a, w));
        }

        let mut vis = vec![false; n + 1];
        let mut ans = i32::MAX;

        fn dfs(
            a: usize,
            g: &Vec<Vec<(usize, i32)>>,
            vis: &mut Vec<bool>,
            ans: &mut i32,
        ) {
            vis[a] = true;

            for &(b, w) in &g[a] {
                *ans = (*ans).min(w);
                if !vis[b] {
                    dfs(b, g, vis, ans);
                }
            }
        }

        dfs(1, &g, &mut vis, &mut ans);
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @param {number[][]} roads
 * @return {number}
 */
var minScore = function (n, roads) {
    const g = Array.from({ length: n + 1 }, () => []);

    for (const [a, b, w] of roads) {
        g[a].push([b, w]);
        g[b].push([a, w]);
    }

    const vis = new Array(n + 1).fill(false);
    let ans = Infinity;

    const dfs = a => {
        vis[a] = true;
        for (const [b, w] of g[a]) {
            ans = Math.min(ans, w);
            if (!vis[b]) dfs(b);
        }
    };

    dfs(1);
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 đã tìm được đáp án. Ta có thể dùng queue để duyệt theo đúng tập node đó; chỉ thay đổi cách duyệt.

<!-- thinking:end -->

Ta cũng có thể dùng BFS để giải bài toán này. Đưa node $1$ vào queue và mở rộng thành phần liên thông theo từng lớp, đồng thời cập nhật đáp án bằng $\textit{ans} = \min(\textit{ans}, w)$ mỗi khi duyệt một cạnh.

Độ phức tạp thời gian là $O(n + m)$, và độ phức tạp không gian là $O(n + m)$, trong đó $n$ và $m$ lần lượt là số node và số cạnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minScore(self, n: int, roads: List[List[int]]) -> int:
        g = [[] for _ in range(n + 1)]
        for a, b, w in roads:
            g[a].append((b, w))
            g[b].append((a, w))
        vis = [False] * (n + 1)
        vis[1] = True
        ans = inf
        q = deque([1])
        while q:
            for _ in range(len(q)):
                a = q.popleft()
                for b, w in g[a]:
                    ans = min(ans, w)
                    if not vis[b]:
                        vis[b] = True
                        q.append(b)
        return ans
```

#### Java

```java
class Solution {
    public int minScore(int n, int[][] roads) {
        List<int[]>[] g = new ArrayList[n + 1];
        Arrays.setAll(g, k -> new ArrayList<>());

        for (int[] e : roads) {
            int a = e[0], b = e[1], w = e[2];
            g[a].add(new int[] {b, w});
            g[b].add(new int[] {a, w});
        }

        boolean[] vis = new boolean[n + 1];
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(1);
        vis[1] = true;
        int ans = Integer.MAX_VALUE;

        while (!q.isEmpty()) {
            for (int k = q.size(); k > 0; --k) {
                int a = q.pollFirst();
                for (int[] nb : g[a]) {
                    int b = nb[0], w = nb[1];
                    ans = Math.min(ans, w);
                    if (!vis[b]) {
                        vis[b] = true;
                        q.offer(b);
                    }
                }
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
    int minScore(int n, vector<vector<int>>& roads) {
        vector<vector<pair<int, int>>> g(n + 1);
        for (auto& e : roads) {
            int a = e[0], b = e[1], w = e[2];
            g[a].push_back({b, w});
            g[b].push_back({a, w});
        }

        vector<bool> vis(n + 1, false);
        int ans = INT_MAX;
        queue<int> q{{1}};
        vis[1] = true;

        while (!q.empty()) {
            for (int k = q.size(); k; --k) {
                int a = q.front();
                q.pop();
                for (auto [b, w] : g[a]) {
                    ans = min(ans, w);
                    if (!vis[b]) {
                        vis[b] = true;
                        q.push(b);
                    }
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minScore(n int, roads [][]int) int {
	g := make([][][2]int, n+1)
	for _, e := range roads {
		a, b, w := e[0], e[1], e[2]
		g[a] = append(g[a], [2]int{b, w})
		g[b] = append(g[b], [2]int{a, w})
	}

	vis := make([]bool, n+1)
	ans := int(1e9)
	q := []int{1}
	vis[1] = true

	for len(q) > 0 {
		for k := len(q); k > 0; k-- {
			a := q[0]
			q = q[1:]
			for _, nb := range g[a] {
				b, w := nb[0], nb[1]
				ans = min(ans, w)
				if !vis[b] {
					vis[b] = true
					q = append(q, b)
				}
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function minScore(n: number, roads: number[][]): number {
    const g: [number, number][][] = Array.from({ length: n + 1 }, () => []);
    for (const [a, b, w] of roads) {
        g[a].push([b, w]);
        g[b].push([a, w]);
    }

    const vis = new Array(n + 1).fill(false);
    let ans = Infinity;
    let q: number[] = [1];
    vis[1] = true;

    while (q.length > 0) {
        const nq: number[] = [];
        for (const a of q) {
            for (const [b, w] of g[a]) {
                ans = Math.min(ans, w);
                if (!vis[b]) {
                    vis[b] = true;
                    nq.push(b);
                }
            }
        }
        q = nq;
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::VecDeque;

impl Solution {
    pub fn min_score(n: i32, roads: Vec<Vec<i32>>) -> i32 {
        let n = n as usize;
        let mut g: Vec<Vec<(usize, i32)>> = vec![vec![]; n + 1];

        for e in roads {
            let a = e[0] as usize;
            let b = e[1] as usize;
            let w = e[2];
            g[a].push((b, w));
            g[b].push((a, w));
        }

        let mut vis = vec![false; n + 1];
        let mut ans = i32::MAX;
        let mut q = VecDeque::new();
        q.push_back(1);
        vis[1] = true;

        while !q.is_empty() {
            for _ in 0..q.len() {
                let a = q.pop_front().unwrap();
                for &(b, w) in &g[a] {
                    ans = ans.min(w);
                    if !vis[b] {
                        vis[b] = true;
                        q.push_back(b);
                    }
                }
            }
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @param {number[][]} roads
 * @return {number}
 */
var minScore = function (n, roads) {
    const g = Array.from({ length: n + 1 }, () => []);

    for (const [a, b, w] of roads) {
        g[a].push([b, w]);
        g[b].push([a, w]);
    }

    const vis = new Array(n + 1).fill(false);
    let ans = Infinity;
    let q = [1];
    vis[1] = true;

    while (q.length > 0) {
        const nq = [];
        for (const a of q) {
            for (const [b, w] of g[a]) {
                ans = Math.min(ans, w);
                if (!vis[b]) {
                    vis[b] = true;
                    nq.push(b);
                }
            }
        }
        q = nq;
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
