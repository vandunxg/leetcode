---
comments: true
difficulty: Hard
rating: 2711
source: Weekly Contest 338 Q4
tags:
    - Tree
    - Graph
    - Topological Sort
    - Array
---

<!-- problem:start -->

# [2603. Collect Coins in a Tree](https://leetcode.com/problems/collect-coins-in-a-tree)

[中文文档](/solution/2600-2699/2603.Collect%20Coins%20in%20a%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Có một cây vô hướng, không gốc gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code>. Cho một số nguyên <code>n</code> và một mảng số nguyên 2 chiều edges có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> cho biết có một cạnh nối hai node <code>a<sub>i</sub></code> và <code>b<sub>i</sub></code> trong cây. Bạn cũng được cho một mảng <code>coins</code> có kích thước <code>n</code>, trong đó <code>coins[i]</code> có thể là <code>0</code> hoặc <code>1</code>; <code>1</code> biểu thị node <code>i</code> có một coin.</p>

<p>Ban đầu, bạn chọn bắt đầu tại bất kỳ node nào trong cây. Sau đó, bạn có thể thực hiện các thao tác sau bất kỳ số lần nào:</p>

<ul>
	<li>Thu thập tất cả coin cách node hiện tại không quá <code>2</code>, hoặc</li>
	<li>Di chuyển đến một node kề trong cây.</li>
</ul>

<p>Hãy tìm <em>số cạnh nhỏ nhất cần đi qua để thu thập tất cả coin và quay lại node ban đầu</em>.</p>

<p>Lưu ý rằng nếu đi qua một cạnh nhiều lần, mỗi lần đều được tính vào đáp án.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2603.Collect%20Coins%20in%20a%20Tree/images/graph-2.png" style="width: 522px; height: 522px;" />
<pre>
<strong>Đầu vào:</strong> coins = [1,0,0,0,0,1], edges = [[0,1],[1,2],[2,3],[3,4],[4,5]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Bắt đầu tại node 2, thu thập coin ở node 0, di chuyển đến node 3, thu thập coin ở node 5 rồi quay lại node 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2603.Collect%20Coins%20in%20a%20Tree/images/graph-4.png" style="width: 522px; height: 522px;" />
<pre>
<strong>Đầu vào:</strong> coins = [0,0,0,1,1,0,0,1], edges = [[0,1],[0,2],[1,3],[1,4],[2,5],[5,6],[5,7]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Bắt đầu tại node 0, thu thập các coin ở node 4 và 3, di chuyển đến node 2, thu thập coin ở node 7, rồi quay lại node 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == coins.length</code></li>
	<li><code>1 &lt;= n &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= coins[i] &lt;= 1</code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li><code>edges</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp topo

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi thu thập mọi coin, chúng ta phải quay lại điểm xuất phát, nên mỗi cạnh được đi qua nhiều nhất hai lần. Một chuyến đi Euler đầy đủ trên cây sẽ tính cả những nhánh không có coin và những lá vốn đã nằm trong khoảng cách $2$ so với một coin. Với $n \le 10^5$, chúng ta cần một cách tỉa cây tuyến tính.
>
> Các node lá không có coin không bao giờ cần thiết, nên ta xóa chúng cho đến khi mọi lá còn lại đều có coin. Vì có thể thu thập coin trong phạm vi khoảng cách $2$, ta có thể bỏ thêm hai lớp lá; phần còn lại là skeleton mà ta phải đi qua theo cả hai chiều.
>
> Một queue tỉa các node lá theo cách giống topological sort: đầu tiên là các lá không có coin, sau đó là hai lượt các lá hiện tại. Đáp án bằng hai lần số cạnh mà cả hai đầu mút vẫn còn nằm trên skeleton đó.

<!-- thinking:end -->

Trước tiên, chúng ta chuyển các cạnh trong $edges$ thành adjacency list $g$, trong đó $g[i]$ biểu diễn tất cả node kề với node $i$, được biểu diễn bằng một set.

Sau đó, chúng ta duyệt qua tất cả node, tìm những node có $coins[i]=0$ và $g[i]$ chỉ có một node (tức là node lá có coin bằng $0$), rồi thêm chúng vào queue $q$.

Tiếp theo, chúng ta liên tục xóa các node khỏi queue và xóa chúng khỏi adjacency list. Sau đó, kiểm tra xem các node kề có thỏa mãn điều kiện $coins[j]=0$ và $g[j]$ chỉ có một node hay không. Nếu có, thêm node đó vào queue $q$. Lặp lại cho đến khi queue rỗng.

Sau thao tác trên, chúng ta thu được một cây mới, trong đó các node lá đều là những node có coin bằng $1$.

Sau đó, chúng ta xóa hai lớp node lá còn lại, cuối cùng thu được một cây mà tất cả node đều cần được đi qua. Chỉ cần đếm số cạnh rồi nhân với $2$ để có đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node.

Các bài toán tương tự:

- [2204. Distance to a Cycle in Undirected Graph](https://github.com/doocs/leetcode/blob/main/solution/2200-2299/2204.Distance%20to%20a%20Cycle%20in%20Undirected%20Graph/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def collectTheCoins(self, coins: List[int], edges: List[List[int]]) -> int:
        g = defaultdict(set)
        for a, b in edges:
            g[a].add(b)
            g[b].add(a)
        n = len(coins)
        q = deque(i for i in range(n) if len(g[i]) == 1 and coins[i] == 0)
        while q:
            i = q.popleft()
            for j in g[i]:
                g[j].remove(i)
                if coins[j] == 0 and len(g[j]) == 1:
                    q.append(j)
            g[i].clear()
        for k in range(2):
            q = [i for i in range(n) if len(g[i]) == 1]
            for i in q:
                for j in g[i]:
                    g[j].remove(i)
                g[i].clear()
        return sum(len(g[a]) > 0 and len(g[b]) > 0 for a, b in edges) * 2
```

#### Java

```java
class Solution {
    public int collectTheCoins(int[] coins, int[][] edges) {
        int n = coins.length;
        Set<Integer>[] g = new Set[n];
        Arrays.setAll(g, k -> new HashSet<>());
        for (var e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        Deque<Integer> q = new ArrayDeque<>();
        for (int i = 0; i < n; ++i) {
            if (coins[i] == 0 && g[i].size() == 1) {
                q.offer(i);
            }
        }
        while (!q.isEmpty()) {
            int i = q.poll();
            for (int j : g[i]) {
                g[j].remove(i);
                if (coins[j] == 0 && g[j].size() == 1) {
                    q.offer(j);
                }
            }
            g[i].clear();
        }
        q.clear();
        for (int k = 0; k < 2; ++k) {
            for (int i = 0; i < n; ++i) {
                if (g[i].size() == 1) {
                    q.offer(i);
                }
            }
            for (int i : q) {
                for (int j : g[i]) {
                    g[j].remove(i);
                }
                g[i].clear();
            }
        }
        int ans = 0;
        for (var e : edges) {
            int a = e[0], b = e[1];
            if (g[a].size() > 0 && g[b].size() > 0) {
                ans += 2;
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
    int collectTheCoins(vector<int>& coins, vector<vector<int>>& edges) {
        int n = coins.size();
        unordered_set<int> g[n];
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].insert(b);
            g[b].insert(a);
        }
        queue<int> q;
        for (int i = 0; i < n; ++i) {
            if (coins[i] == 0 && g[i].size() == 1) {
                q.push(i);
            }
        }
        while (!q.empty()) {
            int i = q.front();
            q.pop();
            for (int j : g[i]) {
                g[j].erase(i);
                if (coins[j] == 0 && g[j].size() == 1) {
                    q.push(j);
                }
            }
            g[i].clear();
        }
        for (int k = 0; k < 2; ++k) {
            vector<int> q;
            for (int i = 0; i < n; ++i) {
                if (g[i].size() == 1) {
                    q.push_back(i);
                }
            }
            for (int i : q) {
                for (int j : g[i]) {
                    g[j].erase(i);
                }
                g[i].clear();
            }
        }
        int ans = 0;
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            if (g[a].size() && g[b].size()) {
                ans += 2;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func collectTheCoins(coins []int, edges [][]int) int {
	n := len(coins)
	g := make([]map[int]bool, n)
	for i := range g {
		g[i] = map[int]bool{}
	}
	for _, e := range edges {
		a, b := e[0], e[1]
		g[a][b] = true
		g[b][a] = true
	}
	q := []int{}
	for i, c := range coins {
		if c == 0 && len(g[i]) == 1 {
			q = append(q, i)
		}
	}
	for len(q) > 0 {
		i := q[0]
		q = q[1:]
		for j := range g[i] {
			delete(g[j], i)
			if coins[j] == 0 && len(g[j]) == 1 {
				q = append(q, j)
			}
		}
		g[i] = map[int]bool{}
	}
	for k := 0; k < 2; k++ {
		q := []int{}
		for i := range coins {
			if len(g[i]) == 1 {
				q = append(q, i)
			}
		}
		for _, i := range q {
			for j := range g[i] {
				delete(g[j], i)
			}
			g[i] = map[int]bool{}
		}
	}
	ans := 0
	for _, e := range edges {
		a, b := e[0], e[1]
		if len(g[a]) > 0 && len(g[b]) > 0 {
			ans += 2
		}
	}
	return ans
}
```

#### TypeScript

```ts
function collectTheCoins(coins: number[], edges: number[][]): number {
    const n = coins.length;
    const g: Set<number>[] = new Array(n).fill(0).map(() => new Set<number>());
    for (const [a, b] of edges) {
        g[a].add(b);
        g[b].add(a);
    }
    let q: number[] = [];
    for (let i = 0; i < n; ++i) {
        if (coins[i] === 0 && g[i].size === 1) {
            q.push(i);
        }
    }
    while (q.length) {
        const i = q.pop()!;
        for (const j of g[i]) {
            g[j].delete(i);
            if (coins[j] === 0 && g[j].size === 1) {
                q.push(j);
            }
        }
        g[i].clear();
    }
    q = [];
    for (let k = 0; k < 2; ++k) {
        for (let i = 0; i < n; ++i) {
            if (g[i].size === 1) {
                q.push(i);
            }
        }
        for (const i of q) {
            for (const j of g[i]) {
                g[j].delete(i);
            }
            g[i].clear();
        }
    }
    let ans = 0;
    for (const [a, b] of edges) {
        if (g[a].size > 0 && g[b].size > 0) {
            ans += 2;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
