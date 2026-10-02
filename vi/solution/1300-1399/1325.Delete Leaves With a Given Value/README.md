---
comments: true
difficulty: Medium
rating: 1407
source: Weekly Contest 172 Q3
tags:
    - Tree
    - Depth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [1325. Delete Leaves With a Given Value](https://leetcode.com/problems/delete-leaves-with-a-given-value)

[中文文档](/solution/1300-1399/1325.Delete%20Leaves%20With%20a%20Given%20Value/README.md)

## Mô tả

<!-- description:start -->

<p>Cho binary tree <code>root</code> và số nguyên <code>target</code>, hãy xóa tất cả <strong>leaf node</strong> có giá trị bằng <code>target</code>.</p>

<p>Lưu ý, sau khi xóa một leaf node có giá trị <code>target</code><strong>, </strong>nếu parent của nó trở thành leaf node và có giá trị <code>target</code>, thì cũng cần xóa node đó (tiếp tục như vậy cho đến khi không thể xóa thêm).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1325.Delete%20Leaves%20With%20a%20Given%20Value/images/sample_1_1684.png" style="width: 500px; height: 112px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> root = [1,2,3,2,null,2,4], target = 2
<strong>Đầu ra:</strong> [1,null,3,null,4]
<strong>Giải thích:</strong> Các leaf node màu xanh có giá trị (target = 2) được xóa (hình bên trái). 
Sau khi xóa, các node mới trở thành leaf node có giá trị (target = 2) (hình ở giữa).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1325.Delete%20Leaves%20With%20a%20Given%20Value/images/sample_2_1684.png" style="width: 400px; height: 154px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> root = [1,3,3,3,2], target = 3
<strong>Đầu ra:</strong> [1,3,null,null,2]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1325.Delete%20Leaves%20With%20a%20Given%20Value/images/sample_3_1684.png" style="width: 500px; height: 166px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> root = [1,2,null,2,null,2], target = 2
<strong>Đầu ra:</strong> [1]
<strong>Giải thích:</strong> Ở mỗi bước, các leaf node màu xanh có giá trị (target = 2) đều bị xóa.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 3000]</code>.</li>
	<li><code>1 &lt;= Node.val, target &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cần xóa các leaf node có giá trị bằng $\textit{target}$, kể cả những node chỉ trở thành lá sau khi node con bị xóa. Nếu kiểm tra preorder, ta có thể bỏ sót node chỉ trở thành lá sau khi các node con biến mất. Postorder giải quyết điều này: đệ quy xuống cả hai node con, rồi xóa node hiện tại nếu nó đã là leaf node có giá trị $target$. Một lượt duyệt xử lý toàn bộ quá trình này.

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
    def removeLeafNodes(
        self, root: Optional[TreeNode], target: int
    ) -> Optional[TreeNode]:
        if root is None:
            return None
        root.left = self.removeLeafNodes(root.left, target)
        root.right = self.removeLeafNodes(root.right, target)
        if root.left is None and root.right is None and root.val == target:
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
    public TreeNode removeLeafNodes(TreeNode root, int target) {
        if (root == null) {
            return null;
        }
        root.left = removeLeafNodes(root.left, target);
        root.right = removeLeafNodes(root.right, target);
        if (root.left == null && root.right == null && root.val == target) {
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
    TreeNode* removeLeafNodes(TreeNode* root, int target) {
        if (!root) {
            return nullptr;
        }
        root->left = removeLeafNodes(root->left, target);
        root->right = removeLeafNodes(root->right, target);
        if (!root->left && !root->right && root->val == target) {
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
func removeLeafNodes(root *TreeNode, target int) *TreeNode {
	if root == nil {
		return nil
	}
	root.Left = removeLeafNodes(root.Left, target)
	root.Right = removeLeafNodes(root.Right, target)
	if root.Left == nil && root.Right == nil && root.Val == target {
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

function removeLeafNodes(root: TreeNode | null, target: number): TreeNode | null {
    if (!root) {
        return null;
    }
    root.left = removeLeafNodes(root.left, target);
    root.right = removeLeafNodes(root.right, target);
    if (!root.left && !root.right && root.val == target) {
        return null;
    }
    return root;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
