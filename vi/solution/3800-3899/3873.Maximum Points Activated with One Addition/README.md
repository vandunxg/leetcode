---
comments: true
difficulty: Hard
rating: 2198
source: Weekly Contest 493 Q4
tags:
    - Union Find
    - Array
    - Hash Table
---

<!-- problem:start -->

# [3873. Maximum Points Activated with One Addition](https://leetcode.com/problems/maximum-points-activated-with-one-addition)

[中文文档](/solution/3800-3899/3873.Maximum%20Points%20Activated%20with%20One%20Addition/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2D <code>points</code>, trong đó <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> biểu diễn tọa độ của điểm thứ <code>i<sup>th</sup></code>. Tất cả tọa độ trong <code>points</code> đều <strong>khác nhau</strong>.</p>

<p>Nếu một điểm được <strong>kích hoạt</strong>, tất cả các điểm có cùng tọa độ <strong>x</strong> hoặc <strong>y</strong> cũng được <strong>kích hoạt</strong>.</p>

<p>Quá trình kích hoạt tiếp tục cho đến khi không thể kích hoạt thêm điểm nào.</p>

<p>Bạn được phép thêm <strong>một điểm bổ sung</strong> tại tọa độ nguyên <code>(x, y)</code> chưa xuất hiện trong <code>points</code>. Quá trình kích hoạt bắt đầu bằng việc <strong>kích hoạt</strong> <strong>điểm mới được thêm</strong> này.</p>

<p>Trả về một số nguyên biểu thị <strong>số lượng lớn nhất</strong> các điểm có thể được kích hoạt, bao gồm cả điểm mới được thêm.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">points = [[1,1],[1,2],[2,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Thêm và kích hoạt một điểm như <code>(1, 3)</code> sẽ dẫn đến các lần kích hoạt sau:</p>

<ul>
	<li><code>(1, 3)</code> có chung <code>x = 1</code> với <code>(1, 1)</code> và <code>(1, 2)</code> -&gt; <code>(1, 1)</code> và <code>(1, 2)</code> được kích hoạt.</li>
	<li><code>(1, 2)</code> có chung <code>y = 2</code> với <code>(2, 2)</code> -&gt; <code>(2, 2)</code> được kích hoạt.</li>
</ul>

<p>Do đó, các điểm được kích hoạt là <code>(1, 3)</code>, <code>(1, 1)</code>, <code>(1, 2)</code>, <code>(2, 2)</code>, tổng cộng 4 điểm. Có thể chứng minh đây là số điểm được kích hoạt lớn nhất.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">points = [[2,2],[1,1],[3,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Thêm và kích hoạt một điểm như <code>(1, 2)</code> sẽ dẫn đến các lần kích hoạt sau:</p>

<ul>
	<li><code>(1, 2)</code> có chung <code>x = 1</code> với <code>(1, 1)</code> -&gt; <code>(1, 1)</code> được kích hoạt.</li>
	<li><code>(1, 2)</code> có chung <code>y = 2</code> với <code>(2, 2)</code> -&gt; <code>(2, 2)</code> được kích hoạt.</li>
</ul>

<p>Do đó, các điểm được kích hoạt là <code>(1, 2)</code>, <code>(1, 1)</code>, <code>(2, 2)</code>, tổng cộng 3 điểm. Có thể chứng minh đây là số điểm được kích hoạt lớn nhất.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">points = [[2,3],[2,2],[1,1],[4,5]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Thêm và kích hoạt một điểm như <code>(2, 1)</code> sẽ dẫn đến các lần kích hoạt sau:</p>

<ul>
	<li><code>(2, 1)</code> có chung <code>x = 2</code> với <code>(2, 3)</code> và <code>(2, 2)</code> -&gt; <code>(2, 3)</code> và <code>(2, 2)</code> được kích hoạt.</li>
	<li><code>(2, 1)</code> có chung <code>y = 1</code> với <code>(1, 1)</code> -&gt; <code>(1, 1)</code> được kích hoạt.</li>
</ul>

<p>Do đó, các điểm được kích hoạt là <code>(2, 1)</code>, <code>(2, 3)</code>, <code>(2, 2)</code>, <code>(1, 1)</code>, tổng cộng 4 điểm.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= points.length &lt;= 10<sup>5</sup></code></li>
	<li><code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code></li>
	<li><code>-10<sup>9</sup> &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>points</code> chứa các tọa độ <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Union-Find

<!-- thinking:start -->

> **Tư duy**
>
> Việc kích hoạt lan truyền qua các điểm có cùng $x$ hoặc $y$. Ta được phép thêm một điểm và muốn kích hoạt nhiều điểm nhất. $n \le 10^5$.
>
> Các điểm có chung $x$ hoặc $y$ nằm trong cùng một component. Union-Find xem các tọa độ như các node: với mỗi điểm $(x,y)$, ta union $x$ với $y$.
>
> Điểm mới có thể nối hai component. Vì vậy, ta chọn hai component lớn nhất rồi cộng thêm một cho điểm mới.
>
> Dịch $y$ thêm $3 \times 10^9$ để các identifier của $x$ và $y$ không bao giờ trùng nhau.

<!-- thinking:end -->

Ta có thể dùng cấu trúc dữ liệu Union-Find để giải bài toán này.

Trước tiên, ta ánh xạ các tọa độ $x$ và $y$ của tất cả điểm vào cùng một cấu trúc Union-Find. Cụ thể, ta cộng một hằng số đủ lớn $m$ (chẳng hạn $3 \times 10^9$) vào mỗi tọa độ $y$ để đảm bảo các tọa độ $x$ và $y$ không xung đột.

Tiếp theo, ta duyệt qua tất cả các điểm và union những điểm có chung tọa độ $x$ hoặc chung tọa độ $y$. Nhờ đó, các điểm có cùng $x$ hoặc $y$ sẽ được nhóm vào cùng một tập hợp.

Cuối cùng, ta đếm số điểm trong mỗi tập hợp và tìm kích thước của hai tập hợp lớn nhất. Vì ta có thể thêm một điểm mới để nối hai tập hợp này, đáp án cuối cùng là tổng kích thước của hai tập hợp lớn nhất cộng $1$.

Độ phức tạp thời gian là $O(n \alpha(n))$, trong đó $n$ là số điểm và $\alpha$ là hàm Ackermann nghịch đảo. Độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class UnionFind:
    def __init__(self):
        self.p = {}
        self.size = {}

    def find(self, x):
        if x not in self.p:
            self.p[x] = x
            self.size[x] = 1
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
    def maxActivated(self, points: List[List[int]]) -> int:
        uf = UnionFind()
        m = int(3e9)

        for x, y in points:
            uf.union(x, y + m)

        cnt = Counter()
        for x, _ in points:
            cnt[uf.find(x)] += 1

        mx1 = mx2 = 0
        for x in cnt.values():
            if mx1 < x:
                mx2 = mx1
                mx1 = x
            elif mx2 < x:
                mx2 = x
        return mx1 + mx2 + 1
```

#### Java

```java
class UnionFind {
    Map<Long, Long> p = new HashMap<>();
    Map<Long, Integer> size = new HashMap<>();

    long find(long x) {
        if (!p.containsKey(x)) {
            p.put(x, x);
            size.put(x, 1);
        }
        if (p.get(x) != x) {
            p.put(x, find(p.get(x)));
        }
        return p.get(x);
    }

    boolean union(long a, long b) {
        long pa = find(a), pb = find(b);
        if (pa == pb) {
            return false;
        }

        int sa = size.get(pa), sb = size.get(pb);
        if (sa > sb) {
            p.put(pb, pa);
            size.put(pa, sa + sb);
        } else {
            p.put(pa, pb);
            size.put(pb, sa + sb);
        }
        return true;
    }
}

class Solution {
    public int maxActivated(int[][] points) {
        UnionFind uf = new UnionFind();
        long m = (long) 3e9;

        for (int[] p : points) {
            uf.union(p[0], p[1] + m);
        }

        Map<Long, Integer> cnt = new HashMap<>();
        for (int[] p : points) {
            cnt.merge(uf.find(p[0]), 1, Integer::sum);
        }

        int mx1 = 0, mx2 = 0;
        for (int x : cnt.values()) {
            if (mx1 < x) {
                mx2 = mx1;
                mx1 = x;
            } else if (mx2 < x) {
                mx2 = x;
            }
        }

        return mx1 + mx2 + 1;
    }
}
```

#### C++

```cpp
class UnionFind {
public:
    unordered_map<long long, long long> p;
    unordered_map<long long, int> sz;

    long long find(long long x) {
        if (!p.count(x)) {
            p[x] = x;
            sz[x] = 1;
        }
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }

    bool unite(long long a, long long b) {
        long long pa = find(a), pb = find(b);
        if (pa == pb) return false;

        if (sz[pa] > sz[pb]) {
            p[pb] = pa;
            sz[pa] += sz[pb];
        } else {
            p[pa] = pb;
            sz[pb] += sz[pa];
        }
        return true;
    }
};

class Solution {
public:
    int maxActivated(vector<vector<int>>& points) {
        UnionFind uf;
        long long m = (long long) 3e9;

        for (auto& p : points) {
            uf.unite(p[0], p[1] + m);
        }

        unordered_map<long long, int> cnt;
        for (auto& p : points) {
            long long root = uf.find(p[0]);
            cnt[root]++;
        }

        int mx1 = 0, mx2 = 0;
        for (auto& [_, x] : cnt) {
            if (mx1 < x) {
                mx2 = mx1;
                mx1 = x;
            } else if (mx2 < x) {
                mx2 = x;
            }
        }

        return mx1 + mx2 + 1;
    }
};
```

#### Go

```go
type UnionFind struct {
	p    map[int64]int64
	size map[int64]int
}

func NewUnionFind() *UnionFind {
	return &UnionFind{
		p:    map[int64]int64{},
		size: map[int64]int{},
	}
}

func (uf *UnionFind) find(x int64) int64 {
	if _, ok := uf.p[x]; !ok {
		uf.p[x] = x
		uf.size[x] = 1
	}
	if uf.p[x] != x {
		uf.p[x] = uf.find(uf.p[x])
	}
	return uf.p[x]
}

func (uf *UnionFind) union(a, b int64) bool {
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

func maxActivated(points [][]int) int {
	uf := NewUnionFind()
	m := int64(3e9)

	for _, p := range points {
		uf.union(int64(p[0]), int64(p[1])+m)
	}

	cnt := map[int64]int{}
	for _, p := range points {
		root := uf.find(int64(p[0]))
		cnt[root]++
	}

	mx1, mx2 := 0, 0
	for _, x := range cnt {
		if mx1 < x {
			mx2 = mx1
			mx1 = x
		} else if mx2 < x {
			mx2 = x
		}
	}

	return mx1 + mx2 + 1
}
```

#### TypeScript

```ts
class UnionFind {
    p: Map<number, number> = new Map();
    size: Map<number, number> = new Map();

    find(x: number): number {
        if (!this.p.has(x)) {
            this.p.set(x, x);
            this.size.set(x, 1);
        }
        if (this.p.get(x)! !== x) {
            this.p.set(x, this.find(this.p.get(x)!));
        }
        return this.p.get(x)!;
    }

    union(a: number, b: number): boolean {
        const pa = this.find(a);
        const pb = this.find(b);
        if (pa === pb) return false;

        const sa = this.size.get(pa)!;
        const sb = this.size.get(pb)!;

        if (sa > sb) {
            this.p.set(pb, pa);
            this.size.set(pa, sa + sb);
        } else {
            this.p.set(pa, pb);
            this.size.set(pb, sa + sb);
        }
        return true;
    }
}

function maxActivated(points: number[][]): number {
    const uf = new UnionFind();
    const m = 3e9;

    for (const [x, y] of points) {
        uf.union(x, y + m);
    }

    const cnt = new Map<number, number>();
    for (const [x] of points) {
        const root = uf.find(x);
        cnt.set(root, (cnt.get(root) ?? 0) + 1);
    }

    let mx1 = 0,
        mx2 = 0;
    for (const x of cnt.values()) {
        if (mx1 < x) {
            mx2 = mx1;
            mx1 = x;
        } else if (mx2 < x) {
            mx2 = x;
        }
    }

    return mx1 + mx2 + 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
