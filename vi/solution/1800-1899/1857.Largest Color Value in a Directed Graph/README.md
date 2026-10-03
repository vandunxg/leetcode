---
comments: true
difficulty: Hard
rating: 2312
source: Weekly Contest 240 Q4
tags:
    - Graph
    - Topological Sort
    - Memoization
    - Hash Table
    - String
    - Dynamic Programming
    - Counting
    - Directed Acyclic Graph
---

<!-- problem:start -->

# [1857. Largest Color Value in a Directed Graph](https://leetcode.com/problems/largest-color-value-in-a-directed-graph)

[中文文档](/solution/1800-1899/1857.Largest%20Color%20Value%20in%20a%20Directed%20Graph/README.md)

## Mô tả

<!-- description:start -->

<p>Có một <strong>đồ thị có hướng</strong> gồm <code>n</code> node được tô màu và <code>m</code> cạnh. Các node được đánh số từ <code>0</code> đến <code>n - 1</code>.</p>

<p>Cho một chuỗi <code>colors</code>, trong đó <code>colors[i]</code> là một chữ cái tiếng Anh viết thường biểu diễn <strong>màu</strong> của node thứ <code>i<sup>th</sup></code> trong đồ thị này (đánh <strong>chỉ số từ 0</strong>). Ngoài ra, cho một mảng 2 chiều <code>edges</code>, trong đó <code>edges[j] = [a<sub>j</sub>, b<sub>j</sub>]</code> cho biết có một <strong>cạnh có hướng</strong> từ node <code>a<sub>j</sub></code> tới node <code>b<sub>j</sub></code>.</p>

<p><strong>Đường đi</strong> hợp lệ trong đồ thị là một dãy node <code>x<sub>1</sub> -&gt; x<sub>2</sub> -&gt; x<sub>3</sub> -&gt; ... -&gt; x<sub>k</sub></code> sao cho tồn tại cạnh có hướng từ <code>x<sub>i</sub></code> tới <code>x<sub>i+1</sub></code> với mọi <code>1 &lt;= i &lt; k</code>. <strong>Giá trị màu</strong> của đường đi là số node mang màu xuất hiện <strong>nhiều nhất</strong> trên đường đi đó.</p>

<p>Trả về <em><strong>giá trị màu lớn nhất</strong> của một đường đi hợp lệ bất kỳ trong đồ thị đã cho, hoặc </em><code>-1</code><em> nếu đồ thị chứa chu trình</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1857.Largest%20Color%20Value%20in%20a%20Directed%20Graph/images/leet1.png" style="width: 400px; height: 182px;" /></p>

<pre>
<strong>Đầu vào:</strong> colors = &quot;abaca&quot;, edges = [[0,1],[0,2],[2,3],[3,4]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Đường đi 0 -&gt; 2 -&gt; 3 -&gt; 4 chứa 3 node có màu <code>&quot;a&quot; (red in the above image)</code>.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1857.Largest%20Color%20Value%20in%20a%20Directed%20Graph/images/leet2.png" style="width: 85px; height: 85px;" /></p>

<pre>
<strong>Đầu vào:</strong> colors = &quot;a&quot;, edges = [[0,0]]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Có một chu trình từ 0 tới 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == colors.length</code></li>
	<li><code>m == edges.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= m &lt;= 10<sup>5</sup></code></li>
	<li><code>colors</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>0 &lt;= a<sub>j</sub>, b<sub>j</sub>&nbsp;&lt; n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp tô pô + Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần số lượng lớn nhất của một màu bất kỳ trên một đường đi có hướng, hoặc $-1$ nếu tồn tại chu trình. Có số lượng đường đi theo cấp số mũ, trong khi $n,m\le 10^5$.
>
> Thứ tự tô pô xử lý một node sau tất cả predecessor của nó. $dp[i][c]$ là số lượng tốt nhất của màu $c$ trên một đường đi kết thúc tại $i$; ta lấy maximum theo từng tọa độ trên các cạnh đi vào rồi cộng màu của chính node đó. Nếu có ít hơn $n$ node được lấy ra khỏi queue thì đồ thị chứa chu trình.

<!-- thinking:end -->

Tính bậc vào của mỗi node và thực hiện sắp xếp tô pô.

Định nghĩa mảng 2 chiều $dp$, trong đó $dp[i][j]$ biểu diễn số node có màu $j$ trên đường đi từ node bắt đầu tới node $i$.

Từ node $i$, duyệt tất cả cạnh đi ra $i \to j$ và cập nhật $dp[j][k] = \max(dp[j][k], dp[i][k] + (c == k))$, trong đó $c$ là màu của node $j$.

Đáp án là giá trị lớn nhất trong mảng $dp$.

Nếu đồ thị có chu trình thì không thể đi qua tất cả node, nên trả về $-1$.

Độ phức tạp thời gian là $O((n + m) \times |\Sigma|)$, độ phức tạp không gian là $O(m + n \times |\Sigma|)$. Ở đây, $|\Sigma|$ là kích thước bảng chữ cái (trong trường hợp này là 26), còn $n$ và $m$ lần lượt là số node và số cạnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestPathValue(self, colors: str, edges: List[List[int]]) -> int:
        n = len(colors)
        indeg = [0] * n
        g = defaultdict(list)
        for a, b in edges:
            g[a].append(b)
            indeg[b] += 1
        q = deque()
        dp = [[0] * 26 for _ in range(n)]
        for i, v in enumerate(indeg):
            if v == 0:
                q.append(i)
                c = ord(colors[i]) - ord('a')
                dp[i][c] += 1
        cnt = 0
        ans = 1
        while q:
            i = q.popleft()
            cnt += 1
            for j in g[i]:
                indeg[j] -= 1
                if indeg[j] == 0:
                    q.append(j)
                c = ord(colors[j]) - ord('a')
                for k in range(26):
                    dp[j][k] = max(dp[j][k], dp[i][k] + (c == k))
                    ans = max(ans, dp[j][k])
        return -1 if cnt < n else ans
```

#### Java

```java
class Solution {
    public int largestPathValue(String colors, int[][] edges) {
        int n = colors.length();
        List<Integer>[] g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        int[] indeg = new int[n];
        for (int[] e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            ++indeg[b];
        }
        Deque<Integer> q = new ArrayDeque<>();
        int[][] dp = new int[n][26];
        for (int i = 0; i < n; ++i) {
            if (indeg[i] == 0) {
                q.offer(i);
                int c = colors.charAt(i) - 'a';
                ++dp[i][c];
            }
        }
        int cnt = 0;
        int ans = 1;
        while (!q.isEmpty()) {
            int i = q.pollFirst();
            ++cnt;
            for (int j : g[i]) {
                if (--indeg[j] == 0) {
                    q.offer(j);
                }
                int c = colors.charAt(j) - 'a';
                for (int k = 0; k < 26; ++k) {
                    dp[j][k] = Math.max(dp[j][k], dp[i][k] + (c == k ? 1 : 0));
                    ans = Math.max(ans, dp[j][k]);
                }
            }
        }
        return cnt == n ? ans : -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int largestPathValue(string colors, vector<vector<int>>& edges) {
        int n = colors.size();
        vector<vector<int>> g(n);
        vector<int> indeg(n);
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].push_back(b);
            ++indeg[b];
        }
        queue<int> q;
        vector<vector<int>> dp(n, vector<int>(26));
        for (int i = 0; i < n; ++i) {
            if (indeg[i] == 0) {
                q.push(i);
                int c = colors[i] - 'a';
                dp[i][c]++;
            }
        }
        int cnt = 0;
        int ans = 1;
        while (!q.empty()) {
            int i = q.front();
            q.pop();
            ++cnt;
            for (int j : g[i]) {
                if (--indeg[j] == 0) q.push(j);
                int c = colors[j] - 'a';
                for (int k = 0; k < 26; ++k) {
                    dp[j][k] = max(dp[j][k], dp[i][k] + (c == k));
                    ans = max(ans, dp[j][k]);
                }
            }
        }
        return cnt == n ? ans : -1;
    }
};
```

#### Go

```go
func largestPathValue(colors string, edges [][]int) int {
	n := len(colors)
	g := make([][]int, n)
	indeg := make([]int, n)
	for _, e := range edges {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		indeg[b]++
	}
	q := []int{}
	dp := make([][]int, n)
	for i := range dp {
		dp[i] = make([]int, 26)
	}
	for i, v := range indeg {
		if v == 0 {
			q = append(q, i)
			c := colors[i] - 'a'
			dp[i][c]++
		}
	}
	cnt := 0
	ans := 1
	for len(q) > 0 {
		i := q[0]
		q = q[1:]
		cnt++
		for _, j := range g[i] {
			indeg[j]--
			if indeg[j] == 0 {
				q = append(q, j)
			}
			c := int(colors[j] - 'a')
			for k := 0; k < 26; k++ {
				t := 0
				if c == k {
					t = 1
				}
				dp[j][k] = max(dp[j][k], dp[i][k]+t)
				ans = max(ans, dp[j][k])
			}
		}
	}
	if cnt == n {
		return ans
	}
	return -1
}
```

#### TypeScript

```ts
function largestPathValue(colors: string, edges: number[][]): number {
    const n = colors.length;
    const indeg = Array(n).fill(0);
    const g: Map<number, number[]> = new Map();
    for (const [a, b] of edges) {
        if (!g.has(a)) g.set(a, []);
        g.get(a)!.push(b);
        indeg[b]++;
    }
    const q: number[] = [];
    const dp: number[][] = Array.from({ length: n }, () => Array(26).fill(0));
    for (let i = 0; i < n; i++) {
        if (indeg[i] === 0) {
            q.push(i);
            const c = colors.charCodeAt(i) - 97;
            dp[i][c]++;
        }
    }
    let cnt = 0;
    let ans = 1;
    while (q.length) {
        const i = q.pop()!;
        cnt++;
        if (g.has(i)) {
            for (const j of g.get(i)!) {
                indeg[j]--;
                if (indeg[j] === 0) q.push(j);
                const c = colors.charCodeAt(j) - 97;
                for (let k = 0; k < 26; k++) {
                    dp[j][k] = Math.max(dp[j][k], dp[i][k] + (c === k ? 1 : 0));
                    ans = Math.max(ans, dp[j][k]);
                }
            }
        }
    }
    return cnt < n ? -1 : ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
