---
comments: true
difficulty: Hard
rating: 2308
source: Weekly Contest 210 Q4
tags:
    - Bit Manipulation
    - Tree
    - Dynamic Programming
    - Bitmask
    - Enumeration
---

<!-- problem:start -->

# [1617. Count Subtrees With Max Distance Between Cities](https://leetcode.com/problems/count-subtrees-with-max-distance-between-cities)

[中文文档](/solution/1600-1699/1617.Count%20Subtrees%20With%20Max%20Distance%20Between%20Cities/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> thành phố được đánh số từ <code>1</code> đến <code>n</code>. Cho mảng <code>edges</code> có kích thước <code>n-1</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> biểu diễn một cạnh hai chiều giữa thành phố <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code>. Giữa mỗi cặp thành phố tồn tại duy nhất một đường đi; nói cách khác, các thành phố tạo thành một <strong>cây</strong>.</p>

<p><strong>Subtree</strong> là một tập con thành phố trong đó mọi thành phố đều đi tới được mọi thành phố khác, và đường đi giữa mỗi cặp chỉ đi qua các thành phố trong tập con. Hai subtree khác nhau nếu một subtree có thành phố mà subtree kia không có.</p>

<p>Với mỗi <code>d</code> từ <code>1</code> đến <code>n-1</code>, hãy tìm số subtree mà <strong>khoảng cách lớn nhất</strong> giữa hai thành phố bất kỳ trong subtree bằng <code>d</code>.</p>

<p>Trả về <em>một mảng kích thước</em> <code>n-1</code><em> </em><em> </em>, trong đó phần tử <code>d<sup>th</sup></code> <em><strong>(đánh chỉ số từ 1)</strong> là số subtree có <strong>khoảng cách lớn nhất</strong> giữa hai thành phố bất kỳ bằng </em><code>d</code>.</p>

<p><strong>Lưu ý</strong>&nbsp;rằng&nbsp;<strong>khoảng cách</strong> giữa hai thành phố là số cạnh trên đường đi giữa chúng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1617.Count%20Subtrees%20With%20Max%20Distance%20Between%20Cities/images/p1.png" style="width: 161px; height: 181px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4, edges = [[1,2],[2,3],[2,4]]
<strong>Đầu ra:</strong> [3,4,0]
<strong>Giải thích:
</strong>Các subtree với tập con {1,2}, {2,3} và {2,4} có khoảng cách lớn nhất bằng 1.
Các subtree với tập con {1,2,3}, {1,2,4}, {2,3,4} và {1,2,3,4} có khoảng cách lớn nhất bằng 2.
Không có subtree nào có hai node với khoảng cách lớn nhất bằng 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, edges = [[1,2]]
<strong>Đầu ra:</strong> [1]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, edges = [[1,2],[2,3]]
<strong>Đầu ra:</strong> [2,1]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 15</code></li>
	<li><code>edges.length == n-1</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>1 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n</code></li>
	<li>Mọi cặp <code>(u<sub>i</sub>, v<sub>i</sub>)</code> đều khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> $n \le 15$ thành phố tạo ra $2^n$ tập con. Một tập con là subtree khi và chỉ khi đồ thị con cảm sinh liên thông; khoảng cách lớn nhất chính là đường kính của cây đó.
>
> Kiểm tra liên thông bằng DFS/BFS, xóa các bit đã thăm khỏi mask; mask bằng 0 nghĩa là đã đi tới mọi thành phố được chọn. Đường kính được tìm bằng hai lần tìm kiếm quen thuộc: tìm đỉnh xa nhất từ một đỉnh bất kỳ, rồi tìm đỉnh xa nhất từ đỉnh đó.
>
> Bỏ qua các mask chỉ có một bit. Với mỗi $\textit{mask}$ còn lại, thực hiện kiểm tra và tăng $\textit{ans}[mx-1]$ lên một.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSubgraphsForEachDiameter(
        self, n: int, edges: List[List[int]]
    ) -> List[int]:
        def dfs(u: int, d: int = 0):
            nonlocal mx, nxt, msk
            if mx < d:
                mx, nxt = d, u
            msk ^= 1 << u
            for v in g[u]:
                if msk >> v & 1:
                    dfs(v, d + 1)

        g = defaultdict(list)
        for u, v in edges:
            u, v = u - 1, v - 1
            g[u].append(v)
            g[v].append(u)
        ans = [0] * (n - 1)
        nxt = mx = 0
        for mask in range(1, 1 << n):
            if mask & (mask - 1) == 0:
                continue
            msk, mx = mask, 0
            cur = msk.bit_length() - 1
            dfs(cur)
            if msk == 0:
                msk, mx = mask, 0
                dfs(nxt)
                ans[mx - 1] += 1
        return ans
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private int msk;
    private int nxt;
    private int mx;

    public int[] countSubgraphsForEachDiameter(int n, int[][] edges) {
        g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int[] e : edges) {
            int u = e[0] - 1, v = e[1] - 1;
            g[u].add(v);
            g[v].add(u);
        }
        int[] ans = new int[n - 1];
        for (int mask = 1; mask < 1 << n; ++mask) {
            if ((mask & (mask - 1)) == 0) {
                continue;
            }
            msk = mask;
            mx = 0;
            int cur = 31 - Integer.numberOfLeadingZeros(msk);
            dfs(cur, 0);
            if (msk == 0) {
                msk = mask;
                mx = 0;
                dfs(nxt, 0);
                ++ans[mx - 1];
            }
        }
        return ans;
    }

    private void dfs(int u, int d) {
        msk ^= 1 << u;
        if (mx < d) {
            mx = d;
            nxt = u;
        }
        for (int v : g[u]) {
            if ((msk >> v & 1) == 1) {
                dfs(v, d + 1);
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> countSubgraphsForEachDiameter(int n, vector<vector<int>>& edges) {
        vector<vector<int>> g(n);
        for (auto& e : edges) {
            int u = e[0] - 1, v = e[1] - 1;
            g[u].emplace_back(v);
            g[v].emplace_back(u);
        }
        vector<int> ans(n - 1);
        int nxt = 0, msk = 0, mx = 0;
        function<void(int, int)> dfs = [&](int u, int d) {
            msk ^= 1 << u;
            if (mx < d) {
                mx = d;
                nxt = u;
            }
            for (int& v : g[u]) {
                if (msk >> v & 1) {
                    dfs(v, d + 1);
                }
            }
        };
        for (int mask = 1; mask < 1 << n; ++mask) {
            if ((mask & (mask - 1)) == 0) {
                continue;
            }
            msk = mask;
            mx = 0;
            int cur = 31 - __builtin_clz(msk);
            dfs(cur, 0);
            if (msk == 0) {
                msk = mask;
                mx = 0;
                dfs(nxt, 0);
                ++ans[mx - 1];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countSubgraphsForEachDiameter(n int, edges [][]int) []int {
	g := make([][]int, n)
	for _, e := range edges {
		u, v := e[0]-1, e[1]-1
		g[u] = append(g[u], v)
		g[v] = append(g[v], u)
	}
	ans := make([]int, n-1)
	var msk, nxt, mx int
	var dfs func(int, int)
	dfs = func(u, d int) {
		msk ^= 1 << u
		if mx < d {
			mx, nxt = d, u
		}
		for _, v := range g[u] {
			if msk>>v&1 == 1 {
				dfs(v, d+1)
			}
		}
	}
	for mask := 1; mask < 1<<n; mask++ {
		if mask&(mask-1) == 0 {
			continue
		}
		msk, mx = mask, 0
		cur := bits.Len(uint(msk)) - 1
		dfs(cur, 0)
		if msk == 0 {
			msk, mx = mask, 0
			dfs(nxt, 0)
			ans[mx-1]++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function countSubgraphsForEachDiameter(n: number, edges: number[][]): number[] {
    const g = Array.from({ length: n }, () => []);
    for (const [u, v] of edges) {
        g[u - 1].push(v - 1);
        g[v - 1].push(u - 1);
    }
    const ans: number[] = new Array(n - 1).fill(0);
    let [mx, msk, nxt] = [0, 0, 0];
    const dfs = (u: number, d: number) => {
        if (mx < d) {
            mx = d;
            nxt = u;
        }
        msk ^= 1 << u;
        for (const v of g[u]) {
            if ((msk >> v) & 1) {
                dfs(v, d + 1);
            }
        }
    };
    for (let mask = 1; mask < 1 << n; ++mask) {
        if ((mask & (mask - 1)) === 0) {
            continue;
        }
        msk = mask;
        mx = 0;
        const cur = 31 - numberOfLeadingZeros(msk);
        dfs(cur, 0);
        if (msk === 0) {
            msk = mask;
            mx = 0;
            dfs(nxt, 0);
            ++ans[mx - 1];
        }
    }
    return ans;
}

function numberOfLeadingZeros(i: number): number {
    if (i == 0) return 32;
    let n = 1;
    if (i >>> 16 == 0) {
        n += 16;
        i <<= 16;
    }
    if (i >>> 24 == 0) {
        n += 8;
        i <<= 8;
    }
    if (i >>> 28 == 0) {
        n += 4;
        i <<= 4;
    }
    if (i >>> 30 == 0) {
        n += 2;
        i <<= 2;
    }
    n -= i >>> 31;
    return n;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 tìm đường kính bằng DFS đệ quy. Cùng cách “xóa mask rồi tìm hai lần đỉnh xa nhất” cũng hoạt động với BFS theo từng lớp, trong đó khoảng cách chính là số lớp và không gặp giới hạn độ sâu đệ quy.
>
> Thuật toán không đổi; chỉ thay cách tìm kiếm bằng queue.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSubgraphsForEachDiameter(
        self, n: int, edges: List[List[int]]
    ) -> List[int]:
        def bfs(u: int) -> int:
            d = -1
            q = deque([u])
            nonlocal msk, nxt
            msk ^= 1 << u
            while q:
                d += 1
                for _ in range(len(q)):
                    nxt = u = q.popleft()
                    for v in g[u]:
                        if msk >> v & 1:
                            msk ^= 1 << v
                            q.append(v)
            return d

        g = defaultdict(list)
        for u, v in edges:
            u, v = u - 1, v - 1
            g[u].append(v)
            g[v].append(u)
        ans = [0] * (n - 1)
        nxt = 0
        for mask in range(1, 1 << n):
            if mask & (mask - 1) == 0:
                continue
            msk = mask
            cur = msk.bit_length() - 1
            bfs(cur)
            if msk == 0:
                msk = mask
                mx = bfs(nxt)
                ans[mx - 1] += 1
        return ans
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private int msk;
    private int nxt;

    public int[] countSubgraphsForEachDiameter(int n, int[][] edges) {
        g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int[] e : edges) {
            int u = e[0] - 1, v = e[1] - 1;
            g[u].add(v);
            g[v].add(u);
        }
        int[] ans = new int[n - 1];
        for (int mask = 1; mask < 1 << n; ++mask) {
            if ((mask & (mask - 1)) == 0) {
                continue;
            }
            msk = mask;
            int cur = 31 - Integer.numberOfLeadingZeros(msk);
            bfs(cur);
            if (msk == 0) {
                msk = mask;
                int mx = bfs(nxt);
                ++ans[mx - 1];
            }
        }
        return ans;
    }

    private int bfs(int u) {
        int d = -1;
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(u);
        msk ^= 1 << u;
        while (!q.isEmpty()) {
            ++d;
            for (int k = q.size(); k > 0; --k) {
                u = q.poll();
                nxt = u;
                for (int v : g[u]) {
                    if ((msk >> v & 1) == 1) {
                        msk ^= 1 << v;
                        q.offer(v);
                    }
                }
            }
        }
        return d;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> countSubgraphsForEachDiameter(int n, vector<vector<int>>& edges) {
        vector<vector<int>> g(n);
        for (auto& e : edges) {
            int u = e[0] - 1, v = e[1] - 1;
            g[u].emplace_back(v);
            g[v].emplace_back(u);
        }
        vector<int> ans(n - 1);
        int nxt = 0, msk = 0;
        auto bfs = [&](int u) -> int {
            int d = -1;
            msk ^= 1 << u;
            queue<int> q{{u}};
            while (!q.empty()) {
                ++d;
                for (int k = q.size(); k; --k) {
                    u = q.front();
                    nxt = u;
                    q.pop();
                    for (int& v : g[u]) {
                        if (msk >> v & 1) {
                            msk ^= 1 << v;
                            q.push(v);
                        }
                    }
                }
            }
            return d;
        };
        for (int mask = 1; mask < 1 << n; ++mask) {
            if ((mask & (mask - 1)) == 0) {
                continue;
            }
            msk = mask;
            int cur = 31 - __builtin_clz(msk);
            bfs(cur);
            if (msk == 0) {
                msk = mask;
                int mx = bfs(nxt);
                ++ans[mx - 1];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countSubgraphsForEachDiameter(n int, edges [][]int) []int {
	g := make([][]int, n)
	for _, e := range edges {
		u, v := e[0]-1, e[1]-1
		g[u] = append(g[u], v)
		g[v] = append(g[v], u)
	}
	ans := make([]int, n-1)
	var msk, nxt int
	bfs := func(u int) int {
		d := -1
		q := []int{u}
		msk ^= 1 << u
		for len(q) > 0 {
			d++
			for k := len(q); k > 0; k-- {
				u = q[0]
				q = q[1:]
				nxt = u
				for _, v := range g[u] {
					if msk>>v&1 == 1 {
						msk ^= 1 << v
						q = append(q, v)
					}
				}
			}
		}
		return d
	}
	for mask := 1; mask < 1<<n; mask++ {
		if mask&(mask-1) == 0 {
			continue
		}
		msk = mask
		cur := bits.Len(uint(msk)) - 1
		bfs(cur)
		if msk == 0 {
			msk = mask
			mx := bfs(nxt)
			ans[mx-1]++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function countSubgraphsForEachDiameter(n: number, edges: number[][]): number[] {
    const g = Array.from({ length: n }, () => []);
    for (const [u, v] of edges) {
        g[u - 1].push(v - 1);
        g[v - 1].push(u - 1);
    }
    const ans: number[] = new Array(n - 1).fill(0);
    let [msk, nxt] = [0, 0];
    const bfs = (u: number) => {
        let d = -1;
        const q = [u];
        msk ^= 1 << u;
        while (q.length) {
            ++d;
            for (let k = q.length; k; --k) {
                u = q.shift()!;
                nxt = u;
                for (const v of g[u]) {
                    if ((msk >> v) & 1) {
                        msk ^= 1 << v;
                        q.push(v);
                    }
                }
            }
        }
        return d;
    };
    for (let mask = 1; mask < 1 << n; ++mask) {
        if ((mask & (mask - 1)) === 0) {
            continue;
        }
        msk = mask;
        const cur = 31 - numberOfLeadingZeros(msk);
        bfs(cur);
        if (msk === 0) {
            msk = mask;
            const mx = bfs(nxt);
            ++ans[mx - 1];
        }
    }
    return ans;
}

function numberOfLeadingZeros(i: number): number {
    if (i == 0) return 32;
    let n = 1;
    if (i >>> 16 == 0) {
        n += 16;
        i <<= 16;
    }
    if (i >>> 24 == 0) {
        n += 8;
        i <<= 8;
    }
    if (i >>> 28 == 0) {
        n += 4;
        i <<= 4;
    }
    if (i >>> 30 == 0) {
        n += 2;
        i <<= 2;
    }
    n -= i >>> 31;
    return n;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
