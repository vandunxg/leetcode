---
comments: true
difficulty: Hard
rating: 2207
source: Weekly Contest 214 Q4
tags:
    - Binary Indexed Tree
    - Segment Tree
    - Array
    - Binary Search
    - Divide and Conquer
    - Ordered Set
    - Merge Sort
---

<!-- problem:start -->

# [1649. Create Sorted Array through Instructions](https://leetcode.com/problems/create-sorted-array-through-instructions)

[中文文档](/solution/1600-1699/1649.Create%20Sorted%20Array%20through%20Instructions/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>instructions</code>, hãy tạo một mảng đã sắp xếp từ các phần tử trong <code>instructions</code>. Ban đầu, container <code>nums</code> rỗng. Với mỗi phần tử trong <code>instructions</code> theo thứ tự <strong>từ trái sang phải</strong>, chèn phần tử đó vào <code>nums</code>. <strong>Chi phí</strong> mỗi lần chèn là giá trị <b>nhỏ hơn</b> trong hai số sau:</p>

<ul>
	<li>Số phần tử hiện có trong <code>nums</code> <strong>nhỏ hơn nghiêm ngặt</strong> <code>instructions[i]</code>.</li>
	<li>Số phần tử hiện có trong <code>nums</code> <strong>lớn hơn nghiêm ngặt</strong> <code>instructions[i]</code>.</li>
</ul>

<p>Ví dụ, khi chèn phần tử <code>3</code> vào <code>nums = [1,2,3,5]</code>, <strong>chi phí</strong> là <code>min(2, 1)</code> (các phần tử <code>1</code> và <code>2</code> nhỏ hơn <code>3</code>, phần tử <code>5</code> lớn hơn <code>3</code>) và <code>nums</code> trở thành <code>[1,2,3,3,5]</code>.</p>

<p>Trả về <em><strong>tổng chi phí</strong> để chèn mọi phần tử từ </em><code>instructions</code><em> vào </em><code>nums</code>. Vì đáp án có thể lớn, hãy trả về kết quả <strong>modulo</strong> <code>10<sup>9</sup> + 7</code></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> instructions = [1,5,6,2]
<strong>Output:</strong> 1
<strong>Explanation:</strong> Bắt đầu với nums = [].
Chèn 1 với chi phí min(0, 0) = 0, khi đó nums = [1].
Chèn 5 với chi phí min(1, 0) = 0, khi đó nums = [1,5].
Chèn 6 với chi phí min(2, 0) = 0, khi đó nums = [1,5,6].
Chèn 2 với chi phí min(1, 2) = 1, khi đó nums = [1,2,5,6].
Tổng chi phí là 0 + 0 + 0 + 1 = 1.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> instructions = [1,2,3,6,5,4]
<strong>Output:</strong> 3
<strong>Explanation:</strong> Bắt đầu với nums = [].
Chèn 1 với chi phí min(0, 0) = 0, khi đó nums = [1].
Chèn 2 với chi phí min(1, 0) = 0, khi đó nums = [1,2].
Chèn 3 với chi phí min(2, 0) = 0, khi đó nums = [1,2,3].
Chèn 6 với chi phí min(3, 0) = 0, khi đó nums = [1,2,3,6].
Chèn 5 với chi phí min(3, 1) = 1, khi đó nums = [1,2,3,5,6].
Chèn 4 với chi phí min(3, 2) = 2, khi đó nums = [1,2,3,4,5,6].
Tổng chi phí là 0 + 0 + 0 + 0 + 1 + 2 = 3.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> instructions = [1,3,3,3,2,4,2,1,2]
<strong>Output:</strong> 4
<strong>Explanation:</strong> Bắt đầu với nums = [].
Chèn 1 với chi phí min(0, 0) = 0, khi đó nums = [1].
Chèn 3 với chi phí min(1, 0) = 0, khi đó nums = [1,3].
Chèn 3 với chi phí min(1, 0) = 0, khi đó nums = [1,3,3].
Chèn 3 với chi phí min(1, 0) = 0, khi đó nums = [1,3,3,3].
Chèn 2 với chi phí min(1, 3) = 1, khi đó nums = [1,2,3,3,3].
Chèn 4 với chi phí min(5, 0) = 0, khi đó nums = [1,2,3,3,3,4].
​​​​​​​Chèn 2 với chi phí min(1, 4) = 1, khi đó nums = [1,2,2,3,3,3,4].
​​​​​​​Chèn 1 với chi phí min(0, 6) = 0, khi đó nums = [1,1,2,2,3,3,3,4].
​​​​​​​Chèn 2 với chi phí min(2, 4) = 2, khi đó nums = [1,1,2,2,2,3,3,3,4].
Tổng chi phí là 0 + 0 + 0 + 0 + 1 + 0 + 1 + 0 + 2 = 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= instructions.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= instructions[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chi phí chèn $x$ là min của số giá trị hiện có nhỏ hơn nghiêm ngặt và lớn hơn nghiêm ngặt. Với $10^5$ lần chèn, duyệt tuyến tính ở mỗi bước là quá chậm.
>
> Miền giá trị cũng có kích thước $10^5$, nên Fenwick tree có thể truy vấn số lượng tiền tố và cộng thêm một phần tử trong $O(\log M)$.
>
> Trước khi chèn $x$, cộng $\min(\texttt{query}(x-1),\, i-\texttt{query}(x))$, sau đó $\texttt{update}(x,1)$ và lấy modulo.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class BinaryIndexedTree:
    def __init__(self, n):
        self.n = n
        self.c = [0] * (n + 1)

    def update(self, x: int, v: int):
        while x <= self.n:
            self.c[x] += v
            x += x & -x

    def query(self, x: int) -> int:
        s = 0
        while x:
            s += self.c[x]
            x -= x & -x
        return s


class Solution:
    def createSortedArray(self, instructions: List[int]) -> int:
        m = max(instructions)
        tree = BinaryIndexedTree(m)
        ans = 0
        mod = 10**9 + 7
        for i, x in enumerate(instructions):
            cost = min(tree.query(x - 1), i - tree.query(x))
            ans += cost
            tree.update(x, 1)
        return ans % mod
```

#### Java

```java
class BinaryIndexedTree {
    private int n;
    private int[] c;

    public BinaryIndexedTree(int n) {
        this.n = n;
        this.c = new int[n + 1];
    }

    public void update(int x, int v) {
        while (x <= n) {
            c[x] += v;
            x += x & -x;
        }
    }

    public int query(int x) {
        int s = 0;
        while (x > 0) {
            s += c[x];
            x -= x & -x;
        }
        return s;
    }
}

class Solution {
    public int createSortedArray(int[] instructions) {
        int m = 0;
        for (int x : instructions) {
            m = Math.max(m, x);
        }
        BinaryIndexedTree tree = new BinaryIndexedTree(m);
        int ans = 0;
        final int mod = (int) 1e9 + 7;
        for (int i = 0; i < instructions.length; ++i) {
            int x = instructions[i];
            int cost = Math.min(tree.query(x - 1), i - tree.query(x));
            ans = (ans + cost) % mod;
            tree.update(x, 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class BinaryIndexedTree {
public:
    BinaryIndexedTree(int _n)
        : n(_n)
        , c(_n + 1) {}

    void update(int x, int delta) {
        while (x <= n) {
            c[x] += delta;
            x += x & -x;
        }
    }

    int query(int x) {
        int s = 0;
        while (x) {
            s += c[x];
            x -= x & -x;
        }
        return s;
    }

private:
    int n;
    vector<int> c;
};

class Solution {
public:
    int createSortedArray(vector<int>& instructions) {
        int m = *max_element(instructions.begin(), instructions.end());
        BinaryIndexedTree tree(m);
        const int mod = 1e9 + 7;
        int ans = 0;
        for (int i = 0; i < instructions.size(); ++i) {
            int x = instructions[i];
            int cost = min(tree.query(x - 1), i - tree.query(x));
            ans = (ans + cost) % mod;
            tree.update(x, 1);
        }
        return ans;
    }
};
```

#### Go

```go
type BinaryIndexedTree struct {
	n int
	c []int
}

func newBinaryIndexedTree(n int) *BinaryIndexedTree {
	c := make([]int, n+1)
	return &BinaryIndexedTree{n, c}
}

func (this *BinaryIndexedTree) update(x, delta int) {
	for x <= this.n {
		this.c[x] += delta
		x += x & -x
	}
}

func (this *BinaryIndexedTree) query(x int) int {
	s := 0
	for x > 0 {
		s += this.c[x]
		x -= x & -x
	}
	return s
}

func createSortedArray(instructions []int) (ans int) {
	m := slices.Max(instructions)
	tree := newBinaryIndexedTree(m)
	const mod = 1e9 + 7
	for i, x := range instructions {
		cost := min(tree.query(x-1), i-tree.query(x))
		ans = (ans + cost) % mod
		tree.update(x, 1)
	}
	return
}
```

#### TypeScript

```ts
class BinaryIndexedTree {
    private n: number;
    private c: number[];

    constructor(n: number) {
        this.n = n;
        this.c = new Array(n + 1).fill(0);
    }

    public update(x: number, v: number): void {
        while (x <= this.n) {
            this.c[x] += v;
            x += x & -x;
        }
    }

    public query(x: number): number {
        let s = 0;
        while (x > 0) {
            s += this.c[x];
            x -= x & -x;
        }
        return s;
    }
}

function createSortedArray(instructions: number[]): number {
    const m = Math.max(...instructions);
    const tree = new BinaryIndexedTree(m);
    let ans = 0;
    const mod = 10 ** 9 + 7;
    for (let i = 0; i < instructions.length; ++i) {
        const x = instructions[i];
        const cost = Math.min(tree.query(x - 1), i - tree.query(x));
        ans = (ans + cost) % mod;
        tree.update(x, 1);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 lưu tần suất trong Fenwick tree. Segment tree với tổng trên đoạn cũng trả lời được các truy vấn số lượng tiền tố tương tự.
>
> Phép cộng tại một điểm cùng các truy vấn trên $[1,x)$ và $(x,M]$ tương ứng với phiên bản Fenwick. Bản Python bị đánh dấu TLE ở đây; Java và C++ vượt qua.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Node:
    def __init__(self):
        self.l = 0
        self.r = 0
        self.v = 0


class SegmentTree:
    def __init__(self, n):
        self.tr = [Node() for _ in range(4 * n)]
        self.build(1, 1, n)

    def build(self, u, l, r):
        self.tr[u].l = l
        self.tr[u].r = r
        if l == r:
            return
        mid = (l + r) >> 1
        self.build(u << 1, l, mid)
        self.build(u << 1 | 1, mid + 1, r)

    def modify(self, u, x, v):
        if self.tr[u].l == x and self.tr[u].r == x:
            self.tr[u].v += v
            return
        mid = (self.tr[u].l + self.tr[u].r) >> 1
        if x <= mid:
            self.modify(u << 1, x, v)
        else:
            self.modify(u << 1 | 1, x, v)
        self.pushup(u)

    def pushup(self, u):
        self.tr[u].v = self.tr[u << 1].v + self.tr[u << 1 | 1].v

    def query(self, u, l, r):
        if self.tr[u].l >= l and self.tr[u].r <= r:
            return self.tr[u].v
        mid = (self.tr[u].l + self.tr[u].r) >> 1
        v = 0
        if l <= mid:
            v = self.query(u << 1, l, r)
        if r > mid:
            v += self.query(u << 1 | 1, l, r)
        return v


class Solution:
    def createSortedArray(self, instructions: List[int]) -> int:
        n = max(instructions)
        tree = SegmentTree(n)
        ans = 0
        for num in instructions:
            a = tree.query(1, 1, num - 1)
            b = tree.query(1, 1, n) - tree.query(1, 1, num)
            ans += min(a, b)
            tree.modify(1, num, 1)
        return ans % int((1e9 + 7))
```

#### Java

```java
class Solution {
    public int createSortedArray(int[] instructions) {
        int n = 100010;
        int mod = (int) 1e9 + 7;
        SegmentTree tree = new SegmentTree(n);
        int ans = 0;
        for (int num : instructions) {
            int a = tree.query(1, 1, num - 1);
            int b = tree.query(1, 1, n) - tree.query(1, 1, num);
            ans += Math.min(a, b);
            ans %= mod;
            tree.modify(1, num, 1);
        }
        return ans;
    }
}

class Node {
    int l;
    int r;
    int v;
}

class SegmentTree {
    private Node[] tr;

    public SegmentTree(int n) {
        tr = new Node[4 * n];
        for (int i = 0; i < tr.length; ++i) {
            tr[i] = new Node();
        }
        build(1, 1, n);
    }

    public void build(int u, int l, int r) {
        tr[u].l = l;
        tr[u].r = r;
        if (l == r) {
            return;
        }
        int mid = (l + r) >> 1;
        build(u << 1, l, mid);
        build(u << 1 | 1, mid + 1, r);
    }

    public void modify(int u, int x, int v) {
        if (tr[u].l == x && tr[u].r == x) {
            tr[u].v += v;
            return;
        }
        int mid = (tr[u].l + tr[u].r) >> 1;
        if (x <= mid) {
            modify(u << 1, x, v);
        } else {
            modify(u << 1 | 1, x, v);
        }
        pushup(u);
    }

    public void pushup(int u) {
        tr[u].v = tr[u << 1].v + tr[u << 1 | 1].v;
    }

    public int query(int u, int l, int r) {
        if (tr[u].l >= l && tr[u].r <= r) {
            return tr[u].v;
        }
        int mid = (tr[u].l + tr[u].r) >> 1;
        int v = 0;
        if (l <= mid) {
            v += query(u << 1, l, r);
        }
        if (r > mid) {
            v += query(u << 1 | 1, l, r);
        }
        return v;
    }
}
```

#### C++

```cpp
class Node {
public:
    int l;
    int r;
    int v;
};

class SegmentTree {
public:
    vector<Node*> tr;

    SegmentTree(int n) {
        tr.resize(4 * n);
        for (int i = 0; i < tr.size(); ++i) tr[i] = new Node();
        build(1, 1, n);
    }

    void build(int u, int l, int r) {
        tr[u]->l = l;
        tr[u]->r = r;
        if (l == r) return;
        int mid = (l + r) >> 1;
        build(u << 1, l, mid);
        build(u << 1 | 1, mid + 1, r);
    }

    void modify(int u, int x, int v) {
        if (tr[u]->l == x && tr[u]->r == x) {
            tr[u]->v += v;
            return;
        }
        int mid = (tr[u]->l + tr[u]->r) >> 1;
        if (x <= mid)
            modify(u << 1, x, v);
        else
            modify(u << 1 | 1, x, v);
        pushup(u);
    }

    void pushup(int u) {
        tr[u]->v = tr[u << 1]->v + tr[u << 1 | 1]->v;
    }

    int query(int u, int l, int r) {
        if (tr[u]->l >= l && tr[u]->r <= r) return tr[u]->v;
        int mid = (tr[u]->l + tr[u]->r) >> 1;
        int v = 0;
        if (l <= mid) v = query(u << 1, l, r);
        if (r > mid) v += query(u << 1 | 1, l, r);
        return v;
    }
};

class Solution {
public:
    int createSortedArray(vector<int>& instructions) {
        int n = *max_element(instructions.begin(), instructions.end());
        int mod = 1e9 + 7;
        SegmentTree* tree = new SegmentTree(n);
        int ans = 0;
        for (int num : instructions) {
            int a = tree->query(1, 1, num - 1);
            int b = tree->query(1, 1, n) - tree->query(1, 1, num);
            ans += min(a, b);
            ans %= mod;
            tree->modify(1, num, 1);
        }
        return ans;
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
