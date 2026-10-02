---
comments: true
difficulty: Medium
tags:
    - Tree
    - Binary Search Tree
    - Recursion
    - Binary Tree
---

<!-- problem:start -->

# [776. Split BST 🔒](https://leetcode.com/problems/split-bst)

[中文文档](/solution/0700-0799/0776.Split%20BST/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một binary search tree (BST) và số nguyên <code>target</code>. Hãy tách cây thành hai cây con sao cho cây thứ nhất chỉ chứa các node có giá trị nhỏ hơn hoặc bằng <code>target</code>, còn cây thứ hai chỉ chứa các node có giá trị lớn hơn <code>target</code>. Cây không nhất thiết phải có node mang giá trị <code>target</code>.</p>

<p>Ngoài ra, cần giữ lại phần lớn cấu trúc ban đầu của cây. Cụ thể, với một node con <code>c</code> có node cha <code>p</code> trong cây ban đầu, nếu sau khi tách cả hai vẫn thuộc cùng một cây con thì <code>c</code> vẫn phải có node cha là <code>p</code>.</p>

<p>Trả về <em>mảng chứa lần lượt root của hai cây con</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0776.Split%20BST/images/split-tree.jpg" style="width: 600px; height: 193px;" />
<pre>
<strong>Đầu vào:</strong> root = [4,2,6,1,3,5,7], target = 2
<strong>Đầu ra:</strong> [[2,1],[4,3,6,null,null,5,7]]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [1], target = 1
<strong>Đầu ra:</strong> [[1],[]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 50]</code>.</li>
	<li><code>0 &lt;= Node.val, target &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tách BST thành hai cây: một cây có giá trị $\le target$ và cây kia có giá trị $>target$; cả hai vẫn là BST. Cây có kích thước nhỏ nên có thể đệ quy dựa trên việc so sánh root với target.
>
> Nếu root $\le target$, toàn bộ cây con trái được giữ lại cùng root; ta tách cây con phải rồi gắn lại phần nhỏ hơn. Trường hợp còn lại xử lý đối xứng.
>
> Trả về root của hai cây con; nếu gặp null thì trả về hai giá trị null.

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
    def splitBST(
        self, root: Optional[TreeNode], target: int
    ) -> List[Optional[TreeNode]]:
        def dfs(root):
            if root is None:
                return [None, None]
            if root.val <= target:
                l, r = dfs(root.right)
                root.right = l
                return [root, r]
            else:
                l, r = dfs(root.left)
                root.left = r
                return [l, root]

        return dfs(root)
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
    private int t;

    public TreeNode[] splitBST(TreeNode root, int target) {
        t = target;
        return dfs(root);
    }

    private TreeNode[] dfs(TreeNode root) {
        if (root == null) {
            return new TreeNode[] {null, null};
        }
        if (root.val <= t) {
            TreeNode[] ans = dfs(root.right);
            root.right = ans[0];
            ans[0] = root;
            return ans;
        } else {
            TreeNode[] ans = dfs(root.left);
            root.left = ans[1];
            ans[1] = root;
            return ans;
        }
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
    int t;

    vector<TreeNode*> splitBST(TreeNode* root, int target) {
        t = target;
        return dfs(root);
    }

    vector<TreeNode*> dfs(TreeNode* root) {
        if (!root) return {nullptr, nullptr};
        if (root->val <= t) {
            auto ans = dfs(root->right);
            root->right = ans[0];
            ans[0] = root;
            return ans;
        } else {
            auto ans = dfs(root->left);
            root->left = ans[1];
            ans[1] = root;
            return ans;
        }
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
func splitBST(root *TreeNode, target int) []*TreeNode {
	if root == nil {
		return []*TreeNode{nil, nil}
	}
	if root.Val <= target {
		ans := splitBST(root.Right, target)
		root.Right = ans[0]
		ans[0] = root
		return ans
	} else {
		ans := splitBST(root.Left, target)
		root.Left = ans[1]
		ans[1] = root
		return ans
	}
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
 * @param {number} target
 * @return {TreeNode[]}
 */
var splitBST = function (root, target) {
    let ans = [null, null];
    if (!root) {
        return ans;
    }
    if (root.val <= target) {
        ans = splitBST(root.right, target);
        root.right = ans[0];
        ans[0] = root;
    } else {
        ans = splitBST(root.left, target);
        root.left = ans[1];
        ans[1] = root;
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
