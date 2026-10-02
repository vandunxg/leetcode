---
comments: true
difficulty: Medium
rating: 1752
source: Biweekly Contest 5 Q3
tags:
    - Union Find
    - Graph
    - Minimum Spanning Tree
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1135. Connecting Cities With Minimum Cost 🔒](https://leetcode.com/problems/connecting-cities-with-minimum-cost)

[中文文档](/solution/1100-1199/1135.Connecting%20Cities%20With%20Minimum%20Cost/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> thành phố được đánh số từ <code>1</code> đến <code>n</code>. Cho số nguyên <code>n</code> và mảng <code>connections</code>, trong đó <code>connections[i] = [x<sub>i</sub>, y<sub>i</sub>, cost<sub>i</sub>]</code> cho biết chi phí kết nối thành phố <code>x<sub>i</sub></code> với thành phố <code>y<sub>i</sub></code> (kết nối hai chiều) là <code>cost<sub>i</sub></code>.</p>

<p>Trả về <em><strong>chi phí</strong> nhỏ nhất để kết nối tất cả </em><code>n</code><em> thành phố sao cho giữa mỗi cặp thành phố đều có ít nhất một đường đi</em>. Nếu không thể kết nối tất cả <code>n</code> thành phố, hãy trả về <code>-1</code>.</p>

<p><strong>Chi phí</strong> là tổng chi phí của các kết nối được sử dụng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1100-1199/1135.Connecting%20Cities%20With%20Minimum%20Cost/images/1314_ex2.png" style="width: 161px; height: 141px;" />
<pre>
<strong>Đầu vào:</strong> n = 3, connections = [[1,2,5],[1,3,6],[2,3,1]]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Chọn bất kỳ 2 cạnh nào cũng kết nối được tất cả thành phố, vì vậy ta chọn 2 cạnh có chi phí nhỏ nhất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1100-1199/1135.Connecting%20Cities%20With%20Minimum%20Cost/images/1314_ex1.png" style="width: 136px; height: 91px;" />
<pre>
<strong>Đầu vào:</strong> n = 4, connections = [[1,2,3],[3,4,4]]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không có cách nào kết nối tất cả thành phố, kể cả khi sử dụng mọi cạnh.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= connections.length &lt;= 10<sup>4</sup></code></li>
	<li><code>connections[i].length == 3</code></li>
	<li><code>1 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= n</code></li>
	<li><code>x<sub>i</sub> != y<sub>i</sub></code></li>
	<li><code>0 &lt;= cost<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán Kruskal

<!-- thinking:start -->

> **Tư duy**
>
> Cách ít tốn kém nhất để kết nối $n$ thành phố là tìm cây khung nhỏ nhất. Sắp xếp các cạnh theo chi phí, rồi union hai đầu mút chưa thuộc cùng component và cộng chi phí cạnh; khi chỉ còn một component, ta đã có cây khung. Nếu hết cạnh trước đó thì đồ thị không liên thông và đáp án là $-1$.

<!-- thinking:end -->

Thuật toán Kruskal là thuật toán tham lam dùng để tìm cây khung nhỏ nhất.

Ý tưởng cơ bản của thuật toán Kruskal là mỗi lần chọn cạnh có trọng số nhỏ nhất trong tập cạnh. Nếu hai đỉnh được nối bởi cạnh này chưa thuộc cùng một thành phần liên thông, ta thêm cạnh đó vào cây khung nhỏ nhất; nếu không thì bỏ qua cạnh.

Với bài này, ta sắp xếp các cạnh theo chi phí kết nối tăng dần và dùng cấu trúc union-find để quản lý các thành phần liên thông. Mỗi lần xét cạnh nhỏ nhất, nếu hai đỉnh ở hai thành phần khác nhau thì gộp chúng lại và cộng chi phí cạnh. Khi số thành phần liên thông còn $1$, tất cả đỉnh đã được kết nối nên ta trả về tổng chi phí; nếu không, trả về $-1$.

Độ phức tạp thời gian là $O(m \times \log m)$, độ phức tạp không gian là $O(n)$. Trong đó, $m$ và $n$ lần lượt là số cạnh và số đỉnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumCost(self, n: int, connections: List[List[int]]) -> int:
        def find(x):
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        connections.sort(key=lambda x: x[2])
        p = list(range(n))
        ans = 0
        for x, y, cost in connections:
            x, y = x - 1, y - 1
            if find(x) == find(y):
                continue
            p[find(x)] = find(y)
            ans += cost
            n -= 1
            if n == 1:
                return ans
        return -1
```

#### Java

```java
class Solution {
    private int[] p;

    public int minimumCost(int n, int[][] connections) {
        Arrays.sort(connections, Comparator.comparingInt(a -> a[2]));
        p = new int[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
        }
        int ans = 0;
        for (int[] e : connections) {
            int x = e[0] - 1, y = e[1] - 1, cost = e[2];
            if (find(x) == find(y)) {
                continue;
            }
            p[find(x)] = find(y);
            ans += cost;
            if (--n == 1) {
                return ans;
            }
        }
        return -1;
    }

    private int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumCost(int n, vector<vector<int>>& connections) {
        vector<int> p(n);
        iota(p.begin(), p.end(), 0);
        sort(connections.begin(), connections.end(), [](auto& a, auto& b) { return a[2] < b[2]; });
        int ans = 0;
        function<int(int)> find = [&](int x) -> int {
            if (p[x] != x) {
                p[x] = find(p[x]);
            }
            return p[x];
        };
        for (auto& e : connections) {
            int x = e[0] - 1, y = e[1] - 1, cost = e[2];
            if (find(x) == find(y)) {
                continue;
            }
            p[find(x)] = find(y);
            ans += cost;
            if (--n == 1) {
                return ans;
            }
        }
        return -1;
    }
};
```

#### Go

```go
func minimumCost(n int, connections [][]int) (ans int) {
	p := make([]int, n)
	for i := range p {
		p[i] = i
	}
	sort.Slice(connections, func(i, j int) bool { return connections[i][2] < connections[j][2] })
	var find func(int) int
	find = func(x int) int {
		if p[x] != x {
			p[x] = find(p[x])
		}
		return p[x]
	}
	for _, e := range connections {
		x, y, cost := e[0]-1, e[1]-1, e[2]
		if find(x) == find(y) {
			continue
		}
		p[find(x)] = find(y)
		ans += cost
		n--
		if n == 1 {
			return
		}
	}
	return -1
}
```

#### TypeScript

```ts
function minimumCost(n: number, connections: number[][]): number {
    const p: number[] = Array.from({ length: n }, (_, i) => i);
    const find = (x: number): number => {
        if (p[x] !== x) {
            p[x] = find(p[x]);
        }
        return p[x];
    };
    connections.sort((a, b) => a[2] - b[2]);
    let ans = 0;
    for (const [x, y, cost] of connections) {
        if (find(x - 1) === find(y - 1)) {
            continue;
        }
        p[find(x - 1)] = find(y - 1);
        ans += cost;
        if (--n === 1) {
            return ans;
        }
    }
    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
