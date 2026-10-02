---
comments: true
difficulty: Easy
tags:
    - Tree
    - Binary Search Tree
    - Binary Tree
---

<!-- problem:start -->

# [700. Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree)

[中文文档](/solution/0700-0799/0700.Search%20in%20a%20Binary%20Search%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một binary search tree (BST) và số nguyên <code>val</code>.</p>

<p>Tìm node trong BST có giá trị bằng <code>val</code> và trả về subtree có gốc là node đó. Nếu không có node như vậy, trả về <code>null</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0700.Search%20in%20a%20Binary%20Search%20Tree/images/tree1.jpg" style="width: 422px; height: 302px;" />
<pre>
<strong>Đầu vào:</strong> root = [4,2,7,1,3], val = 2
<strong>Đầu ra:</strong> [2,1,3]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0700.Search%20in%20a%20Binary%20Search%20Tree/images/tree2.jpg" style="width: 422px; height: 302px;" />
<pre>
<strong>Đầu vào:</strong> root = [4,2,7,1,3], val = 5
<strong>Đầu ra:</strong> []
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 5000]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 10<sup>7</sup></code></li>
	<li><code>root</code> là một binary search tree.</li>
	<li><code>1 &lt;= val &lt;= 10<sup>7</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần subtree có gốc tại node mang giá trị $\textit{val}$. Vì $n \le 5000$, có thể duyệt toàn bộ cây, nhưng cách đó không tận dụng thứ tự của BST.
>
> Nếu giá trị hiện tại bằng giá trị cần tìm thì node đó là đáp án; nếu lớn hơn, giá trị cần tìm chỉ có thể nằm ở subtree bên trái, còn lại chỉ có thể ở bên phải. Nhờ vậy, mỗi lần so sánh ta loại bỏ được toàn bộ một phía.
>
> Đệ quy phù hợp với cách duyệt này: trả về node hiện tại nếu node rỗng hoặc tìm thấy giá trị, nếu không thì tiếp tục đi xuống trái hoặc phải. Nếu cây suy biến thành một chuỗi, độ phức tạp thời gian và bộ nhớ stack vẫn là $O(n)$.

<!-- thinking:end -->

Ta kiểm tra node hiện tại có null hay giá trị của nó bằng giá trị cần tìm không. Nếu đúng, trả về node hiện tại.

Nếu không, khi giá trị node hiện tại lớn hơn giá trị cần tìm, ta đệ quy tìm ở subtree bên trái; ngược lại, tìm ở subtree bên phải.

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
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if root is None or root.val == val:
            return root
        return (
            self.searchBST(root.left, val)
            if root.val > val
            else self.searchBST(root.right, val)
        )
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
    public TreeNode searchBST(TreeNode root, int val) {
        if (root == null || root.val == val) {
            return root;
        }
        return root.val > val ? searchBST(root.left, val) : searchBST(root.right, val);
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
    TreeNode* searchBST(TreeNode* root, int val) {
        if (!root || root->val == val) {
            return root;
        }
        return root->val > val ? searchBST(root->left, val) : searchBST(root->right, val);
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
 func searchBST(root *TreeNode, val int) *TreeNode {
    if root == nil || root.Val == val {
        return root
    }
    if root.Val > val {
        return searchBST(root.Left, val)
    }
    return searchBST(root.Right, val)
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

function searchBST(root: TreeNode | null, val: number): TreeNode | null {
    if (root === null || root.val === val) {
        return root;
    }
    return root.val > val ? searchBST(root.left, val) : searchBST(root.right, val);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
