---
comments: true
difficulty: Hard
rating: 2276
source: Biweekly Contest 120 Q4
tags:
    - Tree
    - Depth-First Search
    - Dynamic Programming
    - Sorting
    - Tree DP
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2973. Find Number of Coins to Place in Tree Nodes](https://leetcode.com/problems/find-number-of-coins-to-place-in-tree-nodes)

[中文文档](/solution/2900-2999/2973.Find%20Number%20of%20Coins%20to%20Place%20in%20Tree%20Nodes/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một cây <strong>vô hướng</strong> gồm <code>n</code> nút được đánh nhãn từ <code>0</code> đến <code>n - 1</code>, và được gốc hóa tại nút <code>0</code>. Cho một mảng số nguyên 2 chiều <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> cho biết có một cạnh nối giữa các nút <code>a<sub>i</sub></code> và <code>b<sub>i</sub></code> trong cây.</p>

<p>Ta cũng được cho một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>cost</code> có độ dài <code>n</code>, trong đó <code>cost[i]</code> là <strong>chi phí</strong> được gán cho nút thứ <code>i<sup>th</sup></code>.</p>

<p>Ta cần đặt một số đồng xu lên mỗi nút của cây. Số đồng xu đặt tại nút <code>i</code> được tính như sau:</p>

<ul>
	<li>Nếu kích thước cây con của nút <code>i</code> nhỏ hơn <code>3</code>, đặt <code>1</code> đồng xu.</li>
	<li>Nếu không, đặt số đồng xu bằng <strong>tích lớn nhất</strong> của các giá trị chi phí được gán cho <code>3</code> nút phân biệt trong cây con của nút <code>i</code>. Nếu tích này <strong>âm</strong>, đặt <code>0</code> đồng xu.</li>
</ul>

<p>Hãy trả về <em>một mảng </em><code>coin</code><em> có kích thước </em><code>n</code><em> sao cho </em><code>coin[i]</code><em> là số đồng xu được đặt tại nút </em><code>i</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2900-2999/2973.Find%20Number%20of%20Coins%20to%20Place%20in%20Tree%20Nodes/images/screenshot-2023-11-10-012641.png" style="width: 600px; height: 233px;" />
<pre>
<strong>Đầu vào:</strong> edges = [[0,1],[0,2],[0,3],[0,4],[0,5]], cost = [1,2,3,4,5,6]
<strong>Đầu ra:</strong> [120,1,1,1,1,1]
<strong>Giải thích:</strong> Đặt 6 * 5 * 4 = 120 đồng xu cho nút 0. Tất cả các nút còn lại đều là nút lá với cây con có kích thước 1, nên đặt 1 đồng xu cho mỗi nút.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2900-2999/2973.Find%20Number%20of%20Coins%20to%20Place%20in%20Tree%20Nodes/images/screenshot-2023-11-10-012614.png" style="width: 800px; height: 374px;" />
<pre>
<strong>Đầu vào:</strong> edges = [[0,1],[0,2],[1,3],[1,4],[1,5],[2,6],[2,7],[2,8]], cost = [1,4,2,3,5,7,8,-4,2]
<strong>Đầu ra:</strong> [280,140,32,1,1,1,1,1,1]
<strong>Giải thích:</strong> Số đồng xu đặt trên mỗi nút là:
- Đặt 8 * 7 * 5 = 280 đồng xu cho nút 0.
- Đặt 7 * 5 * 4 = 140 đồng xu cho nút 1.
- Đặt 8 * 2 * 2 = 32 đồng xu cho nút 2.
- Tất cả các nút còn lại đều là nút lá với cây con có kích thước 1, nên đặt 1 đồng xu cho mỗi nút.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2900-2999/2973.Find%20Number%20of%20Coins%20to%20Place%20in%20Tree%20Nodes/images/screenshot-2023-11-10-012513.png" style="width: 300px; height: 277px;" />
<pre>
<strong>Đầu vào:</strong> edges = [[0,1],[0,2]], cost = [1,2,-2]
<strong>Đầu ra:</strong> [0,1,1]
<strong>Giải thích:</strong> Nút 1 và 2 là các nút lá với cây con có kích thước 1, nên đặt 1 đồng xu cho mỗi nút. Với nút 0, tích chi phí duy nhất có thể là 2 * 1 * -2 = -4. Do đó, đặt 0 đồng xu cho nút 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
	<li><code>cost.length == n</code></li>
	<li><code>1 &lt;= |cost[i]| &lt;= 10<sup>4</sup></code></li>
	<li>Dữ liệu đầu vào được tạo sao cho <code>edges</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Với cây con có ít hơn ba nút, ta đặt $1$ đồng xu; nếu không, số đồng xu là tích lớn nhất của ba chi phí, hoặc $0$ nếu tích đó âm. Tích lớn nhất hoặc là ba giá trị lớn nhất, hoặc là hai giá trị nhỏ nhất (âm) nhân với giá trị lớn nhất. Một cây con có thể có $n$ nút, nên không thể truyền toàn bộ danh sách lên phía trên.
>
> DFS trả về một danh sách đã sắp xếp, chỉ giữ lại hai giá trị nhỏ nhất và ba giá trị lớn nhất. Sau khi gộp các nút con, ta cắt gọn danh sách rồi tính đáp án cho nút hiện tại.

<!-- thinking:end -->

Theo mô tả bài toán, có hai trường hợp đối với số đồng xu đặt tại mỗi nút $a$:

- Nếu số nút trong cây con tương ứng với nút $a$ nhỏ hơn $3$, đặt $1$ đồng xu;
- Nếu số nút trong cây con tương ứng với nút $a$ lớn hơn hoặc bằng $3$, ta cần chọn $3$ nút khác nhau trong cây con, tính giá trị lớn nhất của tích các chi phí tương ứng, rồi đặt số đồng xu tương ứng tại nút $a$. Nếu tích lớn nhất là số âm, đặt $0$ đồng xu.

Trường hợp đầu tiên khá đơn giản, ta chỉ cần đếm số nút trong cây con của mỗi nút trong quá trình duyệt.

Với trường hợp thứ hai, nếu tất cả chi phí đều dương, ta nên chọn $3$ nút có chi phí lớn nhất; nếu có chi phí âm, ta nên chọn $2$ nút có chi phí nhỏ nhất và $1$ nút có chi phí lớn nhất. Vì vậy, ta cần duy trì $2$ chi phí nhỏ nhất và $3$ chi phí lớn nhất trong mỗi cây con.

Trước tiên, ta xây dựng danh sách kề $g$ dựa trên mảng hai chiều $edges$ đã cho, trong đó $g[a]$ biểu diễn tất cả các nút kề với nút $a$.

Tiếp theo, ta thiết kế hàm $dfs(a, fa)$, trả về một mảng $res$ lưu $2$ chi phí nhỏ nhất và $3$ chi phí lớn nhất trong cây con của nút $a$ (có thể không đủ $5$ phần tử).

Trong hàm $dfs(a, fa)$, ta thêm chi phí $cost[a]$ của nút $a$ vào mảng $res$, sau đó duyệt qua tất cả các nút kề $b$ của nút $a$. Nếu $b$ không phải là nút cha $fa$ của nút $a$, ta thêm kết quả của $dfs(b, a)$ vào mảng $res$.

Sau đó, ta sắp xếp mảng $res$, tính số đồng xu đặt tại nút $a$ dựa trên độ dài $m$ của mảng $res$, rồi cập nhật $ans[a]$:

- Nếu $m \ge 3$, số đồng xu đặt tại nút $a$ là $\max(0, res[m - 1] \times res[m - 2] \times res[m - 3], res[0] \times res[1] \times res[m - 1])$; ngược lại, số đồng xu đặt tại nút $a$ là $1$;
- Nếu $m > 5$, ta chỉ cần giữ lại $2$ phần tử đầu tiên và $3$ phần tử cuối cùng của mảng $res$.

Cuối cùng, ta gọi hàm $dfs(0, -1)$ và trả về mảng đáp án $ans$.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số lượng nút.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def placedCoins(self, edges: List[List[int]], cost: List[int]) -> List[int]:
        def dfs(a: int, fa: int) -> List[int]:
            res = [cost[a]]
            for b in g[a]:
                if b != fa:
                    res.extend(dfs(b, a))
            res.sort()
            if len(res) >= 3:
                ans[a] = max(res[-3] * res[-2] * res[-1], res[0] * res[1] * res[-1], 0)
            if len(res) > 5:
                res = res[:2] + res[-3:]
            return res

        n = len(cost)
        g = [[] for _ in range(n)]
        for a, b in edges:
            g[a].append(b)
            g[b].append(a)
        ans = [1] * n
        dfs(0, -1)
        return ans
```

#### Java

```java
class Solution {
    private int[] cost;
    private List<Integer>[] g;
    private long[] ans;

    public long[] placedCoins(int[][] edges, int[] cost) {
        int n = cost.length;
        this.cost = cost;
        ans = new long[n];
        g = new List[n];
        Arrays.fill(ans, 1);
        Arrays.setAll(g, i -> new ArrayList<>());
        for (int[] e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        dfs(0, -1);
        return ans;
    }

    private List<Integer> dfs(int a, int fa) {
        List<Integer> res = new ArrayList<>();
        res.add(cost[a]);
        for (int b : g[a]) {
            if (b != fa) {
                res.addAll(dfs(b, a));
            }
        }
        Collections.sort(res);
        int m = res.size();
        if (m >= 3) {
            long x = (long) res.get(m - 1) * res.get(m - 2) * res.get(m - 3);
            long y = (long) res.get(0) * res.get(1) * res.get(m - 1);
            ans[a] = Math.max(0, Math.max(x, y));
        }
        if (m >= 5) {
            res = List.of(res.get(0), res.get(1), res.get(m - 3), res.get(m - 2), res.get(m - 1));
        }
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<long long> placedCoins(vector<vector<int>>& edges, vector<int>& cost) {
        int n = cost.size();
        vector<long long> ans(n, 1);
        vector<int> g[n];
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].push_back(b);
            g[b].push_back(a);
        }
        function<vector<int>(int, int)> dfs = [&](int a, int fa) -> vector<int> {
            vector<int> res = {cost[a]};
            for (int b : g[a]) {
                if (b != fa) {
                    auto t = dfs(b, a);
                    res.insert(res.end(), t.begin(), t.end());
                }
            }
            sort(res.begin(), res.end());
            int m = res.size();
            if (m >= 3) {
                long long x = 1LL * res[m - 1] * res[m - 2] * res[m - 3];
                long long y = 1LL * res[0] * res[1] * res[m - 1];
                ans[a] = max({0LL, x, y});
            }
            if (m >= 5) {
                res = {res[0], res[1], res[m - 1], res[m - 2], res[m - 3]};
            }
            return res;
        };
        dfs(0, -1);
        return ans;
    }
};
```

#### Go

```go
func placedCoins(edges [][]int, cost []int) []int64 {
	n := len(cost)
	g := make([][]int, n)
	for _, e := range edges {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	ans := make([]int64, n)
	for i := range ans {
		ans[i] = int64(1)
	}
	var dfs func(a, fa int) []int
	dfs = func(a, fa int) []int {
		res := []int{cost[a]}
		for _, b := range g[a] {
			if b != fa {
				res = append(res, dfs(b, a)...)
			}
		}
		sort.Ints(res)
		m := len(res)
		if m >= 3 {
			x := res[m-1] * res[m-2] * res[m-3]
			y := res[0] * res[1] * res[m-1]
			ans[a] = max(0, int64(x), int64(y))
		}
		if m >= 5 {
			res = append(res[:2], res[m-3:]...)
		}
		return res
	}
	dfs(0, -1)
	return ans
}
```

#### TypeScript

```ts
function placedCoins(edges: number[][], cost: number[]): number[] {
    const n = cost.length;
    const ans: number[] = Array(n).fill(1);
    const g: number[][] = Array.from({ length: n }, () => []);
    for (const [a, b] of edges) {
        g[a].push(b);
        g[b].push(a);
    }
    const dfs = (a: number, fa: number): number[] => {
        const res: number[] = [cost[a]];
        for (const b of g[a]) {
            if (b !== fa) {
                res.push(...dfs(b, a));
            }
        }
        res.sort((a, b) => a - b);
        const m = res.length;
        if (m >= 3) {
            const x = res[m - 1] * res[m - 2] * res[m - 3];
            const y = res[0] * res[1] * res[m - 1];
            ans[a] = Math.max(0, x, y);
        }
        if (m > 5) {
            res.splice(2, m - 5);
        }
        return res;
    };
    dfs(0, -1);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
