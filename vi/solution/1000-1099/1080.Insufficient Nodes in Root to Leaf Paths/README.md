---
comments: true
difficulty: Medium
rating: 1804
source: Weekly Contest 140 Q3
tags:
    - Tree
    - Depth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [1080. Insufficient Nodes in Root to Leaf Paths](https://leetcode.com/problems/insufficient-nodes-in-root-to-leaf-paths)

[中文文档](/solution/1000-1099/1080.Insufficient%20Nodes%20in%20Root%20to%20Leaf%20Paths/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một cây nhị phân và số nguyên <code>limit</code>, hãy đồng thời xóa tất cả <strong>node không đủ điều kiện</strong> trong cây và trả về <em>root của cây nhị phân sau khi xóa</em>.</p>

<p>Một node được gọi là <strong>không đủ điều kiện</strong> nếu mọi đường đi từ root đến <strong>leaf</strong> đi qua node đó đều có tổng nhỏ hơn nghiêm ngặt <code>limit</code>.</p>

<p><strong>Leaf</strong> là node không có node con.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1080.Insufficient%20Nodes%20in%20Root%20to%20Leaf%20Paths/images/insufficient-11.png" style="width: 500px; height: 207px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,2,3,4,-99,-99,7,8,9,-99,-99,12,13,-99,14], limit = 1
<strong>Đầu ra:</strong> [1,2,3,4,null,null,7,8,9,null,14]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1080.Insufficient%20Nodes%20in%20Root%20to%20Leaf%20Paths/images/insufficient-3.png" style="width: 400px; height: 274px;" />
<pre>
<strong>Đầu vào:</strong> root = [5,4,8,11,null,17,4,7,1,null,null,5,3], limit = 22
<strong>Đầu ra:</strong> [5,4,8,11,null,17,4,7,null,null,null,5]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1080.Insufficient%20Nodes%20in%20Root%20to%20Leaf%20Paths/images/screen-shot-2019-06-11-at-83301-pm.png" style="width: 250px; height: 199px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,2,-3,-5,null,4,null], limit = -1
<strong>Đầu ra:</strong> [1,null,-3,4]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 5000]</code>.</li>
	<li><code>-10<sup>5</sup> &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= limit &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cần xóa các node chỉ nằm trên những đường đi từ root đến leaf có tổng nhỏ hơn $\textit{limit}$. Ta chỉ xác định được một đường đi có đạt điều kiện tại leaf; node cha chỉ được giữ lại nếu còn ít nhất một node con sống sót. Với $n\le 5000$, một lượt duyệt postorder là đủ.
>
> Khi đệ quy xuống cây, ta trừ giá trị node hiện tại khỏi $\textit{limit}$. Xóa leaf nếu phần còn lại vẫn dương. Node nội bộ chỉ bị xóa khi cả hai node con đều đã trở thành null.
>
> Mỗi lời gọi trả về root mới của cây con, có thể là null.

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
    def sufficientSubset(
        self, root: Optional[TreeNode], limit: int
    ) -> Optional[TreeNode]:
        if root is None:
            return None
        limit -= root.val
        if root.left is None and root.right is None:
            return None if limit > 0 else root
        root.left = self.sufficientSubset(root.left, limit)
        root.right = self.sufficientSubset(root.right, limit)
        return None if root.left is None and root.right is None else root
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
    public TreeNode sufficientSubset(TreeNode root, int limit) {
        if (root == null) {
            return null;
        }
        limit -= root.val;
        if (root.left == null && root.right == null) {
            return limit > 0 ? null : root;
        }
        root.left = sufficientSubset(root.left, limit);
        root.right = sufficientSubset(root.right, limit);
        return root.left == null && root.right == null ? null : root;
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
    TreeNode* sufficientSubset(TreeNode* root, int limit) {
        if (!root) {
            return nullptr;
        }
        limit -= root->val;
        if (!root->left && !root->right) {
            return limit > 0 ? nullptr : root;
        }
        root->left = sufficientSubset(root->left, limit);
        root->right = sufficientSubset(root->right, limit);
        return !root->left && !root->right ? nullptr : root;
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
func sufficientSubset(root *TreeNode, limit int) *TreeNode {
	if root == nil {
		return nil
	}

	limit -= root.Val
	if root.Left == nil && root.Right == nil {
		if limit > 0 {
			return nil
		}
		return root
	}

	root.Left = sufficientSubset(root.Left, limit)
	root.Right = sufficientSubset(root.Right, limit)

	if root.Left == nil && root.Right == nil {
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

function sufficientSubset(root: TreeNode | null, limit: number): TreeNode | null {
    if (root === null) {
        return null;
    }
    limit -= root.val;
    if (root.left === null && root.right === null) {
        return limit > 0 ? null : root;
    }
    root.left = sufficientSubset(root.left, limit);
    root.right = sufficientSubset(root.right, limit);
    return root.left === null && root.right === null ? null : root;
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
 * @param {number} limit
 * @return {TreeNode}
 */
var sufficientSubset = function (root, limit) {
    if (root === null) {
        return null;
    }
    limit -= root.val;
    if (root.left === null && root.right === null) {
        return limit > 0 ? null : root;
    }
    root.left = sufficientSubset(root.left, limit);
    root.right = sufficientSubset(root.right, limit);
    return root.left === null && root.right === null ? null : root;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
