---
comments: true
difficulty: Hard
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Topological Sort
---

<!-- problem:start -->

# [2204. Distance to a Cycle in Undirected Graph 🔒](https://leetcode.com/problems/distance-to-a-cycle-in-undirected-graph)

[中文文档](/solution/2200-2299/2204.Distance%20to%20a%20Cycle%20in%20Undirected%20Graph/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên dương <code>n</code> biểu thị số lượng đỉnh của một <strong>đồ thị vô hướng liên thông</strong> có chứa <strong>chính xác một</strong> chu trình. Các đỉnh được đánh số từ <code>0</code> đến <code>n - 1</code> (<strong>bao gồm cả hai đầu mút</strong>).</p>

<p>Bạn cũng được cho một mảng số nguyên 2 chiều <code>edges</code>, trong đó <code>edges[i] = [node1<sub>i</sub>, node2<sub>i</sub>]</code> biểu thị có một cạnh <strong>hai chiều</strong> nối <code>node1<sub>i</sub></code> và <code>node2<sub>i</sub></code> trong đồ thị.</p>

<p>Khoảng cách giữa hai đỉnh <code>a</code> và <code>b</code> được định nghĩa là <strong>số cạnh nhỏ nhất</strong> cần đi qua để đi từ <code>a</code> đến <code>b</code>.</p>

<p>Trả về <em>một mảng số nguyên <code>answer</code></em><em> có kích thước </em><code>n</code><em>, trong đó </em><code>answer[i]</code><em> là khoảng cách <strong>nhỏ nhất</strong> giữa đỉnh thứ </em><code>i<sup>th</sup></code><em> và <strong>bất kỳ</strong> đỉnh nào trên chu trình.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2204.Distance%20to%20a%20Cycle%20in%20Undirected%20Graph/images/image-20220315154238-1.png" style="width: 350px; height: 237px;" />
<pre>
<strong>Đầu vào:</strong> n = 7, edges = [[1,2],[2,4],[4,3],[3,1],[0,1],[5,2],[6,5]]
<strong>Đầu ra:</strong> [1,0,0,0,0,1,2]
<strong>Giải thích:</strong>
Các đỉnh 1, 2, 3 và 4 tạo thành chu trình.
Khoảng cách từ 0 đến 1 là 1.
Khoảng cách từ 1 đến 1 là 0.
Khoảng cách từ 2 đến 2 là 0.
Khoảng cách từ 3 đến 3 là 0.
Khoảng cách từ 4 đến 4 là 0.
Khoảng cách từ 5 đến 2 là 1.
Khoảng cách từ 6 đến 2 là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2204.Distance%20to%20a%20Cycle%20in%20Undirected%20Graph/images/image-20220315154634-1.png" style="width: 400px; height: 297px;" />
<pre>
<strong>Đầu vào:</strong> n = 9, edges = [[0,1],[1,2],[0,2],[2,6],[6,7],[6,8],[0,3],[3,4],[3,5]]
<strong>Đầu ra:</strong> [0,0,0,1,2,2,1,2,2]
<strong>Giải thích:</strong>
Các đỉnh 0, 1 và 2 tạo thành chu trình.
Khoảng cách từ 0 đến 0 là 0.
Khoảng cách từ 1 đến 1 là 0.
Khoảng cách từ 2 đến 2 là 0.
Khoảng cách từ 3 đến 1 là 1.
Khoảng cách từ 4 đến 1 là 2.
Khoảng cách từ 5 đến 1 là 2.
Khoảng cách từ 6 đến 2 là 1.
Khoảng cách từ 7 đến 2 là 2.
Khoảng cách từ 8 đến 2 là 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>edges.length == n</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>0 &lt;= node1<sub>i</sub>, node2<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>node1<sub>i</sub> != node2<sub>i</sub></code></li>
	<li>Đồ thị liên thông.</li>
	<li>Đồ thị có chính xác một chu trình.</li>
	<li>Có nhiều nhất một cạnh giữa mỗi cặp đỉnh.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp topo

<!-- thinking:start -->

> **Tư duy**
>
> Đồ thị này gồm một chu trình với các cây gắn vào. Nếu chạy BFS từ mọi đỉnh để tìm chu trình, độ phức tạp sẽ là $O(n^2)$. Các đỉnh trên chu trình có khoảng cách $0$; khoảng cách của một đỉnh thuộc cây là độ dài đường đi duy nhất từ đỉnh đó đến chu trình.
>
> Các đỉnh bậc $1$ không bao giờ nằm trên chu trình, nên có thể lần lượt loại bỏ chúng như các lá. Đưa chúng vào queue, ghi lại đỉnh cha $f[i]$ khi loại bỏ một đỉnh, đồng thời giảm bậc của các đỉnh kề. Phần còn lại chính là chu trình.
>
> Điền khoảng cách theo thứ tự ngược với thứ tự loại bỏ: các đỉnh trên chu trình giữ giá trị $0$, còn đỉnh $i$ bị loại bỏ nhận $ans[f[i]]+1$.

<!-- thinking:end -->

Trước hết, ta chuyển các cạnh trong $edges$ thành một adjacency list $g$, trong đó $g[i]$ biểu diễn tất cả các đỉnh kề với đỉnh $i$ dưới dạng một set.

Tiếp theo, ta xóa các đỉnh theo từng lớp từ ngoài vào trong cho đến khi chỉ còn lại một chu trình. Cụ thể như sau:

Đầu tiên, ta tìm tất cả các đỉnh có bậc bằng $1$ và xóa chúng khỏi đồ thị. Nếu sau khi xóa, bậc của đỉnh kề với nó trở thành $1$, ta thêm đỉnh đó vào queue $q$. Trong quá trình này, ta ghi lại các đỉnh đã xóa theo thứ tự vào $seq$; đồng thời dùng mảng $f$ để ghi lại đỉnh kề của mỗi đỉnh gần chu trình hơn, tức là $f[i]$ biểu diễn đỉnh kề với đỉnh $i$ gần chu trình hơn.

Cuối cùng, ta khởi tạo mảng đáp án $ans$ có độ dài $n$, trong đó $ans[i]$ biểu diễn khoảng cách nhỏ nhất từ đỉnh $i$ đến bất kỳ đỉnh nào trên chu trình, ban đầu $ans[i] = 0$. Sau đó, ta bắt đầu duyệt từ cuối $seq$. Với mỗi đỉnh $i$, ta có thể lấy giá trị của $ans[i]$ từ đỉnh kề $f[i]$, tức là $ans[i] = ans[f[i]] + 1$.

Sau khi duyệt xong, trả về mảng đáp án $ans$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số lượng đỉnh.

Bài toán tương tự:

- [2603. Collect Coins in a Tree](https://github.com/doocs/leetcode/blob/main/solution/2600-2699/2603.Collect%20Coins%20in%20a%20Tree/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def distanceToCycle(self, n: int, edges: List[List[int]]) -> List[int]:
        g = defaultdict(set)
        for a, b in edges:
            g[a].add(b)
            g[b].add(a)
        q = deque(i for i in range(n) if len(g[i]) == 1)
        f = [0] * n
        seq = []
        while q:
            i = q.popleft()
            seq.append(i)
            for j in g[i]:
                g[j].remove(i)
                f[i] = j
                if len(g[j]) == 1:
                    q.append(j)
            g[i].clear()
        ans = [0] * n
        for i in seq[::-1]:
            ans[i] = ans[f[i]] + 1
        return ans
```

#### Java

```java
class Solution {
    public int[] distanceToCycle(int n, int[][] edges) {
        Set<Integer>[] g = new Set[n];
        Arrays.setAll(g, k -> new HashSet<>());
        for (var e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        Deque<Integer> q = new ArrayDeque<>();
        for (int i = 0; i < n; ++i) {
            if (g[i].size() == 1) {
                q.offer(i);
            }
        }
        int[] f = new int[n];
        Deque<Integer> seq = new ArrayDeque<>();
        while (!q.isEmpty()) {
            int i = q.poll();
            seq.push(i);
            for (int j : g[i]) {
                g[j].remove(i);
                f[i] = j;
                if (g[j].size() == 1) {
                    q.offer(j);
                }
            }
        }
        int[] ans = new int[n];
        while (!seq.isEmpty()) {
            int i = seq.pop();
            ans[i] = ans[f[i]] + 1;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> distanceToCycle(int n, vector<vector<int>>& edges) {
        unordered_set<int> g[n];
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].insert(b);
            g[b].insert(a);
        }
        queue<int> q;
        for (int i = 0; i < n; ++i) {
            if (g[i].size() == 1) {
                q.push(i);
            }
        }
        int f[n];
        int seq[n];
        int k = 0;
        while (!q.empty()) {
            int i = q.front();
            q.pop();
            seq[k++] = i;
            for (int j : g[i]) {
                g[j].erase(i);
                f[i] = j;
                if (g[j].size() == 1) {
                    q.push(j);
                }
            }
            g[i].clear();
        }
        vector<int> ans(n);
        for (; k; --k) {
            int i = seq[k - 1];
            ans[i] = ans[f[i]] + 1;
        }
        return ans;
    }
};
```

#### Go

```go
func distanceToCycle(n int, edges [][]int) []int {
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
	for i := 0; i < n; i++ {
		if len(g[i]) == 1 {
			q = append(q, i)
		}
	}
	f := make([]int, n)
	seq := []int{}
	for len(q) > 0 {
		i := q[0]
		q = q[1:]
		seq = append(seq, i)
		for j := range g[i] {
			delete(g[j], i)
			f[i] = j
			if len(g[j]) == 1 {
				q = append(q, j)
			}
		}
		g[i] = map[int]bool{}
	}
	ans := make([]int, n)
	for k := len(seq) - 1; k >= 0; k-- {
		i := seq[k]
		ans[i] = ans[f[i]] + 1
	}
	return ans
}
```

#### TypeScript

```ts
function distanceToCycle(n: number, edges: number[][]): number[] {
    const g: Set<number>[] = new Array(n).fill(0).map(() => new Set<number>());
    for (const [a, b] of edges) {
        g[a].add(b);
        g[b].add(a);
    }
    const q: number[] = [];
    for (let i = 0; i < n; ++i) {
        if (g[i].size === 1) {
            q.push(i);
        }
    }
    const f: number[] = Array(n).fill(0);
    const seq: number[] = [];
    while (q.length) {
        const i = q.pop()!;
        seq.push(i);
        for (const j of g[i]) {
            g[j].delete(i);
            f[i] = j;
            if (g[j].size === 1) {
                q.push(j);
            }
        }
        g[i].clear();
    }
    const ans: number[] = Array(n).fill(0);
    while (seq.length) {
        const i = seq.pop()!;
        ans[i] = ans[f[i]] + 1;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
