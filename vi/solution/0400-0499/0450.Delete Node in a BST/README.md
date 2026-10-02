---
comments: true
difficulty: Medium
tags:
    - Tree
    - Binary Search Tree
    - Binary Tree
---

<!-- problem:start -->

# [450. Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst)

[中文文档](/solution/0400-0499/0450.Delete%20Node%20in%20a%20BST/README.md)

## Mô tả

<!-- description:start -->

<p>Cho tham chiếu đến node gốc của một BST và một key, hãy xóa node có key đã cho khỏi BST. Trả về <em><strong>tham chiếu đến node gốc</strong> (có thể đã được cập nhật) của BST</em>.</p>

<p>Về cơ bản, thao tác xóa gồm hai bước:</p>

<ol>
	<li>Tìm node cần xóa.</li>
	<li>Nếu tìm thấy node, xóa node đó.</li>
</ol>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0450.Delete%20Node%20in%20a%20BST/images/del_node_1.jpg" style="width: 800px; height: 214px;" />
<pre>
<strong>Đầu vào:</strong> root = [5,3,6,2,4,null,7], key = 3
<strong>Đầu ra:</strong> [5,4,6,2,null,null,7]
<strong>Giải thích:</strong> Key cần xóa là 3. Ta tìm node có giá trị 3 rồi xóa nó.
Một kết quả hợp lệ là [5,4,6,2,null,null,7], như cây BST ở trên.
Lưu ý rằng [5,2,6,null,4,null,7] cũng là một kết quả hợp lệ và được chấp nhận.
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0450.Delete%20Node%20in%20a%20BST/images/del_node_supp.jpg" style="width: 350px; height: 255px;" />
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [5,3,6,2,4,null,7], key = 0
<strong>Đầu ra:</strong> [5,3,6,2,4,null,7]
<strong>Giải thích:</strong> Cây không có node nào mang giá trị = 0.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [], key = 0
<strong>Đầu ra:</strong> []
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[0, 10<sup>4</sup>]</code>.</li>
	<li><code>-10<sup>5</sup> &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
	<li>Mỗi node có giá trị <strong>khác nhau</strong>.</li>
	<li><code>root</code> là một cây tìm kiếm nhị phân hợp lệ.</li>
	<li><code>-10<sup>5</sup> &lt;= key &lt;= 10<sup>5</sup></code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể giải bài toán với độ phức tạp thời gian <code>O(height of tree)</code> không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Khi xóa, cần giữ đúng thứ tự của BST. Node có một node con sẽ được thay bằng node con đó; node có hai node con cần dùng node kế nhiệm hoặc tiền nhiệm.
>
> Dựa vào phép so sánh để đệ quy sang trái hoặc phải. Khi tìm thấy node cần xóa: trả về node con phải nếu không có node con trái, trả về node con trái nếu không có node con phải; nếu có cả hai, gắn toàn bộ cây con trái vào node ngoài cùng bên trái của cây con phải rồi trả về cây con phải.
>
> Node ngoài cùng bên trái đó là node kế nhiệm theo thứ tự inorder và không có node con trái, nên thứ tự inorder vẫn được bảo toàn sau khi nối cây con.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def deleteNode(self, root: Optional[TreeNode], key: int) -> Optional[TreeNode]:
        if root is None:
            return None
        if root.val > key:
            root.left = self.deleteNode(root.left, key)
            return root
        if root.val < key:
            root.right = self.deleteNode(root.right, key)
            return root
        if root.left is None:
            return root.right
        if root.right is None:
            return root.left
        node = root.right
        while node.left:
            node = node.left
        node.left = root.left
        root = root.right
        return root
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
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null) {
            return null;
        }
        if (root.val > key) {
            root.left = deleteNode(root.left, key);
            return root;
        }
        if (root.val < key) {
            root.right = deleteNode(root.right, key);
            return root;
        }
        if (root.left == null) {
            return root.right;
        }
        if (root.right == null) {
            return root.left;
        }
        TreeNode node = root.right;
        while (node.left != null) {
            node = node.left;
        }
        node.left = root.left;
        root = root.right;
        return root;
    }
}
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
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        if (!root) return root;
        if (root->val > key) {
            root->left = deleteNode(root->left, key);
            return root;
        }
        if (root->val < key) {
            root->right = deleteNode(root->right, key);
            return root;
        }
        if (!root->left) return root->right;
        if (!root->right) return root->left;
        TreeNode* node = root->right;
        while (node->left) node = node->left;
        node->left = root->left;
        root = root->right;
        return root;
    }
};
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
func deleteNode(root *TreeNode, key int) *TreeNode {
	if root == nil {
		return nil
	}
	if root.Val > key {
		root.Left = deleteNode(root.Left, key)
		return root
	}
	if root.Val < key {
		root.Right = deleteNode(root.Right, key)
		return root
	}
	if root.Left == nil {
		return root.Right
	}
	if root.Right == nil {
		return root.Left
	}
	node := root.Right
	for node.Left != nil {
		node = node.Left
	}
	node.Left = root.Left
	root = root.Right
	return root
}
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

function deleteNode(root: TreeNode | null, key: number): TreeNode | null {
    if (root == null) {
        return root;
    }
    const { val, left, right } = root;
    if (val > key) {
        root.left = deleteNode(left, key);
    } else if (val < key) {
        root.right = deleteNode(right, key);
    } else {
        if (left == null && right == null) {
            root = null;
        } else if (left == null || right == null) {
            root = left || right;
        } else {
            if (right.left == null) {
                right.left = left;
                root = right;
            } else {
                let minPreNode = right;
                while (minPreNode.left.left != null) {
                    minPreNode = minPreNode.left;
                }
                const minVal = minPreNode.left.val;
                root.val = minVal;
                minPreNode.left = deleteNode(minPreNode.left, minVal);
            }
        }
    }
    return root;
}
```

#### Rust

```rust
// Definition for a binary tree node.
// #[derive(Debug, PartialEq, Eq)]
// pub struct TreeNode {
//   pub val: i32,
//   pub left: Option<Rc<RefCell<TreeNode>>>,
//   pub right: Option<Rc<RefCell<TreeNode>>>,
// }
//
// impl TreeNode {
//   #[inline]
//   pub fn new(val: i32) -> Self {
//     TreeNode {
//       val,
//       left: None,
//       right: None
//     }
//   }
// }
use std::cell::RefCell;
use std::rc::Rc;
impl Solution {
    fn dfs(root: &Option<Rc<RefCell<TreeNode>>>) -> i32 {
        let node = root.as_ref().unwrap().borrow();
        if node.left.is_none() {
            return node.val;
        }
        Self::dfs(&node.left)
    }

    pub fn delete_node(
        mut root: Option<Rc<RefCell<TreeNode>>>,
        key: i32,
    ) -> Option<Rc<RefCell<TreeNode>>> {
        if root.is_some() {
            let mut node = root.as_mut().unwrap().borrow_mut();
            match node.val.cmp(&key) {
                std::cmp::Ordering::Less => {
                    node.right = Self::delete_node(node.right.take(), key);
                }
                std::cmp::Ordering::Greater => {
                    node.left = Self::delete_node(node.left.take(), key);
                }
                std::cmp::Ordering::Equal => {
                    match (node.left.is_some(), node.right.is_some()) {
                        (false, false) => {
                            return None;
                        }
                        (true, false) => {
                            return node.left.take();
                        }
                        (false, true) => {
                            return node.right.take();
                        }
                        (true, true) => {
                            if node.right.as_ref().unwrap().borrow().left.is_none() {
                                let mut r = node.right.take();
                                r.as_mut().unwrap().borrow_mut().left = node.left.take();
                                return r;
                            } else {
                                let val = Self::dfs(&node.right);
                                node.val = val;
                                node.right = Self::delete_node(node.right.take(), val);
                            }
                        }
                    };
                }
            }
        }
        root
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
