---
comments: true
difficulty: Hard
tags:
    - Design
    - Segment Tree
    - Ordered Set
---

<!-- problem:start -->

# [715. Range Module](https://leetcode.com/problems/range-module)

[中文文档](/solution/0700-0799/0715.Range%20Module/README.md)

## Mô tả

<!-- description:start -->

<p>Range Module là module theo dõi các khoảng số. Hãy thiết kế cấu trúc dữ liệu để theo dõi các khoảng được biểu diễn dưới dạng <strong>nửa kín</strong> và truy vấn chúng.</p>

<p><strong>Khoảng nửa kín</strong> <code>[left, right)</code> gồm mọi số thực <code>x</code> thỏa mãn <code>left &lt;= x &lt; right</code>.</p>

<p>Hãy cài đặt class <code>RangeModule</code>:</p>

<ul>
	<li><code>RangeModule()</code> Khởi tạo object của cấu trúc dữ liệu.</li>
	<li><code>void addRange(int left, int right)</code> Thêm <strong>khoảng nửa kín</strong> <code>[left, right)</code>, theo dõi mọi số thực trong khoảng đó. Nếu khoảng mới giao một phần với các số đang được theo dõi, hãy thêm những số trong <code>[left, right)</code> chưa được theo dõi.</li>
	<li><code>boolean queryRange(int left, int right)</code> Trả về <code>true</code> nếu hiện đang theo dõi mọi số thực trong khoảng <code>[left, right)</code>, nếu không thì trả về <code>false</code>.</li>
	<li><code>void removeRange(int left, int right)</code> Ngừng theo dõi mọi số thực hiện đang được theo dõi trong <strong>khoảng nửa kín</strong> <code>[left, right)</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;RangeModule&quot;, &quot;addRange&quot;, &quot;removeRange&quot;, &quot;queryRange&quot;, &quot;queryRange&quot;, &quot;queryRange&quot;]
[[], [10, 20], [14, 16], [10, 14], [13, 15], [16, 17]]
<strong>Đầu ra</strong>
[null, null, null, true, false, true]

<strong>Giải thích</strong>
RangeModule rangeModule = new RangeModule();
rangeModule.addRange(10, 20);
rangeModule.removeRange(14, 16);
rangeModule.queryRange(10, 14); // return True,(Every number in [10, 14) is being tracked)
rangeModule.queryRange(13, 15); // return False,(Numbers like 14, 14.03, 14.17 in [13, 15) are not being tracked)
rangeModule.queryRange(16, 17); // return True, (The number 16 in [16, 17) is still being tracked, despite the remove operation)
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= left &lt; right &lt;= 10<sup>9</sup></code></li>
	<li>Có nhiều nhất <code>10<sup>4</sup></code> lần gọi đến <code>addRange</code>, <code>queryRange</code> và <code>removeRange</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Segment Tree

<!-- thinking:start -->

> **Tư duy**
>
> Cần hỗ trợ thêm, xóa và truy vấn độ phủ trên các khoảng nửa kín có điểm đầu mút đến $10^9$ với khoảng $10^4$ thao tác. Gộp một danh sách khoảng tường minh có thể phải duyệt nhiều đoạn con trong mỗi lần cập nhật.
>
> Đây là các thao tác gán giá trị và truy vấn trên khoảng. Segment tree xử lý chúng trong thời gian logarithm; miền giá trị quá lớn để cấp phát toàn bộ cây, nên ta tạo node khi cần và đẩy các nhãn gán lazy xuống.
>
> Mỗi node lưu trạng thái đoạn của nó có được phủ hoàn toàn hay không. Nếu cập nhật bao trọn đoạn của node, ta gán nhãn cho node đó; truy vấn trả về $\textit{v}$ nếu node được phủ hoàn toàn, nếu không thì đẩy lazy xuống và kết hợp kết quả của hai node con bằng phép AND. Mỗi thao tác tốn $O(\log n)$.

<!-- thinking:end -->

Theo đề bài, ta cần duy trì một tập các khoảng và hỗ trợ thêm, xóa, truy vấn khoảng. Có thể dùng segment tree để quản lý các thao tác thêm và xóa khoảng.

Segment tree chia toàn bộ khoảng thành nhiều khoảng con rời nhau, với số lượng không quá $\log(width)$. Để cập nhật giá trị một phần tử, chỉ cần cập nhật $\log(width)$ khoảng; các khoảng này đều nằm trong một khoảng lớn chứa phần tử đó. Khi cập nhật khoảng, ta cần dùng **lazy propagation** để đảm bảo hiệu quả.

- Mỗi node của segment tree biểu diễn một khoảng;
- Segment tree có một root node duy nhất, biểu diễn toàn bộ miền cần quản lý, chẳng hạn $[1,N]$;
- Mỗi leaf node của segment tree biểu diễn một khoảng cơ sở có độ dài $1$, $[x,x]$;
- Với mỗi node nội bộ $[l,r]$, node con trái là $[l,mid]$, node con phải là $[mid+1,r]$, trong đó $mid=⌊(l+r)/2⌋$ (làm tròn xuống).

Do miền dữ liệu của bài toán rất lớn, ta có thể cài đặt segment tree cấp phát động. Nghĩa là chỉ cấp phát node khi cần thay vì cấp phát toàn bộ node ngay từ đầu. Cách này tiết kiệm bộ nhớ, nhưng cần dùng **lazy propagation** để xử lý cập nhật khoảng.

Mỗi thao tác có độ phức tạp thời gian $O(\log n)$. Độ phức tạp không gian là $O(m \times \log n)$, trong đó $m$ là số thao tác và $n$ là miền dữ liệu.

<!-- tabs:start -->

#### Python3

```python
class Node:
    __slots__ = ['left', 'right', 'add', 'v']

    def __init__(self):
        self.left = None
        self.right = None
        self.add = 0
        self.v = False


class SegmentTree:
    __slots__ = ['root']

    def __init__(self):
        self.root = Node()

    def modify(self, left, right, v, l=1, r=int(1e9), node=None):
        if node is None:
            node = self.root
        if l >= left and r <= right:
            if v == 1:
                node.add = 1
                node.v = True
            else:
                node.add = -1
                node.v = False
            return
        self.pushdown(node)
        mid = (l + r) >> 1
        if left <= mid:
            self.modify(left, right, v, l, mid, node.left)
        if right > mid:
            self.modify(left, right, v, mid + 1, r, node.right)
        self.pushup(node)

    def query(self, left, right, l=1, r=int(1e9), node=None):
        if node is None:
            node = self.root
        if l >= left and r <= right:
            return node.v
        self.pushdown(node)
        mid = (l + r) >> 1
        v = True
        if left <= mid:
            v = v and self.query(left, right, l, mid, node.left)
        if right > mid:
            v = v and self.query(left, right, mid + 1, r, node.right)
        return v

    def pushup(self, node):
        node.v = bool(node.left and node.left.v and node.right and node.right.v)

    def pushdown(self, node):
        if node.left is None:
            node.left = Node()
        if node.right is None:
            node.right = Node()
        if node.add:
            node.left.add = node.right.add = node.add
            node.left.v = node.add == 1
            node.right.v = node.add == 1
            node.add = 0


class RangeModule:
    def __init__(self):
        self.tree = SegmentTree()

    def addRange(self, left: int, right: int) -> None:
        self.tree.modify(left, right - 1, 1)

    def queryRange(self, left: int, right: int) -> bool:
        return self.tree.query(left, right - 1)

    def removeRange(self, left: int, right: int) -> None:
        self.tree.modify(left, right - 1, -1)


# Your RangeModule object will be instantiated and called as such:
# obj = RangeModule()
# obj.addRange(left,right)
# param_2 = obj.queryRange(left,right)
# obj.removeRange(left,right)
```

#### Java

```java
class Node {
    Node left;
    Node right;
    int add;
    boolean v;
}

class SegmentTree {
    private Node root = new Node();

    public SegmentTree() {
    }

    public void modify(int left, int right, int v) {
        modify(left, right, v, 1, (int) 1e9, root);
    }

    public void modify(int left, int right, int v, int l, int r, Node node) {
        if (l >= left && r <= right) {
            node.v = v == 1;
            node.add = v;
            return;
        }
        pushdown(node);
        int mid = (l + r) >> 1;
        if (left <= mid) {
            modify(left, right, v, l, mid, node.left);
        }
        if (right > mid) {
            modify(left, right, v, mid + 1, r, node.right);
        }
        pushup(node);
    }

    public boolean query(int left, int right) {
        return query(left, right, 1, (int) 1e9, root);
    }

    public boolean query(int left, int right, int l, int r, Node node) {
        if (l >= left && r <= right) {
            return node.v;
        }
        pushdown(node);
        int mid = (l + r) >> 1;
        boolean v = true;
        if (left <= mid) {
            v = v && query(left, right, l, mid, node.left);
        }
        if (right > mid) {
            v = v && query(left, right, mid + 1, r, node.right);
        }
        return v;
    }

    public void pushup(Node node) {
        node.v = node.left != null && node.left.v && node.right != null && node.right.v;
    }

    public void pushdown(Node node) {
        if (node.left == null) {
            node.left = new Node();
        }
        if (node.right == null) {
            node.right = new Node();
        }
        if (node.add != 0) {
            node.left.add = node.add;
            node.right.add = node.add;
            node.left.v = node.add == 1;
            node.right.v = node.add == 1;
            node.add = 0;
        }
    }
}

class RangeModule {
    private SegmentTree tree = new SegmentTree();

    public RangeModule() {
    }

    public void addRange(int left, int right) {
        tree.modify(left, right - 1, 1);
    }

    public boolean queryRange(int left, int right) {
        return tree.query(left, right - 1);
    }

    public void removeRange(int left, int right) {
        tree.modify(left, right - 1, -1);
    }
}

/**
 * Your RangeModule object will be instantiated and called as such:
 * RangeModule obj = new RangeModule();
 * obj.addRange(left,right);
 * boolean param_2 = obj.queryRange(left,right);
 * obj.removeRange(left,right);
 */
```

#### C++

```cpp
template <class T>
class CachedObj {
public:
    void* operator new(size_t s) {
        if (!head) {
            T* a = new T[SIZE];
            for (size_t i = 0; i < SIZE; ++i)
                add(a + i);
        }
        T* p = head;
        head = head->CachedObj<T>::next;
        return p;
    }
    void operator delete(void* p, size_t) {
        if (p) add(static_cast<T*>(p));
    }
    virtual ~CachedObj() {}

protected:
    T* next;

private:
    static T* head;
    static const size_t SIZE;
    static void add(T* p) {
        p->CachedObj<T>::next = head;
        head = p;
    }
};
template <class T>
T* CachedObj<T>::head = 0;
template <class T>
const size_t CachedObj<T>::SIZE = 10000;
class Node : public CachedObj<Node> {
public:
    Node* left;
    Node* right;
    int add;
    bool v;
};

class SegmentTree {
private:
    Node* root;

public:
    SegmentTree() {
        root = new Node();
    }

    void modify(int left, int right, int v) {
        modify(left, right, v, 1, 1e9, root);
    }

    void modify(int left, int right, int v, int l, int r, Node* node) {
        if (l >= left && r <= right) {
            node->v = v == 1;
            node->add = v;
            return;
        }
        pushdown(node);
        int mid = (l + r) >> 1;
        if (left <= mid) modify(left, right, v, l, mid, node->left);
        if (right > mid) modify(left, right, v, mid + 1, r, node->right);
        pushup(node);
    }

    bool query(int left, int right) {
        return query(left, right, 1, 1e9, root);
    }

    bool query(int left, int right, int l, int r, Node* node) {
        if (l >= left && r <= right) return node->v;
        pushdown(node);
        int mid = (l + r) >> 1;
        bool v = true;
        if (left <= mid) v = v && query(left, right, l, mid, node->left);
        if (right > mid) v = v && query(left, right, mid + 1, r, node->right);
        return v;
    }

    void pushup(Node* node) {
        node->v = node->left && node->left->v && node->right && node->right->v;
    }

    void pushdown(Node* node) {
        if (!node->left) node->left = new Node();
        if (!node->right) node->right = new Node();
        if (node->add) {
            node->left->add = node->right->add = node->add;
            node->left->v = node->right->v = node->add == 1;
            node->add = 0;
        }
    }
};

class RangeModule {
public:
    SegmentTree* tree;

    RangeModule() {
        tree = new SegmentTree();
    }

    void addRange(int left, int right) {
        tree->modify(left, right - 1, 1);
    }

    bool queryRange(int left, int right) {
        return tree->query(left, right - 1);
    }

    void removeRange(int left, int right) {
        tree->modify(left, right - 1, -1);
    }
};

/**
 * Your RangeModule object will be instantiated and called as such:
 * RangeModule* obj = new RangeModule();
 * obj->addRange(left,right);
 * bool param_2 = obj->queryRange(left,right);
 * obj->removeRange(left,right);
 */
```

#### Go

```go
const N int = 1e9

type node struct {
	lch   *node
	rch   *node
	added bool
	lazy  int
}

type segmentTree struct {
	root *node
}

func newSegmentTree() *segmentTree {
	return &segmentTree{
		root: new(node),
	}
}

func (t *segmentTree) update(n *node, l, r, i, j, x int) {
	if l >= i && r <= j {
		n.added = x == 1
		n.lazy = x
		return
	}
	t.pushdown(n)
	m := int(uint(l+r) >> 1)
	if i <= m {
		t.update(n.lch, l, m, i, j, x)
	}
	if j > m {
		t.update(n.rch, m+1, r, i, j, x)
	}
	t.pushup(n)
}

func (t *segmentTree) query(n *node, l, r, i, j int) bool {
	if l >= i && r <= j {
		return n.added
	}
	t.pushdown(n)
	v := true
	m := int(uint(l+r) >> 1)
	if i <= m {
		v = v && t.query(n.lch, l, m, i, j)
	}
	if j > m {
		v = v && t.query(n.rch, m+1, r, i, j)
	}
	return v
}

func (t *segmentTree) pushup(n *node) {
	n.added = n.lch.added && n.rch.added
}

func (t *segmentTree) pushdown(n *node) {
	if n.lch == nil {
		n.lch = new(node)
	}
	if n.rch == nil {
		n.rch = new(node)
	}
	if n.lazy != 0 {
		n.lch.added = n.lazy == 1
		n.rch.added = n.lazy == 1
		n.lch.lazy = n.lazy
		n.rch.lazy = n.lazy
		n.lazy = 0
	}
}

type RangeModule struct {
	t *segmentTree
}

func Constructor() RangeModule {
	return RangeModule{
		t: newSegmentTree(),
	}
}

func (this *RangeModule) AddRange(left int, right int) {
	this.t.update(this.t.root, 1, N, left, right-1, 1)
}

func (this *RangeModule) QueryRange(left int, right int) bool {
	return this.t.query(this.t.root, 1, N, left, right-1)
}

func (this *RangeModule) RemoveRange(left int, right int) {
	this.t.update(this.t.root, 1, N, left, right-1, -1)
}

/**
 * Your RangeModule object will be instantiated and called as such:
 * obj := Constructor();
 * obj.AddRange(left,right);
 * param_2 := obj.QueryRange(left,right);
 * obj.RemoveRange(left,right);
 */
```

#### TypeScript

```ts
class Node {
    left: Node | null;
    right: Node | null;
    add: number;
    v: boolean;

    constructor() {
        this.left = null;
        this.right = null;
        this.add = 0;
        this.v = false;
    }
}

class SegmentTree {
    private root: Node;

    constructor() {
        this.root = new Node();
    }

    modify(
        left: number,
        right: number,
        v: number,
        l: number = 1,
        r: number = 1e9,
        node: Node | null = null,
    ): void {
        if (node === null) {
            node = this.root;
        }

        if (l >= left && r <= right) {
            node.v = v === 1;
            node.add = v;
            return;
        }

        this.pushdown(node);

        const mid = (l + r) >> 1;

        if (left <= mid) {
            this.modify(left, right, v, l, mid, node.left);
        }

        if (right > mid) {
            this.modify(left, right, v, mid + 1, r, node.right);
        }

        this.pushup(node);
    }

    query(
        left: number,
        right: number,
        l: number = 1,
        r: number = 1e9,
        node: Node | null = null,
    ): boolean {
        if (node === null) {
            node = this.root;
        }

        if (l >= left && r <= right) {
            return node.v;
        }

        this.pushdown(node);

        const mid = (l + r) >> 1;
        let result = true;

        if (left <= mid) {
            result = result && this.query(left, right, l, mid, node.left);
        }

        if (right > mid) {
            result = result && this.query(left, right, mid + 1, r, node.right);
        }

        return result;
    }

    pushup(node: Node): void {
        node.v = !!(node.left && node.left.v && node.right && node.right.v);
    }

    pushdown(node: Node): void {
        if (node.left === null) {
            node.left = new Node();
        }

        if (node.right === null) {
            node.right = new Node();
        }

        if (node.add !== 0) {
            node.left.add = node.add;
            node.right.add = node.add;
            node.left.v = node.add === 1;
            node.right.v = node.add === 1;
            node.add = 0;
        }
    }
}

class RangeModule {
    private tree: SegmentTree;

    constructor() {
        this.tree = new SegmentTree();
    }

    addRange(left: number, right: number): void {
        this.tree.modify(left, right - 1, 1);
    }

    queryRange(left: number, right: number): boolean {
        return this.tree.query(left, right - 1);
    }

    removeRange(left: number, right: number): void {
        this.tree.modify(left, right - 1, -1);
    }
}

/**
 * Your RangeModule object will be instantiated and called as such:
 * var obj = new RangeModule()
 * obj.addRange(left,right)
 * var param_2 = obj.queryRange(left,right)
 * obj.removeRange(left,right)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
