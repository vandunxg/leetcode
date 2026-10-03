---
comments: true
difficulty: Medium
rating: 1682
source: Biweekly Contest 93 Q2
tags:
    - Greedy
    - Graph
    - Array
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2497. Maximum Star Sum of a Graph](https://leetcode.com/problems/maximum-star-sum-of-a-graph)

[中文文档](/solution/2400-2499/2497.Maximum%20Star%20Sum%20of%20a%20Graph/README.md)

## Mô tả

<!-- description:start -->

<p>Có một đồ thị vô hướng gồm <code>n</code> nút được đánh số từ <code>0</code> đến <code>n - 1</code>. Bạn được cho một mảng số nguyên <code>vals</code> có độ dài <code>n</code> và <strong>được đánh chỉ số từ 0</strong>, trong đó <code>vals[i]</code> biểu thị giá trị của nút thứ <code>i<sup>th</sup></code>.</p>

<p>Bạn cũng được cho một mảng số nguyên 2D <code>edges</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> biểu thị rằng có một cạnh <strong>vô hướng</strong> nối các nút <code>a<sub>i</sub></code> và <code>b<sub>i.</sub></code>.</p>

<p><strong>Đồ thị hình sao</strong> là một đồ thị con của đồ thị đã cho, có một nút trung tâm và <code>0</code> hoặc nhiều nút kề. Nói cách khác, đó là một tập con các cạnh của đồ thị đã cho sao cho tồn tại một nút chung trên tất cả các cạnh.</p>

<p>Hình dưới đây minh họa các đồ thị hình sao có lần lượt <code>3</code> và <code>4</code> nút kề, với nút màu xanh là trung tâm.</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2497.Maximum%20Star%20Sum%20of%20a%20Graph/images/max-star-sum-descdrawio.png" style="width: 400px; height: 179px;" />
<p><strong>Tổng hình sao</strong> là tổng giá trị của tất cả các nút xuất hiện trong đồ thị hình sao.</p>

<p>Cho một số nguyên <code>k</code>, hãy trả về <em><strong>tổng hình sao lớn nhất</strong> của một đồ thị hình sao chứa <strong>không quá</strong> </em><code>k</code><em> cạnh.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2497.Maximum%20Star%20Sum%20of%20a%20Graph/images/max-star-sum-example1drawio.png" style="width: 300px; height: 291px;" />
<pre>
<strong>Đầu vào:</strong> vals = [1,2,3,4,10,-10,-20], edges = [[0,1],[1,2],[1,3],[3,4],[3,5],[3,6]], k = 2
<strong>Đầu ra:</strong> 16
<strong>Giải thích:</strong> Hình trên biểu diễn đồ thị đầu vào.
Đồ thị hình sao có tổng lớn nhất được đánh dấu màu xanh. Nó có nút 3 làm trung tâm và chứa các nút kề 1 và 4.
Có thể chứng minh rằng không thể tạo được đồ thị hình sao có tổng lớn hơn 16.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> vals = [-5], edges = [], k = 0
<strong>Đầu ra:</strong> -5
<strong>Giải thích:</strong> Chỉ có một đồ thị hình sao khả dĩ, chính là nút 0.
Vì vậy, ta trả về -5.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == vals.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= vals[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= edges.length &lt;= min(n * (n - 1) / 2</code><code>, 10<sup>5</sup>)</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li><code>0 &lt;= k &lt;= n - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một đồ thị hình sao gồm một nút trung tâm và nhiều nhất $k$ cạnh. Có thể bỏ qua các nút kề có giá trị âm. Với $n\le 10^5$, ta sắp xếp các giá trị dương của các nút kề theo thứ tự giảm dần, cộng $k$ giá trị đầu tiên vào nút trung tâm, rồi chọn nút trung tâm tốt nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxStarSum(self, vals: List[int], edges: List[List[int]], k: int) -> int:
        g = defaultdict(list)
        for a, b in edges:
            if vals[b] > 0:
                g[a].append(vals[b])
            if vals[a] > 0:
                g[b].append(vals[a])
        for bs in g.values():
            bs.sort(reverse=True)
        return max(v + sum(g[i][:k]) for i, v in enumerate(vals))
```

#### Java

```java
class Solution {
    public int maxStarSum(int[] vals, int[][] edges, int k) {
        int n = vals.length;
        List<Integer>[] g = new List[n];
        Arrays.setAll(g, key -> new ArrayList<>());
        for (var e : edges) {
            int a = e[0], b = e[1];
            if (vals[b] > 0) {
                g[a].add(vals[b]);
            }
            if (vals[a] > 0) {
                g[b].add(vals[a]);
            }
        }
        for (var e : g) {
            Collections.sort(e, (a, b) -> b - a);
        }
        int ans = Integer.MIN_VALUE;
        for (int i = 0; i < n; ++i) {
            int v = vals[i];
            for (int j = 0; j < Math.min(g[i].size(), k); ++j) {
                v += g[i].get(j);
            }
            ans = Math.max(ans, v);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxStarSum(vector<int>& vals, vector<vector<int>>& edges, int k) {
        int n = vals.size();
        vector<vector<int>> g(n);
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            if (vals[b] > 0) g[a].emplace_back(vals[b]);
            if (vals[a] > 0) g[b].emplace_back(vals[a]);
        }
        for (auto& e : g) sort(e.rbegin(), e.rend());
        int ans = INT_MIN;
        for (int i = 0; i < n; ++i) {
            int v = vals[i];
            for (int j = 0; j < min((int) g[i].size(), k); ++j) v += g[i][j];
            ans = max(ans, v);
        }
        return ans;
    }
};
```

#### Go

```go
func maxStarSum(vals []int, edges [][]int, k int) (ans int) {
	n := len(vals)
	g := make([][]int, n)
	for _, e := range edges {
		a, b := e[0], e[1]
		if vals[b] > 0 {
			g[a] = append(g[a], vals[b])
		}
		if vals[a] > 0 {
			g[b] = append(g[b], vals[a])
		}
	}
	for _, e := range g {
		sort.Sort(sort.Reverse(sort.IntSlice(e)))
	}
	ans = math.MinInt32
	for i, v := range vals {
		for j := 0; j < min(len(g[i]), k); j++ {
			v += g[i][j]
		}
		ans = max(ans, v)
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
