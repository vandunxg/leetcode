---
comments: true
difficulty: Medium
rating: 1565
source: Weekly Contest 442 Q2
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Graph
    - Array
    - Hash Table
---

<!-- problem:start -->

# [3493. Properties Graph](https://leetcode.com/problems/properties-graph)

[中文文档](/solution/3400-3499/3493.Properties%20Graph/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên 2D <code>properties</code> có kích thước <code>n x m</code> và một số nguyên <code>k</code>.</p>

<p>Định nghĩa hàm <code>intersect(a, b)</code> trả về <strong>số lượng số nguyên phân biệt</strong> xuất hiện trong cả hai mảng <code>a</code> và <code>b</code>.</p>

<p>Xây dựng một đồ thị <strong>vô hướng</strong>, trong đó mỗi chỉ số <code>i</code> tương ứng với <code>properties[i]</code>. Có một cạnh giữa node <code>i</code> và node <code>j</code> khi và chỉ khi <code>intersect(properties[i], properties[j]) &gt;= k</code>, với <code>i</code> và <code>j</code> thuộc khoảng <code>[0, n - 1]</code> và <code>i != j</code>.</p>

<p>Hãy trả về số <strong>thành phần liên thông</strong> trong đồ thị được tạo thành.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">properties = [[1,2],[1,1],[3,4],[4,5],[5,6],[7,7]], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đồ thị được tạo thành có 3 thành phần liên thông:</p>

<p><img height="171" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3493.Properties%20Graph/images/image.png" width="279" /></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">properties = [[1,2,3],[2,3,4],[4,3,5]], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đồ thị được tạo thành có 1 thành phần liên thông:</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3493.Properties%20Graph/images/screenshot-from-2025-02-27-23-58-34.png" style="width: 219px; height: 171px;" /></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">properties = [[1,1],[1,1]], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>intersect(properties[0], properties[1]) = 1</code>, nhỏ hơn <code>k</code>. Điều này có nghĩa là không có cạnh nào giữa <code>properties[0]</code> và <code>properties[1]</code> trong đồ thị.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == properties.length &lt;= 100</code></li>
	<li><code>1 &lt;= m == properties[i].length &lt;= 100</code></li>
	<li><code>1 &lt;= properties[i][j] &lt;= 100</code></li>
	<li><code>1 &lt;= k &lt;= m</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + DFS

<!-- thinking:start -->

> **Tư duy**
>
> Hai node kề nhau khi các tập thuộc tính của chúng có chung ít nhất $k$ giá trị. Vì $n,m\le 100$, ta có thể xây dựng đồ thị và đếm số thành phần.
>
> Các list có thể chứa phần tử trùng lặp; chuyển chúng thành các tập hợp giúp tránh đếm trùng phần giao.
>
> So sánh mọi cặp, thêm các cạnh vô hướng, rồi dùng DFS/BFS để đếm số thành phần.

<!-- thinking:end -->

Trước tiên, chúng ta chuyển mỗi mảng thuộc tính thành một hash table và lưu chúng trong một mảng hash table $\textit{ss}$. Ta định nghĩa một đồ thị $\textit{g}$, trong đó $\textit{g}[i]$ lưu các chỉ số của những mảng thuộc tính được nối với $\textit{properties}[i]$.

Sau đó, chúng ta duyệt qua tất cả các hash table thuộc tính. Với mỗi cặp hash table thuộc tính $(i, j)$ sao cho $j < i$, ta kiểm tra xem số phần tử chung giữa chúng có ít nhất là $k$ hay không. Nếu có, ta thêm một cạnh từ $i$ đến $j$ trong đồ thị $\textit{g}$, đồng thời thêm một cạnh từ $j$ đến $i$.

Cuối cùng, chúng ta dùng Depth-First Search (DFS) để tính số thành phần liên thông trong đồ thị $\textit{g}$.

Độ phức tạp thời gian là $O(n^2 \times m)$, còn độ phức tạp không gian là $O(n \times m)$, trong đó $n$ là độ dài của các mảng thuộc tính và $m$ là số phần tử trong một mảng thuộc tính.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfComponents(self, properties: List[List[int]], k: int) -> int:
        def dfs(i: int) -> None:
            vis[i] = True
            for j in g[i]:
                if not vis[j]:
                    dfs(j)

        n = len(properties)
        ss = list(map(set, properties))
        g = [[] for _ in range(n)]
        for i, s1 in enumerate(ss):
            for j in range(i):
                s2 = ss[j]
                if len(s1 & s2) >= k:
                    g[i].append(j)
                    g[j].append(i)
        ans = 0
        vis = [False] * n
        for i in range(n):
            if not vis[i]:
                dfs(i)
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private boolean[] vis;

    public int numberOfComponents(int[][] properties, int k) {
        int n = properties.length;
        g = new List[n];
        Set<Integer>[] ss = new Set[n];
        Arrays.setAll(g, i -> new ArrayList<>());
        Arrays.setAll(ss, i -> new HashSet<>());
        for (int i = 0; i < n; ++i) {
            for (int x : properties[i]) {
                ss[i].add(x);
            }
        }
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                int cnt = 0;
                for (int x : ss[i]) {
                    if (ss[j].contains(x)) {
                        ++cnt;
                    }
                }
                if (cnt >= k) {
                    g[i].add(j);
                    g[j].add(i);
                }
            }
        }

        int ans = 0;
        vis = new boolean[n];
        for (int i = 0; i < n; ++i) {
            if (!vis[i]) {
                dfs(i);
                ++ans;
            }
        }
        return ans;
    }

    private void dfs(int i) {
        vis[i] = true;
        for (int j : g[i]) {
            if (!vis[j]) {
                dfs(j);
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfComponents(vector<vector<int>>& properties, int k) {
        int n = properties.size();
        unordered_set<int> ss[n];
        vector<int> g[n];
        for (int i = 0; i < n; ++i) {
            for (int x : properties[i]) {
                ss[i].insert(x);
            }
        }
        for (int i = 0; i < n; ++i) {
            auto& s1 = ss[i];
            for (int j = 0; j < i; ++j) {
                auto& s2 = ss[j];
                int cnt = 0;
                for (int x : s1) {
                    if (s2.contains(x)) {
                        ++cnt;
                    }
                }
                if (cnt >= k) {
                    g[i].push_back(j);
                    g[j].push_back(i);
                }
            }
        }
        int ans = 0;
        vector<bool> vis(n);
        auto dfs = [&](this auto&& dfs, int i) -> void {
            vis[i] = true;
            for (int j : g[i]) {
                if (!vis[j]) {
                    dfs(j);
                }
            }
        };
        for (int i = 0; i < n; ++i) {
            if (!vis[i]) {
                dfs(i);
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfComponents(properties [][]int, k int) (ans int) {
	n := len(properties)
	ss := make([]map[int]struct{}, n)
	g := make([][]int, n)

	for i := 0; i < n; i++ {
		ss[i] = make(map[int]struct{})
		for _, x := range properties[i] {
			ss[i][x] = struct{}{}
		}
	}

	for i := 0; i < n; i++ {
		for j := 0; j < i; j++ {
			cnt := 0
			for x := range ss[i] {
				if _, ok := ss[j][x]; ok {
					cnt++
				}
			}
			if cnt >= k {
				g[i] = append(g[i], j)
				g[j] = append(g[j], i)
			}
		}
	}

	vis := make([]bool, n)
	var dfs func(int)
	dfs = func(i int) {
		vis[i] = true
		for _, j := range g[i] {
			if !vis[j] {
				dfs(j)
			}
		}
	}

	for i := 0; i < n; i++ {
		if !vis[i] {
			dfs(i)
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function numberOfComponents(properties: number[][], k: number): number {
    const n = properties.length;
    const ss: Set<number>[] = Array.from({ length: n }, () => new Set());
    const g: number[][] = Array.from({ length: n }, () => []);

    for (let i = 0; i < n; i++) {
        for (const x of properties[i]) {
            ss[i].add(x);
        }
    }

    for (let i = 0; i < n; i++) {
        for (let j = 0; j < i; j++) {
            let cnt = 0;
            for (const x of ss[i]) {
                if (ss[j].has(x)) {
                    cnt++;
                }
            }
            if (cnt >= k) {
                g[i].push(j);
                g[j].push(i);
            }
        }
    }

    let ans = 0;
    const vis: boolean[] = Array(n).fill(false);

    const dfs = (i: number) => {
        vis[i] = true;
        for (const j of g[i]) {
            if (!vis[j]) {
                dfs(j);
            }
        }
    };

    for (let i = 0; i < n; i++) {
        if (!vis[i]) {
            dfs(i);
            ans++;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
