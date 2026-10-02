---
comments: true
difficulty: Easy
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [965. Univalued Binary Tree](https://leetcode.com/problems/univalued-binary-tree)

[中文文档](/solution/0900-0999/0965.Univalued%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cây nhị phân được gọi là <strong>đồng nhất giá trị</strong> nếu mọi node trong cây đều có cùng giá trị.</p>

<p>Cho <code>root</code> của một cây nhị phân, trả về <code>true</code><em> nếu cây đã cho </em><strong>đồng nhất giá trị</strong><em>, nếu không thì trả về </em><code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0965.Univalued%20Binary%20Tree/images/unival_bst_1.png" style="width: 265px; height: 172px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,1,1,1,1,null,1]
<strong>Đầu ra:</strong> true
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0965.Univalued%20Binary%20Tree/images/unival_bst_2.png" style="width: 198px; height: 169px;" />
<pre>
<strong>Đầu vào:</strong> root = [2,2,2,5,2]
<strong>Đầu ra:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong đoạn <code>[1, 100]</code>.</li>
	<li><code>0 &lt;= Node.val &lt; 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Kiểm tra xem mọi node có cùng một giá trị hay không. Lưu giá trị root là $x$ rồi dùng DFS, yêu cầu mỗi node có giá trị bằng $x$. Node null luôn được xem là thỏa mãn.

<!-- thinking:end -->

Gọi giá trị của node gốc là $x$, sau đó định nghĩa hàm $\text{dfs}(\text{root})$ để kiểm tra giá trị của node hiện tại có bằng $x$ hay không, đồng thời hai cây con trái và phải của nó có phải là cây nhị phân đồng nhất giá trị hay không.

Trong hàm $\text{dfs}(\text{root})$, nếu node hiện tại là null thì trả về $\text{true}$. Ngược lại, nếu giá trị node hiện tại bằng $x$ và hai cây con trái, phải cũng là cây nhị phân đồng nhất giá trị thì trả về $\text{true}$; nếu không thì trả về $\text{false}$.

Trong hàm chính, ta gọi $\text{dfs}(\text{root})$ và trả về kết quả.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node trong cây.

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
    def isUnivalTree(self, root: Optional[TreeNode]) -> bool:
        def dfs(root: Optional[TreeNode]) -> bool:
            if root is None:
                return True
            return root.val == x and dfs(root.left) and dfs(root.right)

        x = root.val
        return dfs(root)
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
    private int x;

    public boolean isUnivalTree(TreeNode root) {
        x = root.val;
        return dfs(root);
    }

    private boolean dfs(TreeNode root) {
        if (root == null) {
            return true;
        }
        return root.val == x && dfs(root.left) && dfs(root.right);
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
    bool isUnivalTree(TreeNode* root) {
        int x = root->val;
        auto dfs = [&](this auto&& dfs, TreeNode* root) -> bool {
            if (!root) {
                return true;
            }
            return root->val == x && dfs(root->left) && dfs(root->right);
        };
        return dfs(root);
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
func isUnivalTree(root *TreeNode) bool {
	x := root.Val
	var dfs func(*TreeNode) bool
	dfs = func(root *TreeNode) bool {
		if root == nil {
			return true
		}
		return root.Val == x && dfs(root.Left) && dfs(root.Right)
	}
	return dfs(root)
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

function isUnivalTree(root: TreeNode | null): boolean {
    const x = root!.val;
    const dfs = (root: TreeNode | null): boolean => {
        if (!root) {
            return true;
        }
        return root.val === x && dfs(root.left) && dfs(root.right);
    };
    return dfs(root);
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
    pub fn is_unival_tree(root: Option<Rc<RefCell<TreeNode>>>) -> bool {
        let x = root.as_ref().unwrap().borrow().val;

        fn dfs(node: Option<Rc<RefCell<TreeNode>>>, x: i32) -> bool {
            if let Some(n) = node {
                let n = n.borrow();
                n.val == x && dfs(n.left.clone(), x) && dfs(n.right.clone(), x)
            } else {
                true
            }
        }

        dfs(root, x)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
