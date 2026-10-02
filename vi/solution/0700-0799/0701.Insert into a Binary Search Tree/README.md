---
comments: true
difficulty: Medium
tags:
    - Tree
    - Binary Search Tree
    - Binary Tree
---

<!-- problem:start -->

# [701. Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree)

[中文文档](/solution/0700-0799/0701.Insert%20into%20a%20Binary%20Search%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho node <code>root</code> của một binary search tree (BST) và một <code>value</code> cần chèn vào cây. Hãy trả về <em>node root của BST sau khi chèn</em>. Đảm bảo giá trị mới chưa tồn tại trong BST ban đầu.</p>

<p><strong>Lưu ý</strong>&nbsp;rằng có thể có&nbsp;nhiều cách chèn hợp lệ, miễn là cây vẫn là BST sau khi chèn. Bạn có thể trả về <strong>bất kỳ cách nào trong số đó</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0701.Insert%20into%20a%20Binary%20Search%20Tree/images/insertbst.jpg" style="width: 752px; height: 221px;" />
<pre>
<strong>Đầu vào:</strong> root = [4,2,7,1,3], val = 5
<strong>Đầu ra:</strong> [4,2,7,1,3,5]
<strong>Giải thích:</strong> Cây dưới đây cũng là đáp án hợp lệ:
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0701.Insert%20into%20a%20Binary%20Search%20Tree/images/bst.jpg" style="width: 352px; height: 301px;" />
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [40,20,60,10,30,50,70], val = 25
<strong>Đầu ra:</strong> [40,20,60,10,30,50,70,null,null,25]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [4,2,7,1,3,null,null,null,null,null,null], val = 5
<strong>Đầu ra:</strong> [4,2,7,1,3,5]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[0,&nbsp;10<sup>4</sup>]</code>.</li>
	<li><code>-10<sup>8</sup> &lt;= Node.val &lt;= 10<sup>8</sup></code></li>
	<li>Mọi giá trị <code>Node.val</code> đều <strong>duy nhất</strong>.</li>
	<li><code>-10<sup>8</sup> &lt;= val &lt;= 10<sup>8</sup></code></li>
	<li>Đảm bảo <code>val</code> <strong>chưa tồn tại</strong> trong BST ban đầu.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Chèn một giá trị mới vào BST và trả về root mới. Với $n \le 10^4$, có thể dùng bất kỳ đường chèn đúng nào.
>
> Quy tắc thứ tự của BST xác định duy nhất vị trí trống: so sánh rồi đi xuống cây cho đến khi tìm thấy vị trí để gắn node mới; không cần xoay cây. Nếu cây rỗng, node mới sẽ là node duy nhất.
>
> Đệ quy vào cây con trái hoặc phải tùy theo $\textit{val}$, rồi gắn cây con được trả về. Nếu cây suy biến thành một chuỗi, thời gian và stack đều tốn $O(n)$.

<!-- thinking:end -->

Nếu node root là null, tạo node mới có giá trị $\textit{val}$ rồi trả về node đó.

Nếu giá trị của root lớn hơn $\textit{val}$, đệ quy chèn $\textit{val}$ vào cây con trái rồi cập nhật root của cây con trái bằng node root được trả về.

Nếu giá trị của root nhỏ hơn $\textit{val}$, đệ quy chèn $\textit{val}$ vào cây con phải rồi cập nhật root của cây con phải bằng node root được trả về.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node trong cây nhị phân.

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
    def insertIntoBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if root is None:
            return TreeNode(val)
        if root.val > val:
            root.left = self.insertIntoBST(root.left, val)
        else:
            root.right = self.insertIntoBST(root.right, val)
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
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if (root == null) {
            return new TreeNode(val);
        }
        if (root.val > val) {
            root.left = insertIntoBST(root.left, val);
        } else {
            root.right = insertIntoBST(root.right, val);
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
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        if (!root) {
            return new TreeNode(val);
        }
        if (root->val > val) {
            root->left = insertIntoBST(root->left, val);
        } else {
            root->right = insertIntoBST(root->right, val);
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
func insertIntoBST(root *TreeNode, val int) *TreeNode {
	if root == nil {
		return &TreeNode{Val: val}
	}
	if root.Val > val {
		root.Left = insertIntoBST(root.Left, val)
	} else {
		root.Right = insertIntoBST(root.Right, val)
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

function insertIntoBST(root: TreeNode | null, val: number): TreeNode | null {
    if (!root) {
        return new TreeNode(val);
    }
    if (root.val > val) {
        root.left = insertIntoBST(root.left, val);
    } else {
        root.right = insertIntoBST(root.right, val);
    }
    return root;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
