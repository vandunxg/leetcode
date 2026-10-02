---
comments: true
difficulty: Medium
rating: 1808
source: Weekly Contest 198 Q2
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Hash Table
    - Counting
    - Tree DP
---

<!-- problem:start -->

# [1519. Number of Nodes in the Sub-Tree With the Same Label](https://leetcode.com/problems/number-of-nodes-in-the-sub-tree-with-the-same-label)

[中文文档](/solution/1500-1599/1519.Number%20of%20Nodes%20in%20the%20Sub-Tree%20With%20the%20Same%20Label/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một cây (đồ thị vô hướng liên thông không có chu trình) gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code> và chính xác <code>n - 1</code> <code>edges</code>. <strong>Root</strong> của cây là node <code>0</code>, và mỗi node có <strong>một label</strong> là ký tự thường trong chuỗi <code>labels</code> (node có số <code>i</code> có label là <code>labels[i]</code>).</p>

<p>Mảng <code>edges</code> có dạng <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code>, nghĩa là có một cạnh giữa node <code>a<sub>i</sub></code> và <code>b<sub>i</sub></code> trong cây.</p>

<p>Trả về <em>mảng có kích thước <code>n</code></em>, trong đó <code>ans[i]</code> là số node trong cây con của <code>i<sup>th</sup></code> node có cùng label với node <code>i</code>.</p>

<p>Cây con của cây <code>T</code> là cây gồm một node trong <code>T</code> và tất cả các node hậu duệ của nó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1519.Number%20of%20Nodes%20in%20the%20Sub-Tree%20With%20the%20Same%20Label/images/q3e1.jpg" style="width: 400px; height: 291px;" />
<pre>
<strong>Input:</strong> n = 7, edges = [[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], labels = &quot;abaedcd&quot;
<strong>Output:</strong> [2,1,1,1,1,1,1]
<strong>Explanation:</strong> Node 0 có label &#39;a&#39; và cây con của nó cũng có node 2 mang label &#39;a&#39;, nên đáp án là 2. Lưu ý rằng mọi node đều thuộc cây con của chính nó.
Node 1 có label &#39;b&#39;. Cây con của node 1 chứa các node 1,4 và 5; vì node 4 và 5 có label khác node 1, đáp án chỉ là 1 (chính node đó).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1519.Number%20of%20Nodes%20in%20the%20Sub-Tree%20With%20the%20Same%20Label/images/q3e2.jpg" style="width: 300px; height: 253px;" />
<pre>
<strong>Input:</strong> n = 4, edges = [[0,1],[1,2],[0,3]], labels = &quot;bbbb&quot;
<strong>Output:</strong> [4,2,1,1]
<strong>Explanation:</strong> Cây con của node 2 chỉ chứa node 2, nên đáp án là 1.
Cây con của node 3 chỉ chứa node 3, nên đáp án là 1.
Cây con của node 1 chứa node 1 và 2, cả hai đều có label &#39;b&#39;, nên đáp án là 2.
Cây con của node 0 chứa các node 0, 1, 2 và 3, tất cả đều có label &#39;b&#39;, nên đáp án là 4.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1519.Number%20of%20Nodes%20in%20the%20Sub-Tree%20With%20the%20Same%20Label/images/q3e3.jpg" style="width: 300px; height: 253px;" />
<pre>
<strong>Input:</strong> n = 5, edges = [[0,1],[0,2],[1,3],[0,4]], labels = &quot;aabab&quot;
<strong>Output:</strong> [3,2,1,1,1]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li><code>labels.length == n</code></li>
	<li><code>labels</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi node, ta cần biết có bao nhiêu node trong cây con của nó có cùng label. Vì $n\le 10^5$, duyệt mới từ mỗi node sẽ có độ phức tạp bậc hai. Chỉ có $26$ label, nên một lần DFS có thể đếm trên toàn cây.
>
> Trước khi đi vào $i$, ghi nhớ tần suất hiện tại của $labels[i]$; sau khi rời đi, đọc lại tần suất đó. Hiệu của hai giá trị là số lượng trong cây con. Lưu snapshot lúc vào trong $ans[i]$ (trừ trước, cộng sau) giúp tránh dùng thêm mảng; bộ nhớ bổ sung chỉ là kích thước alphabet.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSubTrees(self, n: int, edges: List[List[int]], labels: str) -> List[int]:
        def dfs(i, fa):
            ans[i] -= cnt[labels[i]]
            cnt[labels[i]] += 1
            for j in g[i]:
                if j != fa:
                    dfs(j, i)
            ans[i] += cnt[labels[i]]

        g = defaultdict(list)
        for a, b in edges:
            g[a].append(b)
            g[b].append(a)
        cnt = Counter()
        ans = [0] * n
        dfs(0, -1)
        return ans
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private String labels;
    private int[] ans;
    private int[] cnt;

    public int[] countSubTrees(int n, int[][] edges, String labels) {
        g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int[] e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        this.labels = labels;
        ans = new int[n];
        cnt = new int[26];
        dfs(0, -1);
        return ans;
    }

    private void dfs(int i, int fa) {
        int k = labels.charAt(i) - 'a';
        ans[i] -= cnt[k];
        cnt[k]++;
        for (int j : g[i]) {
            if (j != fa) {
                dfs(j, i);
            }
        }
        ans[i] += cnt[k];
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> countSubTrees(int n, vector<vector<int>>& edges, string labels) {
        vector<vector<int>> g(n);
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].push_back(b);
            g[b].push_back(a);
        }
        vector<int> ans(n);
        int cnt[26]{};
        function<void(int, int)> dfs = [&](int i, int fa) {
            int k = labels[i] - 'a';
            ans[i] -= cnt[k];
            cnt[k]++;
            for (int& j : g[i]) {
                if (j != fa) {
                    dfs(j, i);
                }
            }
            ans[i] += cnt[k];
        };
        dfs(0, -1);
        return ans;
    }
};
```

#### Go

```go
func countSubTrees(n int, edges [][]int, labels string) []int {
	g := make([][]int, n)
	for _, e := range edges {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	ans := make([]int, n)
	cnt := [26]int{}
	var dfs func(int, int)
	dfs = func(i, fa int) {
		k := labels[i] - 'a'
		ans[i] -= cnt[k]
		cnt[k]++
		for _, j := range g[i] {
			if j != fa {
				dfs(j, i)
			}
		}
		ans[i] += cnt[k]
	}
	dfs(0, -1)
	return ans
}
```

#### TypeScript

```ts
function countSubTrees(n: number, edges: number[][], labels: string): number[] {
    const dfs = (i: number, fa: number) => {
        const k = labels.charCodeAt(i) - 97;
        ans[i] -= cnt[k];
        cnt[k]++;
        for (const j of g[i]) {
            if (j !== fa) {
                dfs(j, i);
            }
        }
        ans[i] += cnt[k];
    };
    const ans = new Array(n).fill(0),
        cnt = new Array(26).fill(0);
    const g: number[][] = Array.from({ length: n }, () => []);
    for (const [a, b] of edges) {
        g[a].push(b);
        g[b].push(a);
    }
    dfs(0, -1);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
