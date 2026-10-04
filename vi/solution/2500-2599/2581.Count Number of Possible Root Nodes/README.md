---
comments: true
difficulty: Hard
rating: 2228
source: Biweekly Contest 99 Q4
tags:
    - Tree
    - Depth-First Search
    - Array
    - Hash Table
    - Dynamic Programming
---

<!-- problem:start -->

# [2581. Count Number of Possible Root Nodes](https://leetcode.com/problems/count-number-of-possible-root-nodes)

[中文文档](/solution/2500-2599/2581.Count%20Number%20of%20Possible%20Root%20Nodes/README.md)

## Mô tả

<!-- description:start -->

<p>Alice có một cây vô hướng gồm <code>n</code> nút được đánh số từ <code>0</code> đến <code>n - 1</code>. Cây được biểu diễn bằng một mảng số nguyên 2 chiều <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> cho biết có một cạnh giữa các nút <code>a<sub>i</sub></code> và <code>b<sub>i</sub></code> trong cây.</p>

<p>Alice muốn Bob tìm gốc của cây. Cô ấy cho phép Bob đưa ra một số <strong>dự đoán</strong> về cây. Trong mỗi dự đoán, Bob thực hiện như sau:</p>

<ul>
	<li>Chọn hai số nguyên <strong>phân biệt</strong> <code>u</code> và <code>v</code> sao cho có một cạnh <code>[u, v]</code> trong cây.</li>
	<li>Nói với Alice rằng <code>u</code> là <strong>cha</strong> của <code>v</code> trong cây.</li>
</ul>

<p>Các dự đoán của Bob được biểu diễn bằng một mảng số nguyên 2 chiều <code>guesses</code>, trong đó <code>guesses[j] = [u<sub>j</sub>, v<sub>j</sub>]</code> cho biết Bob dự đoán <code>u<sub>j</sub></code> là cha của <code>v<sub>j</sub></code>.</p>

<p>Vì Alice lười, cô ấy không trả lời từng dự đoán của Bob mà chỉ nói rằng <strong>ít nhất</strong> <code>k</code> dự đoán của Bob là <code>true</code>.</p>

<p>Cho các mảng số nguyên 2 chiều <code>edges</code>, <code>guesses</code> và số nguyên <code>k</code>, hãy trả về <em><strong>số lượng nút có thể</strong> làm gốc của cây của Alice</em>. Nếu không có cây nào thỏa mãn, trả về <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2581.Count%20Number%20of%20Possible%20Root%20Nodes/images/ex-1.png" style="width: 727px; height: 250px;" /></p>

<pre>
<strong>Đầu vào:</strong> edges = [[0,1],[1,2],[1,3],[4,2]], guesses = [[1,3],[0,1],[1,0],[2,4]], k = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Gốc = 0, các dự đoán đúng = [1,3], [0,1], [2,4]
Gốc = 1, các dự đoán đúng = [1,3], [1,0], [2,4]
Gốc = 2, các dự đoán đúng = [1,3], [1,0], [2,4]
Gốc = 3, các dự đoán đúng = [1,0], [2,4]
Gốc = 4, các dự đoán đúng = [1,3], [1,0]
Chọn 0, 1 hoặc 2 làm nút gốc sẽ có 3 dự đoán đúng.

</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2581.Count%20Number%20of%20Possible%20Root%20Nodes/images/ex-2.png" style="width: 600px; height: 303px;" /></p>

<pre>
<strong>Đầu vào:</strong> edges = [[0,1],[1,2],[2,3],[3,4]], guesses = [[1,0],[3,4],[2,1],[3,2]], k = 1
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong>
Gốc = 0, các dự đoán đúng = [3,4]
Gốc = 1, các dự đoán đúng = [1,0], [3,4]
Gốc = 2, các dự đoán đúng = [1,0], [2,1], [3,4]
Gốc = 3, các dự đoán đúng = [1,0], [2,1], [3,2], [3,4]
Gốc = 4, các dự đoán đúng = [1,0], [2,1], [3,2]
Chọn bất kỳ nút nào làm gốc cũng sẽ có ít nhất 1 dự đoán đúng.

</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>edges.length == n - 1</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= guesses.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub>, u<sub>j</sub>, v<sub>j</sub> &lt;= n - 1</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li><code>u<sub>j</sub> != v<sub>j</sub></code></li>
	<li><code>edges</code> biểu diễn một cây hợp lệ.</li>
	<li><code>guesses[j]</code> là một cạnh của cây.</li>
	<li><code>guesses</code> không có phần tử trùng lặp.</li>
	<li><code>0 &lt;= k &lt;= guesses.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tree DP (đổi gốc)

<!-- thinking:start -->

> **Tư duy**
>
> Chưa biết gốc, còn các dự đoán là những cạnh có hướng. Một nút hợp lệ nếu khi chọn nó làm gốc thì có ít nhất $k$ dự đoán đúng. Chạy DFS từ mọi gốc sẽ có độ phức tạp $O(n^2)$.
>
> Trước tiên, đếm số dự đoán đúng $\textit{cnt}$ khi chọn gốc là $0$. Khi chuyển gốc sang nút kề $j$, cạnh $(i,j)$ không còn là quan hệ cha-con và thêm $(j,i)$ nếu đó là một dự đoán. DFS lần hai sẽ thăm mọi gốc với phép cập nhật $O(1)$ này.

<!-- thinking:end -->

Trước hết, chúng ta duyệt qua tập cạnh đã cho $edges$ và chuyển nó thành danh sách kề $g$, trong đó $g[i]$ biểu diễn các nút kề với nút $i$. Chúng ta dùng một hash map $gs$ để lưu tập các dự đoán đã cho $guesses$.

Sau đó, bắt đầu từ nút $0$, chúng ta thực hiện DFS để đếm số cạnh trong $guesses$ giữa tất cả các nút có thể đi tới từ nút $0$. Biến $cnt$ được dùng để lưu số lượng này.

Tiếp theo, bắt đầu từ nút $0$, chúng ta thực hiện DFS để đếm số cạnh trong $guesses$ của mỗi cây khi chọn $0$ làm gốc. Nếu số lượng này lớn hơn hoặc bằng $k$, thì nút đó có thể là nút gốc, và chúng ta tăng đáp án lên $1$.

Vì vậy, bài toán trở thành đếm số cạnh trong $guesses$ của mỗi cây khi chọn từng nút làm gốc. Ta đã biết có $cnt$ cạnh trong $guesses$ giữa tất cả các nút có thể đi tới từ nút $0$. Có thể duy trì giá trị này bằng cách cộng hoặc trừ cạnh hiện tại trong $guesses$ khi thực hiện DFS.

Giả sử hiện đang duyệt nút $i$ và $cnt$ biểu diễn số cạnh trong $guesses$ khi $i$ là nút gốc. Với mỗi nút kề $j$ của $i$, ta cần tính số cạnh trong $guesses$ khi $j$ là nút gốc. Nếu $(i, j)$ thuộc $guesses$, thì trong cây có gốc là $j$ sẽ không còn cạnh $(i, j)$, nên $cnt$ phải giảm đi $1$. Nếu $(j, i)$ thuộc $guesses$, thì trong cây có gốc là $j$ sẽ có thêm cạnh $(i, j)$, nên $cnt$ phải tăng lên $1$. Do đó, $f[j] = f[i] + (j, i) \in guesses - (i, j) \in guesses$. Trong đó, $f[i]$ biểu diễn số cạnh trong $guesses$ khi $i$ là nút gốc.

Độ phức tạp thời gian là $O(n + m)$ và độ phức tạp không gian là $O(n + m)$, trong đó $n$ và $m$ lần lượt là độ dài của $edges$ và $guesses$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def rootCount(
        self, edges: List[List[int]], guesses: List[List[int]], k: int
    ) -> int:
        def dfs1(i, fa):
            nonlocal cnt
            for j in g[i]:
                if j != fa:
                    cnt += gs[(i, j)]
                    dfs1(j, i)

        def dfs2(i, fa):
            nonlocal ans, cnt
            ans += cnt >= k
            for j in g[i]:
                if j != fa:
                    cnt -= gs[(i, j)]
                    cnt += gs[(j, i)]
                    dfs2(j, i)
                    cnt -= gs[(j, i)]
                    cnt += gs[(i, j)]

        g = defaultdict(list)
        for a, b in edges:
            g[a].append(b)
            g[b].append(a)
        gs = Counter((u, v) for u, v in guesses)
        cnt = 0
        dfs1(0, -1)
        ans = 0
        dfs2(0, -1)
        return ans
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private Map<Long, Integer> gs = new HashMap<>();
    private int ans;
    private int k;
    private int cnt;
    private int n;

    public int rootCount(int[][] edges, int[][] guesses, int k) {
        this.k = k;
        n = edges.length + 1;
        g = new List[n];
        Arrays.setAll(g, e -> new ArrayList<>());
        for (var e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        for (var e : guesses) {
            int a = e[0], b = e[1];
            gs.merge(f(a, b), 1, Integer::sum);
        }
        dfs1(0, -1);
        dfs2(0, -1);
        return ans;
    }

    private void dfs1(int i, int fa) {
        for (int j : g[i]) {
            if (j != fa) {
                cnt += gs.getOrDefault(f(i, j), 0);
                dfs1(j, i);
            }
        }
    }

    private void dfs2(int i, int fa) {
        ans += cnt >= k ? 1 : 0;
        for (int j : g[i]) {
            if (j != fa) {
                int a = gs.getOrDefault(f(i, j), 0);
                int b = gs.getOrDefault(f(j, i), 0);
                cnt -= a;
                cnt += b;
                dfs2(j, i);
                cnt -= b;
                cnt += a;
            }
        }
    }

    private long f(int i, int j) {
        return 1L * i * n + j;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int rootCount(vector<vector<int>>& edges, vector<vector<int>>& guesses, int k) {
        int n = edges.size() + 1;
        vector<int> g[n];
        unordered_map<long long, int> gs;
        auto f = [&](int i, int j) {
            return 1LL * i * n + j;
        };
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].push_back(b);
            g[b].push_back(a);
        }
        for (auto& e : guesses) {
            int a = e[0], b = e[1];
            gs[f(a, b)]++;
        }
        int ans = 0;
        int cnt = 0;

        function<void(int, int)> dfs1 = [&](int i, int fa) {
            for (int& j : g[i]) {
                if (j != fa) {
                    cnt += gs[f(i, j)];
                    dfs1(j, i);
                }
            }
        };

        function<void(int, int)> dfs2 = [&](int i, int fa) {
            ans += cnt >= k;
            for (int& j : g[i]) {
                if (j != fa) {
                    int a = gs[f(i, j)];
                    int b = gs[f(j, i)];
                    cnt -= a;
                    cnt += b;
                    dfs2(j, i);
                    cnt -= b;
                    cnt += a;
                }
            }
        };
        dfs1(0, -1);
        dfs2(0, -1);
        return ans;
    }
};
```

#### Go

```go
func rootCount(edges [][]int, guesses [][]int, k int) (ans int) {
	n := len(edges) + 1
	g := make([][]int, n)
	gs := map[int]int{}
	for _, e := range edges {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	f := func(i, j int) int {
		return i*n + j
	}
	for _, e := range guesses {
		a, b := e[0], e[1]
		gs[f(a, b)]++
	}

	cnt := 0
	var dfs1 func(i, fa int)
	var dfs2 func(i, fa int)
	dfs1 = func(i, fa int) {
		for _, j := range g[i] {
			if j != fa {
				cnt += gs[f(i, j)]
				dfs1(j, i)
			}
		}
	}
	dfs2 = func(i, fa int) {
		if cnt >= k {
			ans++
		}
		for _, j := range g[i] {
			if j != fa {
				a, b := gs[f(i, j)], gs[f(j, i)]
				cnt -= a
				cnt += b
				dfs2(j, i)
				cnt -= b
				cnt += a
			}
		}
	}
	dfs1(0, -1)
	dfs2(0, -1)
	return
}
```

#### TypeScript

```ts
function rootCount(edges: number[][], guesses: number[][], k: number): number {
    const n = edges.length + 1;
    const g: number[][] = Array.from({ length: n }, () => []);
    const gs: Map<number, number> = new Map();
    const f = (i: number, j: number) => i * n + j;
    for (const [a, b] of edges) {
        g[a].push(b);
        g[b].push(a);
    }
    for (const [a, b] of guesses) {
        const x = f(a, b);
        gs.set(x, gs.has(x) ? gs.get(x)! + 1 : 1);
    }
    let ans = 0;
    let cnt = 0;
    const dfs1 = (i: number, fa: number): void => {
        for (const j of g[i]) {
            if (j !== fa) {
                cnt += gs.get(f(i, j)) || 0;
                dfs1(j, i);
            }
        }
    };
    const dfs2 = (i: number, fa: number): void => {
        ans += cnt >= k ? 1 : 0;
        for (const j of g[i]) {
            if (j !== fa) {
                const a = gs.get(f(i, j)) || 0;
                const b = gs.get(f(j, i)) || 0;
                cnt -= a;
                cnt += b;
                dfs2(j, i);
                cnt -= b;
                cnt += a;
            }
        }
    };
    dfs1(0, -1);
    dfs2(0, -1);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
