---
comments: true
difficulty: Easy
tags:
    - Tree
    - Depth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [671. Second Minimum Node In a Binary Tree](https://leetcode.com/problems/second-minimum-node-in-a-binary-tree)

[中文文档](/solution/0600-0699/0671.Second%20Minimum%20Node%20In%20a%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một binary tree đặc biệt không rỗng gồm các node có giá trị không âm, trong đó mỗi node có đúng <code>two</code> hoặc <code>zero</code> node con. Nếu một node có hai node con thì giá trị của node đó là giá trị nhỏ hơn trong hai node con. Cụ thể, thuộc tính&nbsp;<code>root.val = min(root.left.val, root.right.val)</code>&nbsp;luôn đúng.</p>

<p>Với binary tree như vậy, hãy tìm giá trị <b>nhỏ thứ hai</b> trong tập hợp các giá trị của toàn bộ node trong cây.</p>

<p>Nếu không tồn tại giá trị nhỏ thứ hai, hãy trả về -1.</p>

<p>&nbsp;</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0671.Second%20Minimum%20Node%20In%20a%20Binary%20Tree/images/smbt1.jpg" style="width: 431px; height: 302px;" />
<pre>
<strong>Đầu vào:</strong> root = [2,2,5,null,null,5,7]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Giá trị nhỏ nhất là 2, giá trị nhỏ thứ hai là 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0671.Second%20Minimum%20Node%20In%20a%20Binary%20Tree/images/smbt2.jpg" style="width: 321px; height: 182px;" />
<pre>
<strong>Đầu vào:</strong> root = [2,2,2]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Giá trị nhỏ nhất là 2, nhưng không có giá trị nhỏ thứ hai.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 25]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 2<sup>31</sup> - 1</code></li>
	<li><code>root.val == min(root.left.val, root.right.val)</code>&nbsp;đúng với mọi node bên trong cây.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi node bên trong cây có giá trị bằng giá trị nhỏ hơn trong hai node con, nên root là giá trị nhỏ nhất toàn cây. Ta chỉ cần tìm giá trị nhỏ thứ hai lớn hơn nghiêm ngặt.
>
> Dùng DFS và lưu giá trị nhỏ nhất lớn hơn root; nếu không có giá trị nào như vậy thì trả về $-1$.

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
    def findSecondMinimumValue(self, root: Optional[TreeNode]) -> int:
        def dfs(root):
            if root:
                dfs(root.left)
                dfs(root.right)
                nonlocal ans, v
                if root.val > v:
                    ans = root.val if ans == -1 else min(ans, root.val)

        ans, v = -1, root.val
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
    private int ans = -1;

    public int findSecondMinimumValue(TreeNode root) {
        dfs(root, root.val);
        return ans;
    }

    private void dfs(TreeNode root, int val) {
        if (root != null) {
            dfs(root.left, val);
            dfs(root.right, val);
            if (root.val > val) {
                ans = ans == -1 ? root.val : Math.min(ans, root.val);
            }
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
    int ans = -1;

    int findSecondMinimumValue(TreeNode* root) {
        dfs(root, root->val);
        return ans;
    }

    void dfs(TreeNode* root, int val) {
        if (!root) return;
        dfs(root->left, val);
        dfs(root->right, val);
        if (root->val > val) ans = ans == -1 ? root->val : min(ans, root->val);
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
func findSecondMinimumValue(root *TreeNode) int {
	ans, v := -1, root.Val
	var dfs func(*TreeNode)
	dfs = func(root *TreeNode) {
		if root == nil {
			return
		}
		dfs(root.Left)
		dfs(root.Right)
		if root.Val > v {
			if ans == -1 || ans > root.Val {
				ans = root.Val
			}
		}
	}
	dfs(root)
	return ans
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
 * @return {number}
 */
var findSecondMinimumValue = function (root) {
    let ans = -1;
    const v = root.val;
    function dfs(root) {
        if (!root) {
            return;
        }
        dfs(root.left);
        dfs(root.right);
        if (root.val > v) {
            if (ans == -1 || ans > root.val) {
                ans = root.val;
            }
        }
    }
    dfs(root);
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
