---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Topological Sort
---

<!-- problem:start -->

# [310. Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees)

[中文文档](/solution/0300-0399/0310.Minimum%20Height%20Trees/README.md)

## Mô tả

<!-- description:start -->

<p>Cây là đồ thị vô hướng mà giữa hai đỉnh bất kỳ có <i>đúng một</i> đường đi. Nói cách khác, mọi đồ thị liên thông không có chu trình đơn đều là cây.</p>

<p>Cho cây gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code> và mảng <code>n - 1</code> cạnh <code>edges</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> biểu thị một cạnh vô hướng nối hai node <code>a<sub>i</sub></code> và <code>b<sub>i</sub></code>. Bạn có thể chọn bất kỳ node nào làm root. Khi chọn node <code>x</code> làm root, cây thu được có chiều cao <code>h</code>. Trong tất cả các cây có root có thể tạo ra, những cây có chiều cao nhỏ nhất (tức <code>min(h)</code>) được gọi là <strong>cây có chiều cao tối thiểu</strong> (MHT).</p>

<p>Hãy trả về <em>danh sách nhãn root của tất cả các <strong>MHT</strong></em>. Bạn có thể trả về đáp án theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p><strong>Chiều cao</strong> của cây có root là số cạnh trên đường đi xuống dài nhất từ root đến một lá.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0310.Minimum%20Height%20Trees/images/e1.jpg" style="width: 800px; height: 213px;" />
<pre>
<strong>Đầu vào:</strong> n = 4, edges = [[1,0],[1,2],[1,3]]
<strong>Đầu ra:</strong> [1]
<strong>Giải thích:</strong> Như hình minh họa, cây có chiều cao 1 khi node mang nhãn 1 được chọn làm root; đây là MHT duy nhất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0310.Minimum%20Height%20Trees/images/e2.jpg" style="width: 800px; height: 321px;" />
<pre>
<strong>Đầu vào:</strong> n = 6, edges = [[3,0],[3,1],[3,2],[3,4],[5,4]]
<strong>Đầu ra:</strong> [3,4]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li>Tất cả các cặp <code>(a<sub>i</sub>, b<sub>i</sub>)</code> đều khác nhau.</li>
	<li>Đảm bảo dữ liệu đầu vào là một cây và <strong>không có cạnh nào bị lặp</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp topo

<!-- thinking:start -->

> **Tư duy**
>
> Chiều cao là khoảng cách từ root đến lá xa nhất. Chạy BFS từ mọi node tốn $O(n^2)$. Tâm của đường kính cây — gồm một hoặc hai node — chính là tập các root tạo ra cây có chiều cao tối thiểu.
>
> Lá có bậc $1$. Ta lần lượt tỉa các lá từ ngoài vào trong theo từng lớp: giảm bậc của các node kề, rồi đưa những node mới có bậc $1$ vào queue. Lớp cuối cùng còn lại chính là tâm. Nếu cây chỉ có một node thì trả về node đó luôn.

<!-- thinking:end -->

Nếu cây chỉ có một node thì node đó là root của cây có chiều cao tối thiểu. Ta có thể trả về node này ngay.

Nếu cây có nhiều node thì sẽ có các lá. Lá là node chỉ kề với đúng một node khác. Ta có thể dùng sắp xếp topo để tỉa các lá từ ngoài vào trong. Khi đến lớp cuối cùng, các node còn lại chính là những root của cây có chiều cao tối thiểu.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMinHeightTrees(self, n: int, edges: List[List[int]]) -> List[int]:
        if n == 1:
            return [0]
        g = [[] for _ in range(n)]
        degree = [0] * n
        for a, b in edges:
            g[a].append(b)
            g[b].append(a)
            degree[a] += 1
            degree[b] += 1
        q = deque(i for i in range(n) if degree[i] == 1)
        ans = []
        while q:
            ans.clear()
            for _ in range(len(q)):
                a = q.popleft()
                ans.append(a)
                for b in g[a]:
                    degree[b] -= 1
                    if degree[b] == 1:
                        q.append(b)
        return ans
```

#### Java

```java
class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {
        if (n == 1) {
            return List.of(0);
        }
        List<Integer>[] g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        int[] degree = new int[n];
        for (int[] e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
            ++degree[a];
            ++degree[b];
        }
        Deque<Integer> q = new ArrayDeque<>();
        for (int i = 0; i < n; ++i) {
            if (degree[i] == 1) {
                q.offer(i);
            }
        }
        List<Integer> ans = new ArrayList<>();
        while (!q.isEmpty()) {
            ans.clear();
            for (int i = q.size(); i > 0; --i) {
                int a = q.poll();
                ans.add(a);
                for (int b : g[a]) {
                    if (--degree[b] == 1) {
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
    vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
        if (n == 1) {
            return {0};
        }
        vector<vector<int>> g(n);
        vector<int> degree(n);
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].push_back(b);
            g[b].push_back(a);
            ++degree[a];
            ++degree[b];
        }
        queue<int> q;
        for (int i = 0; i < n; ++i) {
            if (degree[i] == 1) {
                q.push(i);
            }
        }
        vector<int> ans;
        while (!q.empty()) {
            ans.clear();
            for (int i = q.size(); i > 0; --i) {
                int a = q.front();
                q.pop();
                ans.push_back(a);
                for (int b : g[a]) {
                    if (--degree[b] == 1) {
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
func findMinHeightTrees(n int, edges [][]int) (ans []int) {
	if n == 1 {
		return []int{0}
	}
	g := make([][]int, n)
	degree := make([]int, n)
	for _, e := range edges {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
		degree[a]++
		degree[b]++
	}
	q := []int{}
	for i, d := range degree {
		if d == 1 {
			q = append(q, i)
		}
	}
	for len(q) > 0 {
		ans = []int{}
		for i := len(q); i > 0; i-- {
			a := q[0]
			q = q[1:]
			ans = append(ans, a)
			for _, b := range g[a] {
				degree[b]--
				if degree[b] == 1 {
					q = append(q, b)
				}
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function findMinHeightTrees(n: number, edges: number[][]): number[] {
    if (n === 1) {
        return [0];
    }
    const g: number[][] = Array.from({ length: n }, () => []);
    const degree: number[] = Array(n).fill(0);
    for (const [a, b] of edges) {
        g[a].push(b);
        g[b].push(a);
        ++degree[a];
        ++degree[b];
    }
    const q: number[] = [];
    for (let i = 0; i < n; ++i) {
        if (degree[i] === 1) {
            q.push(i);
        }
    }
    const ans: number[] = [];
    while (q.length > 0) {
        ans.length = 0;
        const t: number[] = [];
        for (const a of q) {
            ans.push(a);
            for (const b of g[a]) {
                if (--degree[b] === 1) {
                    t.push(b);
                }
            }
        }
        q.splice(0, q.length, ...t);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
