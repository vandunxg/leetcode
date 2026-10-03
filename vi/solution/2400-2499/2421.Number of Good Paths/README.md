---
comments: true
difficulty: Hard
rating: 2444
source: Weekly Contest 312 Q4
tags:
    - Tree
    - Union Find
    - Graph
    - Array
    - Hash Table
    - Sorting
---

<!-- problem:start -->

# [2421. Number of Good Paths](https://leetcode.com/problems/number-of-good-paths)

[中文文档](/solution/2400-2499/2421.Number%20of%20Good%20Paths/README.md)

## Mô tả

<!-- description:start -->

<p>Có một cây (tức là một đồ thị liên thông, vô hướng và không có chu trình) gồm <code>n</code> đỉnh được đánh số từ <code>0</code> đến <code>n - 1</code> và có đúng <code>n - 1</code> cạnh.</p>

<p>Bạn được cho một mảng số nguyên <code>vals</code> có độ dài <code>n</code>, được đánh chỉ số từ <strong>0</strong>, trong đó <code>vals[i]</code> biểu thị giá trị của đỉnh thứ <code>i<sup>th</sup></code>. Bạn cũng được cho một mảng số nguyên 2 chiều <code>edges</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> biểu thị có một cạnh <strong>vô hướng</strong> nối đỉnh <code>a<sub>i</sub></code> và đỉnh <code>b<sub>i</sub></code>.</p>

<p>Một <strong>đường đi tốt</strong> là một đường đi đơn thỏa mãn các điều kiện sau:</p>

<ol>
	<li>Đỉnh bắt đầu và đỉnh kết thúc có <strong>cùng</strong> giá trị.</li>
	<li>Mọi đỉnh nằm giữa đỉnh bắt đầu và đỉnh kết thúc đều có giá trị <strong>nhỏ hơn hoặc bằng</strong> giá trị của đỉnh bắt đầu (tức là giá trị của đỉnh bắt đầu phải là giá trị lớn nhất trên đường đi).</li>
</ol>

<p>Hãy trả về <em>số lượng đường đi tốt phân biệt</em>.</p>

<p>Lưu ý rằng một đường đi và đường đi ngược lại được tính là <strong>cùng một</strong> đường đi. Ví dụ, <code>0 -&gt; 1</code> được xem là giống với <code>1 -&gt; 0</code>. Một đỉnh đơn lẻ cũng được xem là một đường đi hợp lệ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2421.Number%20of%20Good%20Paths/images/f9caaac15b383af9115c5586779dec5.png" style="width: 400px; height: 333px;" />
<pre>
<strong>Đầu vào:</strong> vals = [1,3,2,1,3], edges = [[0,1],[0,2],[2,3],[2,4]]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Có 5 đường đi tốt chỉ gồm một đỉnh.
Có thêm 1 đường đi tốt: 1 -&gt; 0 -&gt; 2 -&gt; 4.
(Đường đi ngược lại 4 -&gt; 2 -&gt; 0 -&gt; 1 được xem là giống với 1 -&gt; 0 -&gt; 2 -&gt; 4.)
Lưu ý rằng 0 -&gt; 2 -&gt; 3 không phải là đường đi tốt vì vals[2] &gt; vals[0].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2421.Number%20of%20Good%20Paths/images/149d3065ec165a71a1b9aec890776ff.png" style="width: 273px; height: 350px;" />
<pre>
<strong>Đầu vào:</strong> vals = [1,1,2,2,3], edges = [[0,1],[1,2],[2,3],[2,4]]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Có 5 đường đi tốt chỉ gồm một đỉnh.
Có thêm 2 đường đi tốt: 0 -&gt; 1 và 2 -&gt; 3.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2421.Number%20of%20Good%20Paths/images/31705e22af3d9c0a557459bc7d1b62d.png" style="width: 100px; height: 88px;" />
<pre>
<strong>Đầu vào:</strong> vals = [1], edges = []
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Cây chỉ gồm một đỉnh, nên có một đường đi tốt.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == vals.length</code></li>
	<li><code>1 &lt;= n &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= vals[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li><code>edges</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Union Find

<!-- thinking:start -->

> **Tư duy**
>
> Trong một đường đi tốt, hai đầu mút không nhỏ hơn bất kỳ đỉnh nào trên đường đi. Việc liệt kê các đường đi không phù hợp với $n\le 3\times 10^4$. Khi thêm các đỉnh theo thứ tự giá trị tăng dần, đỉnh mới là đỉnh lớn nhất trong thành phần của nó, nên các đường đi có hai đầu mút cùng giá trị đó chính là những đường đi tốt được tạo ra ở bước hiện tại.
>
> Sắp xếp theo $vals$ và dùng Union Find: chỉ gộp các đỉnh kề có giá trị không lớn hơn giá trị hiện tại; cộng tích số lượng đỉnh có giá trị $v$ trong hai thành phần. Ban đầu, có $n$ đường đi chỉ gồm một đỉnh.

<!-- thinking:end -->

Để đảm bảo đỉnh bắt đầu (hoặc đỉnh kết thúc) của đường đi lớn hơn hoặc bằng mọi đỉnh trên đường đi, trước hết ta sắp xếp tất cả các đỉnh theo thứ tự tăng dần, sau đó lần lượt thêm chúng vào các thành phần liên thông, cụ thể như sau:

Khi duyệt đến đỉnh $a$, với đỉnh kề $b$ có giá trị nhỏ hơn hoặc bằng $vals[a]$, nếu chúng không thuộc cùng một thành phần liên thông thì ta gộp chúng. Ta có thể chọn mọi đỉnh trong thành phần liên thông chứa đỉnh $a$ có giá trị $vals[a]$ làm đỉnh bắt đầu, và mọi đỉnh trong thành phần liên thông chứa đỉnh $b$ có giá trị $vals[a]$ làm đỉnh kết thúc. Tích của số lượng hai loại đỉnh này là phần đóng góp vào đáp án khi thêm đỉnh $a$.

Độ phức tạp thời gian là $O(n \times \log n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfGoodPaths(self, vals: List[int], edges: List[List[int]]) -> int:
        def find(x):
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        g = defaultdict(list)
        for a, b in edges:
            g[a].append(b)
            g[b].append(a)

        n = len(vals)
        p = list(range(n))
        size = defaultdict(Counter)
        for i, v in enumerate(vals):
            size[i][v] = 1

        ans = n
        for v, a in sorted(zip(vals, range(n))):
            for b in g[a]:
                if vals[b] > v:
                    continue
                pa, pb = find(a), find(b)
                if pa != pb:
                    ans += size[pa][v] * size[pb][v]
                    p[pa] = pb
                    size[pb][v] += size[pa][v]
        return ans
```

#### Java

```java
class Solution {
    private int[] p;

    public int numberOfGoodPaths(int[] vals, int[][] edges) {
        int n = vals.length;
        p = new int[n];
        int[][] arr = new int[n][2];
        List<Integer>[] g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int[] e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        Map<Integer, Map<Integer, Integer>> size = new HashMap<>();
        for (int i = 0; i < n; ++i) {
            p[i] = i;
            arr[i] = new int[] {vals[i], i};
            size.computeIfAbsent(i, k -> new HashMap<>()).put(vals[i], 1);
        }
        Arrays.sort(arr, (a, b) -> a[0] - b[0]);
        int ans = n;
        for (var e : arr) {
            int v = e[0], a = e[1];
            for (int b : g[a]) {
                if (vals[b] > v) {
                    continue;
                }
                int pa = find(a), pb = find(b);
                if (pa != pb) {
                    ans += size.get(pa).getOrDefault(v, 0) * size.get(pb).getOrDefault(v, 0);
                    p[pa] = pb;
                    size.get(pb).put(
                        v, size.get(pb).getOrDefault(v, 0) + size.get(pa).getOrDefault(v, 0));
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
    int numberOfGoodPaths(vector<int>& vals, vector<vector<int>>& edges) {
        int n = vals.size();
        vector<int> p(n);
        iota(p.begin(), p.end(), 0);
        function<int(int)> find;
        find = [&](int x) {
            if (p[x] != x) {
                p[x] = find(p[x]);
            }
            return p[x];
        };
        vector<vector<int>> g(n);
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].push_back(b);
            g[b].push_back(a);
        }
        unordered_map<int, unordered_map<int, int>> size;
        vector<pair<int, int>> arr(n);
        for (int i = 0; i < n; ++i) {
            arr[i] = {vals[i], i};
            size[i][vals[i]] = 1;
        }
        sort(arr.begin(), arr.end());
        int ans = n;
        for (auto [v, a] : arr) {
            for (int b : g[a]) {
                if (vals[b] > v) {
                    continue;
                }
                int pa = find(a), pb = find(b);
                if (pa != pb) {
                    ans += size[pa][v] * size[pb][v];
                    p[pa] = pb;
                    size[pb][v] += size[pa][v];
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfGoodPaths(vals []int, edges [][]int) int {
	n := len(vals)
	p := make([]int, n)
	size := map[int]map[int]int{}
	type pair struct{ v, i int }
	arr := make([]pair, n)
	for i, v := range vals {
		p[i] = i
		if size[i] == nil {
			size[i] = map[int]int{}
		}
		size[i][v] = 1
		arr[i] = pair{v, i}
	}

	var find func(x int) int
	find = func(x int) int {
		if p[x] != x {
			p[x] = find(p[x])
		}
		return p[x]
	}

	sort.Slice(arr, func(i, j int) bool { return arr[i].v < arr[j].v })
	g := make([][]int, n)
	for _, e := range edges {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	ans := n
	for _, e := range arr {
		v, a := e.v, e.i
		for _, b := range g[a] {
			if vals[b] > v {
				continue
			}
			pa, pb := find(a), find(b)
			if pa != pb {
				ans += size[pb][v] * size[pa][v]
				p[pa] = pb
				size[pb][v] += size[pa][v]
			}
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
