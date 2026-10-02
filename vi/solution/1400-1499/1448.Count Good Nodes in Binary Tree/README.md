---
comments: true
difficulty: Medium
rating: 1360
source: Biweekly Contest 26 Q3
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [1448. Count Good Nodes in Binary Tree](https://leetcode.com/problems/count-good-nodes-in-binary-tree)

[中文文档](/solution/1400-1499/1448.Count%20Good%20Nodes%20in%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một cây nhị phân <code>root</code>, một node <em>X</em> trong cây được gọi là <strong>good</strong> nếu trên đường đi từ root đến <em>X</em> không có node nào có giá trị <em>lớn hơn</em> X.</p>

<p>Trả về số lượng node <strong>good</strong> trong cây nhị phân.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1448.Count%20Good%20Nodes%20in%20Binary%20Tree/images/test_sample_1.png" style="width: 263px; height: 156px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> root = [3,1,4,3,null,1,5]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Các node màu xanh dương là các node <strong>good</strong>.
Node gốc (3) luôn là một node good.
Node 4 -&gt; (3,4) là giá trị lớn nhất trên đường đi bắt đầu từ node gốc.
Node 5 -&gt; (3,4,5) là giá trị lớn nhất trên đường đi.
Node 3 -&gt; (3,1,3) là giá trị lớn nhất trên đường đi.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1448.Count%20Good%20Nodes%20in%20Binary%20Tree/images/test_sample_2.png" style="width: 157px; height: 161px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> root = [3,3,null,4,2]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Node 2 -&gt; (3, 3, 2) không phải là node good, vì "3" lớn hơn nó.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [1]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Node gốc được xem là <strong>good</strong>.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số lượng node trong cây nhị phân nằm trong khoảng <code>[1, 10^5]</code>.</li>
	<li>Giá trị của mỗi node nằm trong khoảng <code>[-10^4, 10^4]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một node là good nếu không có node nào lớn hơn nó xuất hiện trên đường đi từ node gốc. Với $n\le 10^5$, DFS truyền giá trị lớn nhất trên đường đi là $mx$; đếm node khi giá trị của nó lớn hơn hoặc bằng $mx$, sau đó cập nhật $mx$.

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
    def goodNodes(self, root: TreeNode) -> int:
        def dfs(root: TreeNode, mx: int):
            if root is None:
                return
            nonlocal ans
            if mx <= root.val:
                ans += 1
                mx = root.val
            dfs(root.left, mx)
            dfs(root.right, mx)

        ans = 0
        dfs(root, -1000000)
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
    private int ans = 0;

    public int goodNodes(TreeNode root) {
        dfs(root, -100000);
        return ans;
    }

    private void dfs(TreeNode root, int mx) {
        if (root == null) {
            return;
        }
        if (mx <= root.val) {
            ++ans;
            mx = root.val;
        }
        dfs(root.left, mx);
        dfs(root.right, mx);
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
    int goodNodes(TreeNode* root) {
        int ans = 0;
        function<void(TreeNode*, int)> dfs = [&](TreeNode* root, int mx) {
            if (!root) {
                return;
            }
            if (mx <= root->val) {
                ++ans;
                mx = root->val;
            }
            dfs(root->left, mx);
            dfs(root->right, mx);
        };
        dfs(root, -1e6);
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
func goodNodes(root *TreeNode) (ans int) {
	var dfs func(*TreeNode, int)
	dfs = func(root *TreeNode, mx int) {
		if root == nil {
			return
		}
		if mx <= root.Val {
			ans++
			mx = root.Val
		}
		dfs(root.Left, mx)
		dfs(root.Right, mx)
	}
	dfs(root, -10001)
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

function goodNodes(root: TreeNode | null): number {
    let ans = 0;
    const dfs = (root: TreeNode | null, mx: number) => {
        if (!root) {
            return;
        }
        if (mx <= root.val) {
            ++ans;
            mx = root.val;
        }
        dfs(root.left, mx);
        dfs(root.right, mx);
    };
    dfs(root, -1e6);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
