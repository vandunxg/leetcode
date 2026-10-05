---
comments: true
difficulty: Hard
rating: 2178
source: Biweekly Contest 174 Q4
tags:
    - Tree
    - Depth-First Search
    - Graph
    - Topological Sort
    - Sorting
---

<!-- problem:start -->

# [3812. Minimum Edge Toggles on a Tree](https://leetcode.com/problems/minimum-edge-toggles-on-a-tree)

[中文文档](/solution/3800-3899/3812.Minimum%20Edge%20Toggles%20on%20a%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một <strong>cây vô hướng</strong> gồm <code>n</code> đỉnh, được đánh số từ 0 đến <code>n - 1</code>. Cây được biểu diễn bằng một mảng số nguyên 2 chiều <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> cho biết có một cạnh nối đỉnh <code>a<sub>i</sub></code> và đỉnh <code>b<sub>i</sub></code> trong cây.</p>

<p>Bạn cũng được cho hai chuỗi <strong>nhị phân</strong> <code>start</code> và <code>target</code> có độ dài <code>n</code>. Với mỗi đỉnh <code>x</code>, <code>start[x]</code> là màu ban đầu và <code>target[x]</code> là màu mong muốn của đỉnh đó.</p>

<p>Trong một thao tác, bạn có thể chọn một cạnh có chỉ số <code>i</code> và <strong>toggle</strong> cả hai đầu mút của nó. Nghĩa là, nếu cạnh đó là <code>[u, v]</code>, màu của hai đỉnh <code>u</code> và <code>v</code> sẽ <strong>đều</strong> chuyển từ <code>&#39;0&#39;</code> sang <code>&#39;1&#39;</code> hoặc từ <code>&#39;1&#39;</code> sang <code>&#39;0&#39;</code>.</p>

<p>Hãy trả về một mảng chứa các chỉ số cạnh mà khi thực hiện các thao tác tương ứng sẽ biến <code>start</code> thành <code>target</code>. Trong số tất cả các dãy hợp lệ có <strong>độ dài nhỏ nhất có thể</strong>, hãy trả về các chỉ số cạnh theo thứ tự <strong>tăng dần</strong>.</p>

<p>Nếu không thể biến đổi <code>start</code> thành <code>target</code>, hãy trả về một mảng chứa duy nhất phần tử bằng -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3800-3899/3812.Minimum%20Edge%20Toggles%20on%20a%20Tree/images/example1.png" style="width: 271px; height: 51px;" />​​​​​​​</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1],[1,2]], start = &quot;010&quot;, target = &quot;100&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Toggle cạnh có chỉ số 0, thao tác này lật các đỉnh 0 và 1.<br />
​​​​​​​Chuỗi thay đổi từ <code>&quot;010&quot;</code> thành <code>&quot;100&quot;</code>, khớp với target.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3800-3899/3812.Minimum%20Edge%20Toggles%20on%20a%20Tree/images/example2.png" style="width: 411px; height: 208px;" /></strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 7, edges = [[0,1],[1,2],[2,3],[3,4],[3,5],[1,6]], start = &quot;0011000&quot;, target = &quot;0010001&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,2,5]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Toggle cạnh có chỉ số 1, thao tác này lật các đỉnh 1 và 2.</li>
	<li>Toggle cạnh có chỉ số 2, thao tác này lật các đỉnh 2 và 3.</li>
	<li>Toggle cạnh có chỉ số 5, thao tác này lật các đỉnh 1 và 6.</li>
</ul>

<p>Sau các thao tác này, chuỗi kết quả trở thành <code>&quot;0010001&quot;</code>, khớp với target.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3800-3899/3812.Minimum%20Edge%20Toggles%20on%20a%20Tree/images/example3.png" style="width: 161px; height: 51px;" />​​​​​​​</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, edges = [[0,1]], start = &quot;00&quot;, target = &quot;01&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[-1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có dãy thao tác toggle cạnh nào có thể biến <code>&quot;00&quot;</code> thành <code>&quot;01&quot;</code>. Vì vậy, ta trả về <code>[-1]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n == start.length == target.length &lt;= 10<sup>5</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
	<li><code>start[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
	<li><code>target[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
	<li>Dữ liệu đầu vào được tạo sao cho <code>edges</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Toggle một cạnh sẽ lật cả hai đầu mút. $n \le 10^5$ loại trừ việc duyệt các tập con. Trên cây, nhu cầu bên trong một cây con được quyết định theo chiều từ dưới lên.
>
> Một lá không khớp với target phải toggle cạnh nối nó với cha, từ đó lật nhu cầu còn lại của cha, nên nhu cầu được truyền lên trên.
>
> DFS trả về việc cây con còn cần toggle cạnh nối với cha hay không: mỗi cạnh con được chọn sẽ đảo nhu cầu hiện tại của đỉnh.
>
> Nếu gốc vẫn cần một lần lật, không có lời giải; ngược lại, các chỉ số cạnh được chọn sau khi sắp xếp tạo thành một dãy hợp lệ ngắn nhất.

<!-- thinking:end -->

Ta định nghĩa một adjacency list $g$ để biểu diễn cây, trong đó $g[a]$ lưu tất cả các đỉnh kề với đỉnh $a$ cùng với chỉ số của các cạnh tương ứng.

Ta xây dựng hàm $\text{dfs}(a, \text{fa})$, hàm này cho biết cạnh giữa đỉnh $a$ và $\text{fa}$ có cần được toggle trong cây con bắt nguồn từ đỉnh $a$ với cha là $\text{fa}$ hay không. Logic của hàm $\text{dfs}(a, \text{fa})$ như sau:

1. Khởi tạo một biến boolean $\text{rev}$, cho biết đỉnh $a$ có cần được toggle hay không. Giá trị ban đầu là $\text{start}[a] \ne \text{target}[a]$.
2. Duyệt qua tất cả các đỉnh kề $b$ của đỉnh $a$ và chỉ số cạnh tương ứng $i$:
    - Nếu $b \ne \text{fa}$, gọi đệ quy $\text{dfs}(b, a)$.
    - Nếu lời gọi đệ quy trả về true, điều đó có nghĩa là cạnh $[a, b]$ trong cây con cần được toggle. Ta thêm chỉ số cạnh $i$ vào danh sách đáp án và đảo $\text{rev}$.
3. Trả về $\text{rev}$.

Cuối cùng, ta gọi $\text{dfs}(0, -1)$. Nếu giá trị trả về là true, điều đó có nghĩa là không thể biến đổi $\text{start}$ thành $\text{target}$, nên ta trả về $[-1]$. Nếu không, ta sắp xếp danh sách đáp án rồi trả về danh sách đó.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số đỉnh của cây.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumFlips(
        self, n: int, edges: List[List[int]], start: str, target: str
    ) -> List[int]:
        g = [[] for _ in range(n)]
        for i, (a, b) in enumerate(edges):
            g[a].append((b, i))
            g[b].append((a, i))

        ans = []

        def dfs(a: int, fa: int) -> bool:
            rev = start[a] != target[a]
            for b, i in g[a]:
                if b != fa and dfs(b, a):
                    ans.append(i)
                    rev = not rev
            return rev

        if dfs(0, -1):
            return [-1]
        ans.sort()
        return ans
```

#### Java

```java
class Solution {
    private final List<Integer> ans = new ArrayList<>();
    private List<int[]>[] g;
    private char[] start;
    private char[] target;

    public List<Integer> minimumFlips(int n, int[][] edges, String start, String target) {
        g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int i = 0; i < n - 1; ++i) {
            int a = edges[i][0], b = edges[i][1];
            g[a].add(new int[] {b, i});
            g[b].add(new int[] {a, i});
        }
        this.start = start.toCharArray();
        this.target = target.toCharArray();
        if (dfs(0, -1)) {
            return List.of(-1);
        }
        Collections.sort(ans);
        return ans;
    }

    private boolean dfs(int a, int fa) {
        boolean rev = start[a] != target[a];
        for (var e : g[a]) {
            int b = e[0], i = e[1];
            if (b != fa && dfs(b, a)) {
                ans.add(i);
                rev = !rev;
            }
        }
        return rev;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> minimumFlips(int n, vector<vector<int>>& edges, string start, string target) {
        vector<pair<int, int>> g[n];
        for (int i = 0; i < n - 1; ++i) {
            int a = edges[i][0], b = edges[i][1];
            g[a].push_back({b, i});
            g[b].push_back({a, i});
        }
        vector<int> ans;
        auto dfs = [&](this auto&& dfs, int a, int fa) -> bool {
            bool rev = start[a] != target[a];
            for (auto [b, i] : g[a]) {
                if (b != fa && dfs(b, a)) {
                    ans.push_back(i);
                    rev = !rev;
                }
            }
            return rev;
        };
        if (dfs(0, -1)) {
            return {-1};
        }
        ranges::sort(ans);
        return ans;
    }
};
```

#### Go

```go
func minimumFlips(n int, edges [][]int, start string, target string) []int {
	g := make([][]struct{ to, idx int }, n)
	for i := 0; i < n-1; i++ {
		a, b := edges[i][0], edges[i][1]
		g[a] = append(g[a], struct{ to, idx int }{b, i})
		g[b] = append(g[b], struct{ to, idx int }{a, i})
	}
	ans := []int{}
	var dfs func(a, fa int) bool
	dfs = func(a, fa int) bool {
		rev := start[a] != target[a]
		for _, p := range g[a] {
			b, i := p.to, p.idx
			if b != fa && dfs(b, a) {
				ans = append(ans, i)
				rev = !rev
			}
		}
		return rev
	}
	if dfs(0, -1) {
		return []int{-1}
	}
	sort.Ints(ans)
	return ans
}
```

#### TypeScript

```ts
function minimumFlips(n: number, edges: number[][], start: string, target: string): number[] {
    const g: number[][][] = Array.from({ length: n }, () => []);
    for (let i = 0; i < n - 1; i++) {
        const [a, b] = edges[i];
        g[a].push([b, i]);
        g[b].push([a, i]);
    }
    const ans: number[] = [];
    const dfs = (a: number, fa: number): boolean => {
        let rev = start[a] !== target[a];
        for (const [b, i] of g[a]) {
            if (b !== fa && dfs(b, a)) {
                ans.push(i);
                rev = !rev;
            }
        }
        return rev;
    };
    if (dfs(0, -1)) {
        return [-1];
    }
    ans.sort((x, y) => x - y);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
