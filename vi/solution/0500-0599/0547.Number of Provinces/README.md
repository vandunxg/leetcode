---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Graph
---

<!-- problem:start -->

# [547. Number of Provinces](https://leetcode.com/problems/number-of-provinces)

[中文文档](/solution/0500-0599/0547.Number%20of%20Provinces/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> thành phố, một số thành phố được kết nối với nhau, một số thì không. Nếu thành phố <code>a</code> kết nối trực tiếp với thành phố <code>b</code>, và thành phố <code>b</code> kết nối trực tiếp với thành phố <code>c</code>, thì thành phố <code>a</code> kết nối gián tiếp với thành phố <code>c</code>.</p>

<p><strong>Tỉnh</strong> là một nhóm thành phố kết nối trực tiếp hoặc gián tiếp với nhau, và không bao gồm thành phố nào bên ngoài nhóm đó.</p>

<p>Cho ma trận <code>isConnected</code> kích thước <code>n x n</code>, trong đó <code>isConnected[i][j] = 1</code> nếu thành phố tại chỉ số <code>i</code> và thành phố tại chỉ số <code>j</code> kết nối trực tiếp, ngược lại <code>isConnected[i][j] = 0</code>.</p>

<p>Hãy trả về <em>tổng số <strong>tỉnh</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0547.Number%20of%20Provinces/images/graph1.jpg" style="width: 222px; height: 142px;" />
<pre>
<strong>Đầu vào:</strong> isConnected = [[1,1,0],[1,1,0],[0,0,1]]
<strong>Đầu ra:</strong> 2
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0547.Number%20of%20Provinces/images/graph2.jpg" style="width: 222px; height: 142px;" />
<pre>
<strong>Đầu vào:</strong> isConnected = [[1,0,0],[0,1,0],[0,0,1]]
<strong>Đầu ra:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 200</code></li>
	<li><code>n == isConnected.length</code></li>
	<li><code>n == isConnected[i].length</code></li>
	<li><code>isConnected[i][j]</code> là <code>1</code> hoặc <code>0</code>.</li>
	<li><code>isConnected[i][i] == 1</code></li>
	<li><code>isConnected[i][j] == isConnected[j][i]</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi tỉnh tương ứng với một thành phần liên thông. Ma trận cho biết những thành phố nào có cạnh nối với nhau, vì vậy chạy DFS hoặc BFS từ mỗi thành phố chưa thăm sẽ đánh dấu một thành phần.
>
> Duyệt các thành phố, bắt đầu DFS tại mỗi chỉ số chưa được thăm rồi tăng đáp án. Mảng visited bảo đảm mỗi thành phố chỉ được duyệt một lần.

<!-- thinking:end -->

Ta tạo mảng $\textit{vis}$ để ghi nhận thành phố nào đã được thăm.

Tiếp theo, ta duyệt từng thành phố $i$. Nếu thành phố chưa được thăm, ta bắt đầu DFS từ đó. Dựa vào ma trận $\textit{isConnected}$, ta tìm các thành phố kết nối trực tiếp với thành phố hiện tại. Những thành phố này cùng thành phố hiện tại thuộc về một tỉnh. Ta tiếp tục DFS cho đến khi đã thăm hết các thành phố trong tỉnh đó. Đây là một tỉnh, nên tăng đáp án $\textit{ans}$ lên $1$. Sau đó, chuyển đến thành phố chưa thăm tiếp theo và lặp lại cho đến khi duyệt hết tất cả thành phố.

Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(n^2)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là số thành phố.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        def dfs(i: int):
            vis[i] = True
            for j, x in enumerate(isConnected[i]):
                if not vis[j] and x:
                    dfs(j)

        n = len(isConnected)
        vis = [False] * n
        ans = 0
        for i in range(n):
            if not vis[i]:
                dfs(i)
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    private int[][] g;
    private boolean[] vis;

    public int findCircleNum(int[][] isConnected) {
        g = isConnected;
        int n = g.length;
        vis = new boolean[n];
        int ans = 0;
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
        for (int j = 0; j < g.length; ++j) {
            if (!vis[j] && g[i][j] == 1) {
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
    int findCircleNum(vector<vector<int>>& isConnected) {
        int n = isConnected.size();
        int ans = 0;
        bool vis[n];
        memset(vis, false, sizeof(vis));
        auto dfs = [&](this auto&& dfs, int i) -> void {
            vis[i] = true;
            for (int j = 0; j < n; ++j) {
                if (!vis[j] && isConnected[i][j]) {
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
func findCircleNum(isConnected [][]int) (ans int) {
	n := len(isConnected)
	vis := make([]bool, n)
	var dfs func(int)
	dfs = func(i int) {
		vis[i] = true
		for j, x := range isConnected[i] {
			if !vis[j] && x == 1 {
				dfs(j)
			}
		}
	}
	for i, v := range vis {
		if !v {
			ans++
			dfs(i)
		}
	}
	return
}
```

#### TypeScript

```ts
function findCircleNum(isConnected: number[][]): number {
    const n = isConnected.length;
    const vis: boolean[] = new Array(n).fill(false);
    const dfs = (i: number) => {
        vis[i] = true;
        for (let j = 0; j < n; ++j) {
            if (!vis[j] && isConnected[i][j]) {
                dfs(j);
            }
        }
    };
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        if (!vis[i]) {
            dfs(i);
            ++ans;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    fn dfs(is_connected: &mut Vec<Vec<i32>>, vis: &mut Vec<bool>, i: usize) {
        vis[i] = true;
        for j in 0..is_connected.len() {
            if vis[j] || is_connected[i][j] == 0 {
                continue;
            }
            Self::dfs(is_connected, vis, j);
        }
    }

    pub fn find_circle_num(mut is_connected: Vec<Vec<i32>>) -> i32 {
        let n = is_connected.len();
        let mut vis = vec![false; n];
        let mut res = 0;
        for i in 0..n {
            if vis[i] {
                continue;
            }
            res += 1;
            Self::dfs(&mut is_connected, &mut vis, i);
        }
        res
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Union-Find

<!-- thinking:start -->

> **Tư duy**
>
> DFS cần stack đệ quy và mảng visited. Union-Find gộp các thành phần theo cạnh: bắt đầu với $n$ thành phần, rồi giảm số lượng khi một cạnh nối hai root khác nhau.
>
> Duyệt nửa trên của ma trận để tránh xét cạnh trùng lặp. Path compression giúp thao tác tìm root nhanh hơn. Các root còn lại đại diện cho các tỉnh.

<!-- thinking:end -->

Ta cũng có thể dùng cấu trúc dữ liệu Union-Find để quản lý các thành phần liên thông. Ban đầu, mỗi thành phố thuộc một thành phần liên thông riêng, nên số tỉnh là $n$.

Tiếp theo, ta duyệt ma trận $\textit{isConnected}$. Nếu hai thành phố $(i, j)$ có kết nối và thuộc hai thành phần liên thông khác nhau, ta gộp chúng thành một thành phần, đồng thời giảm số tỉnh đi $1$.

Cuối cùng, trả về số tỉnh.

Độ phức tạp thời gian là $O(n^2 \times \log n)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số thành phố và $\log n$ là độ phức tạp thời gian của path compression trong cấu trúc dữ liệu Union-Find.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        def find(x: int) -> int:
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        n = len(isConnected)
        p = list(range(n))
        ans = n
        for i in range(n):
            for j in range(i + 1, n):
                if isConnected[i][j]:
                    pa, pb = find(i), find(j)
                    if pa != pb:
                        p[pa] = pb
                        ans -= 1
        return ans
```

#### Java

```java
class Solution {
    private int[] p;

    public int findCircleNum(int[][] isConnected) {
        int n = isConnected.length;
        p = new int[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
        }
        int ans = n;
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                if (isConnected[i][j] == 1) {
                    int pa = find(i), pb = find(j);
                    if (pa != pb) {
                        p[pa] = pb;
                        --ans;
                    }
                }
            }
        }
        return ans;
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
    int findCircleNum(vector<vector<int>>& isConnected) {
        int n = isConnected.size();
        int p[n];
        iota(p, p + n, 0);
        auto find = [&](this auto&& find, int x) -> int {
            if (p[x] != x) {
                p[x] = find(p[x]);
            }
            return p[x];
        };
        int ans = n;
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                if (isConnected[i][j]) {
                    int pa = find(i), pb = find(j);
                    if (pa != pb) {
                        p[pa] = pb;
                        --ans;
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
func findCircleNum(isConnected [][]int) (ans int) {
	n := len(isConnected)
	p := make([]int, n)
	for i := range p {
		p[i] = i
	}
	var find func(x int) int
	find = func(x int) int {
		if p[x] != x {
			p[x] = find(p[x])
		}
		return p[x]
	}
	ans = n
	for i := 0; i < n; i++ {
		for j := 0; j < n; j++ {
			if isConnected[i][j] == 1 {
				pa, pb := find(i), find(j)
				if pa != pb {
					p[pa] = pb
					ans--
				}
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function findCircleNum(isConnected: number[][]): number {
    const n = isConnected.length;
    const p: number[] = Array.from({ length: n }, (_, i) => i);
    const find = (x: number): number => {
        if (p[x] !== x) {
            p[x] = find(p[x]);
        }
        return p[x];
    };
    let ans = n;
    for (let i = 0; i < n; ++i) {
        for (let j = i + 1; j < n; ++j) {
            if (isConnected[i][j]) {
                const pa = find(i);
                const pb = find(j);
                if (pa !== pb) {
                    p[pa] = pb;
                    --ans;
                }
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
