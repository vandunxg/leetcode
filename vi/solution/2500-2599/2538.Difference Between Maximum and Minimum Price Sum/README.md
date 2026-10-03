---
comments: true
difficulty: Hard
rating: 2397
source: Weekly Contest 328 Q4
tags:
    - Tree
    - Depth-First Search
    - Array
    - Dynamic Programming
    - Tree DP
---

<!-- problem:start -->

# [2538. Difference Between Maximum and Minimum Price Sum](https://leetcode.com/problems/difference-between-maximum-and-minimum-price-sum)

[Tài liệu tiếng Trung](/solution/2500-2599/2538.Difference%20Between%20Maximum%20and%20Minimum%20Price%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một cây vô hướng, ban đầu chưa chọn gốc, gồm <code>n</code> nút được đánh số từ <code>0</code> đến <code>n - 1</code>. Bạn được cho số nguyên <code>n</code> và một mảng số nguyên 2 chiều <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> cho biết có một cạnh nối nút <code>a<sub>i</sub></code> và nút <code>b<sub>i</sub></code> trong cây.</p>

<p>Mỗi nút có một giá trị giá. Bạn được cho một mảng số nguyên <code>price</code>, trong đó <code>price[i]</code> là giá của nút thứ <code>i<sup>th</sup></code>.</p>

<p><strong>Tổng giá</strong> của một đường đi là tổng giá của tất cả các nút nằm trên đường đi đó.</p>

<p>Cây có thể được chọn bất kỳ nút nào <code>root</code> làm gốc. <strong>Chi phí</strong> sau khi chọn <code>root</code> là hiệu giữa <strong>tổng giá</strong> lớn nhất và nhỏ nhất trong tất cả các đường đi bắt đầu từ <code>root</code>.</p>

<p>Hãy trả về <em><strong>chi phí</strong> có thể đạt giá trị <strong>lớn nhất</strong></em> <em>trong tất cả các cách chọn gốc</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2538.Difference%20Between%20Maximum%20and%20Minimum%20Price%20Sum/images/example14.png" style="width: 556px; height: 231px;" />
<pre>
<strong>Đầu vào:</strong> n = 6, edges = [[0,1],[1,2],[1,3],[3,4],[3,5]], price = [9,8,7,6,10,5]
<strong>Đầu ra:</strong> 24
<strong>Giải thích:</strong> Hình trên biểu diễn cây sau khi chọn nút 2 làm gốc. Phần thứ nhất (màu đỏ) cho biết đường đi có tổng giá lớn nhất. Phần thứ hai (màu xanh dương) cho biết đường đi có tổng giá nhỏ nhất.
- Đường đi thứ nhất gồm các nút [2,1,3,4]: giá tương ứng là [7,8,6,10], và tổng giá là 31.
- Đường đi thứ hai chỉ gồm nút [2] có giá [7].
Hiệu giữa tổng giá lớn nhất và nhỏ nhất là 24. Có thể chứng minh rằng 24 là chi phí lớn nhất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2538.Difference%20Between%20Maximum%20and%20Minimum%20Price%20Sum/images/p1_example2.png" style="width: 352px; height: 184px;" />
<pre>
<strong>Đầu vào:</strong> n = 3, edges = [[0,1],[1,2]], price = [1,1,1]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Hình trên biểu diễn cây sau khi chọn nút 0 làm gốc. Phần thứ nhất (màu đỏ) cho biết đường đi có tổng giá lớn nhất. Phần thứ hai (màu xanh dương) cho biết đường đi có tổng giá nhỏ nhất.
- Đường đi thứ nhất gồm các nút [0,1,2]: giá tương ứng là [1,1,1], và tổng giá là 3.
- Đường đi thứ hai chỉ gồm nút [0] có giá [1].
Hiệu giữa tổng giá lớn nhất và nhỏ nhất là 2. Có thể chứng minh rằng 2 là chi phí lớn nhất.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>edges</code> biểu diễn một cây hợp lệ.</li>
	<li><code>price.length == n</code></li>
	<li><code>1 &lt;= price[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chi phí của một đường đi bằng tổng giá các nút trừ đi giá của đầu mút nhỏ hơn; ta muốn tìm giá trị lớn nhất trên mọi đường đi. Vì các giá đều dương, đây là tổng giá trên đường đi trừ đi giá của một đầu mút. Liệt kê tất cả các đường đi sẽ có độ phức tạp $O(n^2)$.
>
> Tree DP duy trì hai giá trị cho mỗi cây con: chuỗi đi xuống dài nhất $a$ vẫn chứa đầu mút xa, và chuỗi dài nhất $b$ sau khi bỏ đầu mút đó. Khi kết hợp $a$ với $d$ của nút con, hoặc $b$ với $c$ của nút con, ta bao quát được đường đi tốt nhất đi qua nút hiện tại.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxOutput(self, n: int, edges: List[List[int]], price: List[int]) -> int:
        def dfs(i, fa):
            a, b = price[i], 0
            for j in g[i]:
                if j != fa:
                    c, d = dfs(j, i)
                    nonlocal ans
                    ans = max(ans, a + d, b + c)
                    a = max(a, price[i] + c)
                    b = max(b, price[i] + d)
            return a, b

        g = defaultdict(list)
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
    private long ans;
    private int[] price;

    public long maxOutput(int n, int[][] edges, int[] price) {
        g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        this.price = price;
        dfs(0, -1);
        return ans;
    }

    private long[] dfs(int i, int fa) {
        long a = price[i], b = 0;
        for (int j : g[i]) {
            if (j != fa) {
                var e = dfs(j, i);
                long c = e[0], d = e[1];
                ans = Math.max(ans, Math.max(a + d, b + c));
                a = Math.max(a, price[i] + c);
                b = Math.max(b, price[i] + d);
            }
        }
        return new long[] {a, b};
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxOutput(int n, vector<vector<int>>& edges, vector<int>& price) {
        vector<vector<int>> g(n);
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].push_back(b);
            g[b].push_back(a);
        }
        using ll = long long;
        using pll = pair<ll, ll>;
        ll ans = 0;
        function<pll(int, int)> dfs = [&](int i, int fa) {
            ll a = price[i], b = 0;
            for (int j : g[i]) {
                if (j != fa) {
                    auto [c, d] = dfs(j, i);
                    ans = max({ans, a + d, b + c});
                    a = max(a, price[i] + c);
                    b = max(b, price[i] + d);
                }
            }
            return pll{a, b};
        };
        dfs(0, -1);
        return ans;
    }
};
```

#### Go

```go
func maxOutput(n int, edges [][]int, price []int) int64 {
	g := make([][]int, n)
	for _, e := range edges {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	type pair struct{ a, b int }
	ans := 0
	var dfs func(i, fa int) pair
	dfs = func(i, fa int) pair {
		a, b := price[i], 0
		for _, j := range g[i] {
			if j != fa {
				e := dfs(j, i)
				c, d := e.a, e.b
				ans = max(ans, max(a+d, b+c))
				a = max(a, price[i]+c)
				b = max(b, price[i]+d)
			}
		}
		return pair{a, b}
	}
	dfs(0, -1)
	return int64(ans)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
