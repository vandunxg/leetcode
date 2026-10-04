---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3004. Maximum Subtree of the Same Color 🔒](https://leetcode.com/problems/maximum-subtree-of-the-same-color)

[中文文档](/solution/3000-3099/3004.Maximum%20Subtree%20of%20the%20Same%20Color/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên 2 chiều <code>edges</code> biểu diễn một cây gồm <code>n</code> node, được đánh số từ <code>0</code> đến <code>n - 1</code>, có gốc là node <code>0</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> nghĩa là có một cạnh nối node <code>v<sub>i</sub></code> và <code>u<sub>i</sub></code>.</p>

<p>Đồng thời, cho một mảng số nguyên <strong>0-indexed</strong> <code>colors</code> có kích thước <code>n</code>, trong đó <code>colors[i]</code> là màu được gán cho node <code>i</code>.</p>

<p>Ta muốn tìm một node <code>v</code> sao cho mọi node trong <span data-keyword="subtree-of-node">cây con</span> của <code>v</code> đều có <strong>cùng</strong> một màu.</p>

<p>Trả về <em>kích thước của cây con như vậy với <strong>số node lớn nhất</strong> có thể.</em></p>

<p>&nbsp;</p>
<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3004.Maximum%20Subtree%20of%20the%20Same%20Color/images/20231216-134026.png" style="padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; width: 221px; height: 132px;" /></strong></p>

<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> edges = [[0,1],[0,2],[0,3]], colors = [1,1,2,3]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Mỗi màu được biểu diễn như sau: 1 -&gt; Đỏ, 2 -&gt; Xanh lá, 3 -&gt; Xanh dương. Ta thấy cây con có gốc tại node 0 có các node con với màu khác nhau. Mọi cây con khác đều có cùng một màu và có kích thước bằng 1. Vì vậy, ta trả về 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> edges = [[0,1],[0,2],[0,3]], colors = [1,1,1,1]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Toàn bộ cây có cùng một màu, và cây con có gốc tại node 0 có số node lớn nhất là 4. Vì vậy, ta trả về 4.
</pre>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3004.Maximum%20Subtree%20of%20the%20Same%20Color/images/20231216-134017.png" style="padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; width: 221px; height: 221px;" /></strong></p>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> edges = [[0,1],[0,2],[2,3],[2,4]], colors = [1,2,3,3,3]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Mỗi màu được biểu diễn như sau: 1 -&gt; Đỏ, 2 -&gt; Xanh lá, 3 -&gt; Xanh dương. Ta thấy cây con có gốc tại node 0 có các node con với màu khác nhau. Những cây con khác đều có cùng một màu, nhưng cây con có gốc tại node 2 có kích thước bằng 3, là lớn nhất. Vì vậy, ta trả về 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == edges.length + 1</code></li>
	<li><code>1 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>edges[i] == [u<sub>i</sub>, v<sub>i</sub>]</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt; n</code></li>
	<li><code>colors.length == n</code></li>
	<li><code>1 &lt;= colors[i] &lt;= 10<sup>5</sup></code></li>
	<li>Dữ liệu đầu vào được tạo sao cho đồ thị được biểu diễn bởi <code>edges</code> là một cây.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Cây có $n \le 5 \times 10^4$ node, nên nếu kiểm tra lại từng cây con để xác định nó có cùng một màu hay không thì sẽ lặp lại nhiều công việc.
>
> Một cây con có cùng một màu khi và chỉ khi node gốc có cùng màu với mọi node con, đồng thời cây con của mỗi node con cũng có cùng một màu. Kích thước được cộng dồn theo thứ tự hậu tố.
>
> Vì vậy, một lần DFS có thể trả về một giá trị boolean và duy trì $\textit{size}$. Chỉ khi cây con hoàn toàn có cùng một màu, ta mới dùng kích thước của nó để cập nhật đáp án.

<!-- thinking:end -->

Trước hết, dựa trên thông tin cạnh được cho trong đề bài, ta xây dựng danh sách kề $g$, trong đó $g[a]$ biểu diễn tất cả node kề với node $a$. Sau đó, ta tạo một mảng $size$ có độ dài $n$, trong đó $size[a]$ biểu diễn số node trong cây con có node $a$ làm gốc.

Tiếp theo, ta thiết kế hàm $dfs(a, fa)$, hàm này trả về việc cây con có node $a$ làm gốc có thỏa mãn yêu cầu của đề bài hay không. Quá trình thực thi hàm $dfs(a, fa)$ như sau:

- Trước tiên, ta dùng biến $ok$ để ghi nhận liệu cây con có node $a$ làm gốc có thỏa mãn yêu cầu hay không, ban đầu $ok$ là $true$.
- Sau đó, ta duyệt qua tất cả node $b$ kề với node $a$. Nếu $b$ không phải là node cha $fa$ của $a$, ta gọi đệ quy $dfs(b, a)$, lưu giá trị trả về vào biến $t$, rồi cập nhật $ok$ thành giá trị của $ok$ và $colors[a] = colors[b] \land t$, trong đó $\land$ biểu diễn phép AND logic. Sau đó, ta cập nhật $size[a] = size[a] + size[b]$.
- Tiếp theo, ta kiểm tra giá trị của $ok$. Nếu $ok$ là $true$, ta cập nhật đáp án $ans = \max(ans, size[a])$.
- Cuối cùng, ta trả về giá trị của $ok$.

Ta gọi $dfs(0, -1)$, trong đó $0$ biểu diễn số hiệu node gốc và $-1$ biểu diễn node gốc không có node cha. Đáp án cuối cùng là $ans$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumSubtreeSize(self, edges: List[List[int]], colors: List[int]) -> int:
        def dfs(a: int, fa: int) -> bool:
            ok = True
            for b in g[a]:
                if b != fa:
                    t = dfs(b, a)
                    ok = ok and colors[a] == colors[b] and t
                    size[a] += size[b]
            if ok:
                nonlocal ans
                ans = max(ans, size[a])
            return ok

        n = len(edges) + 1
        g = [[] for _ in range(n)]
        size = [1] * n
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
    private List<Integer>[] g;
    private int[] colors;
    private int[] size;
    private int ans;

    public int maximumSubtreeSize(int[][] edges, int[] colors) {
        int n = edges.length + 1;
        g = new List[n];
        size = new int[n];
        this.colors = colors;
        Arrays.fill(size, 1);
        Arrays.setAll(g, i -> new ArrayList<>());
        for (var e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        dfs(0, -1);
        return ans;
    }

    private boolean dfs(int a, int fa) {
        boolean ok = true;
        for (int b : g[a]) {
            if (b != fa) {
                boolean t = dfs(b, a);
                ok = ok && colors[a] == colors[b] && t;
                size[a] += size[b];
            }
        }
        if (ok) {
            ans = Math.max(ans, size[a]);
        }
        return ok;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumSubtreeSize(vector<vector<int>>& edges, vector<int>& colors) {
        int n = edges.size() + 1;
        vector<int> g[n];
        vector<int> size(n, 1);
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].push_back(b);
            g[b].push_back(a);
        }
        int ans = 0;
        function<bool(int, int)> dfs = [&](int a, int fa) {
            bool ok = true;
            for (int b : g[a]) {
                if (b != fa) {
                    bool t = dfs(b, a);
                    ok = ok && colors[a] == colors[b] && t;
                    size[a] += size[b];
                }
            }
            if (ok) {
                ans = max(ans, size[a]);
            }
            return ok;
        };
        dfs(0, -1);
        return ans;
    }
};
```

#### Go

```go
func maximumSubtreeSize(edges [][]int, colors []int) (ans int) {
	n := len(edges) + 1
	g := make([][]int, n)
	for _, e := range edges {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	size := make([]int, n)
	var dfs func(int, int) bool
	dfs = func(a, fa int) bool {
		size[a] = 1
		ok := true
		for _, b := range g[a] {
			if b != fa {
				t := dfs(b, a)
				ok = ok && t && colors[a] == colors[b]
				size[a] += size[b]
			}
		}
		if ok {
			ans = max(ans, size[a])
		}
		return ok
	}
	dfs(0, -1)
	return
}
```

#### TypeScript

```ts
function maximumSubtreeSize(edges: number[][], colors: number[]): number {
    const n = edges.length + 1;
    const g: number[][] = Array.from({ length: n }, () => []);
    for (const [a, b] of edges) {
        g[a].push(b);
        g[b].push(a);
    }
    const size: number[] = Array(n).fill(1);
    let ans = 0;
    const dfs = (a: number, fa: number): boolean => {
        let ok = true;
        for (const b of g[a]) {
            if (b !== fa) {
                const t = dfs(b, a);
                ok = ok && t && colors[a] === colors[b];
                size[a] += size[b];
            }
        }
        if (ok) {
            ans = Math.max(ans, size[a]);
        }
        return ok;
    };
    dfs(0, -1);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
