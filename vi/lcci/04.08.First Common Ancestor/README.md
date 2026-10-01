---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [04.08. First Common Ancestor](https://leetcode.cn/problems/first-common-ancestor-lcci)

[中文文档](/lcci/04.08.First%20Common%20Ancestor/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế một thuật toán và viết code để tìm tổ tiên chung đầu tiên của hai node trong một binary tree. Tránh lưu trữ các node bổ sung trong một data structure. LƯU Ý: Đây không nhất thiết là một binary search tree.</p>

<p>Ví dụ, cho binary tree sau: root = [3,5,1,6,2,0,8,null,null,7,4]</p>

<pre>

    3

   / \

  5   1

 / \ / \
6  2 0  8

  / \
 7   4

</pre>

<p><strong>Ví dụ 1:</strong></p>

<pre>

<strong>Input:</strong> root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1

<strong>Input:</strong> 3

<strong>Giải thích:</strong> Tổ tiên chung đầu tiên của node 5 và node 1 là node 3.</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>

<strong>Input:</strong> root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4

<strong>Output:</strong> 5

<strong>Giải thích:</strong> Tổ tiên chung đầu tiên của node 5 và node 4 là node 5.</pre>

<p><strong>Ghi chú:</strong></p>

<ul>
	<li>Tất cả giá trị của các node đều đôi một khác nhau.</li>
	<li>p, q là hai node khác nhau và cả hai đều có thể được tìm thấy trong binary tree đã cho.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> LCA là node sâu nhất bao phủ cả $p$ và $q$. Lưu hai đường đi từ root đến node có thể thực hiện được nhưng sẽ tốn thêm bộ nhớ.
>
> Nếu $p$ và $q$ nằm trong hai subtree khác nhau, root hiện tại là đáp án; nếu chúng nằm về một phía, đáp án nằm sâu hơn ở phía đó.
>
> Đệ quy: trả về node nếu node đó là null hoặc bằng $p$/$q$; trả về root khi cả hai phía đều khác null, nếu không thì trả về phía khác null. Mỗi node được duyệt một lần.

<!-- thinking:end -->

Đầu tiên, chúng ta kiểm tra xem root có phải là null hoặc root có bằng $\textit{p}$ hay $\textit{q}$ không. Nếu có, chúng ta trả về trực tiếp root.

Sau đó, chúng ta đệ quy tìm kiếm trong cây con trái và cây con phải để lần lượt lấy $\textit{left}$ và $\textit{right}$. Nếu cả $\textit{left}$ và $\textit{right}$ đều không null, điều đó có nghĩa là $\textit{p}$ và $\textit{q}$ lần lượt nằm trong cây con trái và cây con phải, nên root là tổ tiên chung thấp nhất. Ngược lại, nếu $\textit{left}$ hoặc $\textit{right}$ là null, điều đó có nghĩa là cả $\textit{p}$ và $\textit{q}$ đều nằm trong cây con khác null, nên root của cây con khác null là tổ tiên chung thấp nhất.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số node trong binary tree.

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
    def lowestCommonAncestor(
        self, root: TreeNode, p: TreeNode, q: TreeNode
    ) -> TreeNode:
        if root is None or root in [p, q]:
            return root
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        return root if left and right else left or right
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
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || root == p || root == q) {
            return root;
        }
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        return left == null ? right : (right == null ? left : root);
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
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (!root || root == p || root == q) {
            return root;
        }
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);
        return left && right ? root : (left ? left : right);
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
func lowestCommonAncestor(root *TreeNode, p *TreeNode, q *TreeNode) *TreeNode {
	if root == nil || root == p || root == q {
		return root
	}
	left := lowestCommonAncestor(root.Left, p, q)
	right := lowestCommonAncestor(root.Right, p, q)
	if left == nil {
		return right
	}
	if right == nil {
		return left
	}
	return root
}
```

#### JavaScript

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {TreeNode}
 */
var lowestCommonAncestor = function (root, p, q) {
    if (!root || root === p || root === q) {
        return root;
    }
    const left = lowestCommonAncestor(root.left, p, q);
    const right = lowestCommonAncestor(root.right, p, q);
    return left && right ? root : left || right;
};
```

#### Swift

```swift
/* class TreeNode {
*    var val: Int
*    var left: TreeNode?
*    var right: TreeNode?
*
*    init(_ val: Int) {
*        self.val = val
*        self.left = nil
*        self.right = nil
*    }
* }
*/

class Solution {
    func lowestCommonAncestor(_ root: TreeNode?, _ p: TreeNode?, _ q: TreeNode?) -> TreeNode? {
        if root == nil || root === p || root === q {
            return root
        }
        let left = lowestCommonAncestor(root?.left, p, q)
        let right = lowestCommonAncestor(root?.right, p, q)
        if left == nil {
            return right
        } else if right == nil {
            return left
        } else {
            return root
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
