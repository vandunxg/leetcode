---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Binary Tree
    - Tree DP
---

<!-- problem:start -->

# [298. Binary Tree Longest Consecutive Sequence 🔒](https://leetcode.com/problems/binary-tree-longest-consecutive-sequence)

[中文文档](/solution/0200-0299/0298.Binary%20Tree%20Longest%20Consecutive%20Sequence/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một binary tree. Hãy trả về <em>độ dài của <strong>đường đi có dãy liên tiếp</strong> dài nhất</em>.</p>

<p><strong>Đường đi có dãy liên tiếp</strong> là đường đi mà giá trị tại mỗi node tăng thêm <strong>1</strong> so với node trước đó.</p>

<p>Lưu ý, đường đi có thể bắt đầu <strong>tại bất kỳ node nào</strong> trong cây, và không được đi từ node lên parent.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0200-0299/0298.Binary%20Tree%20Longest%20Consecutive%20Sequence/images/consec1-1-tree.jpg" style="width: 306px; height: 400px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,null,3,2,4,null,null,null,5]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Đường đi có dãy liên tiếp dài nhất là 3-4-5, nên trả về 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0200-0299/0298.Binary%20Tree%20Longest%20Consecutive%20Sequence/images/consec1-2-tree.jpg" style="width: 249px; height: 400px;" />
<pre>
<strong>Đầu vào:</strong> root = [2,null,3,2,null,1]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Đường đi có dãy liên tiếp dài nhất là 2-3, không phải 3-2-1, nên trả về 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 3 * 10<sup>4</sup>]</code>.</li>
	<li><code>-3 * 10<sup>4</sup> &lt;= Node.val &lt;= 3 * 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đường đi liên tiếp là một chuỗi node đi xuống, trong đó các giá trị tăng thêm $1$. Duyệt hậu tự cho ta độ dài tương ứng từ mỗi node con; ta chỉ nối dài khi giá trị node con lớn hơn đúng 1, nếu không thì bắt đầu lại từ $1$.
>
> $dfs$ trả về độ dài lớn nhất của dãy liên tiếp bắt đầu từ node hiện tại, đồng thời cập nhật đáp án toàn cục.

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
    def longestConsecutive(self, root: Optional[TreeNode]) -> int:
        def dfs(root: Optional[TreeNode]) -> int:
            if root is None:
                return 0
            l = dfs(root.left) + 1
            r = dfs(root.right) + 1
            if root.left and root.left.val - root.val != 1:
                l = 1
            if root.right and root.right.val - root.val != 1:
                r = 1
            t = max(l, r)
            nonlocal ans
            ans = max(ans, t)
            return t

        ans = 0
        dfs(root)
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
    private int ans;

    public int longestConsecutive(TreeNode root) {
        dfs(root);
        return ans;
    }

    private int dfs(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int l = dfs(root.left) + 1;
        int r = dfs(root.right) + 1;
        if (root.left != null && root.left.val - root.val != 1) {
            l = 1;
        }
        if (root.right != null && root.right.val - root.val != 1) {
            r = 1;
        }
        int t = Math.max(l, r);
        ans = Math.max(ans, t);
        return t;
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
    int longestConsecutive(TreeNode* root) {
        int ans = 0;
        function<int(TreeNode*)> dfs = [&](TreeNode* root) {
            if (!root) {
                return 0;
            }
            int l = dfs(root->left) + 1;
            int r = dfs(root->right) + 1;
            if (root->left && root->left->val - root->val != 1) {
                l = 1;
            }
            if (root->right && root->right->val - root->val != 1) {
                r = 1;
            }
            int t = max(l, r);
            ans = max(ans, t);
            return t;
        };
        dfs(root);
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
func longestConsecutive(root *TreeNode) (ans int) {
	var dfs func(*TreeNode) int
	dfs = func(root *TreeNode) int {
		if root == nil {
			return 0
		}
		l := dfs(root.Left) + 1
		r := dfs(root.Right) + 1
		if root.Left != nil && root.Left.Val-root.Val != 1 {
			l = 1
		}
		if root.Right != nil && root.Right.Val-root.Val != 1 {
			r = 1
		}
		t := max(l, r)
		ans = max(ans, t)
		return t
	}
	dfs(root)
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

function longestConsecutive(root: TreeNode | null): number {
    let ans = 0;
    const dfs = (root: TreeNode | null): number => {
        if (root === null) {
            return 0;
        }
        let l = dfs(root.left) + 1;
        let r = dfs(root.right) + 1;
        if (root.left && root.left.val - root.val !== 1) {
            l = 1;
        }
        if (root.right && root.right.val - root.val !== 1) {
            r = 1;
        }
        const t = Math.max(l, r);
        ans = Math.max(ans, t);
        return t;
    };
    dfs(root);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
