---
comments: true
difficulty: Hard
rating: 1859
source: Biweekly Contest 181 Q3
tags:
    - Bit Manipulation
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Graph
    - Array
    - Enumeration
---

<!-- problem:start -->

# [3910. Count Connected Subgraphs with Even Node Sum](https://leetcode.com/problems/count-connected-subgraphs-with-even-node-sum)

[中文文档](/solution/3900-3999/3910.Count%20Connected%20Subgraphs%20with%20Even%20Node%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một đồ thị vô hướng gồm <code>n</code> node, được đánh nhãn từ 0 đến <code>n - 1</code>. Node <code>i</code> có một <strong>giá trị</strong> là <code>nums[i]</code>, bằng 0 hoặc 1. Các cạnh của đồ thị được cho bởi một mảng 2D <code>edges</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> biểu diễn một cạnh nối node <code>u<sub>i</sub></code> và node <code>v<sub>i</sub></code>.</p>

<p>Với một <strong>tập con khác rỗng</strong> <code>s</code> của các node trong đồ thị, ta định nghĩa <strong>đồ thị con cảm sinh</strong> của <code>s</code> như sau:</p>

<ul>
	<li>Chỉ giữ lại các node thuộc <code>s</code>.</li>
	<li>Chỉ giữ lại các cạnh có cả hai đầu mút đều thuộc <code>s</code>.</li>
</ul>

<p>Hãy trả về một số nguyên biểu diễn số lượng <strong>tập con khác rỗng</strong> <code>s</code> của các node trong đồ thị thỏa mãn:</p>

<ul>
	<li><strong>Đồ thị con cảm sinh</strong> của <code>s</code> là <strong>liên thông</strong>.</li>
	<li><strong>Tổng</strong> các <strong>giá trị</strong> node trong <code>s</code> là <strong>chẵn</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,0,1], edges = [[0,1],[1,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;"><code>s</code></th>
			<th style="border: 1px solid black;">liên thông?</th>
			<th style="border: 1px solid black;">tổng giá trị các node</th>
			<th style="border: 1px solid black;">được đếm?</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;"><code>[0]</code></td>
			<td style="border: 1px solid black;">Có</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">Không</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[1]</code></td>
			<td style="border: 1px solid black;">Có</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">Có</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[2]</code></td>
			<td style="border: 1px solid black;">Có</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">Không</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[0,1]</code></td>
			<td style="border: 1px solid black;">Có</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">Không</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[0,2]</code></td>
			<td style="border: 1px solid black;">Không, node 0 và node 2 không liên thông.</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">Không</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[1,2]</code></td>
			<td style="border: 1px solid black;">Có</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">Không</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[0,1,2]</code></td>
			<td style="border: 1px solid black;">Có</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">Có</td>
		</tr>
	</tbody>
</table>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1], edges = []</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;"><code>s</code></th>
			<th style="border: 1px solid black;">liên thông?</th>
			<th style="border: 1px solid black;">tổng giá trị các node</th>
			<th style="border: 1px solid black;">được đếm?</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;"><code>[0]</code></td>
			<td style="border: 1px solid black;">Có</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">Không</td>
		</tr>
	</tbody>
</table>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 13</code></li>
	<li><code>nums[i]</code> là 0 hoặc 1.</li>
	<li><code>0 &lt;= edges.length &lt;= n * (n - 1) / 2</code></li>
	<li><code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code></li>
	<li><code>0 &lt;= u<sub>i</sub> &lt; v<sub>i</sub> &lt; n</code></li>
	<li>Tất cả các cạnh đều <strong>phân biệt</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê bằng bitmask + DFS

<!-- thinking:start -->

> **Tư duy**
>
> Việc đếm các đồ thị con cảm sinh liên thông nói chung khá khó, nhưng $n\le 13$ nên chỉ có $2^n-1\le 8191$ tập con khác rỗng, và ta có thể kiểm tra lần lượt từng tập.
>
> Trước tiên, bỏ qua các tập có tổng giá trị node lẻ. Với một tập chẵn, đánh dấu các node nằm ngoài tập bằng bitmask $\textit{vis}$, rồi bắt đầu DFS từ một đỉnh bất kỳ thuộc tập và chỉ đi trong tập đó. Nếu cuối cùng $\textit{vis}$ có đủ cả $n$ bit bằng 1, đồ thị con cảm sinh là liên thông.
>
> Danh sách kề giúp mỗi lần DFS có độ phức tạp $O(n+m)$, nên tổng độ phức tạp là $O(2^n(n+m))$.

<!-- thinking:end -->

Ta nhận thấy số lượng node trong bài toán không vượt quá $13$, vì vậy có thể liệt kê tất cả các tập con khác rỗng $s$ của các node. Với mỗi tập con, ta tính tổng giá trị các node và kiểm tra xem đồ thị con cảm sinh của nó có liên thông hay không.

Cụ thể, ta dùng một số nguyên $sub$ để biểu diễn tập con $s$, trong đó bit thứ $i$ của $sub$ bằng $1$ nếu node $i$ thuộc tập con, và bằng $0$ nếu ngược lại. Với mỗi tập con, trước tiên ta tính tổng giá trị các node. Nếu tổng là số lẻ, ta bỏ qua tập con này; nếu không, ta dùng DFS để kiểm tra xem đồ thị con cảm sinh có liên thông hay không. Ta dùng một số nguyên $vis$ để biểu diễn các node đã thăm: ban đầu, bit thứ $i$ của $vis$ bằng $1$ nếu node $i$ không thuộc tập con, và bằng $0$ nếu node $i$ thuộc tập con. Ta bắt đầu DFS từ một node bất kỳ trong tập con $s$, duyệt qua tất cả các node kề với nó và đánh dấu các node đã thăm trong $vis$ là $1$. Cuối cùng, nếu tất cả các bit của $vis$ đều bằng $1$, điều đó có nghĩa là đồ thị con cảm sinh của tập con $s$ liên thông, nên ta tăng đáp án lên $1$.

Độ phức tạp thời gian là $O(2^n \times (n + m))$ và độ phức tạp không gian là $O(n + m)$, trong đó $n$ và $m$ lần lượt là số lượng node và cạnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def evenSumSubgraphs(self, nums: list[int], edges: list[list[int]]) -> int:
        n = len(nums)
        g = [[] for _ in range(n)]
        for u, v in edges:
            g[u].append(v)
            g[v].append(u)
        m = (1 << n) - 1
        ans = 0
        for sub in range(1, m + 1):
            s = sum(x for i, x in enumerate(nums) if sub >> i & 1)
            if s % 2:
                continue
            vis = m ^ sub

            def dfs(u: int) -> None:
                nonlocal vis
                vis |= 1 << u
                for v in g[u]:
                    if (vis >> v & 1) == 0:
                        dfs(v)

            dfs(sub.bit_length() - 1)
            if vis == m:
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    private int vis;
    private int m;
    private List<Integer>[] g;

    public int evenSumSubgraphs(int[] nums, int[][] edges) {
        int n = nums.length;
        g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int[] e : edges) {
            g[e[0]].add(e[1]);
            g[e[1]].add(e[0]);
        }
        m = (1 << n) - 1;
        int ans = 0;
        for (int sub = 1; sub <= m; sub++) {
            int s = 0;
            for (int i = 0; i < n; i++) {
                if (((sub >> i) & 1) == 1) {
                    s += nums[i];
                }
            }
            if (s % 2 != 0) {
                continue;
            }
            vis = m ^ sub;
            dfs(Integer.numberOfTrailingZeros(sub));
            if (vis == m) {
                ans++;
            }
        }
        return ans;
    }

    private void dfs(int u) {
        vis |= 1 << u;
        for (int v : g[u]) {
            if (((vis >> v) & 1) == 0) {
                dfs(v);
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int evenSumSubgraphs(vector<int>& nums, vector<vector<int>>& edges) {
        int n = nums.size();
        vector<vector<int>> g(n);
        for (auto& e : edges) {
            g[e[0]].push_back(e[1]);
            g[e[1]].push_back(e[0]);
        }
        int m = (1 << n) - 1;
        int ans = 0;
        int vis;

        auto dfs = [&](this auto dfs, int u) -> void {
            vis |= 1 << u;
            for (int v : g[u]) {
                if (((vis >> v) & 1) == 0) {
                    dfs(v);
                }
            }
        };

        for (int sub = 1; sub <= m; sub++) {
            int s = 0;
            for (int i = 0; i < n; i++) {
                if ((sub >> i) & 1) {
                    s += nums[i];
                }
            }
            if (s % 2 != 0) {
                continue;
            }
            vis = m ^ sub;
            dfs(31 - __builtin_clz(sub));
            if (vis == m) {
                ans++;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func evenSumSubgraphs(nums []int, edges [][]int) int {
	n := len(nums)
	g := make([][]int, n)
	for _, e := range edges {
		g[e[0]] = append(g[e[0]], e[1])
		g[e[1]] = append(g[e[1]], e[0])
	}
	m := (1 << n) - 1
	ans := 0
	var vis int

	var dfs func(int)
	dfs = func(u int) {
		vis |= 1 << u
		for _, v := range g[u] {
			if (vis >> v & 1) == 0 {
				dfs(v)
			}
		}
	}

	for sub := 1; sub <= m; sub++ {
		s := 0
		for i := 0; i < n; i++ {
			if sub>>i&1 == 1 {
				s += nums[i]
			}
		}
		if s%2 != 0 {
			continue
		}
		vis = m ^ sub
		dfs(bits.Len(uint(sub)) - 1)
		if vis == m {
			ans++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function evenSumSubgraphs(nums: number[], edges: number[][]): number {
    const n = nums.length;
    const g: number[][] = Array.from({ length: n }, () => []);
    for (const [u, v] of edges) {
        g[u].push(v);
        g[v].push(u);
    }
    const m = (1 << n) - 1;
    let ans = 0;
    let vis = 0;

    const dfs = (u: number): void => {
        vis |= 1 << u;
        for (const v of g[u]) {
            if (((vis >> v) & 1) === 0) {
                dfs(v);
            }
        }
    };

    for (let sub = 1; sub <= m; sub++) {
        let s = 0;
        for (let i = 0; i < n; i++) {
            if ((sub >> i) & 1) {
                s += nums[i];
            }
        }
        if (s % 2 !== 0) {
            continue;
        }
        vis = m ^ sub;
        dfs(sub.toString(2).length - 1);
        if (vis === m) {
            ans++;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
