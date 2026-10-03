---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Dynamic Programming
---

<!-- problem:start -->

# [2378. Choose Edges to Maximize Score in a Tree 🔒](https://leetcode.com/problems/choose-edges-to-maximize-score-in-a-tree)

[中文文档](/solution/2300-2399/2378.Choose%20Edges%20to%20Maximize%20Score%20in%20a%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một cây <strong>có trọng số</strong> gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code>.</p>

<p>Cây có node <code>0</code> làm <strong>gốc</strong> và được biểu diễn bằng một mảng <strong>2D</strong> <code>edges</code> có kích thước <code>n</code>, trong đó <code>edges[i] = [par<sub>i</sub>, weight<sub>i</sub>]</code> cho biết node <code>par<sub>i</sub></code> là <strong>cha</strong> của node <code>i</code>, và cạnh nối chúng có trọng số bằng <code>weight<sub>i</sub></code>. Vì node gốc <strong>không có</strong> node cha, nên <code>edges[0] = [-1, -1]</code>.</p>

<p>Chọn một số cạnh trong cây sao cho không có hai cạnh được chọn nào <strong>kề nhau</strong>, đồng thời tối đa hóa <strong>tổng</strong> trọng số của các cạnh được chọn.</p>

<p>Trả về <em><strong>tổng lớn nhất</strong> của trọng số các cạnh được chọn</em>.</p>

<p><strong>Lưu ý</strong>:</p>

<ul>
	<li>Bạn có thể <strong>không chọn</strong> cạnh nào trong cây; khi đó tổng trọng số sẽ là <code>0</code>.</li>
	<li>Hai cạnh <code>Edge<sub>1</sub></code> và <code>Edge<sub>2</sub></code> trong cây được gọi là <strong>kề nhau</strong> nếu chúng có một node <strong>chung</strong>.
	<ul>
		<li>Nói cách khác, chúng kề nhau nếu <code>Edge<sub>1</sub></code> nối các node <code>a</code> và <code>b</code>, còn <code>Edge<sub>2</sub></code> nối các node <code>b</code> và <code>c</code>.</li>
	</ul>
	</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2378.Choose%20Edges%20to%20Maximize%20Score%20in%20a%20Tree/images/treedrawio.png" style="width: 271px; height: 221px;" />
<pre>
<strong>Đầu vào:</strong> edges = [[-1,-1],[0,5],[0,10],[2,6],[2,4]]
<strong>Đầu ra:</strong> 11
<strong>Giải thích:</strong> Sơ đồ trên cho thấy các cạnh cần chọn được tô màu đỏ.
Tổng điểm là 5 + 6 = 11.
Có thể chứng minh rằng không thể đạt được điểm số tốt hơn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2378.Choose%20Edges%20to%20Maximize%20Score%20in%20a%20Tree/images/treee1293712983719827.png" style="width: 221px; height: 181px;" />
<pre>
<strong>Đầu vào:</strong> edges = [[-1,-1],[0,5],[0,-6],[0,7]]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Ta chọn cạnh có trọng số 7.
Lưu ý rằng ta không thể chọn nhiều hơn một cạnh vì tất cả các cạnh đều kề nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == edges.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>par<sub>0</sub> == weight<sub>0</sub> == -1</code></li>
	<li><code>0 &lt;= par<sub>i</sub> &lt;= n - 1</code> với mọi <code>i &gt;= 1</code>.</li>
	<li><code>par<sub>i</sub> != i</code></li>
	<li><code>-10<sup>6</sup> &lt;= weight<sub>i</sub> &lt;= 10<sup>6</sup></code> với mọi <code>i &gt;= 1</code>.</li>
	<li><code>edges</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tree DP

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần chọn một matching gồm các cạnh của cây sao cho tổng trọng số lớn nhất. Mỗi lựa chọn chỉ ảnh hưởng đến hai đầu mút, nên chỉ cần tree DP.
>
> $dfs(i)$ trả về trọng số tốt nhất khi cạnh nối với node cha được chọn và khi không được chọn. Trường hợp đầu cộng các giá trị “không chọn” của các node con; trường hợp sau có thể đổi trạng thái của nhiều nhất một node con sang “được chọn” và cộng trọng số của cạnh đó.

<!-- thinking:end -->

Ta xây dựng hàm $dfs(i)$, biểu thị tổng trọng số lớn nhất của các cạnh được chọn trong cây con có gốc là node $i$, sao cho không có hai cạnh được chọn nào kề nhau. Hàm này trả về hai giá trị $(a, b)$. Giá trị đầu tiên $a$ biểu thị tổng trọng số của các cạnh được chọn khi cạnh nối node hiện tại $i$ với node cha được chọn. Giá trị thứ hai $b$ biểu thị tổng trọng số của các cạnh được chọn khi cạnh nối node hiện tại $i$ với node cha không được chọn.

Ta có thể nhận thấy với node hiện tại $i$:

- Nếu cạnh nối $i$ với node cha được chọn, thì không cạnh nào nối $i$ với các node con được chọn. Khi đó, giá trị $a$ của node hiện tại là tổng các giá trị $b$ của tất cả node con.
- Nếu cạnh nối $i$ với node cha không được chọn, thì ta có thể chọn nhiều nhất một cạnh nối $i$ với các node con. Khi đó, giá trị $b$ của node hiện tại là tổng các giá trị $a$ của node con được chọn và các giá trị $b$ của node con không được chọn, cộng với trọng số của cạnh nối $i$ với node con được chọn.

Ta gọi hàm $dfs(0)$; giá trị thứ hai được trả về là đáp án, tức tổng trọng số của các cạnh được chọn khi cạnh nối node gốc với node cha của nó không được chọn.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxScore(self, edges: List[List[int]]) -> int:
        def dfs(i):
            a = b = t = 0
            for j, w in g[i]:
                x, y = dfs(j)
                a += y
                b += y
                t = max(t, x - y + w)
            b += t
            return a, b

        g = defaultdict(list)
        for i, (p, w) in enumerate(edges[1:], 1):
            g[p].append((i, w))
        return dfs(0)[1]
```

#### Java

```java
class Solution {
    private List<int[]>[] g;

    public long maxScore(int[][] edges) {
        int n = edges.length;
        g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int i = 1; i < n; ++i) {
            int p = edges[i][0], w = edges[i][1];
            g[p].add(new int[] {i, w});
        }
        return dfs(0)[1];
    }

    private long[] dfs(int i) {
        long a = 0, b = 0, t = 0;
        for (int[] nxt : g[i]) {
            int j = nxt[0], w = nxt[1];
            long[] s = dfs(j);
            a += s[1];
            b += s[1];
            t = Math.max(t, s[0] - s[1] + w);
        }
        b += t;
        return new long[] {a, b};
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxScore(vector<vector<int>>& edges) {
        int n = edges.size();
        vector<vector<pair<int, int>>> g(n);
        for (int i = 1; i < n; ++i) {
            int p = edges[i][0], w = edges[i][1];
            g[p].emplace_back(i, w);
        }
        using ll = long long;
        using pll = pair<ll, ll>;
        function<pll(int)> dfs = [&](int i) -> pll {
            ll a = 0, b = 0, t = 0;
            for (auto& [j, w] : g[i]) {
                auto [x, y] = dfs(j);
                a += y;
                b += y;
                t = max(t, x - y + w);
            }
            b += t;
            return make_pair(a, b);
        };
        return dfs(0).second;
    }
};
```

#### Go

```go
func maxScore(edges [][]int) int64 {
	n := len(edges)
	g := make([][][2]int, n)
	for i := 1; i < n; i++ {
		p, w := edges[i][0], edges[i][1]
		g[p] = append(g[p], [2]int{i, w})
	}
	var dfs func(int) [2]int
	dfs = func(i int) [2]int {
		var a, b, t int
		for _, e := range g[i] {
			j, w := e[0], e[1]
			s := dfs(j)
			a += s[1]
			b += s[1]
			t = max(t, s[0]-s[1]+w)
		}
		b += t
		return [2]int{a, b}
	}
	return int64(dfs(0)[1])
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
