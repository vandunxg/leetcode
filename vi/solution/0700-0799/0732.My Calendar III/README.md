---
comments: true
difficulty: Hard
tags:
    - Design
    - Segment Tree
    - Binary Search
    - Ordered Set
    - Prefix Sum
---

<!-- problem:start -->

# [732. My Calendar III](https://leetcode.com/problems/my-calendar-iii)

[中文文档](/solution/0700-0799/0732.My%20Calendar%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>k</code> sự kiện đặt lịch chồng lấn khi giao của chúng không rỗng (tức là tồn tại một khoảng thời gian chung cho cả <code>k</code> sự kiện).</p>

<p>Cho các sự kiện <code>[startTime, endTime)</code>. Sau mỗi sự kiện, hãy trả về số nguyên <code>k</code> biểu thị số sự kiện chồng lấn tối đa trong tất cả sự kiện đã đặt đến thời điểm đó.</p>

<p>Hãy triển khai class <code>MyCalendarThree</code>:</p>

<ul>
	<li><code>MyCalendarThree()</code> Khởi tạo object.</li>
	<li><code>int book(int startTime, int endTime)</code> Trả về số nguyên <code>k</code> lớn nhất sao cho lịch có <code>k</code> sự kiện chồng lấn.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;MyCalendarThree&quot;, &quot;book&quot;, &quot;book&quot;, &quot;book&quot;, &quot;book&quot;, &quot;book&quot;, &quot;book&quot;]
[[], [10, 20], [50, 60], [10, 40], [5, 15], [5, 10], [25, 55]]
<strong>Đầu ra</strong>
[null, 1, 1, 2, 3, 3, 3]

<strong>Giải thích</strong>
MyCalendarThree myCalendarThree = new MyCalendarThree();
myCalendarThree.book(10, 20); // return 1
myCalendarThree.book(50, 60); // return 1
myCalendarThree.book(10, 40); // return 2
myCalendarThree.book(5, 15); // return 3
myCalendarThree.book(5, 10); // return 3
myCalendarThree.book(25, 55); // return 3

</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= startTime &lt; endTime &lt;= 10<sup>9</sup></code></li>
	<li>Sẽ có tối đa <code>400</code> lần gọi <code>book</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Segment Tree

<!-- thinking:start -->

> **Tư duy**
>
> Sau mỗi lần đặt lịch, ta cần trả về số lượt chồng lấn tối đa hiện tại; tổng cộng có tối đa $400$ lần gọi. Có thể quét mảng chênh lệch, nhưng cách đó vẫn phải duyệt qua mọi key.
>
> Đây là bài toán range-add / range-max giống Calendar II, nhưng không giới hạn ở mức $2$. Dynamic segment tree cập nhật đoạn và query toàn bộ timeline.
>
> Mỗi node lưu giá trị lớn nhất và lazy add. Mỗi lần book mất $O(\log n)$.

<!-- thinking:end -->

Segment tree chia toàn bộ đoạn thành nhiều đoạn con không giao nhau, với số đoạn con không vượt quá $\log(\text{width})$. Để cập nhật giá trị một phần tử, ta chỉ cần cập nhật $\log(\text{width})$ đoạn; các đoạn này nằm trong những đoạn lớn hơn bao quanh phần tử đó. Khi cập nhật đoạn, ta dùng **lazy propagation** để đảm bảo hiệu quả.

- Mỗi node của segment tree biểu diễn một đoạn.
- Segment tree có một node root duy nhất biểu diễn toàn bộ miền, chẳng hạn $[1, N]$.
- Mỗi node lá của segment tree biểu diễn đoạn đơn vị có độ dài $1$, $[x, x]$.
- Với mỗi node trong $[l, r]$, node con trái biểu diễn $[l, \text{mid}]$, node con phải biểu diễn $[\text{mid} + 1, r]$, trong đó $\text{mid} = \lfloor(l + r) / 2\rfloor$ (tức phép chia lấy sàn).

Trong bài này, các node của segment tree lưu những thông tin sau:

1. Số lần đặt lịch lớn nhất trong đoạn, $v$.
2. Giá trị đánh dấu lazy propagation, $\text{add}$.

Vì miền thời gian lên đến $10^9$, ta tạo node động khi cần.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, với $n$ là số lần đặt lịch.

<!-- tabs:start -->

#### Python3

```python
class Node:
    def __init__(self, l, r):
        self.left = None
        self.right = None
        self.l = l
        self.r = r
        self.mid = (l + r) >> 1
        self.v = 0
        self.add = 0


class SegmentTree:
    def __init__(self):
        self.root = Node(1, int(1e9 + 1))

    def modify(self, l: int, r: int, v: int, node: Node = None):
        if l > r:
            return
        if node is None:
            node = self.root
        if node.l >= l and node.r <= r:
            node.v += v
            node.add += v
            return
        self.pushdown(node)
        if l <= node.mid:
            self.modify(l, r, v, node.left)
        if r > node.mid:
            self.modify(l, r, v, node.right)
        self.pushup(node)

    def query(self, l: int, r: int, node: Node = None) -> int:
        if l > r:
            return 0
        if node is None:
            node = self.root
        if node.l >= l and node.r <= r:
            return node.v
        self.pushdown(node)
        v = 0
        if l <= node.mid:
            v = max(v, self.query(l, r, node.left))
        if r > node.mid:
            v = max(v, self.query(l, r, node.right))
        return v

    def pushup(self, node: Node):
        node.v = max(node.left.v, node.right.v)

    def pushdown(self, node: Node):
        if node.left is None:
            node.left = Node(node.l, node.mid)
        if node.right is None:
            node.right = Node(node.mid + 1, node.r)
        if node.add:
            node.left.v += node.add
            node.right.v += node.add
            node.left.add += node.add
            node.right.add += node.add
            node.add = 0


class MyCalendarThree:
    def __init__(self):
        self.tree = SegmentTree()

    def book(self, start: int, end: int) -> int:
        self.tree.modify(start + 1, end, 1)
        return self.tree.query(1, int(1e9 + 1))


# Your MyCalendarThree object will be instantiated and called as such:
# obj = MyCalendarThree()
# param_1 = obj.book(start,end)
```

#### Java

```java
class Node {
    Node left;
    Node right;
    int l;
    int r;
    int mid;
    int v;
    int add;
    public Node(int l, int r) {
        this.l = l;
        this.r = r;
        this.mid = (l + r) >> 1;
    }
}

class SegmentTree {
    private Node root = new Node(1, (int) 1e9 + 1);

    public SegmentTree() {
    }

    public void modify(int l, int r, int v) {
        modify(l, r, v, root);
    }

    public void modify(int l, int r, int v, Node node) {
        if (l > r) {
            return;
        }
        if (node.l >= l && node.r <= r) {
            node.v += v;
            node.add += v;
            return;
        }
        pushdown(node);
        if (l <= node.mid) {
            modify(l, r, v, node.left);
        }
        if (r > node.mid) {
            modify(l, r, v, node.right);
        }
        pushup(node);
    }

    public int query(int l, int r) {
        return query(l, r, root);
    }

    public int query(int l, int r, Node node) {
        if (l > r) {
            return 0;
        }
        if (node.l >= l && node.r <= r) {
            return node.v;
        }
        pushdown(node);
        int v = 0;
        if (l <= node.mid) {
            v = Math.max(v, query(l, r, node.left));
        }
        if (r > node.mid) {
            v = Math.max(v, query(l, r, node.right));
        }
        return v;
    }

    public void pushup(Node node) {
        node.v = Math.max(node.left.v, node.right.v);
    }

    public void pushdown(Node node) {
        if (node.left == null) {
            node.left = new Node(node.l, node.mid);
        }
        if (node.right == null) {
            node.right = new Node(node.mid + 1, node.r);
        }
        if (node.add != 0) {
            Node left = node.left, right = node.right;
            left.add += node.add;
            right.add += node.add;
            left.v += node.add;
            right.v += node.add;
            node.add = 0;
        }
    }
}

class MyCalendarThree {
    private SegmentTree tree = new SegmentTree();

    public MyCalendarThree() {
    }

    public int book(int start, int end) {
        tree.modify(start + 1, end, 1);
        return tree.query(1, (int) 1e9 + 1);
    }
}

/**
 * Your MyCalendarThree object will be instantiated and called as such:
 * MyCalendarThree obj = new MyCalendarThree();
 * int param_1 = obj.book(start,end);
 */
```

#### C++

```cpp
class Node {
public:
    Node* left;
    Node* right;
    int l;
    int r;
    int mid;
    int v;
    int add;

    Node(int l, int r) {
        this->l = l;
        this->r = r;
        this->mid = (l + r) >> 1;
        this->left = this->right = nullptr;
        v = add = 0;
    }
};

class SegmentTree {
private:
    Node* root;

public:
    SegmentTree() {
        root = new Node(1, 1e9 + 1);
    }

    void modify(int l, int r, int v) {
        modify(l, r, v, root);
    }

    void modify(int l, int r, int v, Node* node) {
        if (l > r) {
            return;
        }
        if (node->l >= l && node->r <= r) {
            node->v += v;
            node->add += v;
            return;
        }
        pushdown(node);
        if (l <= node->mid) {
            modify(l, r, v, node->left);
        }
        if (r > node->mid) {
            modify(l, r, v, node->right);
        }
        pushup(node);
    }

    int query(int l, int r) {
        return query(l, r, root);
    }

    int query(int l, int r, Node* node) {
        if (l > r) {
            return 0;
        }
        if (node->l >= l && node->r <= r) return node->v;
        pushdown(node);
        int v = 0;
        if (l <= node->mid) {
            v = max(v, query(l, r, node->left));
        }
        if (r > node->mid) {
            v = max(v, query(l, r, node->right));
        }
        return v;
    }

    void pushup(Node* node) {
        node->v = max(node->left->v, node->right->v);
    }

    void pushdown(Node* node) {
        if (!node->left) {
            node->left = new Node(node->l, node->mid);
        }
        if (!node->right) {
            node->right = new Node(node->mid + 1, node->r);
        }
        if (node->add) {
            Node* left = node->left;
            Node* right = node->right;
            left->v += node->add;
            right->v += node->add;
            left->add += node->add;
            right->add += node->add;
            node->add = 0;
        }
    }
};

class MyCalendarThree {
public:
    SegmentTree* tree;

    MyCalendarThree() {
        tree = new SegmentTree();
    }

    int book(int start, int end) {
        tree->modify(start + 1, end, 1);
        return tree->query(1, 1e9 + 1);
    }
};

/**
 * Your MyCalendarThree object will be instantiated and called as such:
 * MyCalendarThree* obj = new MyCalendarThree();
 * int param_1 = obj->book(start,end);
 */
```

#### Go

```go
type node struct {
	left      *node
	right     *node
	l, mid, r int
	v, add    int
}

func newNode(l, r int) *node {
	return &node{
		l:   l,
		r:   r,
		mid: int(uint(l+r) >> 1),
	}
}

type segmentTree struct {
	root *node
}

func newSegmentTree() *segmentTree {
	return &segmentTree{
		root: newNode(1, 1e9+1),
	}
}

func (t *segmentTree) modify(l, r, v int, n *node) {
	if l > r {
		return
	}
	if n.l >= l && n.r <= r {
		n.v += v
		n.add += v
		return
	}
	t.pushdown(n)
	if l <= n.mid {
		t.modify(l, r, v, n.left)
	}
	if r > n.mid {
		t.modify(l, r, v, n.right)
	}
	t.pushup(n)
}

func (t *segmentTree) query(l, r int, n *node) int {
	if l > r {
		return 0
	}
	if n.l >= l && n.r <= r {
		return n.v
	}
	t.pushdown(n)
	v := 0
	if l <= n.mid {
		v = max(v, t.query(l, r, n.left))
	}
	if r > n.mid {
		v = max(v, t.query(l, r, n.right))
	}
	return v
}

func (t *segmentTree) pushup(n *node) {
	n.v = max(n.left.v, n.right.v)
}

func (t *segmentTree) pushdown(n *node) {
	if n.left == nil {
		n.left = newNode(n.l, n.mid)
	}
	if n.right == nil {
		n.right = newNode(n.mid+1, n.r)
	}
	if n.add != 0 {
		n.left.add += n.add
		n.right.add += n.add
		n.left.v += n.add
		n.right.v += n.add
		n.add = 0
	}
}

type MyCalendarThree struct {
	tree *segmentTree
}

func Constructor() MyCalendarThree {
	return MyCalendarThree{newSegmentTree()}
}

func (this *MyCalendarThree) Book(start int, end int) int {
	this.tree.modify(start+1, end, 1, this.tree.root)
	return this.tree.query(1, int(1e9)+1, this.tree.root)
}

/**
 * Your MyCalendarThree object will be instantiated and called as such:
 * obj := Constructor();
 * param_1 := obj.Book(start,end);
 */
```

#### TypeScript

```ts
class Node {
    left: Node | null = null;
    right: Node | null = null;
    l: number;
    r: number;
    mid: number;
    v: number = 0;
    add: number = 0;

    constructor(l: number, r: number) {
        this.l = l;
        this.r = r;
        this.mid = (l + r) >> 1;
    }
}

class SegmentTree {
    private root: Node = new Node(1, 1e9 + 1);

    constructor() {}

    modify(l: number, r: number, v: number, node: Node = this.root): void {
        if (l > r) {
            return;
        }
        if (node.l >= l && node.r <= r) {
            node.v += v;
            node.add += v;
            return;
        }
        this.pushdown(node);
        if (l <= node.mid) {
            this.modify(l, r, v, node.left!);
        }
        if (r > node.mid) {
            this.modify(l, r, v, node.right!);
        }
        this.pushup(node);
    }

    query(l: number, r: number, node: Node = this.root): number {
        if (l > r) {
            return 0;
        }
        if (node.l >= l && node.r <= r) {
            return node.v;
        }
        this.pushdown(node);
        let v = 0;
        if (l <= node.mid) {
            v = Math.max(v, this.query(l, r, node.left!));
        }
        if (r > node.mid) {
            v = Math.max(v, this.query(l, r, node.right!));
        }
        return v;
    }

    private pushup(node: Node): void {
        node.v = Math.max(node.left!.v, node.right!.v);
    }

    private pushdown(node: Node): void {
        if (node.left === null) {
            node.left = new Node(node.l, node.mid);
        }
        if (node.right === null) {
            node.right = new Node(node.mid + 1, node.r);
        }
        if (node.add !== 0) {
            const left = node.left!;
            const right = node.right!;
            left.add += node.add;
            right.add += node.add;
            left.v += node.add;
            right.v += node.add;
            node.add = 0;
        }
    }
}

class MyCalendarThree {
    private tree: SegmentTree;

    constructor() {
        this.tree = new SegmentTree();
    }

    book(start: number, end: number): number {
        this.tree.modify(start + 1, end, 1);
        return this.tree.query(1, 1e9 + 1);
    }
}

/**
 * Your MyCalendarThree object will be instantiated and called as such:
 * var obj = new MyCalendarThree()
 * var param_1 = obj.book(startTime, endTime)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
