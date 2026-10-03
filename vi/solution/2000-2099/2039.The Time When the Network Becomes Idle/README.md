---
comments: true
difficulty: Medium
rating: 1865
source: Biweekly Contest 63 Q3
tags:
    - Breadth-First Search
    - Graph
    - Array
---

<!-- problem:start -->

# [2039. The Time When the Network Becomes Idle](https://leetcode.com/problems/the-time-when-the-network-becomes-idle)

[中文文档](/solution/2000-2099/2039.The%20Time%20When%20the%20Network%20Becomes%20Idle/README.md)

## Mô tả

<!-- description:start -->

<p>Có một mạng gồm <code>n</code> server, được đánh số từ <code>0</code> đến <code>n - 1</code>. Cho một mảng số nguyên 2 chiều <code>edges</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> cho biết có một kênh truyền message giữa các server <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code>, và chúng có thể truyền trực tiếp <strong>cho nhau</strong> <strong>bất kỳ</strong> số lượng message nào trong <strong>một</strong> giây. Ngoài ra, cho một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>patience</code> có độ dài <code>n</code>.</p>

<p>Tất cả server đều <strong>được kết nối</strong>, nghĩa là một message có thể được truyền từ server này đến bất kỳ server nào khác, trực tiếp hoặc gián tiếp qua các kênh truyền message.</p>

<p>Server có nhãn <code>0</code> là server <strong>master</strong>. Các server còn lại là server <strong>data</strong>. Mỗi data server cần gửi message của mình đến master server để xử lý và chờ phản hồi. Các message di chuyển giữa các server theo cách <strong>tối ưu</strong>, vì vậy mỗi message đều mất thời gian <strong>ít nhất</strong> để đến master server. Master server sẽ xử lý <strong>ngay lập tức</strong> tất cả message mới đến và gửi phản hồi về server ban đầu qua <strong>đường đi ngược lại</strong> với đường đi mà message đã sử dụng.</p>

<p>Ở đầu giây <code>0</code>, mỗi data server gửi message của mình đi để xử lý. Bắt đầu từ giây <code>1</code>, ở <strong>đầu</strong> của <strong>mỗi</strong> giây, mỗi data server sẽ kiểm tra xem mình đã nhận được phản hồi cho message đã gửi hay chưa (bao gồm cả các phản hồi vừa đến) từ master server:</p>

<ul>
	<li>Nếu chưa nhận được, server sẽ định kỳ <strong>gửi lại</strong> message. Data server <code>i</code> sẽ gửi lại message sau mỗi <code>patience[i]</code> giây, nghĩa là data server <code>i</code> sẽ gửi lại message nếu đã <strong>trôi qua</strong> <code>patience[i]</code> giây kể từ <strong>lần gần nhất</strong> message được gửi từ server này.</li>
	<li>Nếu đã nhận được, server sẽ <strong>không gửi lại message nữa</strong>.</li>
</ul>

<p>Mạng trở nên <strong>nhàn</strong> khi <strong>không còn</strong> message nào đang được truyền giữa các server hoặc đang đến các server.</p>

<p>Trả về <em><strong>giây sớm nhất</strong> kể từ đó mạng trở nên <strong>nhàn</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="example 1" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2039.The%20Time%20When%20the%20Network%20Becomes%20Idle/images/quiet-place-example1.png" style="width: 750px; height: 384px;" />
<pre>
<strong>Đầu vào:</strong> edges = [[0,1],[1,2]], patience = [0,2,1]
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong>
Ở (đầu) giây 0,
- Data server 1 gửi message của mình (ký hiệu là 1A) đến master server.
- Data server 2 gửi message của mình (ký hiệu là 2A) đến master server.

Ở giây 1,

- Message 1A đến master server. Master server xử lý ngay lập tức message 1A và gửi phản hồi 1A trở lại.
- Server 1 chưa nhận được phản hồi nào. Đã trôi qua 1 giây (1 &lt; patience[1] = 2) kể từ khi server này gửi message, vì vậy server không gửi lại message.
- Server 2 chưa nhận được phản hồi nào. Đã trôi qua 1 giây (1 == patience[2] = 1) kể từ khi server này gửi message, vì vậy server gửi lại message (ký hiệu là 2B).

Ở giây 2,

- Phản hồi 1A đến server 1. Server 1 sẽ không gửi lại message nữa.
- Message 2A đến master server. Master server xử lý ngay lập tức message 2A và gửi phản hồi 2A trở lại.
- Server 2 gửi lại message (ký hiệu là 2C).
  ...
  Ở giây 4,
- Phản hồi 2A đến server 2. Server 2 sẽ không gửi lại message nữa.
  ...
  Ở giây 7, phản hồi 2D đến server 2.

Bắt đầu từ đầu giây 8, không còn message nào đang được truyền giữa các server hoặc đang đến các server.
Đây là thời điểm mạng trở nên nhàn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="example 2" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2039.The%20Time%20When%20the%20Network%20Becomes%20Idle/images/network_a_quiet_place_2.png" style="width: 100px; height: 85px;" />
<pre>
<strong>Đầu vào:</strong> edges = [[0,1],[0,2],[1,2]], patience = [0,10,10]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Data server 1 và 2 nhận được phản hồi ở đầu giây 2.
Mạng trở nên nhàn từ đầu giây 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == patience.length</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>patience[0] == 0</code></li>
	<li><code>1 &lt;= patience[i] &lt;= 10<sup>5</sup></code> với <code>1 &lt;= i &lt; n</code></li>
	<li><code>1 &lt;= edges.length &lt;= min(10<sup>5</sup>, n * (n - 1) / 2)</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt; n</code></li>
	<li><code>u<sub>i</sub> != v<sub>i</sub></code></li>
	<li>Không có cạnh trùng lặp.</li>
	<li>Mỗi server có thể đi đến mọi server khác trực tiếp hoặc gián tiếp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Các cạnh có trọng số bằng nhau, nên độ trễ chính là khoảng cách đường đi ngắn nhất. BFS từ $0$ cho ta $d$; thời gian khứ hồi là $2d$. Các server gửi lại message sau mỗi $patience$ cho đến khi nhận được phản hồi.
>
> Lần gửi cuối cùng là $(2d-1)//patience \times patience$, cộng thêm thời gian phản hồi và một giây xử lý. Lấy giá trị lớn nhất trên tất cả node.
>
> Đồ thị vô hướng và liên thông, nên chỉ cần một lần BFS.

<!-- thinking:end -->

Trước tiên, ta xây dựng một đồ thị vô hướng $g$ dựa trên mảng 2 chiều $edges$, trong đó $g[u]$ biểu diễn tất cả node kề với node $u$.

Sau đó, ta có thể dùng tìm kiếm theo chiều rộng (BFS) để tìm khoảng cách ngắn nhất $d_i$ từ mỗi node $i$ đến master server. Thời điểm sớm nhất mà node $i$ có thể nhận được phản hồi sau khi gửi message là $2 \times d_i$. Vì mỗi data server $i$ gửi lại message sau mỗi $patience[i]$ giây, thời điểm cuối cùng mỗi data server gửi message là $(2 \times d_i - 1) / patience[i] \times patience[i]$. Do đó, thời điểm muộn nhất mạng trở nên nhàn là $(2 \times d_i - 1) / patience[i] \times patience[i] + 2 \times d_i$, cộng thêm 1 giây để xử lý. Ta tìm thời điểm lớn nhất trong các thời điểm này; đó là thời điểm sớm nhất mạng trở nên nhàn.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def networkBecomesIdle(self, edges: List[List[int]], patience: List[int]) -> int:
        g = defaultdict(list)
        for u, v in edges:
            g[u].append(v)
            g[v].append(u)
        q = deque([0])
        vis = {0}
        ans = d = 0
        while q:
            d += 1
            t = d * 2
            for _ in range(len(q)):
                u = q.popleft()
                for v in g[u]:
                    if v not in vis:
                        vis.add(v)
                        q.append(v)
                        ans = max(ans, (t - 1) // patience[v] * patience[v] + t + 1)
        return ans
```

#### Java

```java
class Solution {
    public int networkBecomesIdle(int[][] edges, int[] patience) {
        int n = patience.length;
        List<Integer>[] g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int[] e : edges) {
            int u = e[0], v = e[1];
            g[u].add(v);
            g[v].add(u);
        }
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(0);
        boolean[] vis = new boolean[n];
        vis[0] = true;
        int ans = 0, d = 0;
        while (!q.isEmpty()) {
            ++d;
            int t = d * 2;
            for (int i = q.size(); i > 0; --i) {
                int u = q.poll();
                for (int v : g[u]) {
                    if (!vis[v]) {
                        vis[v] = true;
                        q.offer(v);
                        ans = Math.max(ans, (t - 1) / patience[v] * patience[v] + t + 1);
                    }
                }
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int networkBecomesIdle(vector<vector<int>>& edges, vector<int>& patience) {
        int n = patience.size();
        vector<int> g[n];
        for (auto& e : edges) {
            int u = e[0], v = e[1];
            g[u].push_back(v);
            g[v].push_back(u);
        }
        queue<int> q{{0}};
        bool vis[n];
        memset(vis, false, sizeof(vis));
        vis[0] = true;
        int ans = 0, d = 0;
        while (!q.empty()) {
            ++d;
            int t = d * 2;
            for (int i = q.size(); i; --i) {
                int u = q.front();
                q.pop();
                for (int v : g[u]) {
                    if (!vis[v]) {
                        vis[v] = true;
                        q.push(v);
                        ans = max(ans, (t - 1) / patience[v] * patience[v] + t + 1);
                    }
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func networkBecomesIdle(edges [][]int, patience []int) (ans int) {
	n := len(patience)
	g := make([][]int, n)
	for _, e := range edges {
		u, v := e[0], e[1]
		g[u] = append(g[u], v)
		g[v] = append(g[v], u)
	}
	q := []int{0}
	vis := make([]bool, n)
	vis[0] = true
	for d := 1; len(q) > 0; d++ {
		t := d * 2
		for i := len(q); i > 0; i-- {
			u := q[0]
			q = q[1:]
			for _, v := range g[u] {
				if !vis[v] {
					vis[v] = true
					q = append(q, v)
					ans = max(ans, (t-1)/patience[v]*patience[v]+t+1)
				}
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function networkBecomesIdle(edges: number[][], patience: number[]): number {
    const n = patience.length;
    const g: number[][] = Array.from({ length: n }, () => []);
    for (const [u, v] of edges) {
        g[u].push(v);
        g[v].push(u);
    }
    const vis: boolean[] = Array.from({ length: n }, () => false);
    vis[0] = true;
    let q: number[] = [0];
    let ans = 0;
    for (let d = 1; q.length > 0; ++d) {
        const t = d * 2;
        const nq: number[] = [];
        for (const u of q) {
            for (const v of g[u]) {
                if (!vis[v]) {
                    vis[v] = true;
                    nq.push(v);
                    ans = Math.max(ans, (((t - 1) / patience[v]) | 0) * patience[v] + t + 1);
                }
            }
        }
        q = nq;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
