---
comments: true
difficulty: Medium
rating: 1558
source: Biweekly Contest 3 Q3
tags:
    - Union Find
    - Array
    - Sorting
---

<!-- problem:start -->

# [1101. The Earliest Moment When Everyone Become Friends 🔒](https://leetcode.com/problems/the-earliest-moment-when-everyone-become-friends)

[中文文档](/solution/1100-1199/1101.The%20Earliest%20Moment%20When%20Everyone%20Become%20Friends/README.md)

## Mô tả

<!-- description:start -->

<p>Có n người trong một nhóm xã hội, được đánh số từ <code>0</code> đến <code>n - 1</code>. Bạn được cho mảng <code>logs</code>, trong đó <code>logs[i] = [timestamp<sub>i</sub>, x<sub>i</sub>, y<sub>i</sub>]</code> cho biết <code>x<sub>i</sub></code> và <code>y<sub>i</sub></code> sẽ trở thành bạn bè vào thời điểm <code>timestamp<sub>i</sub></code>.</p>

<p>Tình bạn có tính <strong>đối xứng</strong>: nếu <code>a</code> là bạn của <code>b</code> thì <code>b</code> cũng là bạn của <code>a</code>. Ngoài ra, <code>a</code> được xem là quen biết <code>b</code> nếu <code>a</code> là bạn của <code>b</code>, hoặc <code>a</code> là bạn của một người quen biết <code>b</code>.</p>

<p>Trả về <em>thời điểm sớm nhất mà mỗi người đã quen biết tất cả những người còn lại</em>. Nếu không có thời điểm nào như vậy, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> logs = [[20190101,0,1],[20190104,3,4],[20190107,2,3],[20190211,1,5],[20190224,2,4],[20190301,0,3],[20190312,1,2],[20190322,4,5]], n = 6
<strong>Output:</strong> 20190301
<strong>Giải thích:</strong> 
Sự kiện đầu tiên xảy ra tại timestamp = 20190101. Sau khi 0 và 1 trở thành bạn bè, các nhóm bạn là [0,1], [2], [3], [4], [5].
Sự kiện thứ hai xảy ra tại timestamp = 20190104. Sau khi 3 và 4 trở thành bạn bè, các nhóm bạn là [0,1], [2], [3,4], [5].
Sự kiện thứ ba xảy ra tại timestamp = 20190107. Sau khi 2 và 3 trở thành bạn bè, các nhóm bạn là [0,1], [2,3,4], [5].
Sự kiện thứ tư xảy ra tại timestamp = 20190211. Sau khi 1 và 5 trở thành bạn bè, các nhóm bạn là [0,1,5], [2,3,4].
Sự kiện thứ năm xảy ra tại timestamp = 20190224. Vì 2 và 4 đã là bạn bè nên không có gì thay đổi.
Sự kiện thứ sáu xảy ra tại timestamp = 20190301. Sau khi 0 và 3 trở thành bạn bè, tất cả mọi người đều trở thành bạn bè.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> logs = [[0,2,0],[1,0,1],[3,0,3],[4,1,2],[7,3,1]], n = 4
<strong>Output:</strong> 3
<strong>Giải thích:</strong> Tại timestamp = 3, tất cả mọi người (tức 0, 1, 2 và 3) đều trở thành bạn bè.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= logs.length &lt;= 10<sup>4</sup></code></li>
	<li><code>logs[i].length == 3</code></li>
	<li><code>0 &lt;= timestamp<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>x<sub>i</sub> != y<sub>i</sub></code></li>
	<li>Tất cả giá trị <code>timestamp<sub>i</sub></code> đều <strong>khác nhau</strong>.</li>
	<li>Mỗi cặp <code>(x<sub>i</sub>, y<sub>i</sub>)</code> xuất hiện nhiều nhất một lần trong đầu vào.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Union-Find

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm thời điểm sớm nhất mà cả $n$ người thuộc cùng một thành phần liên thông. Các log không được sắp xếp sẵn, nên hãy sắp xếp theo timestamp rồi xử lý lần lượt.
>
> Nếu hai người vẫn thuộc hai tập khác nhau, hợp nhất chúng rồi giảm số thành phần đi một. Union-Find giúp thao tác tìm/hợp nhất có độ phức tạp gần như hằng số. Khi số thành phần còn $1$, mọi người đã liên thông và timestamp hiện tại là đáp án; nếu hết log trước đó thì trả về $-1$.

<!-- thinking:end -->

Ta sắp xếp tất cả log theo timestamp tăng dần rồi duyệt lần lượt. Dùng cấu trúc Union-Find để kiểm tra hai người trong log hiện tại đã thuộc cùng một nhóm bạn chưa. Nếu chưa, hợp nhất hai nhóm; khi tất cả mọi người nằm trong một nhóm bạn, trả về timestamp của log hiện tại.

Nếu đã duyệt hết log mà mọi người vẫn chưa thuộc cùng một nhóm bạn, trả về $-1$.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số lượng log.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def earliestAcq(self, logs: List[List[int]], n: int) -> int:
        def find(x):
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        p = list(range(n))
        for t, x, y in sorted(logs):
            if find(x) == find(y):
                continue
            p[find(x)] = find(y)
            n -= 1
            if n == 1:
                return t
        return -1
```

#### Java

```java
class Solution {
    private int[] p;

    public int earliestAcq(int[][] logs, int n) {
        Arrays.sort(logs, (a, b) -> a[0] - b[0]);
        p = new int[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
        }
        for (int[] log : logs) {
            int t = log[0], x = log[1], y = log[2];
            if (find(x) == find(y)) {
                continue;
            }
            p[find(x)] = find(y);
            if (--n == 1) {
                return t;
            }
        }
        return -1;
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
    int earliestAcq(vector<vector<int>>& logs, int n) {
        sort(logs.begin(), logs.end());
        vector<int> p(n);
        iota(p.begin(), p.end(), 0);
        function<int(int)> find = [&](int x) {
            return p[x] == x ? x : p[x] = find(p[x]);
        };
        for (auto& log : logs) {
            int x = find(log[1]);
            int y = find(log[2]);
            if (x != y) {
                p[x] = y;
                --n;
            }
            if (n == 1) {
                return log[0];
            }
        }
        return -1;
    }
};
```

#### Go

```go
func earliestAcq(logs [][]int, n int) int {
	sort.Slice(logs, func(i, j int) bool { return logs[i][0] < logs[j][0] })
	p := make([]int, n)
	for i := range p {
		p[i] = i
	}
	var find func(int) int
	find = func(x int) int {
		if p[x] != x {
			p[x] = find(p[x])
		}
		return p[x]
	}
	for _, log := range logs {
		t, x, y := log[0], log[1], log[2]
		if find(x) == find(y) {
			continue
		}
		p[find(x)] = find(y)
		n--
		if n == 1 {
			return t
		}
	}
	return -1
}
```

#### TypeScript

```ts
function earliestAcq(logs: number[][], n: number): number {
    const p: number[] = Array(n)
        .fill(0)
        .map((_, i) => i);
    const find = (x: number): number => {
        if (p[x] !== x) {
            p[x] = find(p[x]);
        }
        return p[x];
    };
    logs.sort((a, b) => a[0] - b[0]);
    for (const [t, x, y] of logs) {
        const rx = find(x);
        const ry = find(y);
        if (rx !== ry) {
            p[rx] = ry;
            if (--n === 1) {
                return t;
            }
        }
    }
    return -1;
}
```

#### Rust

```rust
struct UnionFind {
    p: Vec<usize>,
    size: Vec<usize>,
}

impl UnionFind {
    fn new(n: usize) -> Self {
        let p: Vec<usize> = (0..n).collect();
        let size = vec![1; n];
        UnionFind { p, size }
    }

    fn find(&mut self, x: usize) -> usize {
        if self.p[x] != x {
            self.p[x] = self.find(self.p[x]);
        }
        self.p[x]
    }

    fn union(&mut self, a: usize, b: usize) -> bool {
        let pa = self.find(a);
        let pb = self.find(b);
        if pa == pb {
            false
        } else if self.size[pa] > self.size[pb] {
            self.p[pb] = pa;
            self.size[pa] += self.size[pb];
            true
        } else {
            self.p[pa] = pb;
            self.size[pb] += self.size[pa];
            true
        }
    }
}

impl Solution {
    pub fn earliest_acq(logs: Vec<Vec<i32>>, n: i32) -> i32 {
        let mut logs = logs;
        logs.sort_by(|a, b| a[0].cmp(&b[0]));
        let mut uf = UnionFind::new(n as usize);
        let mut n = n;
        for log in logs {
            let t = log[0];
            let x = log[1] as usize;
            let y = log[2] as usize;
            if uf.union(x, y) {
                n -= 1;
                if n == 1 {
                    return t;
                }
            }
        }
        -1
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 viết trực tiếp thao tác path compression và không hợp nhất theo kích thước. Lời giải 2 đóng gói `UnionFind` với path compression và union-by-size; `union` cho biết có thực hiện hợp nhất hay không, còn vòng lặp chính chỉ cần theo dõi số thành phần. Thời điểm mọi người trở thành bạn bè không đổi, nhưng cây sẽ nông hơn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class UnionFind:
    __slots__ = ('p', 'size')

    def __init__(self, n):
        self.p = list(range(n))
        self.size = [1] * n

    def find(self, x: int) -> int:
        if self.p[x] != x:
            self.p[x] = self.find(self.p[x])
        return self.p[x]

    def union(self, a: int, b: int) -> bool:
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
    def earliestAcq(self, logs: List[List[int]], n: int) -> int:
        uf = UnionFind(n)
        for t, x, y in sorted(logs):
            if uf.union(x, y):
                n -= 1
                if n == 1:
                    return t
        return -1
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
    public int earliestAcq(int[][] logs, int n) {
        Arrays.sort(logs, (a, b) -> a[0] - b[0]);
        UnionFind uf = new UnionFind(n);
        for (int[] log : logs) {
            int t = log[0], x = log[1], y = log[2];
            if (uf.union(x, y) && --n == 1) {
                return t;
            }
        }
        return -1;
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
    int earliestAcq(vector<vector<int>>& logs, int n) {
        sort(logs.begin(), logs.end());
        UnionFind uf(n);
        for (auto& log : logs) {
            int t = log[0], x = log[1], y = log[2];
            if (uf.unite(x, y) && --n == 1) {
                return t;
            }
        }
        return -1;
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

func earliestAcq(logs [][]int, n int) int {
	sort.Slice(logs, func(i, j int) bool { return logs[i][0] < logs[j][0] })
	uf := newUnionFind(n)
	for _, log := range logs {
		t, x, y := log[0], log[1], log[2]
		if uf.union(x, y) {
			n--
			if n == 1 {
				return t
			}
		}
	}
	return -1
}
```

#### TypeScript

```ts
class UnionFind {
    private p: number[];
    private size: number[];

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
        const pa = this.find(a);
        const pb = this.find(b);
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

function earliestAcq(logs: number[][], n: number): number {
    logs.sort((a, b) => a[0] - b[0]);
    const uf = new UnionFind(n);
    for (const [t, x, y] of logs) {
        if (uf.union(x, y) && --n === 1) {
            return t;
        }
    }
    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
