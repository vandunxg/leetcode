---
comments: true
difficulty: Easy
tags:
    - Tree
    - Depth-First Search
    - Binary Search Tree
    - Binary Tree
---

<!-- problem:start -->

# [938. Range Sum of BST](https://leetcode.com/problems/range-sum-of-bst)

[中文文档](/solution/0900-0999/0938.Range%20Sum%20of%20BST/README.md)

## Mô tả

<!-- description:start -->

<p>Cho node <code>root</code> của một binary search tree và hai số nguyên <code>low</code>, <code>high</code>. Trả về <em>tổng giá trị của tất cả node có giá trị nằm trong đoạn <strong>bao gồm cả hai đầu mút</strong> </em><code>[low, high]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0938.Range%20Sum%20of%20BST/images/bst1.jpg" style="width: 400px; height: 222px;" />
<pre>
<strong>Input:</strong> root = [10,5,15,3,7,null,18], low = 7, high = 15
<strong>Output:</strong> 32
<strong>Giải thích:</strong> Các node 7, 10 và 15 nằm trong đoạn [7, 15]. 7 + 10 + 15 = 32.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0938.Range%20Sum%20of%20BST/images/bst2.jpg" style="width: 400px; height: 335px;" />
<pre>
<strong>Input:</strong> root = [10,5,15,3,7,13,18,1,null,6], low = 6, high = 10
<strong>Output:</strong> 23
<strong>Giải thích:</strong> Các node 6, 7 và 10 nằm trong đoạn [6, 10]. 6 + 7 + 10 = 23.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 2 * 10<sup>4</sup>]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= low &lt;= high &lt;= 10<sup>5</sup></code></li>
	<li>Mọi giá trị <code>Node.val</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Tính tổng giá trị các node trong BST nằm trong đoạn $[low,high]$. Có thể duyệt toàn bộ cây, nhưng thứ tự của search tree giúp cắt tỉa: bỏ qua cây con trái khi giá trị hiện tại đã $\le low$, và bỏ qua cây con phải khi giá trị hiện tại đã $\ge high$. Đệ quy và cộng các node nằm trong đoạn.

<!-- thinking:end -->

Ta thiết kế hàm $dfs(root)$ để tính tổng giá trị của các node trong cây con có gốc $root$ mà giá trị nằm trong đoạn $[low, high]$. Đáp án là $dfs(root)$.

Hàm $dfs(root)$ hoạt động như sau:

- Nếu $root$ là null, trả về $0$.
- Nếu giá trị $x$ của $root$ nằm trong đoạn $[low, high]$, giá trị ban đầu của đáp án từ hàm $dfs(root)$ là $x$; nếu không thì là $0$.
- Nếu $x > low$, cây con trái của $root$ có thể chứa node với giá trị trong đoạn $[low, high]$, vì vậy cần gọi đệ quy $dfs(root.left)$ rồi cộng kết quả vào đáp án.
- Nếu $x < high$, cây con phải của $root$ có thể chứa node với giá trị trong đoạn $[low, high]$, vì vậy cần gọi đệ quy $dfs(root.right)$ rồi cộng kết quả vào đáp án.
- Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, với $n$ là số node trong binary search tree.

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
    def rangeSumBST(self, root: Optional[TreeNode], low: int, high: int) -> int:
        def dfs(root: Optional[TreeNode]) -> int:
            if root is None:
                return 0
            x = root.val
            ans = x if low <= x <= high else 0
            if x > low:
                ans += dfs(root.left)
            if x < high:
                ans += dfs(root.right)
            return ans

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
    public int rangeSumBST(TreeNode root, int low, int high) {
        return dfs(root, low, high);
    }

    private int dfs(TreeNode root, int low, int high) {
        if (root == null) {
            return 0;
        }
        int x = root.val;
        int ans = low <= x && x <= high ? x : 0;
        if (x > low) {
            ans += dfs(root.left, low, high);
        }
        if (x < high) {
            ans += dfs(root.right, low, high);
        }
        return ans;
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
    int rangeSumBST(TreeNode* root, int low, int high) {
        function<int(TreeNode*)> dfs = [&](TreeNode* root) {
            if (!root) {
                return 0;
            }
            int x = root->val;
            int ans = low <= x && x <= high ? x : 0;
            if (x > low) {
                ans += dfs(root->left);
            }
            if (x < high) {
                ans += dfs(root->right);
            }
            return ans;
        };
        return dfs(root);
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
func rangeSumBST(root *TreeNode, low int, high int) int {
	var dfs func(*TreeNode) int
	dfs = func(root *TreeNode) (ans int) {
		if root == nil {
			return 0
		}
		x := root.Val
		if low <= x && x <= high {
			ans += x
		}
		if x > low {
			ans += dfs(root.Left)
		}
		if x < high {
			ans += dfs(root.Right)
		}
		return
	}
	return dfs(root)
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

function rangeSumBST(root: TreeNode | null, low: number, high: number): number {
    const dfs = (root: TreeNode | null): number => {
        if (!root) {
            return 0;
        }
        const { val, left, right } = root;
        let ans = low <= val && val <= high ? val : 0;
        if (val > low) {
            ans += dfs(left);
        }
        if (val < high) {
            ans += dfs(right);
        }
        return ans;
    };
    return dfs(root);
}
```

#### C#

```cs
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left;
 *     public TreeNode right;
 *     public TreeNode(int val=0, TreeNode left=null, TreeNode right=null) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
public class Solution {
    public int RangeSumBST(TreeNode root, int low, int high) {
        return dfs(root, low, high);
    }

    private int dfs(TreeNode root, int low, int high) {
        if (root == null) {
            return 0;
        }
        int x = root.val;
        int ans = low <= x && x <= high ? x : 0;
        if (x > low) {
            ans += dfs(root.left, low, high);
        }
        if (x < high) {
            ans += dfs(root.right, low, high);
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
