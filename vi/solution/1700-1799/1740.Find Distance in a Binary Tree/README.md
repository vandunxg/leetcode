---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Hash Table
    - Binary Tree
    - Lowest Common Ancestor
    - Binary Lifting
---

<!-- problem:start -->

# [1740. Find Distance in a Binary Tree 🔒](https://leetcode.com/problems/find-distance-in-a-binary-tree)

[中文文档](/solution/1700-1799/1740.Find%20Distance%20in%20a%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho root của một cây nhị phân và hai số nguyên <code>p</code> và <code>q</code>, hãy trả về <em><strong>khoảng cách</strong> giữa node có giá trị </em><code>p</code><em> và node có giá trị </em><code>q</code><em> trong cây</em>.</p>

<p><strong>Khoảng cách</strong> giữa hai node là số cạnh trên đường đi từ node này đến node kia.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1700-1799/1740.Find%20Distance%20in%20a%20Binary%20Tree/images/binarytree.png" />
<pre>
<strong>Đầu vào:</strong> root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 0
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có 3 cạnh giữa 5 và 0: 5-3-1-0.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1700-1799/1740.Find%20Distance%20in%20a%20Binary%20Tree/images/binarytree.png" />
<pre>
<strong>Đầu vào:</strong> root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 7
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có 2 cạnh giữa 5 và 7: 5-2-7.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1700-1799/1740.Find%20Distance%20in%20a%20Binary%20Tree/images/binarytree.png" />
<pre>
<strong>Đầu vào:</strong> root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 5
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Khoảng cách giữa một node và chính nó là 0.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 10<sup>4</sup>]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 10<sup>9</sup></code></li>
	<li>All <code>Node.val</code> are <strong>unique</strong>.</li>
	<li><code>p</code> và <code>q</code> là các giá trị trong cây.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Khoảng cách giữa hai node là tổng độ sâu của chúng tính từ LCA. Các giá trị là duy nhất, nên ta tìm LCA rồi đo độ sâu.
>
> $\textit{lca}$ trả về $p$, $q$ hoặc null; khi cả hai node con đều khác rỗng thì đó là điểm rẽ. DFS từ node này đến $p$ và đến $q$, rồi cộng hai độ sâu.

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
    def findDistance(self, root: Optional[TreeNode], p: int, q: int) -> int:
        def lca(root, p, q):
            if root is None or root.val in [p, q]:
                return root
            left = lca(root.left, p, q)
            right = lca(root.right, p, q)
            if left is None:
                return right
            if right is None:
                return left
            return root

        def dfs(root, v):
            if root is None:
                return -1
            if root.val == v:
                return 0
            left, right = dfs(root.left, v), dfs(root.right, v)
            if left == right == -1:
                return -1
            return 1 + max(left, right)

        g = lca(root, p, q)
        return dfs(g, p) + dfs(g, q)
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
    public int findDistance(TreeNode root, int p, int q) {
        TreeNode g = lca(root, p, q);
        return dfs(g, p) + dfs(g, q);
    }

    private int dfs(TreeNode root, int v) {
        if (root == null) {
            return -1;
        }
        if (root.val == v) {
            return 0;
        }
        int left = dfs(root.left, v);
        int right = dfs(root.right, v);
        if (left == -1 && right == -1) {
            return -1;
        }
        return 1 + Math.max(left, right);
    }

    private TreeNode lca(TreeNode root, int p, int q) {
        if (root == null || root.val == p || root.val == q) {
            return root;
        }
        TreeNode left = lca(root.left, p, q);
        TreeNode right = lca(root.right, p, q);
        if (left == null) {
            return right;
        }
        if (right == null) {
            return left;
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
    int findDistance(TreeNode* root, int p, int q) {
        TreeNode* g = lca(root, p, q);
        return dfs(g, p) + dfs(g, q);
    }

    TreeNode* lca(TreeNode* root, int p, int q) {
        if (!root || root->val == p || root->val == q) return root;
        TreeNode* left = lca(root->left, p, q);
        TreeNode* right = lca(root->right, p, q);
        if (!left) return right;
        if (!right) return left;
        return root;
    }

    int dfs(TreeNode* root, int v) {
        if (!root) return -1;
        if (root->val == v) return 0;
        int left = dfs(root->left, v);
        int right = dfs(root->right, v);
        if (left == -1 && right == -1) return -1;
        return 1 + max(left, right);
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
func findDistance(root *TreeNode, p int, q int) int {
	var lca func(root *TreeNode, p int, q int) *TreeNode
	lca = func(root *TreeNode, p int, q int) *TreeNode {
		if root == nil || root.Val == p || root.Val == q {
			return root
		}
		left, right := lca(root.Left, p, q), lca(root.Right, p, q)
		if left == nil {
			return right
		}
		if right == nil {
			return left
		}
		return root
	}
	var dfs func(root *TreeNode, v int) int
	dfs = func(root *TreeNode, v int) int {
		if root == nil {
			return -1
		}
		if root.Val == v {
			return 0
		}
		left, right := dfs(root.Left, v), dfs(root.Right, v)
		if left == -1 && right == -1 {
			return -1
		}
		return 1 + max(left, right)
	}
	g := lca(root, p, q)
	return dfs(g, p) + dfs(g, q)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
