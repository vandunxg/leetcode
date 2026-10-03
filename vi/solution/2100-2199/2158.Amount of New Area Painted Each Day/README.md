---
comments: true
difficulty: Hard
tags:
    - Segment Tree
    - Array
    - Ordered Set
---

<!-- problem:start -->

# [2158. Amount of New Area Painted Each Day 🔒](https://leetcode.com/problems/amount-of-new-area-painted-each-day)

[中文文档](/solution/2100-2199/2158.Amount%20of%20New%20Area%20Painted%20Each%20Day/README.md)

## Mô tả

<!-- description:start -->

<p>Có một bức tranh dài và mảnh, có thể được biểu diễn bằng một trục số. Bạn được cho một mảng số nguyên 2D <code>paint</code> có độ dài <code>n</code>, được <strong>đánh chỉ số từ 0</strong>, trong đó <code>paint[i] = [start<sub>i</sub>, end<sub>i</sub>]</code>. Điều này có nghĩa là vào ngày thứ <code>i<sup>th</sup></code>, bạn cần sơn phần diện tích <strong>nằm giữa</strong> <code>start<sub>i</sub></code> và <code>end<sub>i</sub></code>.</p>

<p>Sơn cùng một phần diện tích nhiều lần sẽ khiến bức tranh không đều, vì vậy bạn chỉ muốn sơn mỗi phần diện tích nhiều nhất <strong>một lần</strong>.</p>

<p>Hãy trả về <em>một mảng số nguyên </em><code>worklog</code><em> có độ dài </em><code>n</code><em>, trong đó </em><code>worklog[i]</code><em> là lượng diện tích <strong>mới</strong> mà bạn đã sơn vào ngày thứ </em><code>i<sup>th</sup></code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2158.Amount%20of%20New%20Area%20Painted%20Each%20Day/images/screenshot-2022-02-01-at-17-16-16-diagram-drawio-diagrams-net.png" style="height: 300px; width: 620px;" />
<pre>
<strong>Đầu vào:</strong> paint = [[1,4],[4,7],[5,8]]
<strong>Đầu ra:</strong> [3,3,1]
<strong>Giải thích:</strong>
Vào ngày 0, sơn toàn bộ phần nằm giữa 1 và 4.
Lượng diện tích mới được sơn vào ngày 0 là 4 - 1 = 3.
Vào ngày 1, sơn toàn bộ phần nằm giữa 4 và 7.
Lượng diện tích mới được sơn vào ngày 1 là 7 - 4 = 3.
Vào ngày 2, sơn phần nằm giữa 7 và 8.
Mọi phần nằm giữa 5 và 7 đã được sơn vào ngày 1.
Lượng diện tích mới được sơn vào ngày 2 là 8 - 7 = 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2158.Amount%20of%20New%20Area%20Painted%20Each%20Day/images/screenshot-2022-02-01-at-17-17-45-diagram-drawio-diagrams-net.png" style="width: 604px; height: 300px;" />
<pre>
<strong>Đầu vào:</strong> paint = [[1,4],[5,8],[4,7]]
<strong>Đầu ra:</strong> [3,3,1]
<strong>Giải thích:</strong>
Vào ngày 0, sơn toàn bộ phần nằm giữa 1 và 4.
Lượng diện tích mới được sơn vào ngày 0 là 4 - 1 = 3.
Vào ngày 1, sơn toàn bộ phần nằm giữa 5 và 8.
Lượng diện tích mới được sơn vào ngày 1 là 8 - 5 = 3.
Vào ngày 2, sơn phần nằm giữa 4 và 5.
Mọi phần nằm giữa 5 và 7 đã được sơn vào ngày 1.
Lượng diện tích mới được sơn vào ngày 2 là 5 - 4 = 1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2158.Amount%20of%20New%20Area%20Painted%20Each%20Day/images/screenshot-2022-02-01-at-17-19-49-diagram-drawio-diagrams-net.png" style="width: 423px; height: 275px;" />
<pre>
<strong>Đầu vào:</strong> paint = [[1,5],[2,4]]
<strong>Đầu ra:</strong> [4,0]
<strong>Giải thích:</strong>
Vào ngày 0, sơn toàn bộ phần nằm giữa 1 và 5.
Lượng diện tích mới được sơn vào ngày 0 là 5 - 1 = 4.
Vào ngày 1, không sơn gì vì mọi phần nằm giữa 2 và 4 đã được sơn vào ngày 0.
Lượng diện tích mới được sơn vào ngày 1 là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= paint.length &lt;= 10<sup>5</sup></code></li>
	<li><code>paint[i].length == 2</code></li>
	<li><code>0 &lt;= start<sub>i</sub> &lt; end<sub>i</sub> &lt;= 5 * 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi ngày sơn một đoạn; ta cần tìm độ dài phần mới được sơn. Các đầu mút và số ngày đều có thể đạt $10^5$, nên sơn từng ô sẽ có độ phức tạp bậc hai. Ta cần thực hiện phép phủ trên một đoạn và truy vấn độ dài đã được phủ.
>
> Segment tree lưu số ô đã được sơn trong một đoạn, cùng với lazy tag “sơn toàn bộ”. Đáp án của ngày đó là độ dài đoạn trừ đi số ô đã được phủ trong kết quả truy vấn, sau đó đánh dấu đoạn là đã được sơn.
>
> Dịch các tọa độ để bắt đầu từ $1$, rồi thực hiện $\texttt{query}$ sau đó $\texttt{modify}$ cho từng ngày.

<!-- thinking:end -->

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
        self.root = Node(1, 10**5 + 10)

    def modify(self, l, r, v, node=None):
        if l > r:
            return
        if node is None:
            node = self.root
        if node.l >= l and node.r <= r:
            node.v = node.r - node.l + 1
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
            v += self.query(l, r, node.left)
        if r > node.mid:
            v += self.query(l, r, node.right)
        return v

    def pushup(self, node):
        node.v = node.left.v + node.right.v

    def pushdown(self, node):
        if node.left is None:
            node.left = Node(node.l, node.mid)
        if node.right is None:
            node.right = Node(node.mid + 1, node.r)
        if node.add:
            left, right = node.left, node.right
            left.v = left.r - left.l + 1
            right.v = right.r - right.l + 1
            left.add = node.add
            right.add = node.add
            node.add = 0


class Solution:
    def amountPainted(self, paint: List[List[int]]) -> List[int]:
        tree = SegmentTree()
        ans = []
        for i, (start, end) in enumerate(paint):
            l, r = start + 1, end
            v = tree.query(l, r)
            ans.append(r - l + 1 - v)
            tree.modify(l, r, 1)
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
    private Node root = new Node(1, 100010);

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
            node.v = node.r - node.l + 1;
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
            v += query(l, r, node.left);
        }
        if (r > node.mid) {
            v += query(l, r, node.right);
        }
        return v;
    }

    public void pushup(Node node) {
        node.v = node.left.v + node.right.v;
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
            left.v = left.r - left.l + 1;
            right.v = right.r - right.l + 1;
            node.add = 0;
        }
    }
}

class Solution {
    public int[] amountPainted(int[][] paint) {
        SegmentTree tree = new SegmentTree();
        int n = paint.length;
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            int l = paint[i][0] + 1;
            int r = paint[i][1];
            int v = tree.query(l, r);
            ans[i] = r - l + 1 - v;
            tree.modify(l, r, 1);
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
        root = new Node(1, 100010);
    }

    void modify(int l, int r, int v) {
        modify(l, r, v, root);
    }

    void modify(int l, int r, int v, Node* node) {
        if (l > r) return;
        if (node->l >= l && node->r <= r) {
            node->v = node->r - node->l + 1;
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
        if (l <= node->mid) v += query(l, r, node->left);
        if (r > node->mid) v += query(l, r, node->right);
        return v;
    }

    void pushup(Node* node) {
        node->v = node->left->v + node->right->v;
    }

    void pushdown(Node* node) {
        if (!node->left) node->left = new Node(node->l, node->mid);
        if (!node->right) node->right = new Node(node->mid + 1, node->r);
        if (node->add) {
            Node* left = node->left;
            Node* right = node->right;
            left->v = left->r - left->l + 1;
            right->v = right->r - right->l + 1;
            left->add = node->add;
            right->add = node->add;
            node->add = 0;
        }
    }
};

class Solution {
public:
    vector<int> amountPainted(vector<vector<int>>& paint) {
        int n = paint.size();
        vector<int> ans(n);
        SegmentTree* tree = new SegmentTree();
        for (int i = 0; i < n; ++i) {
            int l = paint[i][0] + 1;
            int r = paint[i][1];
            int v = tree->query(l, r);
            ans[i] = r - l + 1 - v;
            tree->modify(l, r, 1);
        }
        return ans;
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
