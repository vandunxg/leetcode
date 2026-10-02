---
comments: true
difficulty: Hard
rating: 2084
source: Weekly Contest 154 Q4
tags:
    - Depth-First Search
    - Graph
    - Bridge
    - Biconnected Component
---

<!-- problem:start -->

# [1192. Critical Connections in a Network](https://leetcode.com/problems/critical-connections-in-a-network)

[中文文档](/solution/1100-1199/1192.Critical%20Connections%20in%20a%20Network/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> server được đánh số từ <code>0</code> đến <code>n - 1</code>, kết nối với nhau bằng các <code>connections</code> hai chiều tạo thành một network. Trong đó, <code>connections[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> biểu diễn kết nối giữa hai server <code>a<sub>i</sub></code> và <code>b<sub>i</sub></code>. Mỗi server có thể đi tới các server khác trực tiếp hoặc gián tiếp qua network.</p>

<p><em>Kết nối trọng yếu</em> là kết nối mà nếu bị xóa, sẽ khiến một số server không thể đi tới một số server khác.</p>

<p>Trả về tất cả kết nối trọng yếu trong network theo bất kỳ thứ tự nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1100-1199/1192.Critical%20Connections%20in%20a%20Network/images/1537_ex1_2.png" style="width: 198px; height: 248px;" />
<pre>
<strong>Đầu vào:</strong> n = 4, connections = [[0,1],[1,2],[2,0],[1,3]]
<strong>Đầu ra:</strong> [[1,3]]
<strong>Giải thích:</strong> [[3,1]] cũng được chấp nhận.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, connections = [[0,1]]
<strong>Đầu ra:</strong> [[0,1]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>n - 1 &lt;= connections.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li>Không có kết nối nào bị lặp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán Tarjan

<!-- thinking:start -->

> **Tư duy**
>
> Kết nối trọng yếu chính là bridge. Kiểm tra từng cạnh bằng union-find hoặc BFS sẽ quá chậm khi có nhiều cạnh. DFS của Tarjan lưu thời điểm khám phá $dfn$ và tổ tiên sớm nhất có thể quay lui đến $low$; cạnh cây thỏa $low[v]>dfn[u]$ là bridge. Trong graph vô hướng, bỏ qua node cha để không xem cạnh cây là back edge.

<!-- thinking:end -->

Có thể xem các “critical connections” trong bài này là “bridge”.

“Bridge”: Trong graph vô hướng liên thông, nếu xóa một cạnh khiến graph mất tính liên thông thì cạnh đó được gọi là “bridge”.

Tương tự, còn có khái niệm “articulation point”.

“Articulation point”: Trong graph vô hướng liên thông, nếu xóa một đỉnh cùng tất cả các cạnh nối với nó khiến graph mất tính liên thông thì đỉnh đó được gọi là “articulation point”.

Tarjan là thuật toán tìm “bridge” và “articulation point” trong graph. Thuật toán dùng depth-first search (DFS), đệ quy thăm các node kề rồi xử lý node hiện tại. Bằng cách ghi nhận “thứ tự thăm: DFN” và cập nhật “node có thể quay lui đến sớm nhất: low” khi xử lý node sau khi đệ quy kết thúc, thuật toán có thể tìm “bridge” và “articulation point” trong $O(n)$ thời gian. Thuật toán này cũng có thể tìm các thành phần liên thông mạnh trong graph có hướng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def criticalConnections(
        self, n: int, connections: List[List[int]]
    ) -> List[List[int]]:
        def tarjan(a: int, fa: int):
            nonlocal now
            now += 1
            dfn[a] = low[a] = now
            for b in g[a]:
                if b == fa:
                    continue
                if not dfn[b]:
                    tarjan(b, a)
                    low[a] = min(low[a], low[b])
                    if low[b] > dfn[a]:
                        ans.append([a, b])
                else:
                    low[a] = min(low[a], dfn[b])

        g = [[] for _ in range(n)]
        for a, b in connections:
            g[a].append(b)
            g[b].append(a)

        dfn = [0] * n
        low = [0] * n
        now = 0
        ans = []
        tarjan(0, -1)
        return ans
```

#### Java

```java
class Solution {
    private int now;
    private List<Integer>[] g;
    private List<List<Integer>> ans = new ArrayList<>();
    private int[] dfn;
    private int[] low;

    public List<List<Integer>> criticalConnections(int n, List<List<Integer>> connections) {
        g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        dfn = new int[n];
        low = new int[n];
        for (var e : connections) {
            int a = e.get(0), b = e.get(1);
            g[a].add(b);
            g[b].add(a);
        }
        tarjan(0, -1);
        return ans;
    }

    private void tarjan(int a, int fa) {
        dfn[a] = low[a] = ++now;
        for (int b : g[a]) {
            if (b == fa) {
                continue;
            }
            if (dfn[b] == 0) {
                tarjan(b, a);
                low[a] = Math.min(low[a], low[b]);
                if (low[b] > dfn[a]) {
                    ans.add(List.of(a, b));
                }
            } else {
                low[a] = Math.min(low[a], dfn[b]);
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> criticalConnections(int n, vector<vector<int>>& connections) {
        int now = 0;
        vector<int> dfn(n);
        vector<int> low(n);
        vector<int> g[n];
        for (auto& e : connections) {
            int a = e[0], b = e[1];
            g[a].push_back(b);
            g[b].push_back(a);
        }
        vector<vector<int>> ans;
        function<void(int, int)> tarjan = [&](int a, int fa) -> void {
            dfn[a] = low[a] = ++now;
            for (int b : g[a]) {
                if (b == fa) {
                    continue;
                }
                if (!dfn[b]) {
                    tarjan(b, a);
                    low[a] = min(low[a], low[b]);
                    if (low[b] > dfn[a]) {
                        ans.push_back({a, b});
                    }
                } else {
                    low[a] = min(low[a], dfn[b]);
                }
            }
        };
        tarjan(0, -1);
        return ans;
    }
};
```

#### Go

```go
func criticalConnections(n int, connections [][]int) (ans [][]int) {
	now := 0
	g := make([][]int, n)
	dfn := make([]int, n)
	low := make([]int, n)
	for _, e := range connections {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	var tarjan func(int, int)
	tarjan = func(a, fa int) {
		now++
		dfn[a], low[a] = now, now
		for _, b := range g[a] {
			if b == fa {
				continue
			}
			if dfn[b] == 0 {
				tarjan(b, a)
				low[a] = min(low[a], low[b])
				if low[b] > dfn[a] {
					ans = append(ans, []int{a, b})
				}
			} else {
				low[a] = min(low[a], dfn[b])
			}
		}
	}
	tarjan(0, -1)
	return
}
```

#### TypeScript

```ts
function criticalConnections(n: number, connections: number[][]): number[][] {
    let now: number = 0;
    const g: number[][] = Array(n)
        .fill(0)
        .map(() => []);
    const dfn: number[] = Array(n).fill(0);
    const low: number[] = Array(n).fill(0);
    for (const [a, b] of connections) {
        g[a].push(b);
        g[b].push(a);
    }
    const ans: number[][] = [];
    const tarjan = (a: number, fa: number) => {
        dfn[a] = low[a] = ++now;
        for (const b of g[a]) {
            if (b === fa) {
                continue;
            }
            if (!dfn[b]) {
                tarjan(b, a);
                low[a] = Math.min(low[a], low[b]);
                if (low[b] > dfn[a]) {
                    ans.push([a, b]);
                }
            } else {
                low[a] = Math.min(low[a], dfn[b]);
            }
        }
    };
    tarjan(0, -1);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
