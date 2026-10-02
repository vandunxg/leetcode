---
comments: true
difficulty: Medium
tags:
    - Stack
    - Tree
    - Depth-First Search
    - Binary Search Tree
    - Linked List
    - Binary Tree
    - Doubly-Linked List
---

<!-- problem:start -->

# [426. Convert Binary Search Tree to Sorted Doubly Linked List 🔒](https://leetcode.com/problems/convert-binary-search-tree-to-sorted-doubly-linked-list)

[中文文档](/solution/0400-0499/0426.Convert%20Binary%20Search%20Tree%20to%20Sorted%20Doubly%20Linked%20List/README.md)

## Mô tả

<!-- description:start -->

<p>Chuyển một <strong>Binary Search Tree</strong> thành <strong>Circular Doubly-Linked List</strong> đã sắp xếp, ngay trên cấu trúc hiện có.</p>

<p>Có thể xem pointer trái và phải tương ứng với pointer predecessor và successor trong doubly-linked list. Với circular doubly linked list, predecessor của phần tử đầu tiên là phần tử cuối cùng, còn successor của phần tử cuối cùng là phần tử đầu tiên.</p>

<p>Ta cần thực hiện chuyển đổi <strong>ngay trên cấu trúc hiện có</strong>. Sau khi chuyển đổi, pointer trái của mỗi node trong cây trỏ đến predecessor, còn pointer phải trỏ đến successor. Hãy trả về pointer đến phần tử nhỏ nhất trong linked list.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0426.Convert%20Binary%20Search%20Tree%20to%20Sorted%20Doubly%20Linked%20List/images/bstdlloriginalbst.png" style="width: 100%; max-width: 300px;" /></p>

<pre>
<strong>Đầu vào:</strong> root = [4,2,5,1,3]

<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0426.Convert%20Binary%20Search%20Tree%20to%20Sorted%20Doubly%20Linked%20List/images/bstdllreturndll.png" style="width: 100%; max-width: 450px;" />
<strong>Đầu ra:</strong> [1,2,3,4,5]

<strong>Giải thích:</strong> Hình dưới đây minh họa BST sau khi chuyển đổi. Đường liền biểu thị quan hệ successor, còn đường nét đứt biểu thị quan hệ predecessor.
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0426.Convert%20Binary%20Search%20Tree%20to%20Sorted%20Doubly%20Linked%20List/images/bstdllreturnbst.png" style="width: 100%; max-width: 450px;" />
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [2,1,3]
<strong>Đầu ra:</strong> [1,2,3]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[0, 2000]</code>.</li>
	<li><code>-1000 &lt;= Node.val &lt;= 1000</code></li>
	<li>Tất cả giá trị trong cây đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Duyệt inorder trên BST cho các node theo thứ tự đã sắp xếp. Nếu gom node vào mảng rồi nối lại, ta sẽ tốn thêm bộ nhớ. Bài toán yêu cầu tạo circular list ngay trên cấu trúc hiện có, tái sử dụng $\textit{left}/\textit{right}$ làm pred/succ.
>
> Trong lúc duyệt inorder, nối $\textit{prev}$ với node hiện tại và lưu node đầu tiên vào $\textit{head}$. Sau đó nối $\textit{head}$ với node cuối cùng để khép kín vòng.
>
> Thứ tự left–root–right giúp các liên kết trùng với thứ tự đã sắp xếp; khi $\textit{prev}$ là null, node hiện tại chính là head.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
"""


class Solution:
    def treeToDoublyList(self, root: 'Optional[Node]') -> 'Optional[Node]':
        def dfs(root):
            if root is None:
                return
            nonlocal prev, head
            dfs(root.left)
            if prev:
                prev.right = root
                root.left = prev
            else:
                head = root
            prev = root
            dfs(root.right)

        if root is None:
            return None
        head = prev = None
        dfs(root)
        prev.right = head
        head.left = prev
        return head
```

#### Java

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val,Node _left,Node _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
*/

class Solution {
    private Node prev;
    private Node head;

    public Node treeToDoublyList(Node root) {
        if (root == null) {
            return null;
        }
        prev = null;
        head = null;
        dfs(root);
        prev.right = head;
        head.left = prev;
        return head;
    }

    private void dfs(Node root) {
        if (root == null) {
            return;
        }
        dfs(root.left);
        if (prev != null) {
            prev.right = root;
            root.left = prev;
        } else {
            head = root;
        }
        prev = root;
        dfs(root.right);
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

    Node() {}

    Node(int _val) {
        val = _val;
        left = NULL;
        right = NULL;
    }

    Node(int _val, Node* _left, Node* _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
*/

class Solution {
public:
    Node* prev;
    Node* head;

    Node* treeToDoublyList(Node* root) {
        if (!root) return nullptr;
        prev = nullptr;
        head = nullptr;
        dfs(root);
        prev->right = head;
        head->left = prev;
        return head;
    }

    void dfs(Node* root) {
        if (!root) return;
        dfs(root->left);
        if (prev) {
            prev->right = root;
            root->left = prev;
        } else
            head = root;
        prev = root;
        dfs(root->right);
    }
};
```

#### Go

```go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Left *Node
 *     Right *Node
 * }
 */

func treeToDoublyList(root *Node) *Node {
	if root == nil {
		return root
	}
	var prev, head *Node

	var dfs func(root *Node)
	dfs = func(root *Node) {
		if root == nil {
			return
		}
		dfs(root.Left)
		if prev != nil {
			prev.Right = root
			root.Left = prev
		} else {
			head = root
		}
		prev = root
		dfs(root.Right)
	}
	dfs(root)
	prev.Right = head
	head.Left = prev
	return head
}
```

#### JavaScript

```js
/**
 * // Definition for a Node.
 * function Node(val, left, right) {
 *      this.val = val;
 *      this.left = left;
 *      this.right = right;
 *  };
 */

/**
 * @param {Node} root
 * @return {Node}
 */
var treeToDoublyList = function (root) {
    if (!root) return root;
    let prev = null;
    let head = null;

    function dfs(root) {
        if (!root) return;
        dfs(root.left);
        if (prev) {
            prev.right = root;
            root.left = prev;
        } else {
            head = root;
        }
        prev = root;
        dfs(root.right);
    }
    dfs(root);
    prev.right = head;
    head.left = prev;
    return head;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
