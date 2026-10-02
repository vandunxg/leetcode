---
comments: true
difficulty: Medium
rating: 1426
source: Biweekly Contest 17 Q3
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [1315. Sum of Nodes with Even-Valued Grandparent](https://leetcode.com/problems/sum-of-nodes-with-even-valued-grandparent)

[中文文档](/solution/1300-1399/1315.Sum%20of%20Nodes%20with%20Even-Valued%20Grandparent/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một binary tree, hãy trả về <em>tổng giá trị của các node có <strong>ông/bà mang giá trị chẵn</strong></em>. Nếu không có node nào có <strong>ông/bà mang giá trị chẵn</strong>, trả về <code>0</code>.</p>

<p><strong>Ông/bà</strong> của một node là parent của parent của node đó, nếu tồn tại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1315.Sum%20of%20Nodes%20with%20Even-Valued%20Grandparent/images/even1-tree.jpg" style="width: 504px; height: 302px;" />
<pre>
<strong>Đầu vào:</strong> root = [6,7,8,2,7,1,3,9,null,1,4,null,null,null,5]
<strong>Đầu ra:</strong> 18
<strong>Giải thích:</strong> Các node màu đỏ có ông/bà mang giá trị chẵn, còn các node màu xanh là những ông/bà mang giá trị chẵn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1315.Sum%20of%20Nodes%20with%20Even-Valued%20Grandparent/images/even2-tree.jpg" style="width: 64px; height: 65px;" />
<pre>
<strong>Đầu vào:</strong> root = [1]
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 10<sup>4</sup>]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Ta tính tổng các node có ông/bà mang giá trị chẵn. Đi ngược từ node con lên sẽ cần parent pointer. Thay vào đó, chỉ cần truyền giá trị của parent xuống: nếu giá trị đó chẵn, các node con của node hiện tại chính là những node có ông/bà mang giá trị chẵn, nên ta cộng giá trị của chúng rồi đệ quy tiếp với giá trị hiện tại. Root không có ông/bà, nên có thể dùng một giá trị lẻ làm giá trị khởi tạo.

<!-- thinking:end -->

Ta định nghĩa hàm $dfs(root, x)$ là tổng giá trị các node thỏa điều kiện trong subtree có $root$ làm root, với $x$ là giá trị của parent của $root$. Đáp án là $dfs(root, 1)$.

Hàm $dfs(root, x)$ hoạt động như sau:

- Nếu $root$ là null, trả về $0$.
- Nếu không, ta đệ quy tính kết quả của subtree trái và phải của $root$, lần lượt là $dfs(root.left, root.val)$ và $dfs(root.right, root.val)$, rồi cộng chúng vào đáp án. Nếu $x$ chẵn, ta kiểm tra các node con trái và phải của $root$ có tồn tại không; nếu có thì cộng giá trị của chúng vào đáp án.
- Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node trong cây.

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
    def sumEvenGrandparent(self, root: TreeNode) -> int:
        def dfs(root: TreeNode, x: int) -> int:
            if root is None:
                return 0
            ans = dfs(root.left, root.val) + dfs(root.right, root.val)
            if x % 2 == 0:
                if root.left:
                    ans += root.left.val
                if root.right:
                    ans += root.right.val
            return ans

        return dfs(root, 1)
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
    public int sumEvenGrandparent(TreeNode root) {
        return dfs(root, 1);
    }

    private int dfs(TreeNode root, int x) {
        if (root == null) {
            return 0;
        }
        int ans = dfs(root.left, root.val) + dfs(root.right, root.val);
        if (x % 2 == 0) {
            if (root.left != null) {
                ans += root.left.val;
            }
            if (root.right != null) {
                ans += root.right.val;
            }
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
    int sumEvenGrandparent(TreeNode* root) {
        function<int(TreeNode*, int)> dfs = [&](TreeNode* root, int x) {
            if (!root) {
                return 0;
            }
            int ans = dfs(root->left, root->val) + dfs(root->right, root->val);
            if (x % 2 == 0) {
                if (root->left) {
                    ans += root->left->val;
                }
                if (root->right) {
                    ans += root->right->val;
                }
            }
            return ans;
        };
        return dfs(root, 1);
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
func sumEvenGrandparent(root *TreeNode) int {
	var dfs func(*TreeNode, int) int
	dfs = func(root *TreeNode, x int) int {
		if root == nil {
			return 0
		}
		ans := dfs(root.Left, root.Val) + dfs(root.Right, root.Val)
		if x%2 == 0 {
			if root.Left != nil {
				ans += root.Left.Val
			}
			if root.Right != nil {
				ans += root.Right.Val
			}
		}
		return ans
	}
	return dfs(root, 1)
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

function sumEvenGrandparent(root: TreeNode | null): number {
    const dfs = (root: TreeNode | null, x: number): number => {
        if (!root) {
            return 0;
        }
        const { val, left, right } = root;
        let ans = dfs(left, val) + dfs(right, val);
        if (x % 2 === 0) {
            ans += left?.val ?? 0;
            ans += right?.val ?? 0;
        }
        return ans;
    };
    return dfs(root, 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
