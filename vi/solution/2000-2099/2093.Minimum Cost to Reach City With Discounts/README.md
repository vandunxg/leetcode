---
comments: true
difficulty: Medium
tags:
    - Graph
    - Shortest Path
    - Dijkstra
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2093. Minimum Cost to Reach City With Discounts 🔒](https://leetcode.com/problems/minimum-cost-to-reach-city-with-discounts)

[中文文档](/solution/2000-2099/2093.Minimum%20Cost%20to%20Reach%20City%20With%20Discounts/README.md)

## Mô tả

<!-- description:start -->

<p>Một loạt đường cao tốc nối <code>n</code> thành phố được đánh số từ <code>0</code> đến <code>n - 1</code>. Bạn được cho một mảng số nguyên 2 chiều <code>highways</code>, trong đó <code>highways[i] = [city1<sub>i</sub>, city2<sub>i</sub>, toll<sub>i</sub>]</code> cho biết có một đường cao tốc nối <code>city1<sub>i</sub></code> và <code>city2<sub>i</sub></code>, cho phép xe đi từ <code>city1<sub>i</sub></code> đến <code>city2<sub>i</sub></code> <strong>và ngược lại</strong> với chi phí <code>toll<sub>i</sub></code>.</p>

<p>Bạn cũng được cho một số nguyên <code>discounts</code>, biểu thị số lượng phiếu giảm giá bạn có. Bạn có thể dùng một phiếu giảm giá để đi qua đường cao tốc thứ <code>i<sup>th</sup></code> với chi phí <code>toll<sub>i</sub> / 2</code> (<strong>phép chia</strong> <strong>nguyên</strong>). Mỗi phiếu giảm giá chỉ được dùng <strong>một lần</strong>, và bạn chỉ có thể dùng tối đa <strong>một</strong> phiếu giảm giá cho mỗi đường cao tốc.</p>

<p>Hãy trả về <em><strong>tổng chi phí nhỏ nhất</strong> để đi từ thành phố </em><code>0</code><em> đến thành phố </em><code>n - 1</code><em>, hoặc </em><code>-1</code><em> nếu không thể đi từ thành phố </em><code>0</code><em> đến thành phố </em><code>n - 1</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong><br />
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2093.Minimum%20Cost%20to%20Reach%20City%20With%20Discounts/images/image-20211129222429-1.png" style="height: 250px; width: 404px;" /></p>

<pre>
<strong>Đầu vào:</strong> n = 5, highways = [[0,1,4],[2,1,3],[1,4,11],[3,2,3],[3,4,2]], discounts = 1
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong>
Đi từ 0 đến 1 với chi phí 4.
Đi từ 1 đến 4 và dùng một phiếu giảm giá, chi phí là 11 / 2 = 5.
Chi phí nhỏ nhất để đi từ 0 đến 4 là 4 + 5 = 9.
</pre>

<p><strong class="example">Ví dụ 2:</strong><br />
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2093.Minimum%20Cost%20to%20Reach%20City%20With%20Discounts/images/image-20211129222650-4.png" style="width: 284px; height: 250px;" /></p>

<pre>
<strong>Đầu vào:</strong> n = 4, highways = [[1,3,17],[1,2,7],[3,2,5],[0,1,6],[3,0,20]], discounts = 20
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong>
Đi từ 0 đến 1 và dùng một phiếu giảm giá, chi phí là 6 / 2 = 3.
Đi từ 1 đến 2 và dùng một phiếu giảm giá, chi phí là 7 / 2 = 3.
Đi từ 2 đến 3 và dùng một phiếu giảm giá, chi phí là 5 / 2 = 2.
Chi phí nhỏ nhất để đi từ 0 đến 3 là 3 + 3 + 2 = 8.
</pre>

<p><strong class="example">Ví dụ 3:</strong><br />
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2093.Minimum%20Cost%20to%20Reach%20City%20With%20Discounts/images/image-20211129222531-3.png" style="width: 275px; height: 250px;" /></p>

<pre>
<strong>Đầu vào:</strong> n = 4, highways = [[0,1,3],[2,3,2]], discounts = 0
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong>
Không thể đi từ 0 đến 3, nên trả về -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 1000</code></li>
	<li><code>1 &lt;= highways.length &lt;= 1000</code></li>
	<li><code>highways[i].length == 3</code></li>
	<li><code>0 &lt;= city1<sub>i</sub>, city2<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>city1<sub>i</sub> != city2<sub>i</sub></code></li>
	<li><code>0 &lt;= toll<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= discounts &lt;= 500</code></li>
	<li>Không có đường cao tốc trùng lặp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đồ thị vô hướng có trọng số cho phép sử dụng tối đa $discounts$ lần giảm giá. Trạng thái là (đỉnh, số lần đã dùng giảm giá); nếu bỏ tọa độ thứ hai thì không thể bảo toàn tính tối ưu. Các trọng số không âm cho phép dùng heap để lấy trạng thái theo chi phí.
>
> Thực hiện relaxation theo hai lựa chọn: đi với giá đầy đủ và giữ nguyên $k$, hoặc đi với nửa giá và tăng $k+1$. Lần đầu đích được lấy ra khỏi heap chính là đáp án tối ưu. `dist[i][k]` ngăn việc quay lại những trạng thái kém hơn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumCost(self, n: int, highways: List[List[int]], discounts: int) -> int:
        g = defaultdict(list)
        for a, b, c in highways:
            g[a].append((b, c))
            g[b].append((a, c))
        q = [(0, 0, 0)]
        dist = [[inf] * (discounts + 1) for _ in range(n)]
        while q:
            cost, i, k = heappop(q)
            if k > discounts:
                continue
            if i == n - 1:
                return cost
            if dist[i][k] > cost:
                dist[i][k] = cost
                for j, v in g[i]:
                    heappush(q, (cost + v, j, k))
                    heappush(q, (cost + v // 2, j, k + 1))
        return -1
```

#### Java

```java
class Solution {
    public int minimumCost(int n, int[][] highways, int discounts) {
        List<int[]>[] g = new List[n];
        for (int i = 0; i < n; ++i) {
            g[i] = new ArrayList<>();
        }
        for (var e : highways) {
            int a = e[0], b = e[1], c = e[2];
            g[a].add(new int[] {b, c});
            g[b].add(new int[] {a, c});
        }
        PriorityQueue<int[]> q = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        q.offer(new int[] {0, 0, 0});
        int[][] dist = new int[n][discounts + 1];
        for (var e : dist) {
            Arrays.fill(e, Integer.MAX_VALUE);
        }
        while (!q.isEmpty()) {
            var p = q.poll();
            int cost = p[0], i = p[1], k = p[2];
            if (k > discounts || dist[i][k] <= cost) {
                continue;
            }
            if (i == n - 1) {
                return cost;
            }
            dist[i][k] = cost;
            for (int[] nxt : g[i]) {
                int j = nxt[0], v = nxt[1];
                q.offer(new int[] {cost + v, j, k});
                q.offer(new int[] {cost + v / 2, j, k + 1});
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumCost(int n, vector<vector<int>>& highways, int discounts) {
        vector<vector<pair<int, int>>> g(n);
        for (auto& e : highways) {
            int a = e[0], b = e[1], c = e[2];
            g[a].push_back({b, c});
            g[b].push_back({a, c});
        }
        priority_queue<tuple<int, int, int>, vector<tuple<int, int, int>>, greater<tuple<int, int, int>>> q;
        q.push({0, 0, 0});
        vector<vector<int>> dist(n, vector<int>(discounts + 1, INT_MAX));
        while (!q.empty()) {
            auto [cost, i, k] = q.top();
            q.pop();
            if (k > discounts || dist[i][k] <= cost) continue;
            if (i == n - 1) return cost;
            dist[i][k] = cost;
            for (auto [j, v] : g[i]) {
                q.push({cost + v, j, k});
                q.push({cost + v / 2, j, k + 1});
            }
        }
        return -1;
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
