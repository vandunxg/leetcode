---
comments: true
difficulty: Medium
rating: 1769
source: Weekly Contest 345 Q4
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Graph
---

<!-- problem:start -->

# [2685. Count the Number of Complete Components](https://leetcode.com/problems/count-the-number-of-complete-components)

[中文文档](/solution/2600-2699/2685.Count%20the%20Number%20of%20Complete%20Components/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code>. Có một đồ thị <strong>vô hướng</strong> gồm <code>n</code> đỉnh, được đánh số từ <code>0</code> đến <code>n - 1</code>. Cho một mảng số nguyên hai chiều <code>edges</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> biểu thị rằng có một cạnh <strong>vô hướng</strong> nối hai đỉnh <code>a<sub>i</sub></code> và <code>b<sub>i</sub></code>.</p>

<p>Hãy trả về <em>số lượng <strong>thành phần liên thông đầy đủ</strong> của đồ thị</em>.</p>

<p>Một <strong>thành phần liên thông</strong> là một đồ thị con của đồ thị, trong đó tồn tại một đường đi giữa mọi cặp đỉnh, và không có đỉnh nào của đồ thị con có cạnh nối với một đỉnh bên ngoài đồ thị con.</p>

<p>Một thành phần liên thông được gọi là <b>đầy đủ</b> nếu giữa mọi cặp đỉnh của nó đều có một cạnh.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2685.Count%20the%20Number%20of%20Complete%20Components/images/screenshot-from-2023-04-11-23-31-23.png" style="width: 671px; height: 270px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> n = 6, edges = [[0,1],[0,2],[1,2],[3,4]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Từ hình trên, có thể thấy rằng tất cả các thành phần của đồ thị này đều đầy đủ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2685.Count%20the%20Number%20of%20Complete%20Components/images/screenshot-from-2023-04-11-23-32-00.png" style="width: 671px; height: 270px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> n = 6, edges = [[0,1],[0,2],[1,2],[3,4],[3,5]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Thành phần chứa các đỉnh 0, 1 và 2 là đầy đủ vì giữa mọi cặp đỉnh đều có một cạnh. Mặt khác, thành phần chứa các đỉnh 3, 4 và 5 không đầy đủ vì không có cạnh giữa các đỉnh 4 và 5. Do đó, số lượng thành phần đầy đủ trong đồ thị này là 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 50</code></li>
	<li><code>0 &lt;= edges.length &lt;= n * (n - 1) / 2</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li>Không có cạnh trùng lặp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một thành phần đầy đủ có mọi cặp đỉnh được nối với nhau. Việc liệt kê một thành phần rồi kiểm tra mọi cặp đỉnh là đủ với $n \le 50$, nhưng chỉ cần một lần DFS là đã có được số lượng đỉnh và cạnh.
>
> Mỗi cạnh vô hướng được đếm hai lần, vì vậy một clique thỏa mãn $x(x-1)=y$. DFS từ mỗi đỉnh chưa được duyệt sẽ cộng dồn $(x,y)$ rồi kiểm tra đẳng thức này.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countCompleteComponents(self, n: int, edges: List[List[int]]) -> int:
        def dfs(i: int) -> (int, int):
            vis[i] = True
            x, y = 1, len(g[i])
            for j in g[i]:
                if not vis[j]:
                    a, b = dfs(j)
                    x += a
                    y += b
            return x, y

        g = defaultdict(list)
        for a, b in edges:
            g[a].append(b)
            g[b].append(a)
        vis = [False] * n
        ans = 0
        for i in range(n):
            if not vis[i]:
                a, b = dfs(i)
                ans += a * (a - 1) == b
        return ans
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private boolean[] vis;

    public int countCompleteComponents(int n, int[][] edges) {
        g = new List[n];
        vis = new boolean[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int[] e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (!vis[i]) {
                int[] t = dfs(i);
                if (t[0] * (t[0] - 1) == t[1]) {
                    ++ans;
                }
            }
        }
        return ans;
    }

    private int[] dfs(int i) {
        vis[i] = true;
        int x = 1, y = g[i].size();
        for (int j : g[i]) {
            if (!vis[j]) {
                int[] t = dfs(j);
                x += t[0];
                y += t[1];
            }
        }
        return new int[] {x, y};
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countCompleteComponents(int n, vector<vector<int>>& edges) {
        vector<vector<int>> g(n);
        bool vis[n];
        memset(vis, false, sizeof(vis));
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].push_back(b);
            g[b].push_back(a);
        }
        function<pair<int, int>(int)> dfs = [&](int i) -> pair<int, int> {
            vis[i] = true;
            int x = 1, y = g[i].size();
            for (int j : g[i]) {
                if (!vis[j]) {
                    auto [a, b] = dfs(j);
                    x += a;
                    y += b;
                }
            }
            return make_pair(x, y);
        };
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (!vis[i]) {
                auto [a, b] = dfs(i);
                if (a * (a - 1) == b) {
                    ++ans;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countCompleteComponents(n int, edges [][]int) (ans int) {
	g := make([][]int, n)
	vis := make([]bool, n)
	for _, e := range edges {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	var dfs func(int) (int, int)
	dfs = func(i int) (int, int) {
		vis[i] = true
		x, y := 1, len(g[i])
		for _, j := range g[i] {
			if !vis[j] {
				a, b := dfs(j)
				x += a
				y += b
			}
		}
		return x, y
	}
	for i := range vis {
		if !vis[i] {
			a, b := dfs(i)
			if a*(a-1) == b {
				ans++
			}
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Phương pháp đơn giản

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 đếm các cạnh một cách tường minh. Thay vào đó, ta lưu tập lân cận đóng của mỗi đỉnh: trong một thành phần đầy đủ, các tập này giống hệt nhau và kích thước của tập bằng số lần tập đó xuất hiện.
>
> Việc nhóm các đỉnh theo tập đó giúp tránh bước kiểm tra cạnh riêng biệt, phù hợp với $n$ nhỏ và cách nhìn dựa trên sự bằng nhau của các tập lân cận.

<!-- thinking:end -->

Các vấn đề cần giải quyết:

1. Làm thế nào để duy trì trạng thái liên kết giữa mỗi node và các node khác?
2. Làm thế nào để xác định liệu nhiều đỉnh có tạo thành một đồ thị liên thông hay không?

Đối với vấn đề thứ nhất: ta có thể duy trì tập các đỉnh kề với mỗi node (bao gồm chính node đó).

Đối với vấn đề thứ hai: sau khi giải quyết vấn đề thứ nhất, ta nhận thấy:

- node đó chứa tất cả các node trong đồ thị liên thông (bao gồm chính nó);
- và chỉ được nối với các node trong đồ thị liên thông đó.

Hãy lấy ví dụ 1 để minh họa:

- Đỉnh 5 chỉ kết nối với chính nó, nên nó tạo thành một đồ thị liên thông.
- Đỉnh 0 kết nối với các đỉnh 0, 1, 2. Tập này giống với tập của các đỉnh 1 và 2.
- Các đỉnh 3 và 4 cũng chứa chính chúng và chứa lẫn nhau.

<!-- tabs:start -->

#### C++

```cpp
class Solution {
public:
    int countCompleteComponents(int n, vector<vector<int>>& edges) {
        int ans = 0;
        vector<set<int>> m(n + 1, set<int>());
        for (int i = 0; i < n; i++) {
            m[i].insert(i);
        }
        for (auto x : edges) {
            m[x[0]].insert(x[1]);
            m[x[1]].insert(x[0]);
        }
        map<set<int>, int> s;
        for (int i = 0; i < n; i++) {
            s[m[i]]++;
        }
        for (auto& [x, y] : s) {
            if (y == x.size()) {
                ans++;
            }
        }
        return ans;
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
