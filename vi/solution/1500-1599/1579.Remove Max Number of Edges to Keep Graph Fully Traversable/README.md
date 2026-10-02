---
comments: true
difficulty: Hard
rating: 2131
source: Weekly Contest 205 Q4
tags:
    - Union Find
    - Graph
---

<!-- problem:start -->

# [1579. Remove Max Number of Edges to Keep Graph Fully Traversable](https://leetcode.com/problems/remove-max-number-of-edges-to-keep-graph-fully-traversable)

[中文文档](/solution/1500-1599/1579.Remove%20Max%20Number%20of%20Edges%20to%20Keep%20Graph%20Fully%20Traversable/README.md)

## Mô tả

<!-- description:start -->

<p>Alice và Bob có một đồ thị vô hướng gồm <code>n</code> node và ba loại cạnh:</p>

<ul>
	<li>Loại 1: Chỉ Alice có thể đi qua.</li>
	<li>Loại 2: Chỉ Bob có thể đi qua.</li>
	<li>Loại 3: Cả Alice và Bob đều có thể đi qua.</li>
</ul>

<p>Cho mảng <code>edges</code>, trong đó <code>edges[i] = [type<sub>i</sub>, u<sub>i</sub>, v<sub>i</sub>]</code> biểu diễn một cạnh hai chiều loại <code>type<sub>i</sub></code> giữa node <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code>. Hãy tìm số cạnh lớn nhất có thể xóa sao cho sau khi xóa, cả Alice và Bob vẫn có thể đi qua toàn bộ đồ thị. Đồ thị được đi qua toàn bộ nếu từ bất kỳ node nào họ cũng có thể đến mọi node khác.</p>

<p>Trả về <em>số cạnh lớn nhất có thể xóa, hoặc trả về</em> <code>-1</code> <em>nếu Alice và Bob không thể đi qua toàn bộ đồ thị.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1579.Remove%20Max%20Number%20of%20Edges%20to%20Keep%20Graph%20Fully%20Traversable/images/ex1.png" style="width: 179px; height: 191px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4, edges = [[3,1,2],[3,2,3],[1,1,3],[1,2,4],[1,1,2],[2,3,4]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích: </strong>Nếu xóa 2 cạnh [1,1,2] và [1,1,3], cả Alice và Bob vẫn có thể đi qua toàn bộ đồ thị. Xóa thêm bất kỳ cạnh nào sẽ làm mất tính chất này. Vì vậy số cạnh lớn nhất có thể xóa là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1579.Remove%20Max%20Number%20of%20Edges%20to%20Keep%20Graph%20Fully%20Traversable/images/ex2.png" style="width: 178px; height: 190px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4, edges = [[3,1,2],[3,2,3],[1,1,4],[2,1,4]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích: </strong>Lưu ý rằng xóa bất kỳ cạnh nào cũng khiến Alice hoặc Bob không thể đi qua toàn bộ đồ thị.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1579.Remove%20Max%20Number%20of%20Edges%20to%20Keep%20Graph%20Fully%20Traversable/images/ex3.png" style="width: 178px; height: 190px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4, edges = [[3,2,3],[1,1,2],[2,3,4]]
<strong>Đầu ra:</strong> -1
<b>Giải thích: </b>Trong đồ thị hiện tại, Alice không thể đi từ các node khác đến node 4. Tương tự, Bob không thể đến node 1. Vì vậy không thể làm đồ thị được đi qua toàn bộ.</pre>

<p>&nbsp;</p>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= edges.length &lt;= min(10<sup>5</sup>, 3 * n * (n - 1) / 2)</code></li>
	<li><code>edges[i].length == 3</code></li>
	<li><code>1 &lt;= type<sub>i</sub> &lt;= 3</code></li>
	<li><code>1 &lt;= u<sub>i</sub> &lt; v<sub>i</sub> &lt;= n</code></li>
	<li>Tất cả bộ <code>(type<sub>i</sub>, u<sub>i</sub>, v<sub>i</sub>)</code> đều phân biệt.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Xóa nhiều cạnh nhất có thể trong khi cả Alice và Bob vẫn đi qua được đồ thị. $n$ và số cạnh có thể lên tới $10^5$. Cạnh loại $3$ phục vụ cả hai nên được giữ trước; các cạnh riêng chỉ bổ sung cho đồ thị của từng người.
>
> Hai rừng disjoint set theo dõi khả năng liên thông của hai người. Thêm cạnh loại $3$ trước và loại bỏ cạnh chung nếu nó tạo chu trình. Sau đó thêm cạnh loại $1$ và $2$ theo cách tương tự. Chỉ tồn tại đáp án khi cả hai rừng đều chỉ còn một thành phần.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class UnionFind:
    def __init__(self, n):
        self.p = list(range(n))
        self.size = [1] * n
        self.cnt = n

    def find(self, x):
        if self.p[x] != x:
            self.p[x] = self.find(self.p[x])
        return self.p[x]

    def union(self, a, b):
        pa, pb = self.find(a - 1), self.find(b - 1)
        if pa == pb:
            return False
        if self.size[pa] > self.size[pb]:
            self.p[pb] = pa
            self.size[pa] += self.size[pb]
        else:
            self.p[pa] = pb
            self.size[pb] += self.size[pa]
        self.cnt -= 1
        return True


class Solution:
    def maxNumEdgesToRemove(self, n: int, edges: List[List[int]]) -> int:
        ufa = UnionFind(n)
        ufb = UnionFind(n)
        ans = 0
        for t, u, v in edges:
            if t == 3:
                if ufa.union(u, v):
                    ufb.union(u, v)
                else:
                    ans += 1
        for t, u, v in edges:
            if t == 1:
                ans += not ufa.union(u, v)
            if t == 2:
                ans += not ufb.union(u, v)
        return ans if ufa.cnt == 1 and ufb.cnt == 1 else -1
```

#### Java

```java
class UnionFind {
    private int[] p;
    private int[] size;
    public int cnt;

    public UnionFind(int n) {
        p = new int[n];
        size = new int[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
            size[i] = 1;
        }
        cnt = n;
    }

    public int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }

    public boolean union(int a, int b) {
        int pa = find(a - 1), pb = find(b - 1);
        if (pa == pb) {
            return false;
        }
        if (size[pa] > size[pb]) {
            p[pb] = pa;
            size[pa] += size[pb];
        } else {
            p[pa] = pb;
            size[pb] += size[pa];
        }
        --cnt;
        return true;
    }
}

class Solution {
    public int maxNumEdgesToRemove(int n, int[][] edges) {
        UnionFind ufa = new UnionFind(n);
        UnionFind ufb = new UnionFind(n);
        int ans = 0;
        for (var e : edges) {
            int t = e[0], u = e[1], v = e[2];
            if (t == 3) {
                if (ufa.union(u, v)) {
                    ufb.union(u, v);
                } else {
                    ++ans;
                }
            }
        }
        for (var e : edges) {
            int t = e[0], u = e[1], v = e[2];
            if (t == 1 && !ufa.union(u, v)) {
                ++ans;
            }
            if (t == 2 && !ufb.union(u, v)) {
                ++ans;
            }
        }
        return ufa.cnt == 1 && ufb.cnt == 1 ? ans : -1;
    }
}
```

#### C++

```cpp
class UnionFind {
public:
    int cnt;

    UnionFind(int n) {
        p = vector<int>(n);
        size = vector<int>(n, 1);
        iota(p.begin(), p.end(), 0);
        cnt = n;
    }

    bool unite(int a, int b) {
        int pa = find(a - 1), pb = find(b - 1);
        if (pa == pb) {
            return false;
        }
        if (size[pa] > size[pb]) {
            p[pb] = pa;
            size[pa] += size[pb];
        } else {
            p[pa] = pb;
            size[pb] += size[pa];
        }
        --cnt;
        return true;
    }

    int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }

private:
    vector<int> p, size;
};

class Solution {
public:
    int maxNumEdgesToRemove(int n, vector<vector<int>>& edges) {
        UnionFind ufa(n);
        UnionFind ufb(n);
        int ans = 0;
        for (auto& e : edges) {
            int t = e[0], u = e[1], v = e[2];
            if (t == 3) {
                if (ufa.unite(u, v)) {
                    ufb.unite(u, v);
                } else {
                    ++ans;
                }
            }
        }
        for (auto& e : edges) {
            int t = e[0], u = e[1], v = e[2];
            ans += t == 1 && !ufa.unite(u, v);
            ans += t == 2 && !ufb.unite(u, v);
        }
        return ufa.cnt == 1 && ufb.cnt == 1 ? ans : -1;
    }
};
```

#### Go

```go
type unionFind struct {
	p, size []int
	cnt     int
}

func newUnionFind(n int) *unionFind {
	p := make([]int, n)
	size := make([]int, n)
	for i := range p {
		p[i] = i
		size[i] = 1
	}
	return &unionFind{p, size, n}
}

func (uf *unionFind) find(x int) int {
	if uf.p[x] != x {
		uf.p[x] = uf.find(uf.p[x])
	}
	return uf.p[x]
}

func (uf *unionFind) union(a, b int) bool {
	pa, pb := uf.find(a-1), uf.find(b-1)
	if pa == pb {
		return false
	}
	if uf.size[pa] > uf.size[pb] {
		uf.p[pb] = pa
		uf.size[pa] += uf.size[pb]
	} else {
		uf.p[pa] = pb
		uf.size[pb] += uf.size[pa]
	}
	uf.cnt--
	return true
}

func maxNumEdgesToRemove(n int, edges [][]int) (ans int) {
	ufa := newUnionFind(n)
	ufb := newUnionFind(n)
	for _, e := range edges {
		t, u, v := e[0], e[1], e[2]
		if t == 3 {
			if ufa.union(u, v) {
				ufb.union(u, v)
			} else {
				ans++
			}
		}
	}
	for _, e := range edges {
		t, u, v := e[0], e[1], e[2]
		if t == 1 && !ufa.union(u, v) {
			ans++
		}
		if t == 2 && !ufb.union(u, v) {
			ans++
		}
	}
	if ufa.cnt == 1 && ufb.cnt == 1 {
		return
	}
	return -1
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
