---
comments: true
difficulty: Medium
rating: 1713
source: Biweekly Contest 21 Q3
tags:
    - Tree
    - Depth-First Search
    - Dynamic Programming
    - Binary Tree
    - Tree DP
---

<!-- problem:start -->

# [1372. Longest ZigZag Path in a Binary Tree](https://leetcode.com/problems/longest-zigzag-path-in-a-binary-tree)

[中文文档](/solution/1300-1399/1372.Longest%20ZigZag%20Path%20in%20a%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một binary tree.</p>

<p>Đường đi ZigZag trong binary tree được định nghĩa như sau:</p>

<ul>
	<li>Chọn <strong>bất kỳ </strong>node nào trong binary tree và một hướng đi (phải hoặc trái).</li>
	<li>Nếu hướng hiện tại là phải, đi đến node con bên phải; nếu không, đi đến node con bên trái.</li>
	<li>Đổi hướng từ phải sang trái hoặc từ trái sang phải.</li>
	<li>Lặp lại bước thứ hai và thứ ba cho đến khi không thể đi tiếp trong cây.</li>
</ul>

<p>Độ dài ZigZag được tính bằng số node đã đi qua trừ 1. (Đường đi chỉ gồm một node có độ dài 0).</p>

<p>Trả về <em>đường đi <strong>ZigZag</strong> dài nhất trong cây</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1372.Longest%20ZigZag%20Path%20in%20a%20Binary%20Tree/images/sample_1_1702.png" style="width: 221px; height: 383px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,null,1,1,1,null,null,1,1,null,1,null,null,null,1]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Đường đi ZigZag dài nhất đi qua các node màu xanh (phải -&gt; trái -&gt; phải).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1372.Longest%20ZigZag%20Path%20in%20a%20Binary%20Tree/images/sample_2_1702.png" style="width: 157px; height: 329px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,1,1,null,1,null,null,1,1,null,1]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Đường đi ZigZag dài nhất đi qua các node màu xanh (trái -&gt; phải -&gt; trái -&gt; phải).
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [1]
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 5 * 10<sup>4</sup>]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đường đi ZigZag phải luân phiên trái và phải khi đi xuống. Nếu bắt đầu duyệt lại từ mọi node và mọi hướng, ta sẽ xử lý lặp các cây con. DFS truyền theo độ dài hiện có nếu bước trước đi sang trái ($l$) hoặc phải ($r$). Khi đi sang node con trái, tiếp tục với $r+1$ và đặt lại độ dài hướng phải; với node con phải thì xử lý đối xứng. Ta lưu giá trị lớn nhất toàn cục.

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
    def longestZigZag(self, root: TreeNode) -> int:
        def dfs(root, l, r):
            if root is None:
                return
            nonlocal ans
            ans = max(ans, l, r)
            dfs(root.left, r + 1, 0)
            dfs(root.right, 0, l + 1)

        ans = 0
        dfs(root, 0, 0)
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

    public int longestZigZag(TreeNode root) {
        dfs(root, 0, 0);
        return ans;
    }

    private void dfs(TreeNode root, int l, int r) {
        if (root == null) {
            return;
        }
        ans = Math.max(ans, Math.max(l, r));
        dfs(root.left, r + 1, 0);
        dfs(root.right, 0, l + 1);
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
    int ans = 0;

    int longestZigZag(TreeNode* root) {
        dfs(root, 0, 0);
        return ans;
    }

    void dfs(TreeNode* root, int l, int r) {
        if (!root) return;
        ans = max(ans, max(l, r));
        dfs(root->left, r + 1, 0);
        dfs(root->right, 0, l + 1);
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
func longestZigZag(root *TreeNode) int {
	ans := 0
	var dfs func(root *TreeNode, l, r int)
	dfs = func(root *TreeNode, l, r int) {
		if root == nil {
			return
		}
		ans = max(ans, max(l, r))
		dfs(root.Left, r+1, 0)
		dfs(root.Right, 0, l+1)
	}
	dfs(root, 0, 0)
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
