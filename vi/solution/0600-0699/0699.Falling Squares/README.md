---
comments: true
difficulty: Hard
tags:
    - Segment Tree
    - Array
    - Ordered Set
---

<!-- problem:start -->

# [699. Falling Squares](https://leetcode.com/problems/falling-squares)

[中文文档](/solution/0600-0699/0699.Falling%20Squares/README.md)

## Mô tả

<!-- description:start -->

<p>Có một số hình vuông lần lượt rơi xuống trục X trên mặt phẳng 2D.</p>

<p>Cho mảng số nguyên 2D <code>positions</code>, trong đó <code>positions[i] = [left<sub>i</sub>, sideLength<sub>i</sub>]</code> biểu diễn hình vuông thứ <code>i<sup>th</sup></code> có cạnh dài <code>sideLength<sub>i</sub></code>, được thả xuống sao cho cạnh trái nằm tại tọa độ X <code>left<sub>i</sub></code>.</p>

<p>Mỗi hình vuông được thả lần lượt từ độ cao phía trên các hình vuông đã rơi. Nó rơi xuống (theo chiều âm của trục Y) cho đến khi <strong>chạm mặt trên của một hình vuông khác</strong> hoặc <strong>chạm trục X</strong>. Hình vuông chỉ chạm cạnh trái/phải của hình vuông khác thì không được tính là rơi lên hình đó. Sau khi tiếp đất, nó cố định tại chỗ và không thể di chuyển.</p>

<p>Sau mỗi lần thả hình vuông, hãy ghi lại <strong>chiều cao của chồng hình vuông cao nhất hiện tại</strong>.</p>

<p>Trả về <em>mảng số nguyên </em><code>ans</code>, trong đó <code>ans[i]</code> là chiều cao nói trên sau khi thả hình vuông thứ <code>i<sup>th</sup></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0699.Falling%20Squares/images/fallingsq1-plane.jpg" style="width: 500px; height: 505px;" />
<pre>
<strong>Đầu vào:</strong> positions = [[1,2],[2,3],[6,1]]
<strong>Đầu ra:</strong> [2,5,5]
<strong>Giải thích:</strong>
Sau lần thả đầu tiên, chồng cao nhất chỉ gồm hình vuông 1, cao 2.
Sau lần thả thứ hai, chồng cao nhất gồm hình vuông 1 và 2, cao 5.
Sau lần thả thứ ba, chồng cao nhất vẫn gồm hình vuông 1 và 2, cao 5.
Vì vậy, ta trả về [2, 5, 5].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> positions = [[100,100],[200,100]]
<strong>Đầu ra:</strong> [100,100]
<strong>Giải thích:</strong>
Sau lần thả đầu tiên, chồng cao nhất chỉ gồm hình vuông 1, cao 100.
Sau lần thả thứ hai, chồng cao nhất có thể là hình vuông 1 hoặc hình vuông 2; cả hai đều cao 100.
Vì vậy, ta trả về [100, 100].
Lưu ý, hình vuông 2 chỉ chạm cạnh phải của hình vuông 1 nên không được tính là rơi lên hình vuông đó.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= positions.length &lt;= 1000</code></li>
	<li><code>1 &lt;= left<sub>i</sub> &lt;= 10<sup>8</sup></code></li>
	<li><code>1 &lt;= sideLength<sub>i</sub> &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Segment Tree

<!-- thinking:start -->

> **Tư duy**
>
> Các hình vuông rơi lên đường biên độ cao hiện tại; sau mỗi lần thả, ta cần tìm chiều cao lớn nhất toàn cục. Tọa độ có thể lên đến $10^9$ nên không thể dùng mảng thông thường.
>
> Dynamic segment tree lưu giá trị lớn nhất trên từng đoạn: query $[l,r]$ để tìm độ cao nơi hình vuông tiếp đất, cộng thêm độ dài cạnh, rồi cập nhật độ cao đó bằng lazy tag. Đồng thời theo dõi giá trị lớn nhất hiện tại.

<!-- thinking:end -->

Theo đề bài, ta cần duy trì một tập các đoạn hỗ trợ thao tác cập nhật và query. Có thể dùng segment tree để giải bài toán này.

Segment tree chia toàn bộ đoạn thành nhiều đoạn con không liên tiếp, với số đoạn con không vượt quá $\log(width)$, trong đó $width$ là độ dài của đoạn. Để cập nhật giá trị một phần tử, ta chỉ cần cập nhật $\log(width)$ đoạn; các đoạn này đều nằm trong một đoạn lớn hơn bao chứa phần tử đó. Khi cập nhật đoạn, cần dùng **lazy propagation** để đảm bảo hiệu quả.

- Mỗi node của segment tree biểu diễn một đoạn;
- Segment tree có một root duy nhất biểu diễn toàn bộ phạm vi cần thống kê, chẳng hạn $[1, n]$;
- Mỗi leaf node của segment tree biểu diễn một đoạn cơ sở có độ dài 1, $[x, x]$;
- Với mỗi node nội bộ $[l, r]$, node con trái là $[l, \textit{mid}]$, node con phải là $[\textit{mid} + 1, r]$, trong đó $\textit{mid} = \frac{l + r}{2}$;

Trong bài này, mỗi node của segment tree lưu các thông tin sau:

1. Chiều cao lớn nhất $v$ của các khối trong đoạn
2. Lazy propagation marker $add$

Ngoài ra, vì phạm vi tọa độ rất lớn, lên đến $10^8$, ta tạo node động khi cần.

Độ phức tạp thời gian của mỗi thao tác query và cập nhật là $O(\log n)$, nên tổng độ phức tạp thời gian là $O(n \log n)$. Độ phức tạp không gian là $O(n)$.

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
        self.root = Node(1, int(1e9))

    def modify(self, l, r, v, node=None):
        if l > r:
            return
        if node is None:
            node = self.root
        if node.l >= l and node.r <= r:
            node.v = v
            node.add = v
            return
        self.pushdown(node)
        if l <= node.mid:
            self.modify(l, r, v, node.left)
        if r > node.mid:
            self.modify(l, r, v, node.right)
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
            v = max(v, self.query(l, r, node.left))
        if r > node.mid:
            v = max(v, self.query(l, r, node.right))
        return v

    def pushup(self, node):
        node.v = max(node.left.v, node.right.v)

    def pushdown(self, node):
        if node.left is None:
            node.left = Node(node.l, node.mid)
        if node.right is None:
            node.right = Node(node.mid + 1, node.r)
        if node.add:
            node.left.v = node.add
            node.right.v = node.add
            node.left.add = node.add
            node.right.add = node.add
            node.add = 0


class Solution:
    def fallingSquares(self, positions: List[List[int]]) -> List[int]:
        ans = []
        mx = 0
        tree = SegmentTree()
        for l, w in positions:
            r = l + w - 1
            h = tree.query(l, r) + w
            mx = max(mx, h)
            ans.append(mx)
            tree.modify(l, r, h)
        return ans
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
    private Node root = new Node(1, (int) 1e9);

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
            node.v = v;
            node.add = v;
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
            left.add = node.add;
            right.add = node.add;
            left.v = node.add;
            right.v = node.add;
            node.add = 0;
        }
    }
}

class Solution {
    public List<Integer> fallingSquares(int[][] positions) {
        List<Integer> ans = new ArrayList<>();
        SegmentTree tree = new SegmentTree();
        int mx = 0;
        for (int[] p : positions) {
            int l = p[0], w = p[1], r = l + w - 1;
            int h = tree.query(l, r) + w;
            mx = Math.max(mx, h);
            ans.add(mx);
            tree.modify(l, r, h);
        }
        return ans;
    }
}
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
        root = new Node(1, 1e9);
    }

    void modify(int l, int r, int v) {
        modify(l, r, v, root);
    }

    void modify(int l, int r, int v, Node* node) {
        if (l > r) return;
        if (node->l >= l && node->r <= r) {
            node->v = v;
            node->add = v;
            return;
        }
        pushdown(node);
        if (l <= node->mid) modify(l, r, v, node->left);
        if (r > node->mid) modify(l, r, v, node->right);
        pushup(node);
    }

    int query(int l, int r) {
        return query(l, r, root);
    }

    int query(int l, int r, Node* node) {
        if (l > r) return 0;
        if (node->l >= l && node->r <= r) return node->v;
        pushdown(node);
        int v = 0;
        if (l <= node->mid) v = max(v, query(l, r, node->left));
        if (r > node->mid) v = max(v, query(l, r, node->right));
        return v;
    }

    void pushup(Node* node) {
        node->v = max(node->left->v, node->right->v);
    }

    void pushdown(Node* node) {
        if (!node->left) node->left = new Node(node->l, node->mid);
        if (!node->right) node->right = new Node(node->mid + 1, node->r);
        if (node->add) {
            Node* left = node->left;
            Node* right = node->right;
            left->v = node->add;
            right->v = node->add;
            left->add = node->add;
            right->add = node->add;
            node->add = 0;
        }
    }
};

class Solution {
public:
    vector<int> fallingSquares(vector<vector<int>>& positions) {
        vector<int> ans;
        SegmentTree* tree = new SegmentTree();
        int mx = 0;
        for (auto& p : positions) {
            int l = p[0], w = p[1], r = l + w - 1;
            int h = tree->query(l, r) + w;
            mx = max(mx, h);
            ans.push_back(mx);
            tree->modify(l, r, h);
        }
        return ans;
    }
};
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
		mid: (l + r) >> 1,
	}
}

type segmentTree struct {
	root *node
}

func newSegmentTree() *segmentTree {
	return &segmentTree{
		root: newNode(1, 1e9),
	}
}

func (t *segmentTree) modify(l, r, v int, n *node) {
	if l > r {
		return
	}
	if n.l >= l && n.r <= r {
		n.v = v
		n.add = v
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
		n.left.add = n.add
		n.right.add = n.add
		n.left.v = n.add
		n.right.v = n.add
		n.add = 0
	}
}

func fallingSquares(positions [][]int) []int {
	ans := make([]int, len(positions))
	t := newSegmentTree()
	mx := 0
	for i, p := range positions {
		l, w, r := p[0], p[1], p[0]+p[1]-1
		h := t.query(l, r, t.root) + w
		mx = max(mx, h)
		ans[i] = mx
		t.modify(l, r, h, t.root)
	}
	return ans
}
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
    private root: Node = new Node(1, 1e9);

    public modify(l: number, r: number, v: number): void {
        this.modifyNode(l, r, v, this.root);
    }

    private modifyNode(l: number, r: number, v: number, node: Node): void {
        if (l > r) {
            return;
        }
        if (node.l >= l && node.r <= r) {
            node.v = v;
            node.add = v;
            return;
        }
        this.pushdown(node);
        if (l <= node.mid) {
            this.modifyNode(l, r, v, node.left!);
        }
        if (r > node.mid) {
            this.modifyNode(l, r, v, node.right!);
        }
        this.pushup(node);
    }

    public query(l: number, r: number): number {
        return this.queryNode(l, r, this.root);
    }

    private queryNode(l: number, r: number, node: Node): number {
        if (l > r) {
            return 0;
        }
        if (node.l >= l && node.r <= r) {
            return node.v;
        }
        this.pushdown(node);
        let v = 0;
        if (l <= node.mid) {
            v = Math.max(v, this.queryNode(l, r, node.left!));
        }
        if (r > node.mid) {
            v = Math.max(v, this.queryNode(l, r, node.right!));
        }
        return v;
    }

    private pushup(node: Node): void {
        node.v = Math.max(node.left!.v, node.right!.v);
    }

    private pushdown(node: Node): void {
        if (node.left == null) {
            node.left = new Node(node.l, node.mid);
        }
        if (node.right == null) {
            node.right = new Node(node.mid + 1, node.r);
        }
        if (node.add != 0) {
            let left = node.left,
                right = node.right;
            left!.add = node.add;
            right!.add = node.add;
            left!.v = node.add;
            right!.v = node.add;
            node.add = 0;
        }
    }
}

function fallingSquares(positions: number[][]): number[] {
    const ans: number[] = [];
    const tree = new SegmentTree();
    let mx = 0;
    for (const [l, w] of positions) {
        const r = l + w - 1;
        const h = tree.query(l, r) + w;
        mx = Math.max(mx, h);
        ans.push(mx);
        tree.modify(l, r, h);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
