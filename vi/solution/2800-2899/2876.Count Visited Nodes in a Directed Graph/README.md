---
comments: true
difficulty: Hard
rating: 2209
source: Weekly Contest 365 Q4
tags:
    - Depth-First Search
    - Graph
    - Topological Sort
    - Memoization
    - Dynamic Programming
    - Kosaraju
    - Tarjan
---

<!-- problem:start -->

# [2876. Count Visited Nodes in a Directed Graph](https://leetcode.com/problems/count-visited-nodes-in-a-directed-graph)

[中文文档](/solution/2800-2899/2876.Count%20Visited%20Nodes%20in%20a%20Directed%20Graph/README.md)

## Mô tả

<!-- description:start -->

<p>Có một đồ thị <strong>có hướng</strong> gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code> và <code>n</code> cạnh có hướng.</p>

<p>Bạn được cho một mảng <strong>đánh chỉ số từ 0</strong> <code>edges</code>, trong đó <code>edges[i]</code> cho biết có một cạnh đi từ node <code>i</code> đến node <code>edges[i]</code>.</p>

<p>Xét quy trình sau trên đồ thị:</p>

<ul>
	<li>Bạn bắt đầu từ một node <code>x</code> và tiếp tục đi qua các cạnh để thăm các node khác cho đến khi đến một node đã được thăm trước đó trong <strong>chính</strong> quy trình này.</li>
</ul>

<p>Trả về <em>một mảng </em><code>answer</code><em>, trong đó </em><code>answer[i]</code><em> là số lượng node <strong>khác nhau</strong> mà bạn sẽ thăm nếu thực hiện quy trình bắt đầu từ node </em><code>i</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2800-2899/2876.Count%20Visited%20Nodes%20in%20a%20Directed%20Graph/images/graaphdrawio-1.png" />
<pre>
<strong>Đầu vào:</strong> edges = [1,2,0,0]
<strong>Đầu ra:</strong> [3,3,3,4]
<strong>Giải thích:</strong> Ta thực hiện quy trình bắt đầu từ mỗi node như sau:
- Bắt đầu từ node 0, ta thăm các node 0 -&gt; 1 -&gt; 2 -&gt; 0. Số node khác nhau ta thăm là 3.
- Bắt đầu từ node 1, ta thăm các node 1 -&gt; 2 -&gt; 0 -&gt; 1. Số node khác nhau ta thăm là 3.
- Bắt đầu từ node 2, ta thăm các node 2 -&gt; 0 -&gt; 1 -&gt; 2. Số node khác nhau ta thăm là 3.
- Bắt đầu từ node 3, ta thăm các node 3 -&gt; 0 -&gt; 1 -&gt; 2 -&gt; 0. Số node khác nhau ta thăm là 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2800-2899/2876.Count%20Visited%20Nodes%20in%20a%20Directed%20Graph/images/graaph2drawio.png" style="width: 191px; height: 251px;" />
<pre>
<strong>Đầu vào:</strong> edges = [1,2,3,4,0]
<strong>Đầu ra:</strong> [5,5,5,5,5]
<strong>Giải thích:</strong> Bắt đầu từ bất kỳ node nào, ta đều có thể thăm mọi node trong đồ thị trong quy trình này.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == edges.length</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= edges[i] &lt;= n - 1</code></li>
	<li><code>edges[i] != i</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Cây cơ bản + Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi node có out-degree bằng một, nên đồ thị là một chu trình với các cây nối vào. Khi đi theo cạnh duy nhất, việc quay lại một node trên đường đi hiện tại cho biết ta đã gặp một chu trình (các node trong chu trình nhận độ dài chu trình; các node bên ngoài cộng thêm khoảng cách đến chu trình), còn khi gặp một node đã có đáp án thì ta tái sử dụng đáp án đó.

<!-- thinking:end -->

Ta có thể dùng một mảng $ans$ để lưu đáp án cho mỗi node và một mảng $vis$ để lưu thứ tự thăm của mỗi node.

Với mỗi node $i$, nếu node này chưa được thăm, ta bắt đầu duyệt từ node $i$. Có hai trường hợp:

- Nếu gặp một node đã được thăm trước đó trong quá trình duyệt, thì ta chắc chắn đã đi vào chu trình và đi quanh chu trình. Với các node bên ngoài chu trình, đáp án của chúng là độ dài chu trình cộng với khoảng cách từ node đó đến chu trình; với các node nằm trong chu trình, đáp án của chúng là độ dài chu trình.
- Nếu gặp một node đã được thăm trước đó trong quá trình duyệt, thì với mỗi node đã thăm, đáp án của nó là khoảng cách từ node hiện tại đến node này cộng với đáp án của node này.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng edges.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countVisitedNodes(self, edges: List[int]) -> List[int]:
        n = len(edges)
        ans = [0] * n
        vis = [0] * n
        for i in range(n):
            if not ans[i]:
                cnt, j = 0, i
                while not vis[j]:
                    cnt += 1
                    vis[j] = cnt
                    j = edges[j]
                cycle, total = 0, cnt + ans[j]
                if not ans[j]:
                    cycle = cnt - vis[j] + 1
                    total = cnt
                j = i
                while not ans[j]:
                    ans[j] = max(total, cycle)
                    total -= 1
                    j = edges[j]
        return ans
```

#### Java

```java
class Solution {
    public int[] countVisitedNodes(List<Integer> edges) {
        int n = edges.size();
        int[] ans = new int[n];
        int[] vis = new int[n];
        for (int i = 0; i < n; ++i) {
            if (ans[i] == 0) {
                int cnt = 0, j = i;
                while (vis[j] == 0) {
                    vis[j] = ++cnt;
                    j = edges.get(j);
                }
                int cycle = 0, total = cnt + ans[j];
                if (ans[j] == 0) {
                    cycle = cnt - vis[j] + 1;
                }
                j = i;
                while (ans[j] == 0) {
                    ans[j] = Math.max(total--, cycle);
                    j = edges.get(j);
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
    vector<int> countVisitedNodes(vector<int>& edges) {
        int n = edges.size();
        vector<int> ans(n), vis(n);
        for (int i = 0; i < n; ++i) {
            if (!ans[i]) {
                int cnt = 0, j = i;
                while (vis[j] == 0) {
                    vis[j] = ++cnt;
                    j = edges[j];
                }
                int cycle = 0, total = cnt + ans[j];
                if (ans[j] == 0) {
                    cycle = cnt - vis[j] + 1;
                }
                j = i;
                while (ans[j] == 0) {
                    ans[j] = max(total--, cycle);
                    j = edges[j];
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countVisitedNodes(edges []int) []int {
	n := len(edges)
	ans := make([]int, n)
	vis := make([]int, n)
	for i := range ans {
		if ans[i] == 0 {
			cnt, j := 0, i
			for vis[j] == 0 {
				cnt++
				vis[j] = cnt
				j = edges[j]
			}
			cycle, total := 0, cnt+ans[j]
			if ans[j] == 0 {
				cycle = cnt - vis[j] + 1
			}
			j = i
			for ans[j] == 0 {
				ans[j] = max(total, cycle)
				total--
				j = edges[j]
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function countVisitedNodes(edges: number[]): number[] {
    const n = edges.length;
    const ans: number[] = Array(n).fill(0);
    const vis: number[] = Array(n).fill(0);
    for (let i = 0; i < n; ++i) {
        if (ans[i] === 0) {
            let [cnt, j] = [0, i];
            while (vis[j] === 0) {
                vis[j] = ++cnt;
                j = edges[j];
            }
            let [cycle, total] = [0, cnt + ans[j]];
            if (ans[j] === 0) {
                cycle = cnt - vis[j] + 1;
            }
            j = i;
            while (ans[j] === 0) {
                ans[j] = Math.max(total--, cycle);
                j = edges[j];
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
