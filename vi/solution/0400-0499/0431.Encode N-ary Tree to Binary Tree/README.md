---
comments: true
difficulty: Hard
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Design
    - Binary Tree
---

<!-- problem:start -->

# [431. Encode N-ary Tree to Binary Tree 🔒](https://leetcode.com/problems/encode-n-ary-tree-to-binary-tree)

[中文文档](/solution/0400-0499/0431.Encode%20N-ary%20Tree%20to%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy thiết kế thuật toán mã hóa cây N-ary thành cây nhị phân và giải mã cây nhị phân để khôi phục cây N-ary ban đầu. Cây N-ary là cây có gốc, trong đó mỗi node có không quá N node con. Tương tự, cây nhị phân là cây có gốc, trong đó mỗi node có không quá 2 node con. Không có giới hạn về cách triển khai thuật toán mã hóa/giải mã. Bạn chỉ cần đảm bảo cây N-ary có thể được mã hóa thành cây nhị phân và cây nhị phân đó có thể được giải mã để khôi phục cấu trúc cây N-ary ban đầu.</p>

<p><em>Dữ liệu đầu vào của cây N-ary được biểu diễn theo thứ tự duyệt từng level; mỗi nhóm node con được phân tách bằng giá trị null (xem ví dụ bên dưới).</em></p>

<p>Ví dụ, bạn có thể mã hóa cây <code>3-ary</code> sau thành cây nhị phân theo cách này:</p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0431.Encode%20N-ary%20Tree%20to%20Binary%20Tree/images/narytreebinarytreeexample.png" style="width: 100%; max-width: 640px" /></p>

<pre>
<strong>Đầu vào:</strong> root = [1,null,3,2,4,null,5,6]
</pre>

<p>Lưu ý, cách trên chỉ là một ví dụ và <em>có thể đúng hoặc không</em>. Bạn không nhất thiết phải làm theo định dạng này; hãy sáng tạo và tự tìm cách tiếp cận khác.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> root = [1,null,3,2,4,null,5,6]
<strong>Đầu ra:</strong> [1,null,3,2,4,null,5,6]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
<strong>Đầu ra:</strong> [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
</pre><p><strong class="example">Ví dụ 3:</strong></p>
<pre><strong>Đầu vào:</strong> root = []
<strong>Đầu ra:</strong> []
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[0, 10<sup>4</sup>]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 10<sup>4</sup></code></li>
	<li>Chiều cao cây N-ary không vượt quá <code>1000</code>.</li>
	<li>Không dùng biến thành viên của class, biến global hoặc static để lưu trạng thái. Thuật toán mã hóa và giải mã phải stateless.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi node nhị phân chỉ có hai pointer, trong khi một node N-ary có thể có nhiều node con. Các node con có thứ tự, vì vậy có thể dùng hai pointer riêng cho node con đầu tiên và node anh em kế tiếp.
>
> Khi mã hóa: left trỏ đến node con đầu tiên; các pointer right nối tiếp những node anh em còn lại. Khi giải mã: bắt đầu từ node con bên trái, lần theo các pointer right và đệ quy khôi phục từng node anh em.
>
> Phép ánh xạ này có thể đảo ngược: mỗi danh sách node con được biểu diễn thành một nhánh right duy nhất bắt đầu từ pointer left.

<!-- thinking:end -->

Ta có thể cho pointer left của cây nhị phân trỏ đến node con đầu tiên của cây N-ary, còn pointer right trỏ đến node anh em kế tiếp của node trong cây N-ary.

Độ phức tạp thời gian và không gian đều là $O(n)$, trong đó $n$ là số node trong cây N-ary.

<!-- tabs:start -->

#### Python3

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val: Optional[int] = None, children: Optional[List['Node']] = None):
        self.val = val
        self.children = children
"""

"""
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None
"""


class Codec:
    # Encodes an n-ary tree to a binary tree.
    def encode(self, root: "Optional[Node]") -> Optional[TreeNode]:
        if root is None:
            return None
        node = TreeNode(root.val)
        if not root.children:
            return node
        left = self.encode(root.children[0])
        node.left = left
        for child in root.children[1:]:
            left.right = self.encode(child)
            left = left.right
        return node

    # Decodes your binary tree to an n-ary tree.
    def decode(self, data: Optional[TreeNode]) -> "Optional[Node]":
        if data is None:
            return None
        node = Node(data.val, [])
        if data.left is None:
            return node
        left = data.left
        while left:
            node.children.append(self.decode(left))
            left = left.right
        return node


# Your Codec object will be instantiated and called as such:
# codec = Codec()
# codec.decode(codec.encode(root))
```

#### Java

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/

/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */

class Codec {
    // Encodes an n-ary tree to a binary tree.
    public TreeNode encode(Node root) {
        if (root == null) {
            return null;
        }
        TreeNode node = new TreeNode(root.val);
        if (root.children == null || root.children.isEmpty()) {
            return node;
        }
        TreeNode left = encode(root.children.get(0));
        node.left = left;
        for (int i = 1; i < root.children.size(); i++) {
            left.right = encode(root.children.get(i));
            left = left.right;
        }
        return node;
    }

    // Decodes your binary tree to an n-ary tree.
    public Node decode(TreeNode data) {
        if (data == null) {
            return null;
        }
        Node node = new Node(data.val, new ArrayList<>());
        if (data.left == null) {
            return node;
        }
        TreeNode left = data.left;
        while (left != null) {
            node.children.add(decode(left));
            left = left.right;
        }
        return node;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.decode(codec.encode(root));
```

#### C++

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */

class Codec {
public:
    // Encodes an n-ary tree to a binary tree.
    TreeNode* encode(Node* root) {
        if (root == nullptr) {
            return nullptr;
        }
        TreeNode* node = new TreeNode(root->val);
        if (root->children.empty()) {
            return node;
        }
        TreeNode* left = encode(root->children[0]);
        node->left = left;
        for (int i = 1; i < root->children.size(); i++) {
            left->right = encode(root->children[i]);
            left = left->right;
        }
        return node;
    }

    // Decodes your binary tree to an n-ary tree.
    Node* decode(TreeNode* data) {
        if (data == nullptr) {
            return nullptr;
        }
        Node* node = new Node(data->val, vector<Node*>());
        if (data->left == nullptr) {
            return node;
        }
        TreeNode* left = data->left;
        while (left != nullptr) {
            node->children.push_back(decode(left));
            left = left->right;
        }
        return node;
    }
};

// Your Codec object will be instantiated and called as such:
// Codec codec;
// codec.decode(codec.encode(root));
```

#### Go

```go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Children []*Node
 * }
 */

/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */

type Codec struct {
}

func Constructor() *Codec {
	return &Codec{}
}

// Encodes an n-ary tree to a binary tree.
func (this *Codec) encode(root *Node) *TreeNode {
	if root == nil {
		return nil
	}
	node := &TreeNode{Val: root.Val}
	if len(root.Children) == 0 {
		return node
	}
	left := this.encode(root.Children[0])
	node.Left = left
	for i := 1; i < len(root.Children); i++ {
		left.Right = this.encode(root.Children[i])
		left = left.Right
	}
	return node
}

// Decodes your binary tree to an n-ary tree.
func (this *Codec) decode(data *TreeNode) *Node {
	if data == nil {
		return nil
	}
	node := &Node{Val: data.Val, Children: []*Node{}}
	if data.Left == nil {
		return node
	}
	left := data.Left
	for left != nil {
		node.Children = append(node.Children, this.decode(left))
		left = left.Right
	}
	return node
}

/**
 * Your Codec object will be instantiated and called as such:
 * obj := Constructor();
 * bst := obj.encode(root);
 * ans := obj.decode(bst);
 */
```

#### TypeScript

```ts
/**
 * Definition for _Node.
 * class _Node {
 *     val: number
 *     children: _Node[]
 *
 *     constructor(v: number) {
 *         this.val = v;
 *         this.children = [];
 *     }
 * }
 */

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

class Codec {
    constructor() {}

    // Encodes an n-ary tree to a binary tree.
    serialize(root: _Node | null): TreeNode | null {
        if (root === null) {
            return null;
        }
        const node = new TreeNode(root.val);
        if (root.children.length === 0) {
            return node;
        }
        let left: TreeNode | null = this.serialize(root.children[0]);
        node.left = left;
        for (let i = 1; i < root.children.length; i++) {
            if (left) {
                left.right = this.serialize(root.children[i]);
                left = left.right;
            }
        }
        return node;
    }

    // Decodes your binary tree back to an n-ary tree.
    deserialize(root: TreeNode | null): _Node | null {
        if (root === null) {
            return null;
        }
        const node = new _Node(root.val);
        if (root.left === null) {
            return node;
        }
        let left: TreeNode | null = root.left;
        while (left !== null) {
            node.children.push(this.deserialize(left));
            left = left.right;
        }
        return node;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.deserialize(codec.serialize(root));
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
