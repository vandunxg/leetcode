---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [04.10. Check SubTree](https://leetcode.cn/problems/check-subtree-lcci)

[中文文档](/lcci/04.10.Check%20SubTree/README.md)

## Mô tả

<!-- description:start -->

<p>T1&nbsp;và T2 là hai binary tree rất lớn, trong đó T1&nbsp;lớn hơn T2 rất nhiều. Hãy tạo một thuật toán để xác định T2 có phải là subtree của T1 hay không.</p>

<p>Tree T2 là subtree của T1&nbsp;nếu tồn tại một node n trong T1&nbsp;sao cho subtree của n giống hệt T2. Nói cách khác, nếu cắt tree tại node n thì hai tree sẽ giống hệt nhau.</p>

<p><strong>Ví dụ 1:</strong></p>

<pre>

<strong> Đầu vào</strong>: t1 = [1, 2, 3], t2 = [2]

<strong> Đầu ra</strong>: true

</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>

<strong> Đầu vào</strong>: t1 = [1, null, 2, 4], t2 = [3, 2]

<strong> Đầu ra</strong>: false

</pre>

<p><strong>Lưu ý: </strong></p>

<ol>
	<li>Giá trị node của cả hai tree nằm trong [0, 20000].</li>
</ol>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Xác định xem $t_2$ có bằng một subtree nào đó của $t_1$ hay không. Serialization kết hợp với tìm kiếm substring có thể dùng được nếu mã hóa các node rỗng một cách cẩn thận.
>
> Có thể thử mọi node của $t_1$ làm root tương ứng với $t_2$; cả cấu trúc và giá trị đều phải khớp.
>
> `dfs` bên trong so sánh hai tree theo từng cặp node; lời gọi bên ngoài thử lại ở left và right của $t_1$ sau khi không khớp. $t_2$ rỗng thì kết quả là true; $t_1$ rỗng thì kết quả là false.

<!-- thinking:end -->

Trước tiên, ta kiểm tra xem $t_2$ có phải là null hay không. Nếu phải, $t_2$ chắc chắn là subtree của $t_1$, nên ta trả về `true`.

Tiếp theo, ta kiểm tra xem $t_1$ có phải là null hay không. Nếu phải, $t_2$ chắc chắn không phải là subtree của $t_1$, nên ta trả về `false`.

Sau đó, ta kiểm tra xem $t_1$ và $t_2$ có bằng nhau hay không. Nếu bằng nhau, $t_2$ là subtree của $t_1$, nên ta trả về `true`. Nếu không, ta đệ quy kiểm tra left subtree và right subtree của $t_1$ có bằng $t_2$ hay không. Nếu một trong hai kết quả là `true`, thì $t_2$ là subtree của $t_1$, nên ta trả về `true`.

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(n)$. Trong đó $n$ là số node trong $t_1$.

<!-- tabs:start -->

#### Python3

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None


class Solution:
    def checkSubTree(self, t1: TreeNode, t2: TreeNode) -> bool:
        def dfs(t1, t2):
            if t2 is None:
                return t1 is None
            if t1 is None or t1.val != t2.val:
                return False
            return dfs(t1.left, t2.left) and dfs(t1.right, t2.right)

        if t2 is None:
            return True
        if t1 is None:
            return False
        if dfs(t1, t2):
            return True
        return self.checkSubTree(t1.left, t2) or self.checkSubTree(t1.right, t2)
```

#### Java

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean checkSubTree(TreeNode t1, TreeNode t2) {
        if (t2 == null) {
            return true;
        }
        if (t1 == null) {
            return false;
        }
        if (dfs(t1, t2)) {
            return true;
        }
        return checkSubTree(t1.left, t2) || checkSubTree(t1.right, t2);
    }

    private boolean dfs(TreeNode t1, TreeNode t2) {
        if (t2 == null) {
            return t1 == null;
        }
        if (t1 == null || t1.val != t2.val) {
            return false;
        }
        return dfs(t1.left, t2.left) && dfs(t1.right, t2.right);
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
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool checkSubTree(TreeNode* t1, TreeNode* t2) {
        if (!t2) {
            return true;
        }
        if (!t1) {
            return false;
        }
        if (dfs(t1, t2)) {
            return true;
        }
        return checkSubTree(t1->left, t2) || checkSubTree(t1->right, t2);
    }

    bool dfs(TreeNode* t1, TreeNode* t2) {
        if (!t2) {
            return !t1;
        }
        if (!t1 || t1->val != t2->val) {
            return false;
        }
        return dfs(t1->left, t2->left) && dfs(t1->right, t2->right);
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
func checkSubTree(t1 *TreeNode, t2 *TreeNode) bool {
	var dfs func(t1, t2 *TreeNode) bool
	dfs = func(t1, t2 *TreeNode) bool {
		if t2 == nil {
			return t1 == nil
		}
		if t1 == nil || t1.Val != t2.Val {
			return false
		}
		return dfs(t1.Left, t2.Left) && dfs(t1.Right, t2.Right)
	}
	if t2 == nil {
		return true
	}
	if t1 == nil {
		return false
	}
	if dfs(t1, t2) {
		return true
	}
	return checkSubTree(t1.Left, t2) || checkSubTree(t1.Right, t2)
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

function checkSubTree(t1: TreeNode | null, t2: TreeNode | null): boolean {
    const dfs = (t1: TreeNode | null, t2: TreeNode | null): boolean => {
        if (!t2) {
            return !t1;
        }
        if (!t1 || t1.val !== t2.val) {
            return false;
        }
        return dfs(t1.left, t2.left) && dfs(t1.right, t2.right);
    };
    if (!t2) {
        return true;
    }
    if (!t1) {
        return false;
    }
    if (dfs(t1, t2)) {
        return true;
    }
    return checkSubTree(t1.left, t2) || checkSubTree(t1.right, t2);
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
    fn dfs(t1: &Option<Rc<RefCell<TreeNode>>>, t2: &Option<Rc<RefCell<TreeNode>>>) -> bool {
        match (t1, t2) {
            (Some(node1), Some(node2)) => {
                let n1 = node1.borrow();
                let n2 = node2.borrow();
                n1.val == n2.val
                    && Solution::dfs(&n1.left, &n2.left)
                    && Solution::dfs(&n1.right, &n2.right)
            }
            (None, Some(_)) => false,
            (Some(_), None) => false,
            _ => true, // Both are None
        }
    }

    pub fn check_sub_tree(
        t1: Option<Rc<RefCell<TreeNode>>>,
        t2: Option<Rc<RefCell<TreeNode>>>,
    ) -> bool {
        match (t1, t2) {
            (Some(node1), Some(node2)) => {
                let n1 = node1.borrow();
                let n2 = node2.borrow();
                Solution::dfs(&Some(Rc::clone(&node1)), &Some(Rc::clone(&node2)))
                    || Solution::check_sub_tree(n1.left.clone(), Some(Rc::clone(&node2)))
                    || Solution::check_sub_tree(n1.right.clone(), Some(Rc::clone(&node2)))
            }
            (Some(_), None) => true,
            (None, Some(_)) => false,
            _ => true, // Both are None or t1 is None
        }
    }
}
```

#### Swift

```swift
/* class TreeNode {
*    var val: Int
*    var left: TreeNode?
*    var right: TreeNode?
*
*    init(_ val: Int, _ left: TreeNode? = nil, _ right: TreeNode? = nil) {
*        self.val = val
*        self.left = left
*        self.right = right
*    }
* }
*/

class Solution {
    func checkSubTree(_ t1: TreeNode?, _ t2: TreeNode?) -> Bool {
        if t2 == nil {
            return true
        }
        if t1 == nil {
            return false
        }
        if isSameTree(t1, t2) {
            return true
        }
        return checkSubTree(t1!.left, t2) || checkSubTree(t1!.right, t2)
    }

    private func isSameTree(_ t1: TreeNode?, _ t2: TreeNode?) -> Bool {
        if t1 == nil && t2 == nil {
            return true
        }
        if t1 == nil || t2 == nil {
            return false
        }
        if t1!.val != t2!.val {
            return false
        }
        return isSameTree(t1!.left, t2!.left) && isSameTree(t1!.right, t2!.right)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
