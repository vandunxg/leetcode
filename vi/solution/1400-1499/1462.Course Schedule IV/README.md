---
comments: true
difficulty: Medium
rating: 1692
source: Biweekly Contest 27 Q3
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Topological Sort
---

<!-- problem:start -->

# [1462. Course Schedule IV](https://leetcode.com/problems/course-schedule-iv)

[中文文档](/solution/1400-1499/1462.Course%20Schedule%20IV/README.md)

## Mô tả

<!-- description:start -->

<p>Có tổng cộng <code>numCourses</code> khóa học bạn phải học, được đánh số từ <code>0</code> đến <code>numCourses - 1</code>. Bạn được cho một mảng <code>prerequisites</code>, trong đó <code>prerequisites[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> cho biết bạn <strong>phải</strong> học khóa <code>a<sub>i</sub></code> trước nếu muốn học khóa <code>b<sub>i</sub></code>.</p>

<ul>
	<li>Ví dụ, cặp <code>[0, 1]</code> cho biết bạn phải học khóa <code>0</code> trước khi có thể học khóa <code>1</code>.</li>
</ul>

<p>Prerequisite cũng có thể là <strong>gián tiếp</strong>. Nếu khóa <code>a</code> là prerequisite của khóa <code>b</code>, và khóa <code>b</code> là prerequisite của khóa <code>c</code>, thì khóa <code>a</code> là prerequisite của khóa <code>c</code>.</p>

<p>Bạn cũng được cho một mảng <code>queries</code>, trong đó <code>queries[j] = [u<sub>j</sub>, v<sub>j</sub>]</code>. Với truy vấn thứ <code>j</code>, hãy trả lời liệu khóa <code>u<sub>j</sub></code> có phải là prerequisite của khóa <code>v<sub>j</sub></code> hay không.</p>

<p>Trả về <i>một mảng boolean</i> <code>answer</code>, trong đó <code>answer[j]</code> là câu trả lời cho truy vấn thứ <code>j</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1462.Course%20Schedule%20IV/images/courses4-1-graph.jpg" style="width: 222px; height: 62px;" />
<pre>
<strong>Đầu vào:</strong> numCourses = 2, prerequisites = [[1,0]], queries = [[0,1],[1,0]]
<strong>Đầu ra:</strong> [false,true]
<strong>Giải thích:</strong> Cặp [1, 0] cho biết bạn phải học khóa 1 trước khi có thể học khóa 0.
Khóa 0 không phải là prerequisite của khóa 1, nhưng điều ngược lại là đúng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> numCourses = 2, prerequisites = [], queries = [[1,0],[0,1]]
<strong>Đầu ra:</strong> [false,false]
<strong>Giải thích:</strong> Không có prerequisite nào, và mỗi khóa học độc lập.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1462.Course%20Schedule%20IV/images/courses4-3-graph.jpg" style="width: 222px; height: 222px;" />
<pre>
<strong>Đầu vào:</strong> numCourses = 3, prerequisites = [[1,2],[1,0],[2,0]], queries = [[1,0],[1,2]]
<strong>Đầu ra:</strong> [true,true]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= numCourses &lt;= 100</code></li>
	<li><code>0 &lt;= prerequisites.length &lt;= (numCourses * (numCourses - 1) / 2)</code></li>
	<li><code>prerequisites[i].length == 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt;= numCourses - 1</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li>Tất cả các cặp <code>[a<sub>i</sub>, b<sub>i</sub>]</code> đều <strong>khác nhau</strong>.</li>
	<li>Đồ thị prerequisite không có chu trình.</li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= numCourses - 1</code></li>
	<li><code>u<sub>i</sub> != v<sub>i</sub></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán Floyd

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi truy vấn hỏi liệu $a$ có phải là prerequisite của $b$ hay không, tức là hỏi về khả năng đi tới. Với số lượng khóa học vừa phải, Floyd trên ma trận Boolean sẽ hoàn tất đồ thị; khi đó mỗi truy vấn chỉ mất $O(1)$.

<!-- thinking:end -->

Ta tạo một mảng 2D $f$, trong đó $f[i][j]$ cho biết node $i$ có thể đi tới node $j$ hay không.

Tiếp theo, ta duyệt qua mảng prerequisite $prerequisites$. Với mỗi phần tử $[a, b]$ trong đó, ta đặt $f[a][b]$ thành $true$.

Sau đó, ta sử dụng thuật toán Floyd để tính khả năng đi tới giữa mọi cặp node.

Cụ thể, ta sử dụng ba vòng lặp lồng nhau: đầu tiên duyệt node trung gian $k$, tiếp theo là node bắt đầu $i$, và cuối cùng là node kết thúc $j$. Ở mỗi lần lặp, nếu node $i$ có thể đi tới node $k$ và node $k$ có thể đi tới node $j$, thì node $i$ cũng có thể đi tới node $j$, và ta đặt $f[i][j]$ thành $true$.

Sau khi tính khả năng đi tới giữa mọi cặp node, với mỗi truy vấn $[a, b]$, ta có thể trả về trực tiếp $f[a][b]$.

Độ phức tạp thời gian là $O(n^3)$, và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkIfPrerequisite(
        self, n: int, prerequisites: List[List[int]], queries: List[List[int]]
    ) -> List[bool]:
        f = [[False] * n for _ in range(n)]
        for a, b in prerequisites:
            f[a][b] = True
        for k in range(n):
            for i in range(n):
                for j in range(n):
                    if f[i][k] and f[k][j]:
                        f[i][j] = True
        return [f[a][b] for a, b in queries]
```

#### Java

```java
class Solution {
    public List<Boolean> checkIfPrerequisite(int n, int[][] prerequisites, int[][] queries) {
        boolean[][] f = new boolean[n][n];
        for (var p : prerequisites) {
            f[p[0]][p[1]] = true;
        }
        for (int k = 0; k < n; ++k) {
            for (int i = 0; i < n; ++i) {
                for (int j = 0; j < n; ++j) {
                    f[i][j] |= f[i][k] && f[k][j];
                }
            }
        }
        List<Boolean> ans = new ArrayList<>();
        for (var q : queries) {
            ans.add(f[q[0]][q[1]]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<bool> checkIfPrerequisite(int n, vector<vector<int>>& prerequisites, vector<vector<int>>& queries) {
        bool f[n][n];
        memset(f, false, sizeof(f));
        for (auto& p : prerequisites) {
            f[p[0]][p[1]] = true;
        }
        for (int k = 0; k < n; ++k) {
            for (int i = 0; i < n; ++i) {
                for (int j = 0; j < n; ++j) {
                    f[i][j] |= (f[i][k] && f[k][j]);
                }
            }
        }
        vector<bool> ans;
        for (auto& q : queries) {
            ans.push_back(f[q[0]][q[1]]);
        }
        return ans;
    }
};
```

#### Go

```go
func checkIfPrerequisite(n int, prerequisites [][]int, queries [][]int) (ans []bool) {
	f := make([][]bool, n)
	for i := range f {
		f[i] = make([]bool, n)
	}
	for _, p := range prerequisites {
		f[p[0]][p[1]] = true
	}
	for k := 0; k < n; k++ {
		for i := 0; i < n; i++ {
			for j := 0; j < n; j++ {
				f[i][j] = f[i][j] || (f[i][k] && f[k][j])
			}
		}
	}
	for _, q := range queries {
		ans = append(ans, f[q[0]][q[1]])
	}
	return
}
```

#### TypeScript

```ts
function checkIfPrerequisite(n: number, prerequisites: number[][], queries: number[][]): boolean[] {
    const f = Array.from({ length: n }, () => Array(n).fill(false));
    prerequisites.forEach(([a, b]) => (f[a][b] = true));
    for (let k = 0; k < n; ++k) {
        for (let i = 0; i < n; ++i) {
            for (let j = 0; j < n; ++j) {
                f[i][j] ||= f[i][k] && f[k][j];
            }
        }
    }
    return queries.map(([a, b]) => f[a][b]);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Sắp xếp topo

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 tốn $O(n^3)$ để hoàn tất khả năng đi tới giữa mọi cặp. Một lượt duyệt topo hợp nhất khả năng đi tới của mỗi node vào các node kế tiếp, điền cùng ma trận $f[i][j]$ trên DAG.

<!-- thinking:end -->

Tương tự Lời giải 1, ta tạo một mảng 2D $f$, trong đó $f[i][j]$ cho biết node $i$ có thể đi tới node $j$ hay không. Ngoài ra, ta tạo một danh sách kề $g$, trong đó $g[i]$ biểu diễn tất cả node kế tiếp của node $i$, và một mảng $indeg$, trong đó $indeg[i]$ là bậc vào của node $i$.

Tiếp theo, ta duyệt qua mảng prerequisite $prerequisites$. Với mỗi phần tử $[a, b]$ trong đó, ta cập nhật danh sách kề $g$ và mảng bậc vào $indeg$.

Sau đó, ta sử dụng sắp xếp topo để tính khả năng đi tới giữa mọi cặp node.

Ta định nghĩa một queue $q$, ban đầu thêm vào queue tất cả node có bậc vào bằng $0$. Sau đó, ta liên tục thực hiện các thao tác sau: lấy node $i$ ở đầu queue, rồi duyệt qua tất cả node $j$ trong $g[i]$, đặt $f[i][j]$ thành $true$. Tiếp theo, ta duyệt node $h$, và nếu $f[h][i]$ là $true$, ta cũng đặt $f[h][j]$ thành $true$. Sau đó, ta giảm bậc vào của $j$ đi $1$. Nếu bậc vào của $j$ trở thành $0$, ta thêm $j$ vào queue.

Sau khi tính khả năng đi tới giữa mọi cặp node, với mỗi truy vấn $[a, b]$, ta có thể trả về trực tiếp $f[a][b]$.

Độ phức tạp thời gian là $O(n^3)$, và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkIfPrerequisite(
        self, n: int, prerequisites: List[List[int]], queries: List[List[int]]
    ) -> List[bool]:
        f = [[False] * n for _ in range(n)]
        g = [[] for _ in range(n)]
        indeg = [0] * n
        for a, b in prerequisites:
            g[a].append(b)
            indeg[b] += 1
        q = deque(i for i, x in enumerate(indeg) if x == 0)
        while q:
            i = q.popleft()
            for j in g[i]:
                f[i][j] = True
                for h in range(n):
                    f[h][j] = f[h][j] or f[h][i]
                indeg[j] -= 1
                if indeg[j] == 0:
                    q.append(j)
        return [f[a][b] for a, b in queries]
```

#### Java

```java
class Solution {
    public List<Boolean> checkIfPrerequisite(int n, int[][] prerequisites, int[][] queries) {
        boolean[][] f = new boolean[n][n];
        List<Integer>[] g = new List[n];
        int[] indeg = new int[n];
        Arrays.setAll(g, i -> new ArrayList<>());
        for (var p : prerequisites) {
            g[p[0]].add(p[1]);
            ++indeg[p[1]];
        }
        Deque<Integer> q = new ArrayDeque<>();
        for (int i = 0; i < n; ++i) {
            if (indeg[i] == 0) {
                q.offer(i);
            }
        }
        while (!q.isEmpty()) {
            int i = q.poll();
            for (int j : g[i]) {
                f[i][j] = true;
                for (int h = 0; h < n; ++h) {
                    f[h][j] |= f[h][i];
                }
                if (--indeg[j] == 0) {
                    q.offer(j);
                }
            }
        }
        List<Boolean> ans = new ArrayList<>();
        for (var qry : queries) {
            ans.add(f[qry[0]][qry[1]]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<bool> checkIfPrerequisite(int n, vector<vector<int>>& prerequisites, vector<vector<int>>& queries) {
        bool f[n][n];
        memset(f, false, sizeof(f));
        vector<int> g[n];
        vector<int> indeg(n);
        for (auto& p : prerequisites) {
            g[p[0]].push_back(p[1]);
            ++indeg[p[1]];
        }
        queue<int> q;
        for (int i = 0; i < n; ++i) {
            if (indeg[i] == 0) {
                q.push(i);
            }
        }
        while (!q.empty()) {
            int i = q.front();
            q.pop();
            for (int j : g[i]) {
                f[i][j] = true;
                for (int h = 0; h < n; ++h) {
                    f[h][j] |= f[h][i];
                }
                if (--indeg[j] == 0) {
                    q.push(j);
                }
            }
        }
        vector<bool> ans;
        for (auto& qry : queries) {
            ans.push_back(f[qry[0]][qry[1]]);
        }
        return ans;
    }
};
```

#### Go

```go
func checkIfPrerequisite(n int, prerequisites [][]int, queries [][]int) (ans []bool) {
	f := make([][]bool, n)
	for i := range f {
		f[i] = make([]bool, n)
	}
	g := make([][]int, n)
	indeg := make([]int, n)
	for _, p := range prerequisites {
		a, b := p[0], p[1]
		g[a] = append(g[a], b)
		indeg[b]++
	}
	q := []int{}
	for i, x := range indeg {
		if x == 0 {
			q = append(q, i)
		}
	}
	for len(q) > 0 {
		i := q[0]
		q = q[1:]
		for _, j := range g[i] {
			f[i][j] = true
			for h := 0; h < n; h++ {
				f[h][j] = f[h][j] || f[h][i]
			}
			indeg[j]--
			if indeg[j] == 0 {
				q = append(q, j)
			}
		}
	}
	for _, q := range queries {
		ans = append(ans, f[q[0]][q[1]])
	}
	return
}
```

#### TypeScript

```ts
function checkIfPrerequisite(n: number, prerequisites: number[][], queries: number[][]): boolean[] {
    const f = Array.from({ length: n }, () => Array(n).fill(false));
    const g: number[][] = Array.from({ length: n }, () => []);
    const indeg: number[] = Array(n).fill(0);
    for (const [a, b] of prerequisites) {
        g[a].push(b);
        ++indeg[b];
    }
    const q: number[] = [];
    for (let i = 0; i < n; ++i) {
        if (indeg[i] === 0) {
            q.push(i);
        }
    }
    while (q.length) {
        const i = q.shift()!;
        for (const j of g[i]) {
            f[i][j] = true;
            for (let h = 0; h < n; ++h) {
                f[h][j] ||= f[h][i];
            }
            if (--indeg[j] === 0) {
                q.push(j);
            }
        }
    }
    return queries.map(([a, b]) => f[a][b]);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
