---
comments: true
difficulty: Medium
rating: 2011
source: Weekly Contest 320 Q3
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Graph
---

<!-- problem:start -->

# [2477. Minimum Fuel Cost to Report to the Capital](https://leetcode.com/problems/minimum-fuel-cost-to-report-to-the-capital)

[中文文档](/solution/2400-2499/2477.Minimum%20Fuel%20Cost%20to%20Report%20to%20the%20Capital/README.md)

## Mô tả

<!-- description:start -->

<p>Một quốc gia có mạng lưới thành phố dạng cây (tức là một đồ thị liên thông, vô hướng và không có chu trình), gồm <code>n</code> thành phố được đánh số từ <code>0</code> đến <code>n - 1</code> và đúng <code>n - 1</code> con đường. Thành phố thủ đô là thành phố <code>0</code>. Bạn được cung cấp một mảng số nguyên 2 chiều <code>roads</code>, trong đó <code>roads[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> biểu thị có một <strong>con đường hai chiều</strong> nối thành phố <code>a<sub>i</sub></code> và thành phố <code>b<sub>i</sub></code>.</p>

<p>Đại diện của mỗi thành phố sẽ tham dự một cuộc họp. Cuộc họp được tổ chức tại thủ đô.</p>

<p>Mỗi thành phố có một chiếc xe. Bạn được cung cấp một số nguyên <code>seats</code> cho biết số chỗ ngồi trong mỗi xe.</p>

<p>Một đại diện có thể dùng xe ở thành phố của mình để di chuyển, hoặc đổi sang xe khác và đi cùng một đại diện khác. Chi phí di chuyển giữa hai thành phố là một lít nhiên liệu.</p>

<p>Hãy trả về <em>số lít nhiên liệu ít nhất cần dùng để đến thủ đô</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2477.Minimum%20Fuel%20Cost%20to%20Report%20to%20the%20Capital/images/a4c380025e3ff0c379525e96a7d63a3.png" style="width: 303px; height: 332px;" />
<pre>
<strong>Đầu vào:</strong> roads = [[0,1],[0,2],[0,3]], seats = 5
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
- Đại diện<sub>1</sub> đi thẳng đến thủ đô với 1 lít nhiên liệu.
- Đại diện<sub>2</sub> đi thẳng đến thủ đô với 1 lít nhiên liệu.
- Đại diện<sub>3</sub> đi thẳng đến thủ đô với 1 lít nhiên liệu.
Lượng nhiên liệu ít nhất cần dùng là 3 lít.
Có thể chứng minh rằng 3 là số lít nhiên liệu ít nhất cần dùng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2477.Minimum%20Fuel%20Cost%20to%20Report%20to%20the%20Capital/images/2.png" style="width: 274px; height: 340px;" />
<pre>
<strong>Đầu vào:</strong> roads = [[3,1],[3,2],[1,0],[0,4],[0,5],[4,6]], seats = 2
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong>
- Đại diện<sub>2</sub> đi thẳng đến thành phố 3 với 1 lít nhiên liệu.
- Đại diện<sub>2</sub> và đại diện<sub>3</sub> đi cùng nhau đến thành phố 1 với 1 lít nhiên liệu.
- Đại diện<sub>2</sub> và đại diện<sub>3</sub> đi cùng nhau đến thủ đô với 1 lít nhiên liệu.
- Đại diện<sub>1</sub> đi thẳng đến thủ đô với 1 lít nhiên liệu.
- Đại diện<sub>5</sub> đi thẳng đến thủ đô với 1 lít nhiên liệu.
- Đại diện<sub>6</sub> đi thẳng đến thành phố 4 với 1 lít nhiên liệu.
- Đại diện<sub>4</sub> và đại diện<sub>6</sub> đi cùng nhau đến thủ đô với 1 lít nhiên liệu.
Lượng nhiên liệu ít nhất cần dùng là 7 lít.
Có thể chứng minh rằng 7 là số lít nhiên liệu ít nhất cần dùng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2477.Minimum%20Fuel%20Cost%20to%20Report%20to%20the%20Capital/images/efcf7f7be6830b8763639cfd01b690a.png" style="width: 108px; height: 86px;" />
<pre>
<strong>Đầu vào:</strong> roads = [], seats = 1
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có đại diện nào cần di chuyển đến thủ đô.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>roads.length == n - 1</code></li>
	<li><code>roads[i].length == 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li><code>roads</code> biểu diễn một cây hợp lệ.</li>
	<li><code>1 &lt;= seats &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + DFS

<!-- thinking:start -->

> **Tư duy**
>
> Mọi người đều đi đến thủ đô và xe chỉ di chuyển về phía root. Với $n\le 10^5$, $sz$ người rời khỏi một node con cần $\lceil sz/seats\rceil$ lít nhiên liệu trên cạnh đó, sau khi đã đi chung xe trong subtree.
>
> DFS từ dưới lên: một node con có kích thước $t$ tốn $\lceil t/seats\rceil$ trên cạnh nối với node cha và cộng $t$ vào kích thước hiện tại. Root không có cạnh đi ra.

<!-- thinking:end -->

Theo mô tả bài toán, ta có thể thấy tất cả xe chỉ di chuyển về phía thủ đô (node $0$).

Giả sử có một node $a$, node tiếp theo là $b$, và node $a$ cần đi qua node $b$ để đến thủ đô. Để số xe (mức tiêu thụ nhiên liệu) của node $a$ là ít nhất, ta nên tham lam cho xe của các node con của node $a$ tập trung về node $a$ trước, sau đó phân chia số người theo số chỗ ngồi $seats$. Số xe ít nhất (mức tiêu thụ nhiên liệu) cần để đến node $b$ là $\lceil \frac{sz}{seats} \rceil$. Trong đó, $sz$ là số node trong subtree có node $a$ làm root.

Ta bắt đầu duyệt DFS từ node $0$, sử dụng biến $sz$ để đếm số node trong subtree có node hiện tại làm root. Ban đầu, $sz = 1$, biểu thị chính node hiện tại. Sau đó, ta duyệt qua tất cả node con của node hiện tại. Với mỗi node con $b$, ta đệ quy tính số node $t$ trong subtree có $b$ làm root, cộng $t$ vào $sz$, rồi cộng $\lceil \frac{t}{seats} \rceil$ vào đáp án. Cuối cùng, trả về $sz$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumFuelCost(self, roads: List[List[int]], seats: int) -> int:
        def dfs(a: int, fa: int) -> int:
            nonlocal ans
            sz = 1
            for b in g[a]:
                if b != fa:
                    t = dfs(b, a)
                    ans += ceil(t / seats)
                    sz += t
            return sz

        g = defaultdict(list)
        for a, b in roads:
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
    private int seats;
    private long ans;

    public long minimumFuelCost(int[][] roads, int seats) {
        int n = roads.length + 1;
        g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        this.seats = seats;
        for (var e : roads) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        dfs(0, -1);
        return ans;
    }

    private int dfs(int a, int fa) {
        int sz = 1;
        for (int b : g[a]) {
            if (b != fa) {
                int t = dfs(b, a);
                ans += (t + seats - 1) / seats;
                sz += t;
            }
        }
        return sz;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minimumFuelCost(vector<vector<int>>& roads, int seats) {
        int n = roads.size() + 1;
        vector<int> g[n];
        for (auto& e : roads) {
            int a = e[0], b = e[1];
            g[a].emplace_back(b);
            g[b].emplace_back(a);
        }
        long long ans = 0;
        function<int(int, int)> dfs = [&](int a, int fa) {
            int sz = 1;
            for (int b : g[a]) {
                if (b != fa) {
                    int t = dfs(b, a);
                    ans += (t + seats - 1) / seats;
                    sz += t;
                }
            }
            return sz;
        };
        dfs(0, -1);
        return ans;
    }
};
```

#### Go

```go
func minimumFuelCost(roads [][]int, seats int) (ans int64) {
	n := len(roads) + 1
	g := make([][]int, n)
	for _, e := range roads {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	var dfs func(int, int) int
	dfs = func(a, fa int) int {
		sz := 1
		for _, b := range g[a] {
			if b != fa {
				t := dfs(b, a)
				ans += int64((t + seats - 1) / seats)
				sz += t
			}
		}
		return sz
	}
	dfs(0, -1)
	return
}
```

#### TypeScript

```ts
function minimumFuelCost(roads: number[][], seats: number): number {
    const n = roads.length + 1;
    const g: number[][] = Array.from({ length: n }, () => []);
    for (const [a, b] of roads) {
        g[a].push(b);
        g[b].push(a);
    }
    let ans = 0;
    const dfs = (a: number, fa: number): number => {
        let sz = 1;
        for (const b of g[a]) {
            if (b !== fa) {
                const t = dfs(b, a);
                ans += Math.ceil(t / seats);
                sz += t;
            }
        }
        return sz;
    };
    dfs(0, -1);
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_fuel_cost(roads: Vec<Vec<i32>>, seats: i32) -> i64 {
        let n = roads.len() + 1;
        let mut g: Vec<Vec<usize>> = vec![vec![]; n];
        for road in roads.iter() {
            let a = road[0] as usize;
            let b = road[1] as usize;
            g[a].push(b);
            g[b].push(a);
        }
        let mut ans = 0;
        fn dfs(a: usize, fa: i32, g: &Vec<Vec<usize>>, ans: &mut i64, seats: i32) -> i32 {
            let mut sz = 1;
            for &b in g[a].iter() {
                if (b as i32) != fa {
                    let t = dfs(b, a as i32, g, ans, seats);
                    *ans += ((t + seats - 1) / seats) as i64;
                    sz += t;
                }
            }
            sz
        }
        dfs(0, -1, &g, &mut ans, seats);
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
