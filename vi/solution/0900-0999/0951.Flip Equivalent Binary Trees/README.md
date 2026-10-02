---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [951. Flip Equivalent Binary Trees](https://leetcode.com/problems/flip-equivalent-binary-trees)

[中文文档](/solution/0900-0999/0951.Flip%20Equivalent%20Binary%20Trees/README.md)

## Mô tả

<!-- description:start -->

<p>Với binary tree <strong>T</strong>, ta định nghĩa <strong>thao tác lật</strong> như sau: chọn node bất kỳ rồi hoán đổi cây con trái và cây con phải.</p>

<p>Binary tree <strong>X</strong> <em>tương đương qua phép lật</em> với binary tree <strong>Y</strong> khi và chỉ khi ta có thể biến <strong>X</strong> thành <strong>Y</strong> bằng một số lần thực hiện thao tác lật.</p>

<p>Cho root của hai binary tree <code>root1</code> và <code>root2</code>, trả về <code>true</code> nếu hai cây tương đương qua phép lật, nếu không thì trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="Flipped Trees Diagram" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0951.Flip%20Equivalent%20Binary%20Trees/images/tree_ex.png" style="width: 500px; height: 220px;" />
<pre>
<strong>Đầu vào:</strong> root1 = [1,2,3,4,5,6,null,null,null,7,8], root2 = [1,3,2,null,6,4,5,null,null,null,null,8,7]
<strong>Đầu ra:</strong> true
<strong>Giải thích: </strong>Ta đã lật tại các node có giá trị 1, 3 và 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> root1 = [], root2 = []
<strong>Đầu ra:</strong> true
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> root1 = [], root2 = [1]
<strong>Đầu ra:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong mỗi cây nằm trong khoảng <code>[0, 100]</code>.</li>
	<li>Mỗi cây có các giá trị node <strong>không trùng nhau</strong> và nằm trong khoảng <code>[0, 99]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Xác định xem hai cây có thể trở nên giống hệt nhau sau khi lật các cây con hay không. Giá trị node không trùng nhau và mỗi cây có tối đa $100$ node. Root phải khớp; vì thao tác lật chỉ hoán đổi hai cây con của một node, đệ quy có thể so sánh cây con cùng thứ tự hoặc đổi chỗ, chỉ cần một trong hai cách thành công.

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
    def flipEquiv(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> bool:
        def dfs(root1, root2):
            if root1 == root2 or (root1 is None and root2 is None):
                return True
            if root1 is None or root2 is None or root1.val != root2.val:
                return False
            return (dfs(root1.left, root2.left) and dfs(root1.right, root2.right)) or (
                dfs(root1.left, root2.right) and dfs(root1.right, root2.left)
            )

        return dfs(root1, root2)
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
    public boolean flipEquiv(TreeNode root1, TreeNode root2) {
        return dfs(root1, root2);
    }

    private boolean dfs(TreeNode root1, TreeNode root2) {
        if (root1 == root2 || (root1 == null && root2 == null)) {
            return true;
        }
        if (root1 == null || root2 == null || root1.val != root2.val) {
            return false;
        }
        return (dfs(root1.left, root2.left) && dfs(root1.right, root2.right))
            || (dfs(root1.left, root2.right) && dfs(root1.right, root2.left));
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
    bool flipEquiv(TreeNode* root1, TreeNode* root2) {
        return dfs(root1, root2);
    }

    bool dfs(TreeNode* root1, TreeNode* root2) {
        if (root1 == root2 || (!root1 && !root2)) return true;
        if (!root1 || !root2 || root1->val != root2->val) return false;
        return (dfs(root1->left, root2->left) && dfs(root1->right, root2->right)) || (dfs(root1->left, root2->right) && dfs(root1->right, root2->left));
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
func flipEquiv(root1 *TreeNode, root2 *TreeNode) bool {
	var dfs func(root1, root2 *TreeNode) bool
	dfs = func(root1, root2 *TreeNode) bool {
		if root1 == root2 || (root1 == nil && root2 == nil) {
			return true
		}
		if root1 == nil || root2 == nil || root1.Val != root2.Val {
			return false
		}
		return (dfs(root1.Left, root2.Left) && dfs(root1.Right, root2.Right)) || (dfs(root1.Left, root2.Right) && dfs(root1.Right, root2.Left))
	}
	return dfs(root1, root2)
}
```

#### TypeScript

```ts
function flipEquiv(root1: TreeNode | null, root2: TreeNode | null): boolean {
    if (root1 === root2) return true;
    if (!root1 || !root2 || root1?.val !== root2?.val) return false;

    const { left: l1, right: r1 } = root1!;
    const { left: l2, right: r2 } = root2!;

    return (flipEquiv(l1, l2) && flipEquiv(r1, r2)) || (flipEquiv(l1, r2) && flipEquiv(r1, l2));
}
```

#### JavaScript

```js
function flipEquiv(root1, root2) {
    if (root1 === root2) return true;
    if (!root1 || !root2 || root1?.val !== root2?.val) return false;

    const { left: l1, right: r1 } = root1;
    const { left: l2, right: r2 } = root2;

    return (flipEquiv(l1, l2) && flipEquiv(r1, r2)) || (flipEquiv(l1, r2) && flipEquiv(r1, l2));
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
