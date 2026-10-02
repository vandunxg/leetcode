---
comments: true
difficulty: Medium
tags:
    - Design
    - Segment Tree
    - Array
    - Binary Search
    - Ordered Set
    - Prefix Sum
---

<!-- problem:start -->

# [731. My Calendar II](https://leetcode.com/problems/my-calendar-ii)

[中文文档](/solution/0700-0799/0731.My%20Calendar%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đang triển khai chương trình lịch. Có thể thêm một sự kiện mới nếu việc đó không gây ra <strong>đặt lịch trùng ba lần</strong>.</p>

<p><strong>Đặt lịch trùng ba lần</strong> xảy ra khi giao của ba sự kiện không rỗng (tức là có một thời điểm chung thuộc cả ba sự kiện).</p>

<p>Một sự kiện được biểu diễn bằng cặp số nguyên <code>startTime</code> và <code>endTime</code>, tương ứng với một lịch đặt trong khoảng nửa mở <code>[startTime, endTime)</code>, gồm các số thực <code>x</code> thỏa mãn <code>startTime &lt;= x &lt; endTime</code>.</p>

<p>Hãy triển khai class <code>MyCalendarTwo</code>:</p>

<ul>
	<li><code>MyCalendarTwo()</code> Khởi tạo đối tượng lịch.</li>
	<li><code>boolean book(int startTime, int endTime)</code> Trả về <code>true</code> nếu có thể thêm sự kiện vào lịch mà không gây <strong>đặt lịch trùng ba lần</strong>. Nếu không, trả về <code>false</code> và không thêm sự kiện vào lịch.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;MyCalendarTwo&quot;, &quot;book&quot;, &quot;book&quot;, &quot;book&quot;, &quot;book&quot;, &quot;book&quot;, &quot;book&quot;]
[[], [10, 20], [50, 60], [10, 40], [5, 15], [5, 10], [25, 55]]
<strong>Đầu ra</strong>
[null, true, true, true, false, true, true]

<strong>Giải thích</strong>
MyCalendarTwo myCalendarTwo = new MyCalendarTwo();
myCalendarTwo.book(10, 20); // return True, The event can be booked. 
myCalendarTwo.book(50, 60); // return True, The event can be booked. 
myCalendarTwo.book(10, 40); // return True, The event can be double booked. 
myCalendarTwo.book(5, 15);  // return False, The event cannot be booked, because it would result in a triple booking.
myCalendarTwo.book(5, 10); // return True, The event can be booked, as it does not use time 10 which is already double booked.
myCalendarTwo.book(25, 55); // return True, The event can be booked, as the time in [25, 40) will be double booked with the third event, the time [40, 50) will be single booked, and the time [50, 55) will be double booked with the second event.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= start &lt; end &lt;= 10<sup>9</sup></code></li>
	<li><code>book</code> được gọi nhiều nhất <code>1000</code> lần.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Difference Array

<!-- thinking:start -->

> **Tư duy**
>
> Có thể đặt lịch trùng hai lần nhưng không được trùng ba lần. Endpoint có thể lên đến $10^9$ với $1000$ lời gọi. Kiểm tra mọi cặp rồi duy trì thêm danh sách các khoảng bị trùng khá rườm rà.
>
> Dùng difference array để biểu diễn số lượt đặt: cộng $+1$ tại thời điểm bắt đầu và $-1$ tại thời điểm kết thúc. Quét prefix theo thứ tự thời gian; nếu tổng vượt quá $2$ thì từ chối lịch đặt và hoàn tác hai cập nhật.
>
> Dùng sorted map thay cho timeline rất lớn. Mỗi lần book quét các key trong $O(n)$, tổng độ phức tạp là $O(n^2)$, vẫn phù hợp với $1000$ lời gọi.

<!-- thinking:end -->

Ta có thể dùng difference array để ghi lại trạng thái đặt lịch tại từng thời điểm. Sau đó, duyệt các thời điểm và tính số sự kiện đang được đặt tại thời điểm hiện tại. Nếu số lượt đặt vượt quá $2$, trả về $\textit{false}$; nếu không, trả về $\textit{true}$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$, với $n$ là số lượt đặt lịch.

<!-- tabs:start -->

#### Python3

```python
class MyCalendarTwo:

    def __init__(self):
        self.sd = SortedDict()

    def book(self, startTime: int, endTime: int) -> bool:
        self.sd[startTime] = self.sd.get(startTime, 0) + 1
        self.sd[endTime] = self.sd.get(endTime, 0) - 1
        s = 0
        for v in self.sd.values():
            s += v
            if s > 2:
                self.sd[startTime] -= 1
                self.sd[endTime] += 1
                return False
        return True


# Your MyCalendarTwo object will be instantiated and called as such:
# obj = MyCalendarTwo()
# param_1 = obj.book(startTime,endTime)
```

#### Java

```java
class MyCalendarTwo {
    private final Map<Integer, Integer> tm = new TreeMap<>();

    public MyCalendarTwo() {
    }

    public boolean book(int startTime, int endTime) {
        tm.merge(startTime, 1, Integer::sum);
        tm.merge(endTime, -1, Integer::sum);
        int s = 0;
        for (int v : tm.values()) {
            s += v;
            if (s > 2) {
                tm.merge(startTime, -1, Integer::sum);
                tm.merge(endTime, 1, Integer::sum);
                return false;
            }
        }
        return true;
    }
}

/**
 * Your MyCalendarTwo object will be instantiated and called as such:
 * MyCalendarTwo obj = new MyCalendarTwo();
 * boolean param_1 = obj.book(startTime,endTime);
 */
```

#### C++

```cpp
class MyCalendarTwo {
public:
    MyCalendarTwo() {
    }

    bool book(int startTime, int endTime) {
        ++m[startTime];
        --m[endTime];
        int s = 0;
        for (auto& [_, v] : m) {
            s += v;
            if (s > 2) {
                --m[startTime];
                ++m[endTime];
                return false;
            }
        }
        return true;
    }

private:
    map<int, int> m;
};

/**
 * Your MyCalendarTwo object will be instantiated and called as such:
 * MyCalendarTwo* obj = new MyCalendarTwo();
 * bool param_1 = obj->book(startTime,endTime);
 */
```

#### Go

```go
type MyCalendarTwo struct {
	rbt *redblacktree.Tree[int, int]
}

func Constructor() MyCalendarTwo {
	return MyCalendarTwo{rbt: redblacktree.New[int, int]()}
}

func (this *MyCalendarTwo) Book(startTime int, endTime int) bool {
	merge := func(x, v int) {
		c, _ := this.rbt.Get(x)
		if c+v == 0 {
			this.rbt.Remove(x)
		} else {
			this.rbt.Put(x, c+v)
		}
	}

	merge(startTime, 1)
	merge(endTime, -1)

	s := 0
	for _, v := range this.rbt.Values() {
		s += v
		if s > 2 {
			merge(startTime, -1)
			merge(endTime, 1)
			return false
		}
	}
	return true
}

/**
 * Your MyCalendarTwo object will be instantiated and called as such:
 * obj := Constructor();
 * param_1 := obj.Book(startTime,endTime);
 */
```

#### TypeScript

```ts
class MyCalendarTwo {
    private tm: Record<number, number> = {};

    constructor() {}

    book(startTime: number, endTime: number): boolean {
        this.tm[startTime] = (this.tm[startTime] ?? 0) + 1;
        this.tm[endTime] = (this.tm[endTime] ?? 0) - 1;
        let s = 0;
        for (const v of Object.values(this.tm)) {
            s += v;
            if (s > 2) {
                if (--this.tm[startTime] === 0) {
                    delete this.tm[startTime];
                }
                if (++this.tm[endTime] === 0) {
                    delete this.tm[endTime];
                }
                return false;
            }
        }
        return true;
    }
}

/**
 * Your MyCalendarTwo object will be instantiated and called as such:
 * var obj = new MyCalendarTwo()
 * var param_1 = obj.book(startTime,endTime)
 */
```

#### JavaScript

```js
var MyCalendarTwo = function () {
    this.tm = {};
};

/**
 * @param {number} startTime
 * @param {number} endTime
 * @return {boolean}
 */
MyCalendarTwo.prototype.book = function (startTime, endTime) {
    this.tm[startTime] = (this.tm[startTime] || 0) + 1;
    this.tm[endTime] = (this.tm[endTime] || 0) - 1;
    let s = 0;

    for (const v of Object.values(this.tm)) {
        s += v;
        if (s > 2) {
            if (--this.tm[startTime] === 0) {
                delete this.tm[startTime];
            }
            if (++this.tm[endTime] === 0) {
                delete this.tm[endTime];
            }
            return false;
        }
    }
    return true;
};

/**
 * Your MyCalendarTwo object will be instantiated and called as such:
 * var obj = new MyCalendarTwo()
 * var param_1 = obj.book(startTime,endTime)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Segment Tree

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 quét lại mọi breakpoint. Có thể dùng lazy segment tree để cộng trên một khoảng và truy vấn giá trị lớn nhất trên khoảng.
>
> Miền thời gian có kích thước $10^9$, vì vậy các node được tạo khi cần và lưu mức độ phủ lớn nhất. Truy vấn $[\textit{start},\textit{end})$; nếu giá trị lớn nhất đã là $2$ thì từ chối, còn không thì cộng thêm một. Mỗi lần book có độ phức tạp $O(\log n)$.

<!-- thinking:end -->

Segment tree chia toàn bộ khoảng thành nhiều khoảng con không giao nhau, với số khoảng con không vượt quá $\log(\textit{width})$. Để cập nhật giá trị của một phần tử, chỉ cần cập nhật $\log(\textit{width})$ khoảng; các khoảng này đều nằm trong một khoảng lớn hơn chứa phần tử đó. Khi cập nhật khoảng, dùng **lazy mark** để đảm bảo hiệu quả.

- Mỗi node của segment tree biểu diễn một khoảng;
- Segment tree có một node root duy nhất biểu diễn toàn bộ miền cần xét, chẳng hạn $[1, N]$;
- Mỗi node lá của segment tree biểu diễn một khoảng đơn vị có độ dài 1, $[x, x]$;
- Với mỗi node trong $[l, r]$, node con trái biểu diễn $[l, \textit{mid}]$ và node con phải biểu diễn $[\textit{mid} + 1, r]$, trong đó $\textit{mid} = \lfloor(l + r) / 2\rfloor$ (tức phép chia lấy phần nguyên).

Trong bài này, mỗi node của segment tree lưu các thông tin sau:

1. Số lượt đặt lịch lớn nhất trong khoảng $v$
2. Lazy mark $\textit{add}$

Vì khoảng thời gian có kích thước rất lớn, đến $10^9$, ta tạo node động khi cần.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, với $n$ là số lượt đặt lịch.

<!-- tabs:start -->

#### Python3

```python
class Node:
    def __init__(self, l: int, r: int):
        self.left = None
        self.right = None
        self.l = l
        self.r = r
        self.mid = (l + r) >> 1
        self.v = 0
        self.add = 0


class SegmentTree:
    def __init__(self):
        self.root = Node(1, 10**9 + 1)

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


class MyCalendarTwo:
    def __init__(self):
        self.tree = SegmentTree()

    def book(self, startTime: int, endTime: int) -> bool:
        if self.tree.query(startTime + 1, endTime) >= 2:
            return False
        self.tree.modify(startTime + 1, endTime, 1)
        return True


# Your MyCalendarTwo object will be instantiated and called as such:
# obj = MyCalendarTwo()
# param_1 = obj.book(startTime,endTime)
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

class MyCalendarTwo {
    private SegmentTree tree = new SegmentTree();

    public MyCalendarTwo() {
    }

    public boolean book(int startTime, int endTime) {
        if (tree.query(startTime + 1, endTime) >= 2) {
            return false;
        }
        tree.modify(startTime + 1, endTime, 1);
        return true;
    }
}

/**
 * Your MyCalendarTwo object will be instantiated and called as such:
 * MyCalendarTwo obj = new MyCalendarTwo();
 * boolean param_1 = obj.book(startTime,endTime);
 */
```

#### C++

```cpp
class Node {
public:
    int l, r, mid, v, add;
    Node* left;
    Node* right;

    Node(int l, int r)
        : l(l)
        , r(r)
        , mid((l + r) >> 1)
        , v(0)
        , add(0)
        , left(nullptr)
        , right(nullptr) {}
};

class SegmentTree {
public:
    Node* root;

    SegmentTree() {
        root = new Node(1, 1e9 + 1);
    }

    void modify(int l, int r, int v, Node* node = nullptr) {
        if (l > r) {
            return;
        }
        if (node == nullptr) {
            node = root;
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

    int query(int l, int r, Node* node = nullptr) {
        if (l > r) {
            return 0;
        }
        if (node == nullptr) {
            node = root;
        }

        if (node->l >= l && node->r <= r) {
            return node->v;
        }
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

private:
    void pushup(Node* node) {
        node->v = max(node->left->v, node->right->v);
    }

    void pushdown(Node* node) {
        if (node->left == nullptr) {
            node->left = new Node(node->l, node->mid);
        }
        if (node->right == nullptr) {
            node->right = new Node(node->mid + 1, node->r);
        }
        if (node->add) {
            node->left->v += node->add;
            node->right->v += node->add;
            node->left->add += node->add;
            node->right->add += node->add;
            node->add = 0;
        }
    }
};

class MyCalendarTwo {
public:
    SegmentTree tree;

    MyCalendarTwo() {}

    bool book(int startTime, int endTime) {
        if (tree.query(startTime + 1, endTime) >= 2) {
            return false;
        }
        tree.modify(startTime + 1, endTime, 1);
        return true;
    }
};

/**
 * Your MyCalendarTwo object will be instantiated and called as such:
 * MyCalendarTwo* obj = new MyCalendarTwo();
 * bool param_1 = obj->book(startTime,endTime);
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

type MyCalendarTwo struct {
	tree *segmentTree
}

func Constructor() MyCalendarTwo {
	return MyCalendarTwo{newSegmentTree()}
}

func (this *MyCalendarTwo) Book(startTime int, endTime int) bool {
	if this.tree.query(startTime+1, endTime, this.tree.root) >= 2 {
		return false
	}
	this.tree.modify(startTime+1, endTime, 1, this.tree.root)
	return true
}

/**
 * Your MyCalendarTwo object will be instantiated and called as such:
 * obj := Constructor();
 * param_1 := obj.Book(startTime,endTime);
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

    modify(l: number, r: number, v: number, node: Node | null = this.root): void {
        if (l > r || !node) {
            return;
        }
        if (node.l >= l && node.r <= r) {
            node.v += v;
            node.add += v;
            return;
        }
        this.pushdown(node);
        if (l <= node.mid) {
            this.modify(l, r, v, node.left);
        }
        if (r > node.mid) {
            this.modify(l, r, v, node.right);
        }
        this.pushup(node);
    }

    query(l: number, r: number, node: Node | null = this.root): number {
        if (l > r || !node) {
            return 0;
        }
        if (node.l >= l && node.r <= r) {
            return node.v;
        }
        this.pushdown(node);
        let v = 0;
        if (l <= node.mid) {
            v = Math.max(v, this.query(l, r, node.left));
        }
        if (r > node.mid) {
            v = Math.max(v, this.query(l, r, node.right));
        }
        return v;
    }

    private pushup(node: Node): void {
        if (node.left && node.right) {
            node.v = Math.max(node.left.v, node.right.v);
        }
    }

    private pushdown(node: Node): void {
        if (!node.left) {
            node.left = new Node(node.l, node.mid);
        }
        if (!node.right) {
            node.right = new Node(node.mid + 1, node.r);
        }
        if (node.add) {
            let left = node.left;
            let right = node.right;
            left.add += node.add;
            right.add += node.add;
            left.v += node.add;
            right.v += node.add;
            node.add = 0;
        }
    }
}

class MyCalendarTwo {
    private tree: SegmentTree = new SegmentTree();

    constructor() {}

    book(startTime: number, endTime: number): boolean {
        if (this.tree.query(startTime + 1, endTime) >= 2) {
            return false;
        }
        this.tree.modify(startTime + 1, endTime, 1);
        return true;
    }
}

/**
 * Your MyCalendarTwo object will be instantiated and called as such:
 * var obj = new MyCalendarTwo()
 * var param_1 = obj.book(startTime,endTime)
 */
```

#### JavaScript

```js
class Node {
    constructor(l, r) {
        this.left = null;
        this.right = null;
        this.l = l;
        this.r = r;
        this.mid = (l + r) >> 1;
        this.v = 0;
        this.add = 0;
    }
}

class SegmentTree {
    constructor() {
        this.root = new Node(1, 1e9 + 1);
    }

    modify(l, r, v, node = this.root) {
        if (l > r || !node) {
            return;
        }
        if (node.l >= l && node.r <= r) {
            node.v += v;
            node.add += v;
            return;
        }
        this.pushdown(node);
        if (l <= node.mid) {
            this.modify(l, r, v, node.left);
        }
        if (r > node.mid) {
            this.modify(l, r, v, node.right);
        }
        this.pushup(node);
    }

    query(l, r, node = this.root) {
        if (l > r || !node) {
            return 0;
        }
        if (node.l >= l && node.r <= r) {
            return node.v;
        }
        this.pushdown(node);
        let v = 0;
        if (l <= node.mid) {
            v = Math.max(v, this.query(l, r, node.left));
        }
        if (r > node.mid) {
            v = Math.max(v, this.query(l, r, node.right));
        }
        return v;
    }

    pushup(node) {
        if (node.left && node.right) {
            node.v = Math.max(node.left.v, node.right.v);
        }
    }

    pushdown(node) {
        if (!node.left) {
            node.left = new Node(node.l, node.mid);
        }
        if (!node.right) {
            node.right = new Node(node.mid + 1, node.r);
        }
        if (node.add) {
            const left = node.left;
            const right = node.right;
            left.add += node.add;
            right.add += node.add;
            left.v += node.add;
            right.v += node.add;
            node.add = 0;
        }
    }
}

var MyCalendarTwo = function () {
    this.tree = new SegmentTree();
};

/**
 * @param {number} startTime
 * @param {number} endTime
 * @return {boolean}
 */
MyCalendarTwo.prototype.book = function (startTime, endTime) {
    if (this.tree.query(startTime + 1, endTime) >= 2) {
        return false;
    }
    this.tree.modify(startTime + 1, endTime, 1);
    return true;
};

/**
 * Your MyCalendarTwo object will be instantiated and called as such:
 * var obj = new MyCalendarTwo()
 * var param_1 = obj.book(startTime,endTime)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
