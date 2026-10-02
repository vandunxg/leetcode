---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Union Find
    - Graph
    - Hash Table
    - Bipartite Graph
---

<!-- problem:start -->

# [947. Most Stones Removed with Same Row or Column](https://leetcode.com/problems/most-stones-removed-with-same-row-or-column)

[中文文档](/solution/0900-0999/0947.Most%20Stones%20Removed%20with%20Same%20Row%20or%20Column/README.md)

## Mô tả

<!-- description:start -->

<p>Trên mặt phẳng 2D, ta đặt <code>n</code> viên đá tại các tọa độ nguyên. Mỗi tọa độ có nhiều nhất một viên đá.</p>

<p>Có thể xóa một viên đá nếu nó nằm cùng <strong>hàng hoặc cột</strong> với một viên đá khác chưa bị xóa.</p>

<p>Cho mảng <code>stones</code> có độ dài <code>n</code>, trong đó <code>stones[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> biểu thị vị trí của viên đá thứ <code>i<sup>th</sup></code>. Hãy trả về <em>số viên đá lớn nhất có thể xóa</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> stones = [[0,0],[0,1],[1,0],[1,2],[2,1],[2,2]]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Một cách xóa 5 viên đá như sau:
1. Xóa viên đá [2,2] vì nó cùng hàng với [2,1].
2. Xóa viên đá [2,1] vì nó cùng cột với [0,1].
3. Xóa viên đá [1,2] vì nó cùng hàng với [1,0].
4. Xóa viên đá [1,0] vì nó cùng cột với [0,0].
5. Xóa viên đá [0,1] vì nó cùng hàng với [0,0].
Không thể xóa viên đá [0,0] vì nó không cùng hàng hoặc cột với viên đá nào khác còn trên mặt phẳng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> stones = [[0,0],[0,2],[1,1],[2,0],[2,2]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Một cách thực hiện 3 lượt như sau:
1. Xóa viên đá [2,2] vì nó cùng hàng với [2,0].
2. Xóa viên đá [2,0] vì nó cùng cột với [0,0].
3. Xóa viên đá [0,2] vì nó cùng hàng với [0,0].
Không thể xóa các viên đá [0,0] và [1,1] vì chúng không cùng hàng hoặc cột với viên đá nào khác còn trên mặt phẳng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> stones = [[0,0]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> [0,0] là viên đá duy nhất trên mặt phẳng nên không thể xóa nó.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= stones.length &lt;= 1000</code></li>
	<li><code>0 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 10<sup>4</sup></code></li>
	<li>Không có hai viên đá nào nằm tại cùng một tọa độ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Union-Find

<!-- thinking:start -->

> **Tư duy**
>
> Các viên đá cùng hàng hoặc cột có thể kết nối với nhau. Mỗi component còn lại một viên đá, nên đáp án bằng $n$ trừ số component. Gộp mọi cặp viên đá cùng hàng hoặc cột; mỗi lần union thành công tương ứng với một viên đá có thể xóa.

<!-- thinking:end -->

Ta dùng union-find để quản lý mối liên hệ giữa các viên đá. Nếu hai viên đá cùng hàng hoặc cột, ta xem chúng có kết nối và dùng union-find để gộp chúng. Cuối cùng, số connected component trong union-find chính là số viên đá có thể còn lại. Vì vậy, số viên đá xóa được bằng tổng số viên đá trừ số viên còn lại. Ta cũng có thể đếm số lần gộp thành công; con số này chính là số viên đá có thể xóa.

Độ phức tạp thời gian là $O(n^2 \times \alpha(n))$ và độ phức tạp không gian là $O(n)$, với $n$ là số viên đá.

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
    def removeStones(self, stones: List[List[int]]) -> int:
        uf = UnionFind(len(stones))
        ans = 0
        for i, (x1, y1) in enumerate(stones):
            for j, (x2, y2) in enumerate(stones[:i]):
                if x1 == x2 or y1 == y2:
                    ans += uf.union(i, j)
        return ans
```

#### Java

```java
class UnionFind {
    private final int[] p;
    private final int[] size;

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
    public int removeStones(int[][] stones) {
        int n = stones.length;
        UnionFind uf = new UnionFind(n);
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (stones[i][0] == stones[j][0] || stones[i][1] == stones[j][1]) {
                    ans += uf.union(i, j) ? 1 : 0;
                }
            }
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
    int removeStones(vector<vector<int>>& stones) {
        int n = stones.size();
        UnionFind uf(n);
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (stones[i][0] == stones[j][0] || stones[i][1] == stones[j][1]) {
                    ans += uf.unite(i, j);
                }
            }
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

func removeStones(stones [][]int) (ans int) {
	n := len(stones)
	uf := newUnionFind(n)
	for i, s1 := range stones {
		for j, s2 := range stones[:i] {
			if s1[0] == s2[0] || s1[1] == s2[1] {
				if uf.union(i, j) {
					ans++
				}
			}
		}
	}
	return
}
```

#### TypeScript

```ts
class UnionFind {
    p: number[];
    size: number[];
    constructor(n: number) {
        this.p = Array.from({ length: n }, (_, i) => i);
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

function removeStones(stones: number[][]): number {
    const n = stones.length;
    const uf = new UnionFind(n);
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < i; ++j) {
            if (stones[i][0] === stones[j][0] || stones[i][1] === stones[j][1]) {
                ans += uf.union(i, j) ? 1 : 0;
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Union-Find (Tối ưu)

<!-- thinking:start -->

> **Tư duy**
>
> Cách 1 xét từng cặp viên đá nên có độ phức tạp bậc hai. Xem hàng và cột là các phần tử: cộng một offset vào chỉ số cột rồi union hàng với cột của mỗi viên đá, nhờ đó các viên đá cùng hàng hoặc cột tự động được nối với nhau. Số component còn lại bằng số root khác nhau của các hàng.

<!-- thinking:end -->

Ta cộng một offset vào tọa độ y của các viên đá để có thể gộp tọa độ x và y trong cùng một cấu trúc. Sau đó, dùng union-find để quản lý mối liên hệ giữa tọa độ x và tọa độ y.

Ta duyệt từng viên đá và gộp tọa độ x với tọa độ y của nó.

Cuối cùng, duyệt lại các viên đá và thêm root node của tọa độ x mỗi viên vào một set. Số phần tử trong set là số viên đá có thể còn lại. Do đó, số viên có thể xóa bằng tổng số viên đá trừ số viên còn lại.

Độ phức tạp thời gian là $O(n \times \alpha(m))$ và độ phức tạp không gian là $O(m)$, trong đó $n$ là số viên đá và $m$ là giá trị tọa độ lớn nhất.

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
    def removeStones(self, stones: List[List[int]]) -> int:
        m = 10001
        uf = UnionFind(m << 1)
        for x, y in stones:
            uf.union(x, y + m)
        return len(stones) - len({uf.find(x) for x, _ in stones})
```

#### Java

```java
class UnionFind {
    private final int[] p;
    private final int[] size;

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
    public int removeStones(int[][] stones) {
        int m = 10001;
        UnionFind uf = new UnionFind(m << 1);
        for (var st : stones) {
            uf.union(st[0], st[1] + m);
        }
        Set<Integer> s = new HashSet<>();
        for (var st : stones) {
            s.add(uf.find(st[0]));
        }
        return stones.length - s.size();
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
    int removeStones(vector<vector<int>>& stones) {
        int m = 10001;
        UnionFind uf(m << 1);
        for (auto& st : stones) {
            uf.unite(st[0], st[1] + m);
        }
        unordered_set<int> s;
        for (auto& st : stones) {
            s.insert(uf.find(st[0]));
        }
        return stones.size() - s.size();
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

func removeStones(stones [][]int) (ans int) {
	m := 10001
	uf := newUnionFind(m << 1)
	for _, st := range stones {
		uf.union(st[0], st[1]+m)
	}
	s := map[int]bool{}
	for _, st := range stones {
		s[uf.find(st[0])] = true
	}
	return len(stones) - len(s)
}
```

#### TypeScript

```ts
class UnionFind {
    p: number[];
    size: number[];
    constructor(n: number) {
        this.p = Array.from({ length: n }, (_, i) => i);
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

function removeStones(stones: number[][]): number {
    const m = 10001;
    const uf = new UnionFind(m << 1);
    for (const [x, y] of stones) {
        uf.union(x, y + m);
    }
    const s = new Set<number>();
    for (const [x, _] of stones) {
        s.add(uf.find(x));
    }
    return stones.length - s.size;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Breadth-First Search

<!-- thinking:start -->

> **Tư duy**
>
> Không dùng union-find, ta có thể tạo graph nối các viên đá cùng hàng hoặc cột rồi chạy BFS để đếm component. Cách cài đặt khá trực tiếp, nhưng trường hợp xấu nhất số cạnh vẫn là bậc hai.

<!-- thinking:end -->

Nối các viên đá cùng hàng hoặc cột. Mỗi connected component chỉ cần giữ lại một viên, nên đáp án bằng số viên đá trừ số component.

Bắt đầu BFS từ mỗi viên đá chưa được thăm để đánh dấu component của nó.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$, với $n$ là số viên đá.

<!-- tabs:start -->

#### TypeScript

```ts
function removeStones(stones: number[][]): number {
    const n = stones.length;
    const g: number[][] = Array.from({ length: n }, () => []);

    for (let i = 0; i < n; i++) {
        const [y, x] = stones[i];
        for (let j = i + 1; j < n; j++) {
            if (y === stones[j][0] || x === stones[j][1]) {
                g[i].push(j);
                g[j].push(i);
            }
        }
    }

    const bfs = (i: number) => {
        const seen = new Set<number>();

        let q = [i];
        while (q.length) {
            const qNext: number[] = [];

            for (const i of q) {
                if (seen.has(i)) continue;
                seen.add(i);
                set.delete(i);
                qNext.push(...g[i]);
            }

            q = qNext;
        }
    };

    const set = new Set(Array.from({ length: n }, (_, i) => i));
    let ans = n;
    for (const i of set) {
        bfs(i);
        ans--;
    }

    return ans;
}
```

#### JavaScript

```js
function removeStones(stones) {
    const n = stones.length;
    const g = Array.from({ length: n }, () => []);

    for (let i = 0; i < n; i++) {
        const [y, x] = stones[i];
        for (let j = i + 1; j < n; j++) {
            if (y === stones[j][0] || x === stones[j][1]) {
                g[i].push(j);
                g[j].push(i);
            }
        }
    }

    const bfs = i => {
        const seen = new Set();

        let q = [i];
        while (q.length) {
            const qNext = [];

            for (const i of q) {
                if (seen.has(i)) continue;
                seen.add(i);
                set.delete(i);
                qNext.push(...g[i]);
            }

            q = qNext;
        }
    };

    const set = new Set(Array.from({ length: n }, (_, i) => i));
    let ans = n;
    for (const i of set) {
        bfs(i);
        ans--;
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
