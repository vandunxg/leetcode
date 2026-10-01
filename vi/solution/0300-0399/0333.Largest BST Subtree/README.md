---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Binary Search Tree
    - Dynamic Programming
    - Binary Tree
    - Tree DP
---

<!-- problem:start -->

# [333. Largest BST Subtree 🔒](https://leetcode.com/problems/largest-bst-subtree)

[中文文档](/solution/0300-0399/0333.Largest%20BST%20Subtree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho root của một binary tree, hãy tìm <span data-keyword="subtree">subtree</span> lớn nhất cũng là Binary Search Tree (BST); lớn nhất ở đây nghĩa là subtree có nhiều node nhất.</p>

<p><strong>Binary Search Tree (BST)</strong> là cây mà mọi node đều thỏa các tính chất sau:</p>

<ul>
	<li>Giá trị trong left subtree nhỏ hơn giá trị của node cha (root).</li>
	<li>Giá trị trong right subtree lớn hơn giá trị của node cha (root).</li>
</ul>

<p><strong>Lưu ý:</strong> Một subtree phải bao gồm tất cả node hậu duệ của nó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0333.Largest%20BST%20Subtree/images/tmp.jpg" style="width: 571px; height: 302px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> root = [10,5,15,1,8,null,7]
<strong>Đầu ra:</strong> 3
<strong>Giải thích: </strong>Trong ví dụ này, BST subtree lớn nhất là phần được tô sáng. Giá trị trả về là kích thước của subtree, bằng 3.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [4,2,7,2,3,5,null,2,null,null,null,null,null,1]
<strong>Đầu ra:</strong> 2
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[0, 10<sup>4</sup>]</code>.</li>
	<li><code>-10<sup>4</sup> &lt;= Node.val &lt;= 10<sup>4</sup></code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể tìm cách giải với độ phức tạp thời gian <code>O(n)</code> không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm kích thước của BST subtree lớn nhất. Nếu kiểm tra riêng từng node thì sẽ lặp lại công việc. Một subtree là BST khi cả hai subtree con đều là BST và giá trị root nằm giữa hai giá trị biên của chúng.
>
> Duyệt postorder và trả về $(\min,\max,size)$. Nếu left-max $<$ root $<$ right-min, ta gộp kích thước hai bên và cập nhật đáp án; nếu không, trả về các sentinel $(-\infty,\infty,0)$ để các node tổ tiên không thể gộp subtree này. Cây rỗng trả về $(\infty,-\infty,0)$ để một node lá vẫn thỏa điều kiện.

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
    def largestBSTSubtree(self, root: Optional[TreeNode]) -> int:
        def dfs(root):
            if root is None:
                return inf, -inf, 0
            lmi, lmx, ln = dfs(root.left)
            rmi, rmx, rn = dfs(root.right)
            nonlocal ans
            if lmx < root.val < rmi:
                ans = max(ans, ln + rn + 1)
                return min(lmi, root.val), max(rmx, root.val), ln + rn + 1
            return -inf, inf, 0

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

    public int largestBSTSubtree(TreeNode root) {
        ans = 0;
        dfs(root);
        return ans;
    }

    private int[] dfs(TreeNode root) {
        if (root == null) {
            return new int[] {Integer.MAX_VALUE, Integer.MIN_VALUE, 0};
        }
        int[] left = dfs(root.left);
        int[] right = dfs(root.right);
        if (left[1] < root.val && root.val < right[0]) {
            ans = Math.max(ans, left[2] + right[2] + 1);
            return new int[] {
                Math.min(root.val, left[0]), Math.max(root.val, right[1]), left[2] + right[2] + 1};
        }
        return new int[] {Integer.MIN_VALUE, Integer.MAX_VALUE, 0};
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
    int ans;

    int largestBSTSubtree(TreeNode* root) {
        ans = 0;
        dfs(root);
        return ans;
    }

    vector<int> dfs(TreeNode* root) {
        if (!root) return {INT_MAX, INT_MIN, 0};
        auto left = dfs(root->left);
        auto right = dfs(root->right);
        if (left[1] < root->val && root->val < right[0]) {
            ans = max(ans, left[2] + right[2] + 1);
            return {min(root->val, left[0]), max(root->val, right[1]), left[2] + right[2] + 1};
        }
        return {INT_MIN, INT_MAX, 0};
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
func largestBSTSubtree(root *TreeNode) int {
	ans := 0
	var dfs func(root *TreeNode) []int
	dfs = func(root *TreeNode) []int {
		if root == nil {
			return []int{math.MaxInt32, math.MinInt32, 0}
		}
		left := dfs(root.Left)
		right := dfs(root.Right)
		if left[1] < root.Val && root.Val < right[0] {
			ans = max(ans, left[2]+right[2]+1)
			return []int{min(root.Val, left[0]), max(root.Val, right[1]), left[2] + right[2] + 1}
		}
		return []int{math.MinInt32, math.MaxInt32, 0}
	}
	dfs(root)
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
