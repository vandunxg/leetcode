---
comments: true
difficulty: Medium
rating: 1426
source: Weekly Contest 511 Q2
tags:
    - Tree
    - Depth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [3997. Count Dominant Nodes in a Binary Tree](https://leetcode.com/problems/count-dominant-nodes-in-a-binary-tree)

[中文文档](/solution/3900-3999/3997.Count%20Dominant%20Nodes%20in%20a%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một <span data-keyword="complete-binary-tree">cây nhị phân hoàn chỉnh</span>.</p>

<p>Một node <code>x</code> được gọi là <strong>dominant</strong> nếu giá trị của nó bằng giá trị lớn nhất trong <span data-keyword="subtree">cây con</span> có gốc tại <code>x</code>.</p>

<p>Trả về số lượng node dominant trong cây.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3997.Count%20Dominant%20Nodes%20in%20a%20Binary%20Tree/images/tnew.png" style="width: 300px; height: 193px;" /></p>

<p><strong>Đầu vào:</strong> <span class="example-io">root = [5,3,8,2,4,7,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các node lá có giá trị 2, 4, 7 và 1 là dominant.</li>
	<li>Node có giá trị 8 là dominant vì giá trị của nó là lớn nhất trong cây con <code>[8, 7, 1]</code>.</li>
	<li>Do đó, đáp án là 5.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3997.Count%20Dominant%20Nodes%20in%20a%20Binary%20Tree/images/t9.png" style="width: 250px; height: 183px;" /></p>

<p><strong>Đầu vào:</strong> <span class="example-io">root = [1,2,3,1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các node lá có giá trị 1, 2 và 3 là dominant.</li>
	<li>Node có giá trị 2 với cây con là <code>[2, 1, 2]</code> là dominant vì giá trị của nó là lớn nhất trong cây con.</li>
	<li>Do đó, đáp án là 4.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 10<sup>5</sup>]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 10<sup>9</sup></code></li>
	<li>Cây được đảm bảo là cây nhị phân hoàn chỉnh.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Một node dominant là node có giá trị bằng giá trị lớn nhất trong cây con. Một lần DFS từ dưới lên có thể tính đồng thời giá trị lớn nhất của hai cây con và so sánh chúng với node hiện tại.
>
> Với node rỗng, trả về $-\infty$; giá trị lớn nhất hiện tại là max của ba giá trị. Nếu giá trị này bằng giá trị của node thì tăng đáp án, rồi trả giá trị lớn nhất đó về cho node cha. Vì $n\le 10^5$, không thể duyệt lại từng cây con.

<!-- thinking:end -->

Một node là dominant nếu giá trị của nó bằng giá trị lớn nhất trong cây con có gốc tại node đó. Vì vậy, với mỗi node, ta chỉ cần giá trị lớn nhất của cây con trái và cây con phải, sau đó so sánh với chính node đó.

Thực hiện DFS từ dưới lên: trả về $-\infty$ với node rỗng (được cài đặt bằng giá trị số nguyên nhỏ nhất của ngôn ngữ), sau đó với node hiện tại, tính $\textit{mx} = \max(\textit{leftMax}, \textit{rightMax}, \textit{node.val})$. Nếu $\textit{mx} = \textit{node.val}$, node là dominant và đáp án tăng thêm một. Cuối cùng, trả về $\textit{mx}$ cho node cha.

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
    def countDominantNodes(self, root: TreeNode | None) -> int:
        def dfs(node: TreeNode | None) -> int:
            if node is None:
                return -inf
            l = dfs(node.left)
            r = dfs(node.right)
            mx = max(l, r, node.val)
            if mx == node.val:
                nonlocal ans
                ans += 1
            return mx

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

    public int countDominantNodes(TreeNode root) {
        dfs(root);
        return ans;
    }

    private int dfs(TreeNode node) {
        if (node == null) {
            return Integer.MIN_VALUE;
        }
        int l = dfs(node.left);
        int r = dfs(node.right);
        int mx = Math.max(Math.max(l, r), node.val);
        if (mx == node.val) {
            ++ans;
        }
        return mx;
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
    int countDominantNodes(TreeNode* root) {
        int ans = 0;
        auto dfs = [&](this auto&& dfs, TreeNode* node) -> int {
            if (!node) {
                return INT_MIN;
            }
            int l = dfs(node->left);
            int r = dfs(node->right);
            int mx = max({l, r, node->val});
            if (mx == node->val) {
                ++ans;
            }
            return mx;
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
func countDominantNodes(root *TreeNode) int {
	ans := 0
	var dfs func(*TreeNode) int
	dfs = func(node *TreeNode) int {
		if node == nil {
			return math.MinInt32
		}
		l := dfs(node.Left)
		r := dfs(node.Right)
		mx := max(l, r, node.Val)
		if mx == node.Val {
			ans++
		}
		return mx
	}
	dfs(root)
	return ans
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

function countDominantNodes(root: TreeNode | null): number {
    let ans = 0;
    const dfs = (node: TreeNode | null): number => {
        if (!node) {
            return -Infinity;
        }
        const l = dfs(node.left);
        const r = dfs(node.right);
        const mx = Math.max(l, r, node.val);
        if (mx === node.val) {
            ++ans;
        }
        return mx;
    };
    dfs(root);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
