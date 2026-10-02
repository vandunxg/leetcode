---
comments: true
difficulty: Medium
tags:
    - Tree
    - Breadth-First Search
    - Design
    - Binary Tree
---

<!-- problem:start -->

# [919. Complete Binary Tree Inserter](https://leetcode.com/problems/complete-binary-tree-inserter)

[中文文档](/solution/0900-0999/0919.Complete%20Binary%20Tree%20Inserter/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Binary tree hoàn chỉnh</strong> là binary tree mà mọi tầng, ngoại trừ tầng cuối có thể chưa đầy, đều được lấp đầy; các node ở tầng cuối được xếp càng sát bên trái càng tốt.</p>

<p>Thiết kế thuật toán chèn node mới vào binary tree hoàn chỉnh sao cho cây vẫn hoàn chỉnh sau khi chèn.</p>

<p>Cài đặt class <code>CBTInserter</code>:</p>

<ul>
	<li><code>CBTInserter(TreeNode root)</code> Khởi tạo cấu trúc dữ liệu với <code>root</code> của binary tree hoàn chỉnh.</li>
	<li><code>int insert(int v)</code> Chèn một <code>TreeNode</code> có giá trị <code>Node.val == val</code> vào cây sao cho cây vẫn hoàn chỉnh, rồi trả về giá trị của node cha của <code>TreeNode</code> vừa chèn.</li>
	<li><code>TreeNode get_root()</code> Trả về root node của cây.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0919.Complete%20Binary%20Tree%20Inserter/images/lc-treeinsert.jpg" style="width: 500px; height: 143px;" />
<pre>
<strong>Đầu vào</strong>
[&quot;CBTInserter&quot;, &quot;insert&quot;, &quot;insert&quot;, &quot;get_root&quot;]
[[[1, 2]], [3], [4], []]
<strong>Đầu ra</strong>
[null, 1, 2, [1, 2, 3, 4]]

<strong>Giải thích</strong>
CBTInserter cBTInserter = new CBTInserter([1, 2]);
cBTInserter.insert(3); // return 1
cBTInserter.insert(4); // return 2
cBTInserter.get_root(); // return [1, 2, 3, 4]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 1000]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 5000</code></li>
	<li><code>root</code> là một binary tree hoàn chỉnh.</li>
	<li><code>0 &lt;= val &lt;= 5000</code></li>
	<li>Số lần gọi <code>insert</code> và <code>get_root</code> không quá <code>10<sup>4</sup></code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Khi chèn vào binary tree hoàn chỉnh, ta luôn lấp chỗ trống ngoài cùng bên trái. Mỗi lần tìm từ root sẽ phải duyệt lại các node đã đầy. Sau khi dùng BFS lưu node vào mảng, ta có thể áp dụng cách đánh chỉ số của heap: node cha của node mới nằm tại $(sz-1)//2$. Khi đó, `insert` và `get_root` đều chạy trong thời gian hằng số.

<!-- thinking:end -->

Ta có thể dùng mảng $tree$ để lưu tất cả node của binary tree hoàn chỉnh. Khi khởi tạo, dùng queue $q$ duyệt cây theo từng tầng và lưu các node vào $tree$.

Khi chèn node, ta tìm node cha $p$ của node mới thông qua mảng $tree$. Sau đó tạo node mới $node$, thêm vào $tree$ và gán làm node con trái hoặc phải của $p$. Cuối cùng, trả về giá trị của $p$.

Khi lấy root node, ta trả về trực tiếp phần tử đầu tiên của mảng $tree$.

Độ phức tạp thời gian khi khởi tạo là $O(n)$; thao tác chèn node và lấy root node đều có độ phức tạp $O(1)$. Độ phức tạp không gian là $O(n)$, với $n$ là số node trong cây.

<!-- tabs:start -->

#### Python3

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class CBTInserter:

    def __init__(self, root: Optional[TreeNode]):
        self.tree = []
        q = deque([root])
        while q:
            for _ in range(len(q)):
                node = q.popleft()
                self.tree.append(node)
                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)

    def insert(self, val: int) -> int:
        p = self.tree[(len(self.tree) - 1) // 2]
        node = TreeNode(val)
        self.tree.append(node)
        if p.left is None:
            p.left = node
        else:
            p.right = node
        return p.val

    def get_root(self) -> Optional[TreeNode]:
        return self.tree[0]


# Your CBTInserter object will be instantiated and called as such:
# obj = CBTInserter(root)
# param_1 = obj.insert(val)
# param_2 = obj.get_root()
```

#### Java

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class CBTInserter {
    private List<TreeNode> tree = new ArrayList<>();

    public CBTInserter(TreeNode root) {
        Deque<TreeNode> q = new ArrayDeque<>();
        q.offer(root);
        while (!q.isEmpty()) {
            for (int i = q.size(); i > 0; --i) {
                TreeNode node = q.poll();
                tree.add(node);
                if (node.left != null) {
                    q.offer(node.left);
                }
                if (node.right != null) {
                    q.offer(node.right);
                }
            }
        }
    }

    public int insert(int val) {
        TreeNode p = tree.get((tree.size() - 1) / 2);
        TreeNode node = new TreeNode(val);
        tree.add(node);
        if (p.left == null) {
            p.left = node;
        } else {
            p.right = node;
        }
        return p.val;
    }

    public TreeNode get_root() {
        return tree.get(0);
    }
}

/**
 * Your CBTInserter object will be instantiated and called as such:
 * CBTInserter obj = new CBTInserter(root);
 * int param_1 = obj.insert(val);
 * TreeNode param_2 = obj.get_root();
 */
```

#### C++

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class CBTInserter {
public:
    CBTInserter(TreeNode* root) {
        queue<TreeNode*> q{{root}};
        while (q.size()) {
            for (int i = q.size(); i; --i) {
                auto node = q.front();
                q.pop();
                tree.push_back(node);
                if (node->left) {
                    q.push(node->left);
                }
                if (node->right) {
                    q.push(node->right);
                }
            }
        }
    }

    int insert(int val) {
        auto p = tree[(tree.size() - 1) / 2];
        auto node = new TreeNode(val);
        tree.push_back(node);
        if (!p->left) {
            p->left = node;
        } else {
            p->right = node;
        }
        return p->val;
    }

    TreeNode* get_root() {
        return tree[0];
    }

private:
    vector<TreeNode*> tree;
};

/**
 * Your CBTInserter object will be instantiated and called as such:
 * CBTInserter* obj = new CBTInserter(root);
 * int param_1 = obj->insert(val);
 * TreeNode* param_2 = obj->get_root();
 */
```

#### Go

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
type CBTInserter struct {
	tree []*TreeNode
}

func Constructor(root *TreeNode) CBTInserter {
	q := []*TreeNode{root}
	tree := []*TreeNode{}
	for len(q) > 0 {
		for i := len(q); i > 0; i-- {
			node := q[0]
			q = q[1:]
			tree = append(tree, node)
			if node.Left != nil {
				q = append(q, node.Left)
			}
			if node.Right != nil {
				q = append(q, node.Right)
			}
		}
	}
	return CBTInserter{tree}
}

func (this *CBTInserter) Insert(val int) int {
	p := this.tree[(len(this.tree)-1)/2]
	node := &TreeNode{val, nil, nil}
	this.tree = append(this.tree, node)
	if p.Left == nil {
		p.Left = node
	} else {
		p.Right = node
	}
	return p.Val
}

func (this *CBTInserter) Get_root() *TreeNode {
	return this.tree[0]
}

/**
 * Your CBTInserter object will be instantiated and called as such:
 * obj := Constructor(root);
 * param_1 := obj.Insert(val);
 * param_2 := obj.Get_root();
 */
```

#### TypeScript

```ts
/**
 * Definition for a binary tree node.
 * class TreeNode {
 *     val: number
 *     left: TreeNode | null
 *     right: TreeNode | null
 *     constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.left = (left===undefined ? null : left)
 *         this.right = (right===undefined ? null : right)
 *     }
 * }
 */

class CBTInserter {
    private tree: TreeNode[] = [];

    constructor(root: TreeNode | null) {
        if (root === null) {
            return;
        }
        const q: TreeNode[] = [root];
        while (q.length) {
            const t: TreeNode[] = [];
            for (const node of q) {
                this.tree.push(node);
                node.left !== null && t.push(node.left);
                node.right !== null && t.push(node.right);
            }
            q.splice(0, q.length, ...t);
        }
    }

    insert(val: number): number {
        const p = this.tree[(this.tree.length - 1) >> 1];
        const node = new TreeNode(val);
        this.tree.push(node);
        if (p.left === null) {
            p.left = node;
        } else {
            p.right = node;
        }
        return p.val;
    }

    get_root(): TreeNode | null {
        return this.tree[0];
    }
}

/**
 * Your CBTInserter object will be instantiated and called as such:
 * var obj = new CBTInserter(root)
 * var param_1 = obj.insert(val)
 * var param_2 = obj.get_root()
 */
```

#### JavaScript

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 */
var CBTInserter = function (root) {
    this.tree = [];
    if (root === null) {
        return;
    }
    const q = [root];
    while (q.length) {
        const t = [];
        for (const node of q) {
            this.tree.push(node);
            node.left !== null && t.push(node.left);
            node.right !== null && t.push(node.right);
        }
        q.splice(0, q.length, ...t);
    }
};

/**
 * @param {number} val
 * @return {number}
 */
CBTInserter.prototype.insert = function (val) {
    const p = this.tree[(this.tree.length - 1) >> 1];
    const node = new TreeNode(val);
    this.tree.push(node);
    if (p.left === null) {
        p.left = node;
    } else {
        p.right = node;
    }
    return p.val;
};

/**
 * @return {TreeNode}
 */
CBTInserter.prototype.get_root = function () {
    return this.tree[0];
};

/**
 * Your CBTInserter object will be instantiated and called as such:
 * var obj = new CBTInserter(root)
 * var param_1 = obj.insert(val)
 * var param_2 = obj.get_root()
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
