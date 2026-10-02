---
comments: true
difficulty: Hard
rating: 2221
source: Weekly Contest 211 Q4
tags:
    - Union Find
    - Array
    - Math
    - Number Theory
---

<!-- problem:start -->

# [1627. Graph Connectivity With Threshold](https://leetcode.com/problems/graph-connectivity-with-threshold)

[中文文档](/solution/1600-1699/1627.Graph%20Connectivity%20With%20Threshold/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> thành phố được đánh số từ <code>1</code> đến <code>n</code>. Hai thành phố khác nhau có nhãn <code>x</code> và <code>y</code> được nối trực tiếp bằng đường hai chiều khi và chỉ khi <code>x</code> và <code>y</code> có ước chung <strong>lớn hơn nghiêm ngặt</strong> <code>threshold</code>. Cụ thể hơn, hai thành phố <code>x</code> và <code>y</code> có đường nối nếu tồn tại số nguyên <code>z</code> thỏa mãn tất cả điều kiện sau:</p>

<ul>
	<li><code>x % z == 0</code>,</li>
	<li><code>y % z == 0</code>, and</li>
	<li><code>z &gt; threshold</code>.</li>
</ul>

<p>Cho hai số nguyên <code>n</code> và <code>threshold</code> cùng mảng <code>queries</code>, hãy xác định với mỗi <code>queries[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> liệu hai thành phố <code>a<sub>i</sub></code> và <code>b<sub>i</sub></code> có liên thông trực tiếp hoặc gián tiếp hay không.&nbsp;(nghĩa là tồn tại một đường đi giữa chúng).</p>

<p>Trả về <em>mảng </em><code>answer</code><em>, trong đó </em><code>answer.length == queries.length</code><em> và </em><code>answer[i]</code><em> là </em><code>true</code><em> nếu với truy vấn thứ </em><code>i<sup>th</sup></code><em> có đường đi giữa </em><code>a<sub>i</sub></code><em> và </em><code>b<sub>i</sub></code><em>, hoặc </em><code>answer[i]</code><em> là </em><code>false</code><em> nếu không có đường đi.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1627.Graph%20Connectivity%20With%20Threshold/images/ex1.jpg" style="width: 382px; height: 181px;" />
<pre>
<strong>Input:</strong> n = 6, threshold = 2, queries = [[1,4],[2,5],[3,6]]
<strong>Output:</strong> [false,false,true]
<strong>Explanation:</strong> Các ước của mỗi số:
1:   1
2:   1, 2
3:   1, <u>3</u>
4:   1, 2, <u>4</u>
5:   1, <u>5</u>
6:   1, 2, <u>3</u>, <u>6</u>
Dùng các ước được gạch chân lớn hơn threshold, chỉ thành phố 3 và 6 có ước chung, nên chúng là
cặp duy nhất được nối trực tiếp. Kết quả của từng truy vấn:
[1,4]   1 không liên thông với 4
[2,5]   2 không liên thông với 5
[3,6]   3 liên thông với 6 qua đường đi 3--6
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1627.Graph%20Connectivity%20With%20Threshold/images/tmp.jpg" style="width: 532px; height: 302px;" />
<pre>
<strong>Input:</strong> n = 6, threshold = 0, queries = [[4,5],[3,4],[3,2],[2,6],[1,3]]
<strong>Output:</strong> [true,true,true,true,true]
<strong>Explanation:</strong> Các ước của mỗi số giống ví dụ trước. Tuy nhiên, vì threshold bằng 0,
có thể dùng mọi ước. Vì mọi số đều có 1 là ước, tất cả thành phố đều liên thông.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1627.Graph%20Connectivity%20With%20Threshold/images/ex3.jpg" style="width: 282px; height: 282px;" />
<pre>
<strong>Input:</strong> n = 5, threshold = 1, queries = [[4,5],[4,5],[3,2],[2,3],[3,4]]
<strong>Output:</strong> [false,false,false,false,false]
<strong>Explanation:</strong> Chỉ thành phố 2 và 4 có ước chung 2 lớn hơn nghiêm ngặt threshold 1, nên chúng là cặp duy nhất được nối trực tiếp.
Lưu ý rằng có thể có nhiều truy vấn cho cùng một cặp nút [x, y], và truy vấn [x, y] tương đương với truy vấn [y, x].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= threshold &lt;= n</code></li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>queries[i].length == 2</code></li>
	<li><code>1 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt;= cities</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Union-Find

<!-- thinking:start -->

> **Tư duy**
>
> Hai thành phố liên thông nếu tồn tại đường đi gồm các cạnh có $\gcd$ lớn hơn $\textit{threshold}$. Tính $\gcd$ cho mọi cặp là quá chậm khi cả $n$ và số truy vấn đều lớn.
>
> Mỗi $z$ lớn hơn threshold nối tất cả các bội của nó, nên hợp nhất các bội sẽ bao phủ mọi cạnh trực tiếp.
>
> Disjoint-set hợp nhất $z,2z,3z,\ldots$ với $z \in (\textit{threshold}, n]$, rồi mỗi truy vấn kiểm tra hai thành phố có chung root hay không.

<!-- thinking:end -->

Ta có thể liệt kê $z$ và các bội của nó, rồi dùng union-find để nối chúng. Khi đó, với mỗi truy vấn $[a, b]$, ta chỉ cần xác định $a$ và $b$ có thuộc cùng một thành phần liên thông hay không.

Độ phức tạp thời gian là $O(n \times \log n \times (\alpha(n) + q))$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ và $q$ lần lượt là số nút và số truy vấn, còn $\alpha$ là hàm ngược của hàm Ackermann.

<!-- tabs:start -->

#### Python3

```python
class UnionFind:
    def __init__(self, n):
        self.p = list(range(n))
        self.size = [1] * n

    def find(self, x):
        if self.p[x] != x:
            self.p[x] = self.find(self.p[x])
        return self.p[x]

    def union(self, a, b):
        pa, pb = self.find(a), self.find(b)
        if pa == pb:
            return False
        if self.size[pa] > self.size[pb]:
            self.p[pb] = pa
            self.size[pa] += self.size[pb]
        else:
            self.p[pa] = pb
            self.size[pb] += self.size[pa]
        return True


class Solution:
    def areConnected(
        self, n: int, threshold: int, queries: List[List[int]]
    ) -> List[bool]:
        uf = UnionFind(n + 1)
        for a in range(threshold + 1, n + 1):
            for b in range(a + a, n + 1, a):
                uf.union(a, b)
        return [uf.find(a) == uf.find(b) for a, b in queries]
```

#### Java

```java
class UnionFind {
    private int[] p;
    private int[] size;

    public UnionFind(int n) {
        p = new int[n];
        size = new int[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
            size[i] = 1;
        }
    }

    public int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }

    public boolean union(int a, int b) {
        int pa = find(a), pb = find(b);
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
        return true;
    }
}

class Solution {
    public List<Boolean> areConnected(int n, int threshold, int[][] queries) {
        UnionFind uf = new UnionFind(n + 1);
        for (int a = threshold + 1; a <= n; ++a) {
            for (int b = a + a; b <= n; b += a) {
                uf.union(a, b);
            }
        }
        List<Boolean> ans = new ArrayList<>();
        for (var q : queries) {
            ans.add(uf.find(q[0]) == uf.find(q[1]));
        }
        return ans;
    }
}
```

#### C++

```cpp
class UnionFind {
public:
    UnionFind(int n) {
        p = vector<int>(n);
        size = vector<int>(n, 1);
        iota(p.begin(), p.end(), 0);
    }

    bool unite(int a, int b) {
        int pa = find(a), pb = find(b);
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
    vector<bool> areConnected(int n, int threshold, vector<vector<int>>& queries) {
        UnionFind uf(n + 1);
        for (int a = threshold + 1; a <= n; ++a) {
            for (int b = a + a; b <= n; b += a) {
                uf.unite(a, b);
            }
        }
        vector<bool> ans;
        for (auto& q : queries) {
            ans.push_back(uf.find(q[0]) == uf.find(q[1]));
        }
        return ans;
    }
};
```

#### Go

```go
type unionFind struct {
	p, size []int
}

func newUnionFind(n int) *unionFind {
	p := make([]int, n)
	size := make([]int, n)
	for i := range p {
		p[i] = i
		size[i] = 1
	}
	return &unionFind{p, size}
}

func (uf *unionFind) find(x int) int {
	if uf.p[x] != x {
		uf.p[x] = uf.find(uf.p[x])
	}
	return uf.p[x]
}

func (uf *unionFind) union(a, b int) bool {
	pa, pb := uf.find(a), uf.find(b)
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
	return true
}

func areConnected(n int, threshold int, queries [][]int) []bool {
	uf := newUnionFind(n + 1)
	for a := threshold + 1; a <= n; a++ {
		for b := a + a; b <= n; b += a {
			uf.union(a, b)
		}
	}
	ans := make([]bool, len(queries))
	for i, q := range queries {
		ans[i] = uf.find(q[0]) == uf.find(q[1])
	}
	return ans
}
```

#### TypeScript

```ts
class UnionFind {
    p: number[];
    size: number[];
    constructor(n: number) {
        this.p = Array(n)
            .fill(0)
            .map((_, i) => i);
        this.size = Array(n).fill(1);
    }

    find(x: number): number {
        if (this.p[x] !== x) {
            this.p[x] = this.find(this.p[x]);
        }
        return this.p[x];
    }

    union(a: number, b: number): boolean {
        const [pa, pb] = [this.find(a), this.find(b)];
        if (pa === pb) {
            return false;
        }
        if (this.size[pa] > this.size[pb]) {
            this.p[pb] = pa;
            this.size[pa] += this.size[pb];
        } else {
            this.p[pa] = pb;
            this.size[pb] += this.size[pa];
        }
        return true;
    }
}

function areConnected(n: number, threshold: number, queries: number[][]): boolean[] {
    const uf = new UnionFind(n + 1);
    for (let a = threshold + 1; a <= n; ++a) {
        for (let b = a * 2; b <= n; b += a) {
            uf.union(a, b);
        }
    }
    return queries.map(([a, b]) => uf.find(a) === uf.find(b));
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
