---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Binary Search Tree
    - Binary Tree
---

<!-- problem:start -->

# [285. Inorder Successor in BST 🔒](https://leetcode.com/problems/inorder-successor-in-bst)

[中文文档](/solution/0200-0299/0285.Inorder%20Successor%20in%20BST/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một binary search tree và node <code>p</code> trong cây. Hãy trả về <em>node kế tiếp của node đó theo thứ tự inorder trong BST</em>. Nếu node đã cho không có node kế tiếp theo thứ tự inorder trong cây, hãy trả về <code>null</code>.</p>

<p>Node kế tiếp của node <code>p</code> là node có key nhỏ nhất nhưng lớn hơn <code>p.val</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0200-0299/0285.Inorder%20Successor%20in%20BST/images/285_example_1.png" style="width: 122px; height: 117px;" />
<pre>
<strong>Đầu vào:</strong> root = [2,1,3], p = 1
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Node kế tiếp theo thứ tự inorder của 1 là node 2. Lưu ý rằng cả p lẫn giá trị trả về đều có kiểu TreeNode.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0200-0299/0285.Inorder%20Successor%20in%20BST/images/285_example_2.png" style="width: 246px; height: 229px;" />
<pre>
<strong>Đầu vào:</strong> root = [5,3,6,2,4,null,null,1], p = 6
<strong>Đầu ra:</strong> null
<strong>Giải thích:</strong> Node hiện tại không có node kế tiếp theo thứ tự inorder, nên đáp án là <code>null</code>.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 10<sup>4</sup>]</code>.</li>
	<li><code>-10<sup>5</sup> &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
	<li>Tất cả node đều có giá trị khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Node kế tiếp theo thứ tự inorder là node có key nhỏ nhất nhưng lớn hơn $p$. Trên BST, ta duyệt tương tự tìm kiếm nhị phân: nếu key hiện tại lớn hơn $p$ thì đó là một ứng viên và ta đi sang trái; nếu không thì đi sang phải.
>
> Ứng viên cuối cùng tìm được trên đường đi đó chính là node kế tiếp; thời gian thực hiện tỷ lệ với chiều cao cây.

<!-- thinking:end -->

Duyệt inorder trên binary search tree cho ra một dãy tăng dần, vì vậy ta có thể áp dụng phương pháp tìm kiếm nhị phân.

Node kế tiếp theo thứ tự inorder của node $p$ trong binary search tree thỏa mãn:

1. Giá trị của node kế tiếp theo thứ tự inorder lớn hơn giá trị của node $p$.
2. Trong tất cả node có giá trị lớn hơn $p$, node kế tiếp theo thứ tự inorder là node có giá trị nhỏ nhất.

Vì vậy, với node hiện tại $root$, nếu $root.val > p.val$ thì $root$ có thể là node kế tiếp theo thứ tự inorder của $p$. Ta lưu $root$ vào $ans$, rồi tìm tiếp ở cây con trái, tức là $root = root.left$. Nếu $root.val \leq p.val$ thì $root$ không thể là node kế tiếp theo thứ tự inorder của $p$, nên ta tìm ở cây con phải, tức là $root = root.right$.

Độ phức tạp thời gian là $O(h)$, với $h$ là chiều cao của binary search tree. Độ phức tạp không gian là $O(1)$.

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
    def inorderSuccessor(self, root: TreeNode, p: TreeNode) -> Optional[TreeNode]:
        ans = None
        while root:
            if root.val > p.val:
                ans = root
                root = root.left
            else:
                root = root.right
        return ans
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
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        TreeNode ans = null;
        while (root != null) {
            if (root.val > p.val) {
                ans = root;
                root = root.left;
            } else {
                root = root.right;
            }
        }
        return ans;
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
    TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
        TreeNode* ans = nullptr;
        while (root) {
            if (root->val > p->val) {
                ans = root;
                root = root->left;
            } else {
                root = root->right;
            }
        }
        return ans;
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
func inorderSuccessor(root *TreeNode, p *TreeNode) (ans *TreeNode) {
	for root != nil {
		if root.Val > p.Val {
			ans = root
			root = root.Left
		} else {
			root = root.Right
		}
	}
	return
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

function inorderSuccessor(root: TreeNode | null, p: TreeNode | null): TreeNode | null {
    let ans: TreeNode | null = null;
    while (root) {
        if (root.val > p.val) {
            ans = root;
            root = root.left;
        } else {
            root = root.right;
        }
    }
    return ans;
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
 * @return {TreeNode}
 */
var inorderSuccessor = function (root, p) {
    let ans = null;
    while (root) {
        if (root.val > p.val) {
            ans = root;
            root = root.left;
        } else {
            root = root.right;
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
