---
comments: true
difficulty: Medium
tags:
    - Tree
    - Binary Search Tree
    - Binary Tree
---

<!-- problem:start -->

# [510. Inorder Successor in BST II 🔒](https://leetcode.com/problems/inorder-successor-in-bst-ii)

[中文文档](/solution/0500-0599/0510.Inorder%20Successor%20in%20BST%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một <code>node</code> trong cây tìm kiếm nhị phân, hãy trả về <em>node kế tiếp của node đó theo thứ tự inorder trong BST</em>. Nếu node đó không có node kế tiếp, trả về <code>null</code>.</p>

<p>Successor của một <code>node</code> là node có key nhỏ nhất nhưng lớn hơn <code>node.val</code>.</p>

<p>Bạn có thể truy cập trực tiếp vào node nhưng không có quyền truy cập vào root của cây. Mỗi node có tham chiếu đến node cha. Dưới đây là định nghĩa của <code>Node</code>:</p>

<pre>
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node parent;
}
</pre>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0510.Inorder%20Successor%20in%20BST%20II/images/285_example_1.png" style="width: 122px; height: 117px;" />
<pre>
<strong>Đầu vào:</strong> tree = [2,1,3], node = 1
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Node kế tiếp theo thứ tự inorder của 1 là 2. Lưu ý rằng cả node đầu vào và giá trị trả về đều có kiểu Node.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0510.Inorder%20Successor%20in%20BST%20II/images/285_example_2.png" style="width: 246px; height: 229px;" />
<pre>
<strong>Đầu vào:</strong> tree = [5,3,6,2,4,null,null,1], node = 6
<strong>Đầu ra:</strong> null
<strong>Giải thích:</strong> Node hiện tại không có node kế tiếp theo thứ tự inorder, nên đáp án là null.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 10<sup>4</sup>]</code>.</li>
	<li><code>-10<sup>5</sup> &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
	<li>Tất cả node đều có giá trị khác nhau.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể giải bài này mà không cần đọc giá trị của bất kỳ node nào không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Xét các trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Cách thông thường để tìm node kế tiếp theo thứ tự inorder là duyệt từ root. Ở đây, ta chỉ có pointer đến node cha chứ không có root của cây.
>
> Có hai trường hợp: nếu có con phải, successor là node ngoài cùng bên trái của cây con đó; nếu không, đi ngược lên các node cha cho đến khi node hiện tại là con trái, khi đó node cha chính là successor. Cả hai cách đều chỉ đi theo một nhánh.

<!-- thinking:end -->

Nếu `node` có cây con phải, node kế tiếp theo thứ tự inorder của `node` là node ngoài cùng bên trái trong cây con phải.

Nếu `node` không có cây con phải, ta đi ngược lên các node cha khi `node` hiện tại là con phải, cho đến khi node cha là null hoặc node hiện tại là con trái của node cha. Khi đó, node cha là node kế tiếp theo thứ tự inorder.

Độ phức tạp thời gian là $O(h)$, trong đó $h$ là chiều cao của cây nhị phân. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None
        self.parent = None
"""

class Solution:
    def inorderSuccessor(self, node: 'Node') -> 'Optional[Node]':
        if node.right:
            node = node.right
            while node.left:
                node = node.left
            return node
        while node.parent and node.parent.right is node:
            node = node.parent
        return node.parent
```

#### Java

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node parent;
};
*/

class Solution {
    public Node inorderSuccessor(Node node) {
        if (node.right != null) {
            node = node.right;
            while (node.left != null) {
                node = node.left;
            }
            return node;
        }
        while (node.parent != null && node.parent.right == node) {
            node = node.parent;
        }
        return node.parent;
    }
}
```

#### C++

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;
    Node* parent;
};
*/

class Solution {
public:
    Node* inorderSuccessor(Node* node) {
        if (node->right) {
            node = node->right;
            while (node->left) {
                node = node->left;
            }
            return node;
        }
        while (node->parent && node->parent->right == node) {
            node = node->parent;
        }
        return node->parent;
    }
};
```

#### Go

```go
/**
 * Definition for Node.
 * type Node struct {
 *     Val int
 *     Left *Node
 *     Right *Node
 *     Parent *Node
 * }
 */

func inorderSuccessor(node *Node) *Node {
	if node.Right != nil {
		node = node.Right
		for node.Left != nil {
			node = node.Left
		}
		return node
	}
	for node.Parent != nil && node == node.Parent.Right {
		node = node.Parent
	}
	return node.Parent
}
```

#### TypeScript

```ts
/**
 * Definition for a binary tree node.
 * class Node {
 *     val: number
 *     left: Node | null
 *     right: Node | null
 *     parent: Node | null
 *     constructor(val?: number, left?: Node | null, right?: Node | null, parent?: Node | null) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.left = (left===undefined ? null : left)
 *         this.right = (right===undefined ? null : right)
 *         this.parent = (parent===undefined ? null : parent)
 *     }
 * }
 */

function inorderSuccessor(node: Node | null): Node | null {
    if (node.right) {
        node = node.right;
        while (node.left) {
            node = node.left;
        }
        return node;
    }
    while (node.parent && node === node.parent.right) {
        node = node.parent;
    }
    return node.parent;
}
```

#### JavaScript

```js
/**
 * // Definition for a Node.
 * function Node(val) {
 *    this.val = val;
 *    this.left = null;
 *    this.right = null;
 *    this.parent = null;
 * };
 */

/**
 * @param {Node} node
 * @return {Node}
 */
var inorderSuccessor = function (node) {
    if (node.right) {
        node = node.right;
        while (node.left) {
            node = node.left;
        }
        return node;
    }
    while (node.parent && node === node.parent.right) {
        node = node.parent;
    }
    return node.parent;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
