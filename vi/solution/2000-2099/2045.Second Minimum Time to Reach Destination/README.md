---
comments: true
difficulty: Hard
rating: 2201
source: Weekly Contest 263 Q4
tags:
    - Breadth-First Search
    - Graph
    - k-Shortest Paths
    - Shortest Path
    - Dijkstra
---

<!-- problem:start -->

# [2045. Second Minimum Time to Reach Destination](https://leetcode.com/problems/second-minimum-time-to-reach-destination)

[中文文档](/solution/2000-2099/2045.Second%20Minimum%20Time%20to%20Reach%20Destination/README.md)

## Mô tả

<!-- description:start -->

<p>Một thành phố được biểu diễn bằng một đồ thị <strong>liên thông hai chiều</strong> gồm <code>n</code> đỉnh, trong đó mỗi đỉnh được đánh số từ <code>1</code> đến <code>n</code> (<strong>bao gồm cả hai đầu</strong>). Các cạnh trong đồ thị được biểu diễn bằng một mảng số nguyên 2 chiều <code>edges</code>, trong đó mỗi <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> biểu thị một cạnh hai chiều giữa đỉnh <code>u<sub>i</sub></code> và đỉnh <code>v<sub>i</sub></code>. Mỗi cặp đỉnh có <strong>nhiều nhất một</strong> cạnh nối với nhau và không đỉnh nào có cạnh nối với chính nó. Thời gian đi qua mọi cạnh là <code>time</code> phút.</p>

<p>Mỗi đỉnh có một đèn giao thông đổi màu từ <strong>xanh</strong> sang <strong>đỏ</strong> và ngược lại sau mỗi <code>change</code> phút. Tất cả đèn đều đổi màu <strong>cùng lúc</strong>. Bạn có thể đi vào một đỉnh tại <strong>bất kỳ thời điểm nào</strong>, nhưng chỉ có thể rời khỏi đỉnh <strong>khi đèn đang xanh</strong>. Bạn <strong>không thể chờ</strong> tại một đỉnh nếu đèn đang <strong>xanh</strong>.</p>

<p><strong>Giá trị nhỏ thứ hai</strong> được định nghĩa là giá trị nhỏ nhất <strong>lớn hơn nghiêm ngặt</strong> giá trị nhỏ nhất.</p>

<ul>
	<li>Ví dụ, giá trị nhỏ thứ hai của <code>[2, 3, 4]</code> là <code>3</code>, còn giá trị nhỏ thứ hai của <code>[2, 2, 4]</code> là <code>4</code>.</li>
</ul>

<p>Cho <code>n</code>, <code>edges</code>, <code>time</code> và <code>change</code>, hãy trả về <em><strong>thời gian nhỏ thứ hai</strong> cần thiết để đi từ đỉnh </em><code>1</code><em> đến đỉnh </em><code>n</code>.</p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li>Bạn có thể đi qua bất kỳ đỉnh nào <strong>bao nhiêu lần cũng được</strong>, <strong>bao gồm cả</strong> <code>1</code> và <code>n</code>.</li>
	<li>Có thể giả sử rằng khi hành trình <strong>bắt đầu</strong>, tất cả đèn đều vừa chuyển sang màu <strong>xanh</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2045.Second%20Minimum%20Time%20to%20Reach%20Destination/images/e1.png" style="width: 200px; height: 250px;" /> &emsp; &emsp; &emsp; &emsp;<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2045.Second%20Minimum%20Time%20to%20Reach%20Destination/images/e2.png" style="width: 200px; height: 250px;" />
<pre>
<strong>Đầu vào:</strong> n = 5, edges = [[1,2],[1,3],[1,4],[3,4],[4,5]], time = 3, change = 5
<strong>Đầu ra:</strong> 13
<strong>Giải thích:</strong>
Hình bên trái minh họa đồ thị đã cho.
Đường đi màu xanh trong hình bên phải là đường đi có thời gian nhỏ nhất.
Thời gian cần thiết là:
- Bắt đầu tại 1, thời gian đã trôi qua = 0
- 1 -&gt; 4: 3 phút, thời gian đã trôi qua = 3
- 4 -&gt; 5: 3 phút, thời gian đã trôi qua = 6
Vì vậy, thời gian nhỏ nhất cần thiết là 6 phút.

Đường đi màu đỏ biểu thị đường đi có thời gian nhỏ thứ hai.

- Bắt đầu tại 1, thời gian đã trôi qua = 0
- 1 -&gt; 3: 3 phút, thời gian đã trôi qua = 3
- 3 -&gt; 4: 3 phút, thời gian đã trôi qua = 6
- Chờ tại 4 trong 4 phút, thời gian đã trôi qua = 10
- 4 -&gt; 5: 3 phút, thời gian đã trôi qua = 13
Vì vậy, thời gian nhỏ thứ hai là 13 phút.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2045.Second%20Minimum%20Time%20to%20Reach%20Destination/images/eg2.png" style="width: 225px; height: 50px;" />
<pre>
<strong>Đầu vào:</strong> n = 2, edges = [[1,2]], time = 3, change = 2
<strong>Đầu ra:</strong> 11
<strong>Giải thích:</strong>
Đường đi có thời gian nhỏ nhất là 1 -&gt; 2 với time = 3 phút.
Đường đi có thời gian nhỏ thứ hai là 1 -&gt; 2 -&gt; 1 -&gt; 2 với time = 11 phút.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>n - 1 &lt;= edges.length &lt;= min(2 * 10<sup>4</sup>, n * (n - 1) / 2)</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>1 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n</code></li>
	<li><code>u<sub>i</sub> != v<sub>i</sub></code></li>
	<li>Không có cạnh trùng lặp.</li>
	<li>Mỗi đỉnh có thể được đi đến trực tiếp hoặc gián tiếp từ mọi đỉnh khác.</li>
	<li><code>1 &lt;= time, change &lt;= 10<sup>3</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các cạnh có cùng trọng số, nhưng đèn giao thông khiến việc chờ phụ thuộc vào số lần đi qua cạnh, đồng thời ta cần thời điểm đến đích ngắn thứ hai theo nghĩa nghiêm ngặt. Nếu chỉ lưu khoảng cách nhỏ nhất thì sẽ làm mất đường đi này.
>
> Lưu số lần đi qua cạnh ít nhất và ít thứ hai của mỗi node. BFS cập nhật số nhỏ nhất khi tìm được giá trị nhỏ hơn, và cập nhật số thứ hai khi độ dài mới lớn hơn giá trị nhỏ nhất nhưng nhỏ hơn giá trị thứ hai hiện tại. Sau khi biết số lần đi qua cạnh để đến $n$, mô phỏng thời gian chờ khi đèn đỏ bằng $time$ và $change$.
>
> Các state trong queue là (node, số cạnh), nhờ đó đường đi thứ hai được giữ lại.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def secondMinimum(
        self, n: int, edges: List[List[int]], time: int, change: int
    ) -> int:
        g = defaultdict(set)
        for u, v in edges:
            g[u].add(v)
            g[v].add(u)
        q = deque([(1, 0)])
        dist = [[inf] * 2 for _ in range(n + 1)]
        dist[1][1] = 0
        while q:
            u, d = q.popleft()
            for v in g[u]:
                if d + 1 < dist[v][0]:
                    dist[v][0] = d + 1
                    q.append((v, d + 1))
                elif dist[v][0] < d + 1 < dist[v][1]:
                    dist[v][1] = d + 1
                    if v == n:
                        break
                    q.append((v, d + 1))
        ans = 0
        for i in range(dist[n][1]):
            ans += time
            if i < dist[n][1] - 1 and (ans // change) % 2 == 1:
                ans = (ans + change) // change * change
        return ans
```

#### Java

```java
class Solution {
    public int secondMinimum(int n, int[][] edges, int time, int change) {
        List<Integer>[] g = new List[n + 1];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int[] e : edges) {
            int u = e[0], v = e[1];
            g[u].add(v);
            g[v].add(u);
        }
        Deque<int[]> q = new LinkedList<>();
        q.offerLast(new int[] {1, 0});
        int[][] dist = new int[n + 1][2];
        for (int i = 0; i < n + 1; ++i) {
            Arrays.fill(dist[i], Integer.MAX_VALUE);
        }
        dist[1][1] = 0;
        while (!q.isEmpty()) {
            int[] e = q.pollFirst();
            int u = e[0], d = e[1];
            for (int v : g[u]) {
                if (d + 1 < dist[v][0]) {
                    dist[v][0] = d + 1;
                    q.offerLast(new int[] {v, d + 1});
                } else if (dist[v][0] < d + 1 && d + 1 < dist[v][1]) {
                    dist[v][1] = d + 1;
                    if (v == n) {
                        break;
                    }
                    q.offerLast(new int[] {v, d + 1});
                }
            }
        }
        int ans = 0;
        for (int i = 0; i < dist[n][1]; ++i) {
            ans += time;
            if (i < dist[n][1] - 1 && (ans / change) % 2 == 1) {
                ans = (ans + change) / change * change;
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
