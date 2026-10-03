---
comments: true
difficulty: Easy
tags:
    - Tree
    - Binary Tree
---

<!-- problem:start -->

# [2236. Root Equals Sum of Children](https://leetcode.com/problems/root-equals-sum-of-children)

[中文文档](/solution/2200-2299/2236.Root%20Equals%20Sum%20of%20Children/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho <code>root</code> của một <strong>cây nhị phân</strong> gồm chính xác <code>3</code> node: root, con trái và con phải của nó.</p>

<p>Trả về <code>true</code> <em>nếu giá trị của root bằng <strong>tổng</strong> giá trị của hai node con, nếu không thì trả về </em><code>false</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2236.Root%20Equals%20Sum%20of%20Children/images/graph3drawio.png" style="width: 281px; height: 199px;" />
<pre>
<strong>Đầu vào:</strong> root = [10,4,6]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Giá trị của root, con trái và con phải lần lượt là 10, 4 và 6.
10 bằng 4 + 6, nên ta trả về true.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2236.Root%20Equals%20Sum%20of%20Children/images/graph3drawio-1.png" style="width: 281px; height: 199px;" />
<pre>
<strong>Đầu vào:</strong> root = [5,3,1]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Giá trị của root, con trái và con phải lần lượt là 5, 3 và 1.
5 không bằng 3 + 1, nên ta trả về false.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Cây chỉ gồm root, con trái và con phải của nó.</li>
	<li><code>-100 &lt;= Node.val &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cây có đúng ba node. Chỉ cần so sánh root với tổng giá trị của hai node con; không cần duyệt cây.

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
    def checkTree(self, root: Optional[TreeNode]) -> bool:
        return root.val == root.left.val + root.right.val
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
    public boolean checkTree(TreeNode root) {
        return root.val == root.left.val + root.right.val;
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
    bool checkTree(TreeNode* root) {
        return root->val == root->left->val + root->right->val;
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
func checkTree(root *TreeNode) bool {
	return root.Val == root.Left.Val+root.Right.Val
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

function checkTree(root: TreeNode | null): boolean {
    return root.val === root.left.val + root.right.val;
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
    pub fn check_tree(root: Option<Rc<RefCell<TreeNode>>>) -> bool {
        let node = root.as_ref().unwrap().borrow();
        let left = node.left.as_ref().unwrap().borrow().val;
        let right = node.right.as_ref().unwrap().borrow().val;
        node.val == left + right
    }
}
```

#### C

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */

bool checkTree(struct TreeNode* root) {
    return root->val == root->left->val + root->right->val;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
