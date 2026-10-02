---
comments: true
difficulty: Hard
tags:
    - Tree
    - Depth-First Search
    - Graph
    - Dynamic Programming
    - Tree DP
---

<!-- problem:start -->

# [834. Sum of Distances in Tree](https://leetcode.com/problems/sum-of-distances-in-tree)

[中文文档](/solution/0800-0899/0834.Sum%20of%20Distances%20in%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một cây vô hướng liên thông có <code>n</code> node, được đánh số từ <code>0</code> đến <code>n - 1</code>, và có <code>n - 1</code> cạnh.</p>

<p>Cho số nguyên <code>n</code> và mảng <code>edges</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> biểu thị một cạnh nối hai node <code>a<sub>i</sub></code> và <code>b<sub>i</sub></code> trong cây.</p>

<p>Hãy trả về mảng <code>answer</code> có độ dài <code>n</code>, trong đó <code>answer[i]</code> là tổng khoảng cách từ node thứ <code>i<sup>th</sup></code> trong cây đến tất cả node còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0834.Sum%20of%20Distances%20in%20Tree/images/lc-sumdist1.jpg" style="width: 304px; height: 224px;" />
<pre>
<strong>Đầu vào:</strong> n = 6, edges = [[0,1],[0,2],[2,3],[2,4],[2,5]]
<strong>Đầu ra:</strong> [8,12,6,10,10,10]
<strong>Giải thích:</strong> Cây được minh họa ở trên.
Ta có dist(0,1) + dist(0,2) + dist(0,3) + dist(0,4) + dist(0,5)
bằng 1 + 1 + 2 + 2 + 2 = 8.
Do đó, answer[0] = 8, các giá trị còn lại được tính tương tự.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0834.Sum%20of%20Distances%20in%20Tree/images/lc-sumdist2.jpg" style="width: 64px; height: 65px;" />
<pre>
<strong>Đầu vào:</strong> n = 1, edges = []
<strong>Đầu ra:</strong> [0]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0834.Sum%20of%20Distances%20in%20Tree/images/lc-sumdist3.jpg" style="width: 144px; height: 145px;" />
<pre>
<strong>Đầu vào:</strong> n = 2, edges = [[1,0]]
<strong>Đầu ra:</strong> [1,1]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li>Đầu vào đã cho biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tree DP (Đổi gốc)

<!-- thinking:start -->

> **Tư duy**
>
> Cần tính tổng khoảng cách từ mọi node. Với $n\le 3\cdot 10^4$, chạy DFS từ từng node làm root sẽ có độ phức tạp bậc hai. Khi đổi root sang node kề, đáp án chỉ thay đổi theo kích thước cây con và phần còn lại của cây.
>
> Chọn node $0$ làm root để tính $ans[0]$ và kích thước các cây con, sau đó DFS lần hai cập nhật đáp án cho mỗi node con bằng $t-\textit{size}[j]+n-\textit{size}[j]$. Hai lượt duyệt đủ để tính tất cả đáp án.

<!-- thinking:end -->

Đầu tiên, chạy DFS để tính kích thước cây con của mỗi node và lưu vào mảng $size$, đồng thời tính tổng khoảng cách từ node $0$ đến tất cả node khác và lưu vào $ans[0]$.

Tiếp theo, chạy DFS lần nữa để tính tổng khoảng cách từ mỗi node khi xem node đó là root. Giả sử đáp án tại node hiện tại $i$ là $t$. Khi chuyển root từ node $i$ sang node $j$, tổng khoảng cách trở thành $t - size[j] + n - size[j]$. Nghĩa là khoảng cách đến node $j$ và các node trong cây con của nó giảm tổng cộng $size[j]$, còn khoảng cách đến các node khác tăng tổng cộng $n - size[j]$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node trong cây.

Bài toán tương tự:

- [2581. Count Number of Possible Root Nodes](https://github.com/doocs/leetcode/blob/main/solution/2500-2599/2581.Count%20Number%20of%20Possible%20Root%20Nodes/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumOfDistancesInTree(self, n: int, edges: List[List[int]]) -> List[int]:
        def dfs1(i: int, fa: int, d: int):
            ans[0] += d
            size[i] = 1
            for j in g[i]:
                if j != fa:
                    dfs1(j, i, d + 1)
                    size[i] += size[j]

        def dfs2(i: int, fa: int, t: int):
            ans[i] = t
            for j in g[i]:
                if j != fa:
                    dfs2(j, i, t - size[j] + n - size[j])

        g = defaultdict(list)
        for a, b in edges:
            g[a].append(b)
            g[b].append(a)

        ans = [0] * n
        size = [0] * n
        dfs1(0, -1, 0)
        dfs2(0, -1, ans[0])
        return ans
```

#### Java

```java
class Solution {
    private int n;
    private int[] ans;
    private int[] size;
    private List<Integer>[] g;

    public int[] sumOfDistancesInTree(int n, int[][] edges) {
        this.n = n;
        g = new List[n];
        ans = new int[n];
        size = new int[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        dfs1(0, -1, 0);
        dfs2(0, -1, ans[0]);
        return ans;
    }

    private void dfs1(int i, int fa, int d) {
        ans[0] += d;
        size[i] = 1;
        for (int j : g[i]) {
            if (j != fa) {
                dfs1(j, i, d + 1);
                size[i] += size[j];
            }
        }
    }

    private void dfs2(int i, int fa, int t) {
        ans[i] = t;
        for (int j : g[i]) {
            if (j != fa) {
                dfs2(j, i, t - size[j] + n - size[j]);
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> sumOfDistancesInTree(int n, vector<vector<int>>& edges) {
        vector<vector<int>> g(n);
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].push_back(b);
            g[b].push_back(a);
        }
        vector<int> ans(n);
        vector<int> size(n);

        function<void(int, int, int)> dfs1 = [&](int i, int fa, int d) {
            ans[0] += d;
            size[i] = 1;
            for (int& j : g[i]) {
                if (j != fa) {
                    dfs1(j, i, d + 1);
                    size[i] += size[j];
                }
            }
        };

        function<void(int, int, int)> dfs2 = [&](int i, int fa, int t) {
            ans[i] = t;
            for (int& j : g[i]) {
                if (j != fa) {
                    dfs2(j, i, t - size[j] + n - size[j]);
                }
            }
        };

        dfs1(0, -1, 0);
        dfs2(0, -1, ans[0]);
        return ans;
    }
};
```

#### Go

```go
func sumOfDistancesInTree(n int, edges [][]int) []int {
	g := make([][]int, n)
	for _, e := range edges {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	ans := make([]int, n)
	size := make([]int, n)
	var dfs1 func(i, fa, d int)
	dfs1 = func(i, fa, d int) {
		ans[0] += d
		size[i] = 1
		for _, j := range g[i] {
			if j != fa {
				dfs1(j, i, d+1)
				size[i] += size[j]
			}
		}
	}
	var dfs2 func(i, fa, t int)
	dfs2 = func(i, fa, t int) {
		ans[i] = t
		for _, j := range g[i] {
			if j != fa {
				dfs2(j, i, t-size[j]+n-size[j])
			}
		}
	}
	dfs1(0, -1, 0)
	dfs2(0, -1, ans[0])
	return ans
}
```

#### TypeScript

```ts
function sumOfDistancesInTree(n: number, edges: number[][]): number[] {
    const g: number[][] = Array.from({ length: n }, () => []);
    for (const [a, b] of edges) {
        g[a].push(b);
        g[b].push(a);
    }
    const ans: number[] = new Array(n).fill(0);
    const size: number[] = new Array(n).fill(0);
    const dfs1 = (i: number, fa: number, d: number) => {
        ans[0] += d;
        size[i] = 1;
        for (const j of g[i]) {
            if (j !== fa) {
                dfs1(j, i, d + 1);
                size[i] += size[j];
            }
        }
    };
    const dfs2 = (i: number, fa: number, t: number) => {
        ans[i] = t;
        for (const j of g[i]) {
            if (j !== fa) {
                dfs2(j, i, t - size[j] + n - size[j]);
            }
        }
    };
    dfs1(0, -1, 0);
    dfs2(0, -1, ans[0]);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
