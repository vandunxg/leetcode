---
comments: true
difficulty: Medium
rating: 1710
source: Weekly Contest 418 Q2
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
---

<!-- problem:start -->

# [3310. Remove Methods From Project](https://leetcode.com/problems/remove-methods-from-project)

[中文文档](/solution/3300-3399/3310.Remove%20Methods%20From%20Project/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đang bảo trì một project có <code>n</code> method được đánh số từ <code>0</code> đến <code>n - 1</code>.</p>

<p>Bạn được cho hai số nguyên <code>n</code> và <code>k</code>, cùng một mảng số nguyên 2D <code>invocations</code>, trong đó <code>invocations[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> cho biết method <code>a<sub>i</sub></code> gọi method <code>b<sub>i</sub></code>.</p>

<p>Có một bug đã biết trong method <code>k</code>. Method <code>k</code>, cùng với mọi method được nó gọi, dù là <strong>trực tiếp</strong> hay <strong>gián tiếp</strong>, đều được xem là <strong>đáng ngờ</strong> và chúng ta muốn xóa chúng.</p>

<p>Một nhóm method chỉ có thể bị xóa nếu không có method nào <strong>bên ngoài</strong> nhóm gọi đến bất kỳ method nào <strong>bên trong</strong> nhóm.</p>

<p>Trả về một mảng chứa tất cả method còn lại sau khi xóa các method <strong>đáng ngờ</strong>. Bạn có thể trả về đáp án theo <em>bất kỳ thứ tự nào</em>. Nếu không thể xóa <strong>tất cả</strong> các method đáng ngờ, thì <strong>không xóa</strong> method nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, k = 1, invocations = [[1,2],[0,1],[3,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,1,2,3]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3310.Remove%20Methods%20From%20Project/images/graph-2.png" style="width: 200px; height: 200px;" /></p>

<p>Method 2 và method 1 là đáng ngờ, nhưng chúng được gọi trực tiếp bởi method 3 và method 0, vốn không đáng ngờ. Vì vậy, ta trả về tất cả phần tử mà không xóa gì.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, k = 0, invocations = [[1,2],[0,2],[0,1],[3,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,4]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3310.Remove%20Methods%20From%20Project/images/graph-3.png" style="width: 200px; height: 200px;" /></p>

<p>Method 0, method 1 và method 2 là đáng ngờ, đồng thời không bị method nào khác gọi trực tiếp. Ta có thể xóa chúng.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, k = 2, invocations = [[1,2],[0,1],[2,0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3310.Remove%20Methods%20From%20Project/images/graph.png" style="width: 200px; height: 200px;" /></p>

<p>Tất cả method đều đáng ngờ. Ta có thể xóa chúng.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= k &lt;= n - 1</code></li>
	<li><code>0 &lt;= invocations.length &lt;= 2 * 10<sup>5</sup></code></li>
	<li><code>invocations[i] == [a<sub>i</sub>, b<sub>i</sub>]</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li><code>invocations[i] != invocations[j]</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Two DFS

<!-- thinking:start -->

> **Tư duy**
>
> Các method có thể đi tới từ $k$ theo các cạnh gọi là đáng ngờ, nhưng ta không được xóa một node mà một method không đáng ngờ vẫn gọi đến. Đồ thị đủ lớn nên chỉ các phép duyệt tuyến tính mới phù hợp.
>
> DFS thứ nhất đánh dấu bao đóng có hướng bắt đầu từ $k$. DFS thứ hai bắt đầu từ mọi node chưa được đánh dấu và đi theo các cạnh vô hướng, xóa đánh dấu đáng ngờ của mọi node mà một method không đáng ngờ có thể đi tới.
>
> Chỉ các node vẫn còn được đánh dấu sau cả hai lượt mới bị xóa; các node còn lại tạo thành đáp án.

<!-- thinking:end -->

Ta có thể bắt đầu từ $k$ để tìm tất cả method đáng ngờ, rồi ghi nhận chúng vào mảng $\textit{suspicious}$. Sau đó, ta duyệt từ $0$ đến $n-1$, bắt đầu từ tất cả method không đáng ngờ, và đánh dấu mọi method có thể đi tới là không đáng ngờ. Cuối cùng, ta trả về tất cả method không đáng ngờ.

Độ phức tạp thời gian là $O(n + m)$, và độ phức tạp không gian là $O(n + m)$. Ở đây, $n$ và $m$ lần lượt là số method và số quan hệ gọi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def remainingMethods(
        self, n: int, k: int, invocations: List[List[int]]
    ) -> List[int]:
        def dfs(i: int):
            suspicious[i] = True
            for j in g[i]:
                if not suspicious[j]:
                    dfs(j)

        def dfs2(i: int):
            vis[i] = True
            for j in f[i]:
                if not vis[j]:
                    suspicious[j] = False
                    dfs2(j)

        f = [[] for _ in range(n)]
        g = [[] for _ in range(n)]
        for a, b in invocations:
            f[a].append(b)
            f[b].append(a)
            g[a].append(b)
        suspicious = [False] * n
        dfs(k)

        vis = [False] * n
        ans = []
        for i in range(n):
            if not suspicious[i] and not vis[i]:
                dfs2(i)
        return [i for i in range(n) if not suspicious[i]]
```

#### Java

```java
class Solution {
    private boolean[] suspicious;
    private boolean[] vis;
    private List<Integer>[] f;
    private List<Integer>[] g;

    public List<Integer> remainingMethods(int n, int k, int[][] invocations) {
        suspicious = new boolean[n];
        vis = new boolean[n];
        f = new List[n];
        g = new List[n];
        Arrays.setAll(f, i -> new ArrayList<>());
        Arrays.setAll(g, i -> new ArrayList<>());
        for (var e : invocations) {
            int a = e[0], b = e[1];
            f[a].add(b);
            f[b].add(a);
            g[a].add(b);
        }
        dfs(k);
        for (int i = 0; i < n; ++i) {
            if (!suspicious[i] && !vis[i]) {
                dfs2(i);
            }
        }
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < n; ++i) {
            if (!suspicious[i]) {
                ans.add(i);
            }
        }
        return ans;
    }

    private void dfs(int i) {
        suspicious[i] = true;
        for (int j : g[i]) {
            if (!suspicious[j]) {
                dfs(j);
            }
        }
    }

    private void dfs2(int i) {
        vis[i] = true;
        for (int j : f[i]) {
            if (!vis[j]) {
                suspicious[j] = false;
                dfs2(j);
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> remainingMethods(int n, int k, vector<vector<int>>& invocations) {
        vector<bool> suspicious(n);
        vector<bool> vis(n);
        vector<int> f[n];
        vector<int> g[n];
        for (const auto& e : invocations) {
            int a = e[0], b = e[1];
            f[a].push_back(b);
            f[b].push_back(a);
            g[a].push_back(b);
        }
        auto dfs = [&](this auto&& dfs, int i) -> void {
            suspicious[i] = true;
            for (int j : g[i]) {
                if (!suspicious[j]) {
                    dfs(j);
                }
            }
        };
        dfs(k);
        auto dfs2 = [&](this auto&& dfs2, int i) -> void {
            vis[i] = true;
            for (int j : f[i]) {
                if (!vis[j]) {
                    suspicious[j] = false;
                    dfs2(j);
                }
            }
        };
        for (int i = 0; i < n; ++i) {
            if (!suspicious[i] && !vis[i]) {
                dfs2(i);
            }
        }
        vector<int> ans;
        for (int i = 0; i < n; ++i) {
            if (!suspicious[i]) {
                ans.push_back(i);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func remainingMethods(n int, k int, invocations [][]int) []int {
	suspicious := make([]bool, n)
	vis := make([]bool, n)
	f := make([][]int, n)
	g := make([][]int, n)

	for _, e := range invocations {
		a, b := e[0], e[1]
		f[a] = append(f[a], b)
		f[b] = append(f[b], a)
		g[a] = append(g[a], b)
	}

	var dfs func(int)
	dfs = func(i int) {
		suspicious[i] = true
		for _, j := range g[i] {
			if !suspicious[j] {
				dfs(j)
			}
		}
	}

	dfs(k)

	var dfs2 func(int)
	dfs2 = func(i int) {
		vis[i] = true
		for _, j := range f[i] {
			if !vis[j] {
				suspicious[j] = false
				dfs2(j)
			}
		}
	}

	for i := 0; i < n; i++ {
		if !suspicious[i] && !vis[i] {
			dfs2(i)
		}
	}

	var ans []int
	for i := 0; i < n; i++ {
		if !suspicious[i] {
			ans = append(ans, i)
		}
	}

	return ans
}
```

#### TypeScript

```ts
function remainingMethods(n: number, k: number, invocations: number[][]): number[] {
    const suspicious: boolean[] = Array(n).fill(false);
    const vis: boolean[] = Array(n).fill(false);
    const f: number[][] = Array.from({ length: n }, () => []);
    const g: number[][] = Array.from({ length: n }, () => []);

    for (const [a, b] of invocations) {
        f[a].push(b);
        f[b].push(a);
        g[a].push(b);
    }

    const dfs = (i: number) => {
        suspicious[i] = true;
        for (const j of g[i]) {
            if (!suspicious[j]) {
                dfs(j);
            }
        }
    };

    dfs(k);

    const dfs2 = (i: number) => {
        vis[i] = true;
        for (const j of f[i]) {
            if (!vis[j]) {
                suspicious[j] = false;
                dfs2(j);
            }
        }
    };

    for (let i = 0; i < n; i++) {
        if (!suspicious[i] && !vis[i]) {
            dfs2(i);
        }
    }

    return Array.from({ length: n }, (_, i) => i).filter(i => !suspicious[i]);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
