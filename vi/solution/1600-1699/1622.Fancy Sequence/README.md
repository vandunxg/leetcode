---
comments: true
difficulty: Hard
rating: 2476
source: Biweekly Contest 37 Q4
tags:
    - Design
    - Segment Tree
    - Math
    - Number Theory
    - Fermat's Little Theorem
---

<!-- problem:start -->

# [1622. Fancy Sequence](https://leetcode.com/problems/fancy-sequence)

[中文文档](/solution/1600-1699/1622.Fancy%20Sequence/README.md)

## Mô tả

<!-- description:start -->

<p>Viết một API tạo ra các dãy fancy bằng các thao tác <code>append</code>, <code>addAll</code> và <code>multAll</code>.</p>

<p>Cài đặt lớp <code>Fancy</code>:</p>

<ul>
	<li><code>Fancy()</code> Khởi tạo đối tượng với một dãy rỗng.</li>
	<li><code>void append(val)</code> Thêm số nguyên <code>val</code> vào cuối dãy.</li>
	<li><code>void addAll(inc)</code> Tăng mọi giá trị hiện có trong dãy thêm số nguyên <code>inc</code>.</li>
	<li><code>void multAll(m)</code> Nhân mọi giá trị hiện có trong dãy với số nguyên <code>m</code>.</li>
	<li><code>int getIndex(idx)</code> Lấy giá trị hiện tại tại chỉ số <code>idx</code> (đánh số từ 0) của dãy theo <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>. Nếu chỉ số lớn hơn hoặc bằng độ dài dãy, trả về <code>-1</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input</strong>
[&quot;Fancy&quot;, &quot;append&quot;, &quot;addAll&quot;, &quot;append&quot;, &quot;multAll&quot;, &quot;getIndex&quot;, &quot;addAll&quot;, &quot;append&quot;, &quot;multAll&quot;, &quot;getIndex&quot;, &quot;getIndex&quot;, &quot;getIndex&quot;]
[[], [2], [3], [7], [2], [0], [3], [10], [2], [0], [1], [2]]
<strong>Output</strong>
[null, null, null, null, null, 10, null, null, null, 26, 34, 20]

<strong>Explanation</strong>
Fancy fancy = new Fancy();
fancy.append(2);   // fancy sequence: [2]
fancy.addAll(3);   // fancy sequence: [2+3] -&gt; [5]
fancy.append(7);   // fancy sequence: [5, 7]
fancy.multAll(2);  // fancy sequence: [5*2, 7*2] -&gt; [10, 14]
fancy.getIndex(0); // return 10
fancy.addAll(3);   // fancy sequence: [10+3, 14+3] -&gt; [13, 17]
fancy.append(10);  // fancy sequence: [13, 17, 10]
fancy.multAll(2);  // fancy sequence: [13*2, 17*2, 10*2] -&gt; [26, 34, 20]
fancy.getIndex(0); // return 26
fancy.getIndex(1); // return 34
fancy.getIndex(2); // return 20
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= val, inc, m &lt;= 100</code></li>
	<li><code>0 &lt;= idx &lt;= 10<sup>5</sup></code></li>
	<li>At most <code>10<sup>5</sup></code> calls total will be made to <code>append</code>, <code>addAll</code>, <code>multAll</code>, and <code>getIndex</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Cây đoạn

<!-- thinking:start -->

> **Tư duy**
>
> Ta liên tục thêm giá trị, rồi cộng hoặc nhân mọi giá trị đã có trong dãy và truy vấn một chỉ số. Có đến $10^5$ thao tác, nên duyệt toàn bộ dãy sau mỗi lần là quá chậm.
>
> Cả hai cập nhật đều tác động lên phần prefix đã tồn tại, còn truy vấn chỉ yêu cầu một vị trí, nên cây đoạn phù hợp. Mỗi nút lưu tổng trên đoạn cùng phép nhân và phép cộng đang chờ đẩy xuống các nút con.
>
> Độ dài nhiều nhất là $10^5$, nên ta tạo nút khi cần trên đoạn $[1,10^5]$. `append` cập nhật một điểm, `addAll` và `multAll` cập nhật prefix hiện tại, còn `getIndex` đọc một điểm; tất cả đều tính modulo $10^9+7$.

<!-- thinking:end -->

Theo đề bài, `append` chèn một số vào cuối, `addAll` cộng cùng một giá trị vào mọi số hiện tại, `multAll` nhân mọi số hiện tại với cùng một giá trị, còn `getIndex` đọc một vị trí. Đây là các thao tác cộng trên đoạn, nhân trên đoạn và truy vấn điểm, có thể được duy trì bằng cây đoạn.

Mỗi nút lưu:

- `v`: tổng các số trong đoạn;
- `mul`: phép nhân đang chờ đẩy xuống các nút con, ban đầu là $1$;
- `add`: phép cộng đang chờ đẩy xuống các nút con, ban đầu là $0$.

Hai tag có nghĩa là: mọi số trong đoạn trước hết được nhân với `mul`, sau đó cộng thêm `add`. Nếu hai cập nhật tác động lên cùng một đoạn, ta gộp các tag thay vì đi xuống các lá. Sau “nhân với $m_1$ rồi cộng $a_1$” và “nhân với $m_2$ rồi cộng $a_2$”:

$$
(x \cdot m_1 + a_1)\cdot m_2 + a_2 = x\cdot (m_1 m_2) + (a_1 m_2 + a_2)
$$

thì phép nhân mới là $m_1 m_2$ và phép cộng mới là $a_1 m_2 + a_2$.

Vì vậy, nhân một đoạn với $m$ sẽ nhân `v`, `mul` và `add` của nút với $m$; cộng $inc$ làm `v` tăng $\textit{length} \times inc$ và `add` tăng $inc$. Khi đẩy xuống, áp dụng cùng quy tắc “nhân rồi cộng” cho cả hai nút con. Trong code, phép cộng và phép nhân là một thao tác: chỉ cộng tương đương “nhân với $1$ rồi cộng $inc$”, còn chỉ nhân tương đương “nhân với $m$ rồi cộng $0$”.

Chỉ số bắt đầu từ $1$ và độ dài nhiều nhất là $10^5$, nên ta tạo nút khi cần trên $[1,10^5]$. `append` tăng $n$ và thêm $val$ tại vị trí $n$; `addAll` và `multAll` cập nhật $[1,n]$; `getIndex` truy vấn vị trí $idx+1$. Mọi phép tính đều theo modulo $10^9+7$.

Độ phức tạp thời gian là $O(m \log n)$ và độ phức tạp không gian là $O(m \log n)$, trong đó $m$ là số thao tác và $n \le 10^5$ là giới hạn độ dài.

<!-- tabs:start -->

#### Python3

```python
MOD = 10**9 + 7


class Node:
    __slots__ = "left", "right", "l", "r", "mid", "v", "add", "mul"

    def __init__(self, l, r):
        self.left = self.right = None
        self.l, self.r = l, r
        self.mid = (l + r) >> 1
        self.v = self.add = 0
        self.mul = 1


class SegmentTree:
    def __init__(self):
        self.root = Node(1, 10**5 + 1)

    def modify(self, l, r, mul, add, node=None):
        if l > r:
            return
        if node is None:
            node = self.root
        if node.l >= l and node.r <= r:
            self.apply(node, mul, add)
            return
        self.pushdown(node)
        if l <= node.mid:
            self.modify(l, r, mul, add, node.left)
        if r > node.mid:
            self.modify(l, r, mul, add, node.right)
        self.pushup(node)

    def query(self, l, r, node=None):
        if l > r:
            return 0
        if node is None:
            node = self.root
        if node.l >= l and node.r <= r:
            return node.v
        self.pushdown(node)
        v = 0
        if l <= node.mid:
            v = (v + self.query(l, r, node.left)) % MOD
        if r > node.mid:
            v = (v + self.query(l, r, node.right)) % MOD
        return v

    def apply(self, node, mul, add):
        node.v = (node.v * mul + (node.r - node.l + 1) * add) % MOD
        node.add = (node.add * mul + add) % MOD
        node.mul = node.mul * mul % MOD

    def pushup(self, node):
        node.v = (node.left.v + node.right.v) % MOD

    def pushdown(self, node):
        if node.left is None:
            node.left = Node(node.l, node.mid)
        if node.right is None:
            node.right = Node(node.mid + 1, node.r)
        if node.add or node.mul != 1:
            self.apply(node.left, node.mul, node.add)
            self.apply(node.right, node.mul, node.add)
            node.add = 0
            node.mul = 1


class Fancy:
    def __init__(self):
        self.n = 0
        self.tree = SegmentTree()

    def append(self, val: int) -> None:
        self.n += 1
        self.tree.modify(self.n, self.n, 1, val)

    def addAll(self, inc: int) -> None:
        self.tree.modify(1, self.n, 1, inc)

    def multAll(self, m: int) -> None:
        self.tree.modify(1, self.n, m, 0)

    def getIndex(self, idx: int) -> int:
        return -1 if idx >= self.n else self.tree.query(idx + 1, idx + 1)
```

#### Java

```java
class Node {
    Node left;
    Node right;
    int l, r, mid;
    long v, add, mul = 1;

    Node(int l, int r) {
        this.l = l;
        this.r = r;
        this.mid = (l + r) >> 1;
    }
}

class SegmentTree {
    private static final int MOD = (int) 1e9 + 7;
    private Node root = new Node(1, (int) 1e5 + 1);

    void modify(int l, int r, int mul, int add) {
        modify(l, r, mul, add, root);
    }

    void modify(int l, int r, int mul, int add, Node node) {
        if (l > r) {
            return;
        }
        if (node.l >= l && node.r <= r) {
            apply(node, mul, add);
            return;
        }
        pushdown(node);
        if (l <= node.mid) {
            modify(l, r, mul, add, node.left);
        }
        if (r > node.mid) {
            modify(l, r, mul, add, node.right);
        }
        pushup(node);
    }

    int query(int l, int r) {
        return query(l, r, root);
    }

    int query(int l, int r, Node node) {
        if (l > r) {
            return 0;
        }
        if (node.l >= l && node.r <= r) {
            return (int) node.v;
        }
        pushdown(node);
        int v = 0;
        if (l <= node.mid) {
            v = (v + query(l, r, node.left)) % MOD;
        }
        if (r > node.mid) {
            v = (v + query(l, r, node.right)) % MOD;
        }
        return v;
    }

    void apply(Node node, long mul, long add) {
        node.v = (node.v * mul + (node.r - node.l + 1) * add) % MOD;
        node.add = (node.add * mul + add) % MOD;
        node.mul = node.mul * mul % MOD;
    }

    void pushup(Node node) {
        node.v = (node.left.v + node.right.v) % MOD;
    }

    void pushdown(Node node) {
        if (node.left == null) {
            node.left = new Node(node.l, node.mid);
        }
        if (node.right == null) {
            node.right = new Node(node.mid + 1, node.r);
        }
        if (node.add != 0 || node.mul != 1) {
            apply(node.left, node.mul, node.add);
            apply(node.right, node.mul, node.add);
            node.add = 0;
            node.mul = 1;
        }
    }
}

class Fancy {
    private int n;
    private SegmentTree tree = new SegmentTree();

    public void append(int val) {
        ++n;
        tree.modify(n, n, 1, val);
    }

    public void addAll(int inc) {
        tree.modify(1, n, 1, inc);
    }

    public void multAll(int m) {
        tree.modify(1, n, m, 0);
    }

    public int getIndex(int idx) {
        return idx >= n ? -1 : tree.query(idx + 1, idx + 1);
    }
}
```

#### C++

```cpp
const int MOD = 1e9 + 7;

class Node {
public:
    Node* left = nullptr;
    Node* right = nullptr;
    int l, r, mid;
    long long v = 0, add = 0, mul = 1;

    Node(int l, int r)
        : l(l)
        , r(r)
        , mid((l + r) >> 1) {}
};

class SegmentTree {
public:
    SegmentTree()
        : root(new Node(1, 1e5 + 1)) {}

    void modify(int l, int r, int mul, int add) {
        modify(l, r, mul, add, root);
    }

    int query(int l, int r) {
        return query(l, r, root);
    }

private:
    Node* root;

    void modify(int l, int r, int mul, int add, Node* node) {
        if (l > r) {
            return;
        }
        if (node->l >= l && node->r <= r) {
            apply(node, mul, add);
            return;
        }
        pushdown(node);
        if (l <= node->mid) {
            modify(l, r, mul, add, node->left);
        }
        if (r > node->mid) {
            modify(l, r, mul, add, node->right);
        }
        pushup(node);
    }

    int query(int l, int r, Node* node) {
        if (l > r) {
            return 0;
        }
        if (node->l >= l && node->r <= r) {
            return node->v;
        }
        pushdown(node);
        int v = 0;
        if (l <= node->mid) {
            v = (v + query(l, r, node->left)) % MOD;
        }
        if (r > node->mid) {
            v = (v + query(l, r, node->right)) % MOD;
        }
        return v;
    }

    void apply(Node* node, long long mul, long long add) {
        node->v = (node->v * mul + (node->r - node->l + 1) * add) % MOD;
        node->add = (node->add * mul + add) % MOD;
        node->mul = node->mul * mul % MOD;
    }

    void pushup(Node* node) {
        node->v = (node->left->v + node->right->v) % MOD;
    }

    void pushdown(Node* node) {
        if (!node->left) {
            node->left = new Node(node->l, node->mid);
        }
        if (!node->right) {
            node->right = new Node(node->mid + 1, node->r);
        }
        if (node->add || node->mul != 1) {
            apply(node->left, node->mul, node->add);
            apply(node->right, node->mul, node->add);
            node->add = 0;
            node->mul = 1;
        }
    }
};

class Fancy {
public:
    void append(int val) {
        ++n;
        tree.modify(n, n, 1, val);
    }

    void addAll(int inc) {
        tree.modify(1, n, 1, inc);
    }

    void multAll(int m) {
        tree.modify(1, n, m, 0);
    }

    int getIndex(int idx) {
        return idx >= n ? -1 : tree.query(idx + 1, idx + 1);
    }

private:
    int n = 0;
    SegmentTree tree;
};
```

#### Go

```go
const mod int64 = 1e9 + 7

type node struct {
	left, right *node
	l, r, mid   int
	v, add, mul int64
}

func newNode(l, r int) *node {
	return &node{l: l, r: r, mid: (l + r) >> 1, mul: 1}
}

type segmentTree struct{ root *node }

func newSegmentTree() *segmentTree {
	return &segmentTree{root: newNode(1, 100001)}
}

func (t *segmentTree) modify(l, r int, mul, add int64, o *node) {
	if l > r {
		return
	}
	if o.l >= l && o.r <= r {
		t.apply(o, mul, add)
		return
	}
	t.pushdown(o)
	if l <= o.mid {
		t.modify(l, r, mul, add, o.left)
	}
	if r > o.mid {
		t.modify(l, r, mul, add, o.right)
	}
	t.pushup(o)
}

func (t *segmentTree) query(l, r int, o *node) int64 {
	if l > r {
		return 0
	}
	if o.l >= l && o.r <= r {
		return o.v
	}
	t.pushdown(o)
	var v int64
	if l <= o.mid {
		v = (v + t.query(l, r, o.left)) % mod
	}
	if r > o.mid {
		v = (v + t.query(l, r, o.right)) % mod
	}
	return v
}

func (t *segmentTree) apply(o *node, mul, add int64) {
	o.v = (o.v*mul + int64(o.r-o.l+1)*add) % mod
	o.add = (o.add*mul + add) % mod
	o.mul = o.mul * mul % mod
}

func (t *segmentTree) pushup(o *node) {
	o.v = (o.left.v + o.right.v) % mod
}

func (t *segmentTree) pushdown(o *node) {
	if o.left == nil {
		o.left = newNode(o.l, o.mid)
	}
	if o.right == nil {
		o.right = newNode(o.mid+1, o.r)
	}
	if o.add != 0 || o.mul != 1 {
		t.apply(o.left, o.mul, o.add)
		t.apply(o.right, o.mul, o.add)
		o.add, o.mul = 0, 1
	}
}

type Fancy struct {
	n    int
	tree *segmentTree
}

func Constructor() Fancy {
	return Fancy{tree: newSegmentTree()}
}

func (f *Fancy) Append(val int) {
	f.n++
	f.tree.modify(f.n, f.n, 1, int64(val), f.tree.root)
}

func (f *Fancy) AddAll(inc int) {
	f.tree.modify(1, f.n, 1, int64(inc), f.tree.root)
}

func (f *Fancy) MultAll(m int) {
	f.tree.modify(1, f.n, int64(m), 0, f.tree.root)
}

func (f *Fancy) GetIndex(idx int) int {
	if idx >= f.n {
		return -1
	}
	return int(f.tree.query(idx+1, idx+1, f.tree.root))
}
```

#### TypeScript

```ts
const mod = BigInt(1e9 + 7);

class Node {
    left: Node | null = null;
    right: Node | null = null;
    l: number;
    r: number;
    mid: number;
    v = 0n;
    add = 0n;
    mul = 1n;

    constructor(l: number, r: number) {
        this.l = l;
        this.r = r;
        this.mid = (l + r) >> 1;
    }
}

class SegmentTree {
    root = new Node(1, 1e5 + 1);

    modify(l: number, r: number, mul: bigint, add: bigint, node = this.root): void {
        if (l > r) {
            return;
        }
        if (node.l >= l && node.r <= r) {
            this.apply(node, mul, add);
            return;
        }
        this.pushdown(node);
        if (l <= node.mid) {
            this.modify(l, r, mul, add, node.left!);
        }
        if (r > node.mid) {
            this.modify(l, r, mul, add, node.right!);
        }
        this.pushup(node);
    }

    query(l: number, r: number, node = this.root): bigint {
        if (l > r) {
            return 0n;
        }
        if (node.l >= l && node.r <= r) {
            return node.v;
        }
        this.pushdown(node);
        let v = 0n;
        if (l <= node.mid) {
            v = (v + this.query(l, r, node.left!)) % mod;
        }
        if (r > node.mid) {
            v = (v + this.query(l, r, node.right!)) % mod;
        }
        return v;
    }

    apply(node: Node, mul: bigint, add: bigint): void {
        node.v = (node.v * mul + BigInt(node.r - node.l + 1) * add) % mod;
        node.add = (node.add * mul + add) % mod;
        node.mul = (node.mul * mul) % mod;
    }

    pushup(node: Node): void {
        node.v = (node.left!.v + node.right!.v) % mod;
    }

    pushdown(node: Node): void {
        node.left ??= new Node(node.l, node.mid);
        node.right ??= new Node(node.mid + 1, node.r);
        if (node.add !== 0n || node.mul !== 1n) {
            this.apply(node.left, node.mul, node.add);
            this.apply(node.right, node.mul, node.add);
            node.add = 0n;
            node.mul = 1n;
        }
    }
}

class Fancy {
    private n = 0;
    private tree = new SegmentTree();

    append(val: number): void {
        this.n++;
        this.tree.modify(this.n, this.n, 1n, BigInt(val));
    }

    addAll(inc: number): void {
        this.tree.modify(1, this.n, 1n, BigInt(inc));
    }

    multAll(m: number): void {
        this.tree.modify(1, this.n, BigInt(m), 0n);
    }

    getIndex(idx: number): number {
        return idx >= this.n ? -1 : Number(this.tree.query(idx + 1, idx + 1));
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Toán học + Nghịch đảo modulo

<!-- thinking:start -->

> **Tư duy**
>
> Cây đoạn vẫn phải đi qua $O(\log n)$ nút trong mỗi lần cập nhật. `addAll` và `multAll` luôn tác động lên mọi số đã có trong dãy, còn số mới thêm không bị ảnh hưởng bởi các phép cộng hoặc nhân trước đó.
>
> Vì vậy, mọi số đã tồn tại sẽ trải qua cùng các phép cộng và nhân về sau. Ta chỉ cần hai biến toàn cục: các số đó còn phải được nhân bao nhiêu, rồi cộng thêm bao nhiêu. Mảng lưu giá trị trước các phép toán này; khi truy vấn, ta nhân và cộng để khôi phục giá trị thực. Modulo là số nguyên tố, nên phép chia cho $a$ được thay bằng phép nhân với nghịch đảo modulo (định lý nhỏ Fermat).

<!-- thinking:end -->

Mỗi `addAll` và `multAll` áp dụng cho mọi số đang tồn tại tại thời điểm đó, còn số được thêm sau không bị ảnh hưởng bởi các cập nhật trước. Do đó, phép nhân và cộng đang chờ của mọi số hiện tại có thể được mô tả bằng hai biến toàn cục: nhân với $a$, rồi cộng $b$. Ban đầu $a=1$ và $b=0$.

Mảng `nums` không lưu giá trị thực hiện tại. Nó lưu các giá trị trước khi nhân với $a$ và cộng $b$, sao cho tại mọi thời điểm

$$
\text{true value} = (a \times \textit{nums}[i] + b) \bmod (10^9+7)
$$

Các thao tác trở thành:

- `append(val)`: số mới này chưa trải qua $a$ và $b$ hiện tại, nên lưu $x$ sao cho $a \times x + b = \textit{val}$, tức là $x = (\textit{val} - b) \times a^{-1}$;
- `addAll(inc)`: mọi giá trị thực tăng thêm $inc$, nên cộng $inc$ vào $b$;
- `multAll(m)`: mọi giá trị thực được nhân với $m$, nên nhân cả $a$ và $b$ với $m$;
- `getIndex(idx)`: trả về $-1$ nếu chỉ số nằm ngoài phạm vi, nếu không thì trả về $a \times \textit{nums}[idx] + b$.

Ở đây $a^{-1}$ là nghịch đảo modulo của $a$ theo modulo $10^9+7$. Vì modulo là số nguyên tố, định lý nhỏ Fermat cho $a^{-1} \equiv a^{MOD-2} \pmod{MOD}$.

Mọi thao tác, ngoại trừ phép tìm nghịch đảo trong `append`, chạy trong $O(1)$; phép tìm nghịch đảo chạy trong $O(\log MOD)$. Độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Fancy:
    def __init__(self):
        self.mod = 10**9 + 7
        self.nums = []
        self.a = 1
        self.b = 0

    def append(self, val: int) -> None:
        x = (val - self.b) * pow(self.a, self.mod - 2, self.mod) % self.mod
        self.nums.append(x)

    def addAll(self, inc: int) -> None:
        self.b = (self.b + inc) % self.mod

    def multAll(self, m: int) -> None:
        self.a = self.a * m % self.mod
        self.b = self.b * m % self.mod

    def getIndex(self, idx: int) -> int:
        if idx >= len(self.nums):
            return -1
        return (self.a * self.nums[idx] + self.b) % self.mod
```

#### Java

```java
class Fancy {
    private static final int MOD = (int) 1e9 + 7;
    private List<Integer> nums = new ArrayList<>();
    private long a = 1, b;

    public void append(int val) {
        long x = (val - b + MOD) % MOD * qpow(a, MOD - 2) % MOD;
        nums.add((int) x);
    }

    public void addAll(int inc) {
        b = (b + inc) % MOD;
    }

    public void multAll(int m) {
        a = a * m % MOD;
        b = b * m % MOD;
    }

    public int getIndex(int idx) {
        if (idx >= nums.size()) {
            return -1;
        }
        return (int) ((a * nums.get(idx) + b) % MOD);
    }

    private long qpow(long x, int n) {
        long res = 1;
        while (n > 0) {
            if ((n & 1) == 1) {
                res = res * x % MOD;
            }
            x = x * x % MOD;
            n >>= 1;
        }
        return res;
    }
}
```

#### C++

```cpp
class Fancy {
public:
    void append(int val) {
        long long x = (val - b + mod) % mod * qpow(a, mod - 2) % mod;
        nums.push_back(x);
    }

    void addAll(int inc) {
        b = (b + inc) % mod;
    }

    void multAll(int m) {
        a = a * m % mod;
        b = b * m % mod;
    }

    int getIndex(int idx) {
        if (idx >= nums.size()) {
            return -1;
        }
        return (a * nums[idx] + b) % mod;
    }

private:
    const int mod = 1e9 + 7;
    vector<long long> nums;
    long long a = 1, b = 0;

    long long qpow(long long x, int n) {
        long long res = 1;
        while (n) {
            if (n & 1) {
                res = res * x % mod;
            }
            x = x * x % mod;
            n >>= 1;
        }
        return res;
    }
};
```

#### Go

```go
const mod int = 1e9 + 7

func qpow(x, n int) int {
	res := 1
	for n > 0 {
		if n&1 == 1 {
			res = res * x % mod
		}
		x = x * x % mod
		n >>= 1
	}
	return res
}

type Fancy struct {
	nums []int
	a, b int
}

func Constructor() Fancy {
	return Fancy{a: 1}
}

func (f *Fancy) Append(val int) {
	x := (val - f.b + mod) % mod * qpow(f.a, mod-2) % mod
	f.nums = append(f.nums, x)
}

func (f *Fancy) AddAll(inc int) {
	f.b = (f.b + inc) % mod
}

func (f *Fancy) MultAll(m int) {
	f.a = f.a * m % mod
	f.b = f.b * m % mod
}

func (f *Fancy) GetIndex(idx int) int {
	if idx >= len(f.nums) {
		return -1
	}
	return (f.a*f.nums[idx] + f.b) % mod
}
```

#### TypeScript

```ts
class Fancy {
    private mod = BigInt(1e9 + 7);
    private nums: bigint[] = [];
    private a = 1n;
    private b = 0n;

    append(val: number): void {
        const x = (((BigInt(val) - this.b) % this.mod) + this.mod) % this.mod;
        this.nums.push((x * this.qpow(this.a, 1e9 + 5)) % this.mod);
    }

    addAll(inc: number): void {
        this.b = (this.b + BigInt(inc)) % this.mod;
    }

    multAll(m: number): void {
        this.a = (this.a * BigInt(m)) % this.mod;
        this.b = (this.b * BigInt(m)) % this.mod;
    }

    getIndex(idx: number): number {
        if (idx >= this.nums.length) {
            return -1;
        }
        return Number((this.a * this.nums[idx] + this.b) % this.mod);
    }

    private qpow(x: bigint, n: number): bigint {
        let res = 1n;
        while (n) {
            if (n & 1) {
                res = (res * x) % this.mod;
            }
            x = (x * x) % this.mod;
            n >>= 1;
        }
        return res;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
