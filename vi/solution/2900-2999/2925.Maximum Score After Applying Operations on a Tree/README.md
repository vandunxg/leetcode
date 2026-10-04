---
comments: true
difficulty: Medium
rating: 1939
source: Weekly Contest 370 Q3
tags:
    - Tree
    - Depth-First Search
    - Dynamic Programming
    - Tree DP
---

<!-- problem:start -->

# [2925. Maximum Score After Applying Operations on a Tree](https://leetcode.com/problems/maximum-score-after-applying-operations-on-a-tree)

[中文文档](/solution/2900-2999/2925.Maximum%20Score%20After%20Applying%20Operations%20on%20a%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Có một cây vô hướng gồm <code>n</code> nút được đánh nhãn từ <code>0</code> đến <code>n - 1</code>, và được gốc hóa tại nút <code>0</code>. Cho một mảng số nguyên 2 chiều <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> cho biết có một cạnh nối giữa các nút <code>a<sub>i</sub></code> và <code>b<sub>i</sub></code> trong cây.</p>

<p>Ta cũng được cho một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>values</code> có độ dài <code>n</code>, trong đó <code>values[i]</code> là <strong>giá trị</strong> gắn với nút thứ <code>i<sup>th</sup></code>.</p>

<p>Ban đầu, điểm số là <code>0</code>. Trong một thao tác, ta có thể:</p>

<ul>
	<li>Chọn một nút bất kỳ <code>i</code>.</li>
	<li>Cộng <code>values[i]</code> vào điểm số.</li>
	<li>Đặt <code>values[i]</code> thành <code>0</code>.</li>
</ul>

<p>Một cây được gọi là <strong>khỏe mạnh</strong> nếu tổng các giá trị trên đường đi từ gốc đến bất kỳ nút lá nào khác 0.</p>

<p>Hãy trả về <em><strong>điểm số lớn nhất</strong> có thể đạt được sau khi thực hiện các thao tác này một số lần bất kỳ sao cho cây vẫn <strong>khỏe mạnh</strong>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2900-2999/2925.Maximum%20Score%20After%20Applying%20Operations%20on%20a%20Tree/images/graph-13-1.png" style="width: 515px; height: 443px;" />
<pre>
<strong>Đầu vào:</strong> edges = [[0,1],[0,2],[0,3],[2,4],[4,5]], values = [5,2,5,2,1,1]
<strong>Đầu ra:</strong> 11
<strong>Giải thích:</strong> Ta có thể chọn các nút 1, 2, 3, 4 và 5. Giá trị của gốc khác 0. Do đó, tổng các giá trị trên đường đi từ gốc đến mọi nút lá đều khác 0. Vì vậy, cây khỏe mạnh và điểm số là values[1] + values[2] + values[3] + values[4] + values[5] = 11.
Có thể chứng minh rằng 11 là điểm số lớn nhất có thể đạt được sau một số lần thao tác bất kỳ trên cây.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2900-2999/2925.Maximum%20Score%20After%20Applying%20Operations%20on%20a%20Tree/images/graph-14-2.png" style="width: 522px; height: 245px;" />
<pre>
<strong>Đầu vào:</strong> edges = [[0,1],[0,2],[1,3],[1,4],[2,5],[2,6]], values = [20,10,9,7,4,3,5]
<strong>Đầu ra:</strong> 40
<strong>Giải thích:</strong> Ta có thể chọn các nút 0, 2, 3 và 4.
- Tổng các giá trị trên đường đi từ 0 đến 4 bằng 10.
- Tổng các giá trị trên đường đi từ 0 đến 3 bằng 10.
- Tổng các giá trị trên đường đi từ 0 đến 5 bằng 3.
- Tổng các giá trị trên đường đi từ 0 đến 6 bằng 5.
Do đó, cây khỏe mạnh và điểm số là values[0] + values[2] + values[3] + values[4] = 40.
Có thể chứng minh rằng 40 là điểm số lớn nhất có thể đạt được sau một số lần thao tác bất kỳ trên cây.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
	<li><code>values.length == n</code></li>
	<li><code>1 &lt;= values[i] &lt;= 10<sup>9</sup></code></li>
	<li>Dữ liệu đầu vào được tạo sao cho <code>edges</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động trên cây

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi đường đi từ gốc đến lá phải còn ít nhất một nút không được chọn; các nút còn lại có thể đóng góp vào điểm số. Việc liệt kê lựa chọn chọn/bỏ qua tại từng nút đồng thời đảm bảo mọi đường đi sẽ nhanh chóng bùng nổ. Tree DP cục bộ hóa ràng buộc: không chọn gốc thì có thể chọn toàn bộ các cây con, còn chọn gốc thì phải giữ cho từng cây con vẫn hợp lệ.
>
> $dfs$ trả về tổng của cây con và lựa chọn hợp lệ tốt nhất. Một nút lá chỉ có thể để bản thân nó không được chọn, nên giá trị thứ hai là $0$. Với một nút không phải lá, ta lấy $\max(values[i]+b, a)$. Đáp án là giá trị thứ hai tại gốc.

<!-- thinking:end -->

Thực chất, bài toán yêu cầu ta chọn một số nút trong toàn bộ cây sao cho tổng giá trị của các nút được chọn là lớn nhất, đồng thời trên mỗi đường đi từ nút gốc đến nút lá có một nút không được chọn.

Ta có thể dùng phương pháp quy hoạch động trên cây để giải bài toán này.

Ta thiết kế hàm $dfs(i, fa)$, trong đó $i$ biểu diễn nút hiện tại, với nút $i$ là gốc của cây con, còn $fa$ biểu diễn nút cha của $i$. Hàm trả về một mảng có độ dài $2$, trong đó $[0]$ là tổng giá trị của tất cả các nút trong cây con, còn $[1]$ là giá trị lớn nhất của cây con thỏa mãn điều kiện trên mỗi đường đi có một nút không được chọn.

Giá trị của $[0]$ có thể thu được trực tiếp bằng cách dùng DFS để cộng dồn giá trị của từng nút, còn giá trị của $[1]$ cần xét hai trường hợp, tùy theo nút $i$ có được chọn hay không. Nếu nút này được chọn, mỗi cây con của nút $i$ phải thỏa mãn điều kiện trên mỗi đường đi có một nút không được chọn; nếu nút này không được chọn, toàn bộ các nút trong mỗi cây con của nút $i$ đều có thể được chọn. Ta lấy giá trị lớn hơn trong hai trường hợp này.

Cần lưu ý rằng giá trị của $[1]$ tại nút lá là $0$, vì nút lá không có cây con nên không cần xét trường hợp mỗi đường đi có một nút không được chọn.

Đáp án là $dfs(0, -1)[1]$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số lượng nút.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumScoreAfterOperations(
        self, edges: List[List[int]], values: List[int]
    ) -> int:
        def dfs(i: int, fa: int = -1) -> (int, int):
            a = b = 0
            leaf = True
            for j in g[i]:
                if j != fa:
                    leaf = False
                    aa, bb = dfs(j, i)
                    a += aa
                    b += bb
            if leaf:
                return values[i], 0
            return values[i] + a, max(values[i] + b, a)

        g = [[] for _ in range(len(values))]
        for a, b in edges:
            g[a].append(b)
            g[b].append(a)
        return dfs(0)[1]
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private int[] values;

    public long maximumScoreAfterOperations(int[][] edges, int[] values) {
        int n = values.length;
        g = new List[n];
        this.values = values;
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        return dfs(0, -1)[1];
    }

    private long[] dfs(int i, int fa) {
        long a = 0, b = 0;
        boolean leaf = true;
        for (int j : g[i]) {
            if (j != fa) {
                leaf = false;
                var t = dfs(j, i);
                a += t[0];
                b += t[1];
            }
        }
        if (leaf) {
            return new long[] {values[i], 0};
        }
        return new long[] {values[i] + a, Math.max(values[i] + b, a)};
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maximumScoreAfterOperations(vector<vector<int>>& edges, vector<int>& values) {
        int n = values.size();
        vector<int> g[n];
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].emplace_back(b);
            g[b].emplace_back(a);
        }
        using ll = long long;
        function<pair<ll, ll>(int, int)> dfs = [&](int i, int fa) -> pair<ll, ll> {
            ll a = 0, b = 0;
            bool leaf = true;
            for (int j : g[i]) {
                if (j != fa) {
                    auto [aa, bb] = dfs(j, i);
                    a += aa;
                    b += bb;
                    leaf = false;
                }
            }
            if (leaf) {
                return {values[i], 0LL};
            }
            return {values[i] + a, max(values[i] + b, a)};
        };
        auto [_, b] = dfs(0, -1);
        return b;
    }
};
```

#### Go

```go
func maximumScoreAfterOperations(edges [][]int, values []int) int64 {
	g := make([][]int, len(values))
	for _, e := range edges {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	var dfs func(int, int) (int64, int64)
	dfs = func(i, fa int) (int64, int64) {
		a, b := int64(0), int64(0)
		leaf := true
		for _, j := range g[i] {
			if j != fa {
				leaf = false
				aa, bb := dfs(j, i)
				a += aa
				b += bb
			}
		}
		if leaf {
			return int64(values[i]), int64(0)
		}
		return int64(values[i]) + a, max(int64(values[i])+b, a)
	}
	_, b := dfs(0, -1)
	return b
}
```

#### TypeScript

```ts
function maximumScoreAfterOperations(edges: number[][], values: number[]): number {
    const g: number[][] = Array.from({ length: values.length }, () => []);
    for (const [a, b] of edges) {
        g[a].push(b);
        g[b].push(a);
    }
    const dfs = (i: number, fa: number): [number, number] => {
        let [a, b] = [0, 0];
        let leaf = true;
        for (const j of g[i]) {
            if (j !== fa) {
                const [aa, bb] = dfs(j, i);
                a += aa;
                b += bb;
                leaf = false;
            }
        }
        if (leaf) {
            return [values[i], 0];
        }
        return [values[i] + a, Math.max(values[i] + b, a)];
    };
    return dfs(0, -1)[1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
