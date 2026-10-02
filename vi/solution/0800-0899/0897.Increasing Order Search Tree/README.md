---
comments: true
difficulty: Easy
tags:
    - Stack
    - Tree
    - Depth-First Search
    - Binary Search Tree
    - Binary Tree
---

<!-- problem:start -->

# [897. Increasing Order Search Tree](https://leetcode.com/problems/increasing-order-search-tree)

[中文文档](/solution/0800-0899/0897.Increasing%20Order%20Search%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một binary search tree. Hãy sắp xếp lại cây theo thứ tự <strong>in-order</strong> sao cho node ngoài cùng bên trái trở thành root, và mỗi node không có node con trái, chỉ có duy nhất node con phải.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0897.Increasing%20Order%20Search%20Tree/images/ex1.jpg" style="width: 600px; height: 350px;" />
<pre>
<strong>Đầu vào:</strong> root = [5,3,6,2,4,null,8,1,null,null,null,7,9]
<strong>Đầu ra:</strong> [1,null,2,null,3,null,4,null,5,null,6,null,7,null,8,null,9]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0897.Increasing%20Order%20Search%20Tree/images/ex2.jpg" style="width: 300px; height: 114px;" />
<pre>
<strong>Đầu vào:</strong> root = [5,1,7]
<strong>Đầu ra:</strong> [1,null,5,null,7]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 100]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt DFS in-order

<!-- thinking:start -->

> **Tư duy**
>
> Chuyển BST thành danh sách tăng dần chỉ có node con phải. Duyệt inorder vốn đã cho thứ tự tăng dần, và cây có tối đa $100$ node, nên ta có thể nối lại các pointer ngay trong lúc duyệt.
>
> Dùng một node giả làm đầu danh sách. Khi thăm một node, nối node đó sau $\textit{prev}$, xóa node con trái của nó, rồi tiếp tục duyệt inorder. Kết quả là node con phải của node giả.

<!-- thinking:end -->

Ta tạo node giả $dummy$, ban đầu node con phải của $dummy$ trỏ đến root $root$, và pointer $prev$ trỏ đến $dummy$.

Ta duyệt binary search tree theo thứ tự in-order. Mỗi khi thăm một node, cho node con phải của $prev$ trỏ đến node đó, đặt node con trái của node hiện tại thành null, rồi gán node hiện tại cho $prev$ để dùng ở bước tiếp theo.

Sau khi duyệt xong, binary search tree ban đầu được biến đổi thành linked list đơn chỉ gồm các node con phải. Ta trả về node con phải của node giả $dummy$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node trong binary search tree.

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
    def increasingBST(self, root: TreeNode) -> TreeNode:
        def dfs(root):
            if root is None:
                return
            nonlocal prev
            dfs(root.left)
            prev.right = root
            root.left = None
            prev = root
            dfs(root.right)

        dummy = prev = TreeNode(right=root)
        dfs(root)
        return dummy.right
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
    private TreeNode prev;
    public TreeNode increasingBST(TreeNode root) {
        TreeNode dummy = new TreeNode(0, null, root);
        prev = dummy;
        dfs(root);
        return dummy.right;
    }

    private void dfs(TreeNode root) {
        if (root == null) {
            return;
        }
        dfs(root.left);
        prev.right = root;
        root.left = null;
        prev = root;
        dfs(root.right);
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
    TreeNode* increasingBST(TreeNode* root) {
        TreeNode* dummy = new TreeNode(0, nullptr, root);
        TreeNode* prev = dummy;
        function<void(TreeNode*)> dfs = [&](TreeNode* root) {
            if (!root) {
                return;
            }
            dfs(root->left);
            prev->right = root;
            root->left = nullptr;
            prev = root;
            dfs(root->right);
        };
        dfs(root);
        return dummy->right;
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
func increasingBST(root *TreeNode) *TreeNode {
	dummy := &TreeNode{Val: 0, Right: root}
	prev := dummy
	var dfs func(root *TreeNode)
	dfs = func(root *TreeNode) {
		if root == nil {
			return
		}
		dfs(root.Left)
		prev.Right = root
		root.Left = nil
		prev = root
		dfs(root.Right)
	}
	dfs(root)
	return dummy.Right
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

function increasingBST(root: TreeNode | null): TreeNode | null {
    const dummy = new TreeNode((right = root));
    let prev = dummy;
    const dfs = (root: TreeNode | null) => {
        if (!root) {
            return;
        }
        dfs(root.left);
        prev.right = root;
        root.left = null;
        prev = root;
        dfs(root.right);
    };
    dfs(root);
    return dummy.right;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
