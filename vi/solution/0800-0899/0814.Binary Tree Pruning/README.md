---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [814. Binary Tree Pruning](https://leetcode.com/problems/binary-tree-pruning)

[中文文档](/solution/0800-0899/0814.Binary%20Tree%20Pruning/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một binary tree. Hãy trả về <em>cây đó sau khi loại bỏ mọi subtree không chứa </em><code>1</code>.</p>

<p>Subtree của một node <code>node</code> gồm chính <code>node</code> đó và tất cả node là hậu duệ của nó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0814.Binary%20Tree%20Pruning/images/1028_2.png" style="width: 500px; height: 140px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,null,0,0,1]
<strong>Đầu ra:</strong> [1,null,0,null,1]
<strong>Giải thích:</strong> 
Chỉ các node màu đỏ thỏa mãn điều kiện &quot;mọi subtree không chứa 1 đều bị loại bỏ&quot;.
Hình bên phải biểu diễn kết quả.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0814.Binary%20Tree%20Pruning/images/1028_1.png" style="width: 500px; height: 115px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,0,1,0,0,0,1]
<strong>Đầu ra:</strong> [1,null,1,null,1]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0814.Binary%20Tree%20Pruning/images/1028.png" style="width: 500px; height: 134px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,1,0,1,1,0,1,0]
<strong>Đầu ra:</strong> [1,1,0,1,1,null,1]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 200]</code>.</li>
	<li><code>Node.val</code> là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Phải loại bỏ mọi subtree không chứa $1$. Duyệt preorder không thể quyết định trước khi xét các node con, vì vậy ta prune cây con trái và phải trước, rồi xóa node hiện tại nếu nó có giá trị $0$ và không còn node con nào.
>
> Sau các lời gọi đệ quy, nếu một node có giá trị $0$ và cả hai cây con đều đã bị loại bỏ thì xóa node đó. Cây có tối đa $200$ node nên chỉ cần một lượt DFS.

<!-- thinking:end -->

Trước tiên, kiểm tra node hiện tại có null hay không. Nếu có, trả về null ngay.

Nếu không, ta đệ quy prune cây con trái và phải, rồi gán lại các cây con đã prune cho hai nhánh tương ứng của node hiện tại. Tiếp đó, kiểm tra giá trị node hiện tại có bằng 0 và cả hai node con đều null hay không. Nếu đúng, trả về null; nếu không, trả về node hiện tại.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node trong binary tree.

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
    def pruneTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if root is None:
            return root
        root.left = self.pruneTree(root.left)
        root.right = self.pruneTree(root.right)
        if root.val == 0 and root.left == root.right:
            return None
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
    public TreeNode pruneTree(TreeNode root) {
        if (root == null) {
            return null;
        }
        root.left = pruneTree(root.left);
        root.right = pruneTree(root.right);
        if (root.val == 0 && root.left == null && root.right == null) {
            return null;
        }
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
    TreeNode* pruneTree(TreeNode* root) {
        if (!root) {
            return root;
        }
        root->left = pruneTree(root->left);
        root->right = pruneTree(root->right);
        if (root->val == 0 && root->left == root->right) {
            return nullptr;
        }
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
func pruneTree(root *TreeNode) *TreeNode {
	if root == nil {
		return nil
	}
	root.Left = pruneTree(root.Left)
	root.Right = pruneTree(root.Right)
	if root.Val == 0 && root.Left == nil && root.Right == nil {
		return nil
	}
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

function pruneTree(root: TreeNode | null): TreeNode | null {
    if (!root) {
        return root;
    }
    root.left = pruneTree(root.left);
    root.right = pruneTree(root.right);
    if (root.val === 0 && root.left === root.right) {
        return null;
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
    pub fn prune_tree(root: Option<Rc<RefCell<TreeNode>>>) -> Option<Rc<RefCell<TreeNode>>> {
        if root.is_none() {
            return None;
        }

        let root = root.unwrap();
        let left = Self::prune_tree(root.borrow_mut().left.take());
        let right = Self::prune_tree(root.borrow_mut().right.take());
        if root.borrow().val == 0 && left.is_none() && right.is_none() {
            return None;
        }

        root.borrow_mut().left = left;
        root.borrow_mut().right = right;
        Some(root)
    }
}
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
 * @return {TreeNode}
 */
var pruneTree = function (root) {
    if (!root) {
        return root;
    }
    root.left = pruneTree(root.left);
    root.right = pruneTree(root.right);
    if (root.val === 0 && root.left === root.right) {
        return null;
    }
    return root;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
