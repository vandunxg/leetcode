---
comments: true
difficulty: Medium
rating: 2036
source: Weekly Contest 450 Q3
tags:
    - Breadth-First Search
    - Array
    - Hash Table
    - Matrix
---

<!-- problem:start -->

# [3552. Grid Teleportation Traversal](https://leetcode.com/problems/grid-teleportation-traversal)

[中文文档](/solution/3500-3599/3552.Grid%20Teleportation%20Traversal/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận ký tự 2D <code>matrix</code> có kích thước <code>m x n</code>, được biểu diễn dưới dạng một mảng các chuỗi, trong đó <code>matrix[i][j]</code> biểu thị ô tại giao điểm của hàng thứ <code>i<sup>th</sup></code> và cột thứ <code>j<sup>th</sup></code>. Mỗi ô thuộc một trong các loại sau:</p>

<ul>
    <li><code>&#39;.&#39;</code> biểu thị ô trống.</li>
    <li><code>&#39;#&#39;</code> biểu thị chướng ngại vật.</li>
    <li>Một chữ cái viết hoa (<code>&#39;A&#39;</code>-<code>&#39;Z&#39;</code>) biểu thị một cổng dịch chuyển.</li>
</ul>

<p>Bạn bắt đầu tại ô trên cùng bên trái <code>(0, 0)</code>, và mục tiêu là đến ô dưới cùng bên phải <code>(m - 1, n - 1)</code>. Bạn có thể di chuyển từ ô hiện tại đến bất kỳ ô kề nào (lên, xuống, trái, phải) miễn là ô đích nằm trong phạm vi ma trận và không phải là chướng ngại vật<strong>.</strong></p>

<p>Nếu bạn bước vào một ô chứa ký tự cổng và chưa từng sử dụng ký tự cổng đó, bạn có thể lập tức dịch chuyển đến bất kỳ ô nào khác trong ma trận có cùng ký tự. Việc dịch chuyển này không tính là một bước di chuyển, nhưng mỗi ký tự cổng chỉ có thể được sử dụng<strong> nhiều nhất </strong>một lần trong hành trình.</p>

<p>Trả về số bước di chuyển <strong>ít nhất</strong> cần thiết để đến ô dưới cùng bên phải. Nếu không thể đến đích, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">matrix = [&quot;A..&quot;,&quot;.A.&quot;,&quot;...&quot;]</span></p>

<p><strong>Đầu ra:</strong> 2</p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3552.Grid%20Teleportation%20Traversal/images/example04140.png" style="width: 151px; height: 151px;" /></p>

<ul>
    <li>Trước bước di chuyển đầu tiên, dịch chuyển từ <code>(0, 0)</code> đến <code>(1, 1)</code>.</li>
    <li>Ở bước di chuyển thứ nhất, di chuyển từ <code>(1, 1)</code> đến <code>(1, 2)</code>.</li>
    <li>Ở bước di chuyển thứ hai, di chuyển từ <code>(1, 2)</code> đến <code>(2, 2)</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">matrix = [&quot;.#...&quot;,&quot;.#.#.&quot;,&quot;.#.#.&quot;,&quot;...#.&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">13</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3552.Grid%20Teleportation%20Traversal/images/ezgifcom-animated-gif-maker.gif" style="width: 251px; height: 201px;" /></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= m == matrix.length &lt;= 10<sup>3</sup></code></li>
    <li><code>1 &lt;= n == matrix[i].length &lt;= 10<sup>3</sup></code></li>
    <li><code>matrix[i][j]</code> là một trong các ký tự <code>&#39;#&#39;</code>, <code>&#39;.&#39;</code> hoặc chữ cái tiếng Anh viết hoa.</li>
    <li><code>matrix[0][0]</code> không phải là chướng ngại vật.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: 0-1 BFS

<!-- thinking:start -->

> **Tư duy**
>
> Một bước sang ô kề có chi phí $1$, còn dịch chuyển đến cổng cùng chữ cái có chi phí $0$; các bức tường không thể đi qua. Đường đi ngắn nhất trên các cạnh có trọng số $0$-$1$ phù hợp với 0-1 BFS thay vì Dijkstra tổng quát.
>
> Lập chỉ mục các cổng theo chữ cái. Lần đầu tiên gặp một chữ cái, đưa mọi cổng khác cùng chữ cái vào đầu deque rồi xóa chữ cái đó để không sử dụng lại. Các bước di chuyển thông thường được đưa vào cuối deque.

<!-- thinking:end -->

Ta có thể dùng 0-1 BFS để giải bài toán này. Ta bắt đầu từ ô trên cùng bên trái và dùng một deque để lưu tọa độ của ô hiện tại. Mỗi khi lấy một ô ra khỏi deque, ta kiểm tra bốn ô kề của nó. Nếu ô kề là ô trống và chưa được thăm, ta thêm ô đó vào deque và cập nhật khoảng cách.

Nếu ô kề là một cổng, ta thêm nó vào đầu deque và cập nhật khoảng cách. Ta cũng cần duy trì một dictionary lưu vị trí của từng cổng để có thể nhanh chóng tìm các vị trí đó khi sử dụng cổng.

Ta cũng cần một mảng 2D để lưu khoảng cách đến từng ô, được khởi tạo bằng vô cực. Ta đặt khoảng cách của điểm bắt đầu bằng 0 rồi bắt đầu BFS.

Trong quá trình BFS, ta kiểm tra xem mỗi ô có phải là đích hay không. Nếu đúng, ta trả về khoảng cách của ô đó. Nếu deque rỗng mà chưa đến được đích, ta trả về -1.

Độ phức tạp thời gian là $O(m \times n)$, và độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minMoves(self, matrix: List[str]) -> int:
        m, n = len(matrix), len(matrix[0])
        g = defaultdict(list)
        for i, row in enumerate(matrix):
            for j, c in enumerate(row):
                if c.isalpha():
                    g[c].append((i, j))
        dirs = (-1, 0, 1, 0, -1)
        dist = [[inf] * n for _ in range(m)]
        dist[0][0] = 0
        q = deque([(0, 0)])
        while q:
            i, j = q.popleft()
            d = dist[i][j]
            if i == m - 1 and j == n - 1:
                return d
            c = matrix[i][j]
            if c in g:
                for x, y in g[c]:
                    if d < dist[x][y]:
                        dist[x][y] = d
                        q.appendleft((x, y))
                del g[c]
            for a, b in pairwise(dirs):
                x, y = i + a, j + b
                if (
                    0 <= x < m
                    and 0 <= y < n
                    and matrix[x][y] != "#"
                    and d + 1 < dist[x][y]
                ):
                    dist[x][y] = d + 1
                    q.append((x, y))
        return -1
```

#### Java

```java
class Solution {
    public int minMoves(String[] matrix) {
        int m = matrix.length, n = matrix[0].length();
        Map<Character, List<int[]>> g = new HashMap<>();
        for (int i = 0; i < m; i++) {
            String row = matrix[i];
            for (int j = 0; j < n; j++) {
                char c = row.charAt(j);
                if (Character.isAlphabetic(c)) {
                    g.computeIfAbsent(c, k -> new ArrayList<>()).add(new int[] {i, j});
                }
            }
        }
        int[] dirs = {-1, 0, 1, 0, -1};
        int INF = Integer.MAX_VALUE / 2;
        int[][] dist = new int[m][n];
        for (int[] arr : dist) Arrays.fill(arr, INF);
        dist[0][0] = 0;
        Deque<int[]> q = new ArrayDeque<>();
        q.add(new int[] {0, 0});
        while (!q.isEmpty()) {
            int[] cur = q.pollFirst();
            int i = cur[0], j = cur[1];
            int d = dist[i][j];
            if (i == m - 1 && j == n - 1) return d;
            char c = matrix[i].charAt(j);
            if (g.containsKey(c)) {
                for (int[] pos : g.get(c)) {
                    int x = pos[0], y = pos[1];
                    if (d < dist[x][y]) {
                        dist[x][y] = d;
                        q.addFirst(new int[] {x, y});
                    }
                }
                g.remove(c);
            }
            for (int idx = 0; idx < 4; idx++) {
                int a = dirs[idx], b = dirs[idx + 1];
                int x = i + a, y = j + b;
                if (0 <= x && x < m && 0 <= y && y < n && matrix[x].charAt(y) != '#'
                    && d + 1 < dist[x][y]) {
                    dist[x][y] = d + 1;
                    q.addLast(new int[] {x, y});
                }
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minMoves(vector<string>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        unordered_map<char, vector<pair<int, int>>> g;
        for (int i = 0; i < m; ++i)
            for (int j = 0; j < n; ++j) {
                char c = matrix[i][j];
                if (isalpha(c)) g[c].push_back({i, j});
            }
        int dirs[5] = {-1, 0, 1, 0, -1};
        int INF = numeric_limits<int>::max() / 2;
        vector<vector<int>> dist(m, vector<int>(n, INF));
        dist[0][0] = 0;
        deque<pair<int, int>> q;
        q.push_back({0, 0});
        while (!q.empty()) {
            auto [i, j] = q.front();
            q.pop_front();
            int d = dist[i][j];
            if (i == m - 1 && j == n - 1) return d;
            char c = matrix[i][j];
            if (g.count(c)) {
                for (auto [x, y] : g[c])
                    if (d < dist[x][y]) {
                        dist[x][y] = d;
                        q.push_front({x, y});
                    }
                g.erase(c);
            }
            for (int idx = 0; idx < 4; ++idx) {
                int x = i + dirs[idx], y = j + dirs[idx + 1];
                if (0 <= x && x < m && 0 <= y && y < n && matrix[x][y] != '#' && d + 1 < dist[x][y]) {
                    dist[x][y] = d + 1;
                    q.push_back({x, y});
                }
            }
        }
        return -1;
    }
};
```

#### Go

```go
type pair struct{ x, y int }

func minMoves(matrix []string) int {
    m, n := len(matrix), len(matrix[0])
    g := make(map[rune][]pair)
    for i := 0; i < m; i++ {
        for j, c := range matrix[i] {
            if unicode.IsLetter(c) {
                g[c] = append(g[c], pair{i, j})
            }
        }
    }
    dirs := []int{-1, 0, 1, 0, -1}
    INF := 1 << 30
    dist := make([][]int, m)
    for i := range dist {
        dist[i] = make([]int, n)
        for j := range dist[i] {
            dist[i][j] = INF
        }
    }
    dist[0][0] = 0
    q := list.New()
    q.PushBack(pair{0, 0})
    for q.Len() > 0 {
        cur := q.Remove(q.Front()).(pair)
        i, j := cur.x, cur.y
        d := dist[i][j]
        if i == m-1 && j == n-1 {
            return d
        }
        c := rune(matrix[i][j])
        if v, ok := g[c]; ok {
            for _, p := range v {
                x, y := p.x, p.y
                if d < dist[x][y] {
                    dist[x][y] = d
                    q.PushFront(pair{x, y})
                }
            }
            delete(g, c)
        }
        for idx := 0; idx < 4; idx++ {
            x, y := i+dirs[idx], j+dirs[idx+1]
            if 0 <= x && x < m && 0 <= y && y < n && matrix[x][y] != '#' && d+1 < dist[x][y] {
                dist[x][y] = d + 1
                q.PushBack(pair{x, y})
            }
        }
    }
    return -1
}
```

#### TypeScript

```ts
function minMoves(matrix: string[]): number {
    const m = matrix.length,
        n = matrix[0].length;
    const g = new Map<string, [number, number][]>();
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            const c = matrix[i][j];
            if (/^[A-Za-z]$/.test(c)) {
                if (!g.has(c)) g.set(c, []);
                g.get(c)!.push([i, j]);
            }
        }
    }

    const dirs = [-1, 0, 1, 0, -1];
    const INF = Number.MAX_SAFE_INTEGER;
    const dist: number[][] = Array.from({ length: m }, () => Array(n).fill(INF));
    dist[0][0] = 0;

    const cap = m * n * 2 + 5;
    const dq = new Array<[number, number]>(cap);
    let l = cap >> 1,
        r = cap >> 1;
    const pushFront = (v: [number, number]) => {
        dq[--l] = v;
    };
    const pushBack = (v: [number, number]) => {
        dq[r++] = v;
    };
    const popFront = (): [number, number] => dq[l++];
    const empty = () => l === r;

    pushBack([0, 0]);

    while (!empty()) {
        const [i, j] = popFront();
        const d = dist[i][j];
        if (i === m - 1 && j === n - 1) return d;

        const c = matrix[i][j];
        if (g.has(c)) {
            for (const [x, y] of g.get(c)!) {
                if (d < dist[x][y]) {
                    dist[x][y] = d;
                    pushFront([x, y]);
                }
            }
            g.delete(c);
        }

        for (let idx = 0; idx < 4; idx++) {
            const x = i + dirs[idx],
                y = j + dirs[idx + 1];
            if (0 <= x && x < m && 0 <= y && y < n && matrix[x][y] !== '#' && d + 1 < dist[x][y]) {
                dist[x][y] = d + 1;
                pushBack([x, y]);
            }
        }
    }
    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
