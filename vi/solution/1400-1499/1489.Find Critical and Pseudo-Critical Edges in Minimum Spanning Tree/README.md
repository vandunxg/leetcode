---
comments: true
difficulty: Hard
rating: 2571
source: Weekly Contest 194 Q4
tags:
    - Union Find
    - Graph
    - Kruskal
    - Minimum Spanning Tree
    - Prim
    - Sorting
    - Strongly Connected Component
    - Borůvka
---

<!-- problem:start -->

# [1489. Find Critical and Pseudo-Critical Edges in Minimum Spanning Tree](https://leetcode.com/problems/find-critical-and-pseudo-critical-edges-in-minimum-spanning-tree)

[中文文档](/solution/1400-1499/1489.Find%20Critical%20and%20Pseudo-Critical%20Edges%20in%20Minimum%20Spanning%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một đồ thị vô hướng liên thông có trọng số với <code>n</code>&nbsp;đỉnh được đánh số từ <code>0</code> đến <code>n - 1</code>,&nbsp;và một mảng <code>edges</code>&nbsp;trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>, weight<sub>i</sub>]</code> biểu diễn một cạnh hai chiều có trọng số giữa các nút&nbsp;<code>a<sub>i</sub></code>&nbsp;và <code>b<sub>i</sub></code>. Cây khung nhỏ nhất (MST) là một tập con các cạnh của đồ thị kết nối tất cả các đỉnh mà không tạo chu trình&nbsp;và có tổng trọng số nhỏ nhất có thể.</p>

<p>Hãy tìm <em>tất cả các cạnh critical và pseudo-critical trong cây khung nhỏ nhất (MST) của đồ thị đã cho</em>. Một cạnh trong MST mà khi xóa khỏi đồ thị sẽ làm tăng trọng số của MST được gọi là&nbsp;<em>cạnh critical</em>. Mặt khác, cạnh pseudo-critical là cạnh có thể xuất hiện trong một số MST nhưng không phải tất cả.</p>

<p>Lưu ý rằng bạn có thể trả về chỉ số của các cạnh theo bất kỳ thứ tự nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1489.Find%20Critical%20and%20Pseudo-Critical%20Edges%20in%20Minimum%20Spanning%20Tree/images/ex1.png" style="width: 259px; height: 262px;" /></p>

<pre>
<strong>Đầu vào:</strong> n = 5, edges = [[0,1,1],[1,2,1],[2,3,2],[0,3,2],[0,4,3],[3,4,3],[1,4,6]]
<strong>Đầu ra:</strong> [[0,1],[2,3,4,5]]
<strong>Giải thích:</strong> Hình trên mô tả đồ thị.
Hình sau đây hiển thị tất cả các MST có thể có:
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1489.Find%20Critical%20and%20Pseudo-Critical%20Edges%20in%20Minimum%20Spanning%20Tree/images/msts.png" style="width: 540px; height: 553px;" />
Lưu ý rằng hai cạnh 0 và 1 xuất hiện trong mọi MST, do đó chúng là các cạnh critical và được trả về trong danh sách đầu tiên của kết quả.
Các cạnh 2, 3, 4 và 5 chỉ thuộc một số MST, do đó chúng được xem là các cạnh pseudo-critical. Chúng được thêm vào danh sách thứ hai của kết quả.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1489.Find%20Critical%20and%20Pseudo-Critical%20Edges%20in%20Minimum%20Spanning%20Tree/images/ex2.png" style="width: 247px; height: 253px;" /></p>

<pre>
<strong>Đầu vào:</strong> n = 4, edges = [[0,1,1],[1,2,1],[2,3,1],[0,3,1]]
<strong>Đầu ra:</strong> [[],[0,1,2,3]]
<strong>Giải thích:</strong> Có thể thấy rằng vì cả 4 cạnh đều có cùng trọng số, chọn bất kỳ 3 cạnh nào trong 4 cạnh đã cho cũng tạo ra một MST. Do đó, cả 4 cạnh đều là pseudo-critical.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= edges.length &lt;= min(200, n * (n - 1) / 2)</code></li>
	<li><code>edges[i].length == 3</code></li>
	<li><code>0 &lt;= a<sub>i</sub> &lt; b<sub>i</sub> &lt; n</code></li>
	<li><code>1 &lt;= weight<sub>i</sub>&nbsp;&lt;= 1000</code></li>
	<li>Tất cả các cặp <code>(a<sub>i</sub>, b<sub>i</sub>)</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n\le 100$ và có nhiều nhất $200$ cạnh, ta có thể chạy MST nhiều lần. Trước hết, Kruskal cho trọng số MST là $v$.
>
> Một cạnh là critical khi và chỉ khi việc xóa cạnh đó làm đồ thị mất liên thông hoặc làm trọng số tăng lên trên $v$. Nếu không, cạnh đó là pseudo-critical khi và chỉ khi ép chọn nó vẫn đạt được $v$. Kiểm tra cả hai điều kiện cho từng cạnh.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class UnionFind:
    def __init__(self, n):
        self.p = list(range(n))
        self.n = n

    def union(self, a, b):
        if self.find(a) == self.find(b):
            return False
        self.p[self.find(a)] = self.find(b)
        self.n -= 1
        return True

    def find(self, x):
        if self.p[x] != x:
            self.p[x] = self.find(self.p[x])
        return self.p[x]


class Solution:
    def findCriticalAndPseudoCriticalEdges(
        self, n: int, edges: List[List[int]]
    ) -> List[List[int]]:
        for i, e in enumerate(edges):
            e.append(i)
        edges.sort(key=lambda x: x[2])
        uf = UnionFind(n)
        v = sum(w for f, t, w, _ in edges if uf.union(f, t))
        ans = [[], []]
        for f, t, w, i in edges:
            uf = UnionFind(n)
            k = sum(z for x, y, z, j in edges if j != i and uf.union(x, y))
            if uf.n > 1 or (uf.n == 1 and k > v):
                ans[0].append(i)
                continue

            uf = UnionFind(n)
            uf.union(f, t)
            k = w + sum(z for x, y, z, j in edges if j != i and uf.union(x, y))
            if k == v:
                ans[1].append(i)
        return ans
```

#### Java

```java
class Solution {
    public List<List<Integer>> findCriticalAndPseudoCriticalEdges(int n, int[][] edges) {
        for (int i = 0; i < edges.length; ++i) {
            int[] e = edges[i];
            int[] t = new int[4];
            System.arraycopy(e, 0, t, 0, 3);
            t[3] = i;
            edges[i] = t;
        }
        Arrays.sort(edges, Comparator.comparingInt(a -> a[2]));
        int v = 0;
        UnionFind uf = new UnionFind(n);
        for (int[] e : edges) {
            int f = e[0], t = e[1], w = e[2];
            if (uf.union(f, t)) {
                v += w;
            }
        }
        List<List<Integer>> ans = new ArrayList<>();
        for (int i = 0; i < 2; ++i) {
            ans.add(new ArrayList<>());
        }
        for (int[] e : edges) {
            int f = e[0], t = e[1], w = e[2], i = e[3];
            uf = new UnionFind(n);
            int k = 0;
            for (int[] ne : edges) {
                int x = ne[0], y = ne[1], z = ne[2], j = ne[3];
                if (j != i && uf.union(x, y)) {
                    k += z;
                }
            }
            if (uf.getN() > 1 || (uf.getN() == 1 && k > v)) {
                ans.get(0).add(i);
                continue;
            }
            uf = new UnionFind(n);
            uf.union(f, t);
            k = w;
            for (int[] ne : edges) {
                int x = ne[0], y = ne[1], z = ne[2], j = ne[3];
                if (j != i && uf.union(x, y)) {
                    k += z;
                }
            }
            if (k == v) {
                ans.get(1).add(i);
            }
        }
        return ans;
    }
}

class UnionFind {
    private int[] p;
    private int n;

    public UnionFind(int n) {
        p = new int[n];
        this.n = n;
        for (int i = 0; i < n; ++i) {
            p[i] = i;
        }
    }

    public int getN() {
        return n;
    }

    public boolean union(int a, int b) {
        if (find(a) == find(b)) {
            return false;
        }
        p[find(a)] = find(b);
        --n;
        return true;
    }

    public int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }
}
```

#### C++

```cpp
class UnionFind {
public:
    vector<int> p;
    int n;

    UnionFind(int _n)
        : n(_n)
        , p(_n) {
        iota(p.begin(), p.end(), 0);
    }

    bool unite(int a, int b) {
        if (find(a) == find(b)) return false;
        p[find(a)] = find(b);
        --n;
        return true;
    }

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }
};

class Solution {
public:
    vector<vector<int>> findCriticalAndPseudoCriticalEdges(int n, vector<vector<int>>& edges) {
        for (int i = 0; i < edges.size(); ++i) edges[i].push_back(i);
        sort(edges.begin(), edges.end(), [](auto& a, auto& b) { return a[2] < b[2]; });
        int v = 0;
        UnionFind uf(n);
        for (auto& e : edges) {
            int f = e[0], t = e[1], w = e[2];
            if (uf.unite(f, t)) v += w;
        }
        vector<vector<int>> ans(2);
        for (auto& e : edges) {
            int f = e[0], t = e[1], w = e[2], i = e[3];
            UnionFind ufa(n);
            int k = 0;
            for (auto& ne : edges) {
                int x = ne[0], y = ne[1], z = ne[2], j = ne[3];
                if (j != i && ufa.unite(x, y)) k += z;
            }
            if (ufa.n > 1 || (ufa.n == 1 && k > v)) {
                ans[0].push_back(i);
                continue;
            }

            UnionFind ufb(n);
            ufb.unite(f, t);
            k = w;
            for (auto& ne : edges) {
                int x = ne[0], y = ne[1], z = ne[2], j = ne[3];
                if (j != i && ufb.unite(x, y)) k += z;
            }
            if (k == v) ans[1].push_back(i);
        }
        return ans;
    }
};
```

#### Go

```go
type unionFind struct {
	p []int
	n int
}

func newUnionFind(n int) *unionFind {
	p := make([]int, n)
	for i := range p {
		p[i] = i
	}
	return &unionFind{p, n}
}

func (uf *unionFind) find(x int) int {
	if uf.p[x] != x {
		uf.p[x] = uf.find(uf.p[x])
	}
	return uf.p[x]
}

func (uf *unionFind) union(a, b int) bool {
	if uf.find(a) == uf.find(b) {
		return false
	}
	uf.p[uf.find(a)] = uf.find(b)
	uf.n--
	return true
}

func findCriticalAndPseudoCriticalEdges(n int, edges [][]int) [][]int {
	for i := range edges {
		edges[i] = append(edges[i], i)
	}
	sort.Slice(edges, func(i, j int) bool {
		return edges[i][2] < edges[j][2]
	})
	v := 0
	uf := newUnionFind(n)
	for _, e := range edges {
		f, t, w := e[0], e[1], e[2]
		if uf.union(f, t) {
			v += w
		}
	}
	ans := make([][]int, 2)
	for _, e := range edges {
		f, t, w, i := e[0], e[1], e[2], e[3]
		uf = newUnionFind(n)
		k := 0
		for _, ne := range edges {
			x, y, z, j := ne[0], ne[1], ne[2], ne[3]
			if j != i && uf.union(x, y) {
				k += z
			}
		}
		if uf.n > 1 || (uf.n == 1 && k > v) {
			ans[0] = append(ans[0], i)
			continue
		}
		uf = newUnionFind(n)
		uf.union(f, t)
		k = w
		for _, ne := range edges {
			x, y, z, j := ne[0], ne[1], ne[2], ne[3]
			if j != i && uf.union(x, y) {
				k += z
			}
		}
		if k == v {
			ans[1] = append(ans[1], i)
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
