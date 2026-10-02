---
comments: true
difficulty: Hard
rating: 1913
source: Biweekly Contest 21 Q4
tags:
    - Tree
    - Depth-First Search
    - Binary Search Tree
    - Dynamic Programming
    - Binary Tree
    - Tree DP
---

<!-- problem:start -->

# [1373. Maximum Sum BST in Binary Tree](https://leetcode.com/problems/maximum-sum-bst-in-binary-tree)

[中文文档](/solution/1300-1399/1373.Maximum%20Sum%20BST%20in%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một <strong>cây nhị phân</strong> <code>root</code>, hãy trả về <em>tổng khóa lớn nhất của một cây con bất kỳ cũng là Cây Tìm kiếm Nhị phân (BST)</em>.</p>

<p>BST được định nghĩa như sau:</p>

<ul>
	<li>Cây con trái của một node chỉ chứa các node có khóa <strong>nhỏ hơn</strong> khóa của node đó.</li>
	<li>Cây con phải của một node chỉ chứa các node có khóa <strong>lớn hơn</strong> khóa của node đó.</li>
	<li>Cả cây con trái và cây con phải cũng phải là cây tìm kiếm nhị phân.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1373.Maximum%20Sum%20BST%20in%20Binary%20Tree/images/sample_1_1709.png" style="width: 320px; height: 250px;" /></p>

<pre>
<strong>Input:</strong> root = [1,4,3,2,4,2,5,null,null,null,null,null,null,4,6]
<strong>Output:</strong> 20
<strong>Giải thích:</strong> Tổng lớn nhất của một cây tìm kiếm nhị phân hợp lệ đạt được ở cây có node gốc mang khóa 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1373.Maximum%20Sum%20BST%20in%20Binary%20Tree/images/sample_2_1709.png" style="width: 134px; height: 180px;" /></p>

<pre>
<strong>Input:</strong> root = [4,3,null,1,2]
<strong>Output:</strong> 2
<strong>Giải thích:</strong> Tổng lớn nhất của một cây tìm kiếm nhị phân hợp lệ đạt được ở cây chỉ gồm node gốc mang khóa 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> root = [-4,-2,-5]
<strong>Output:</strong> 0
<strong>Giải thích:</strong> Tất cả giá trị đều âm. Hãy trả về một BST rỗng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 4 * 10<sup>4</sup>]</code>.</li>
	<li><code>-4 * 10<sup>4</sup> &lt;= Node.val &lt;= 4 * 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Trong số các cây con là BST, cần tìm cây có tổng khóa lớn nhất. Nếu kiểm tra tính chất BST từ trên xuống, ta sẽ lặp lại công việc. Duyệt hậu tự có thể trả về bộ bốn giá trị: cờ BST, giá trị nhỏ nhất, giá trị lớn nhất và tổng. Khi cả hai cây con đều là BST và $l_{mx}<\textit{root}.val<r_{mi}$, cây hiện tại hợp lệ và tổng của nó được dùng để cập nhật đáp án. Cây rỗng là BST có giá trị nhỏ nhất $+\infty$ và lớn nhất $-\infty$.

<!-- thinking:end -->

Để xác định một cây có phải là cây tìm kiếm nhị phân hay không, cây đó cần thỏa mãn bốn điều kiện sau:

- Cây con trái là cây tìm kiếm nhị phân;
- Cây con phải là cây tìm kiếm nhị phân;
- Giá trị lớn nhất trong cây con trái nhỏ hơn giá trị của node gốc;
- Giá trị nhỏ nhất trong cây con phải lớn hơn giá trị của node gốc.

Vì vậy, ta thiết kế hàm $dfs(root)$ trả về bộ bốn giá trị $(bst, mi, mx, s)$, trong đó:

- $bst$ cho biết cây có gốc $root$ có phải là cây tìm kiếm nhị phân hay không. Nếu đúng thì $bst = 1$, ngược lại $bst = 0$;
- $mi$ là giá trị nhỏ nhất trong cây có gốc $root$;
- $mx$ là giá trị lớn nhất trong cây có gốc $root$;
- $s$ là tổng giá trị của tất cả node trong cây có gốc $root$.

Hàm $dfs(root)$ hoạt động như sau:

Nếu $root$ là node rỗng, trả về $(1, +\infty, -\infty, 0)$, nghĩa là cây rỗng là một cây tìm kiếm nhị phân, giá trị nhỏ nhất và lớn nhất lần lượt là dương vô cực và âm vô cực, còn tổng giá trị các node bằng $0$.

Nếu không, ta đệ quy tính cây con trái và cây con phải của $root$, lần lượt nhận được $(lbst, lmi, lmx, ls)$ và $(rbst, rmi, rmx, rs)$, rồi kiểm tra node $root$ có thỏa mãn điều kiện của cây tìm kiếm nhị phân hay không.

Nếu $lbst = 1$, $rbst = 1$ và $lmx < root.val < rmi$, cây có gốc $root$ là cây tìm kiếm nhị phân. Khi đó, tổng giá trị các node là $s = ls + rs + root.val$. Ta cập nhật đáp án $ans = \max(ans, s)$ rồi trả về $(1, \min(lmi, root.val), \max(rmx, root.val), s)$.

Ngược lại, cây có gốc $root$ không phải là cây tìm kiếm nhị phân, nên ta trả về $(0, 0, 0, 0)$.

Ta gọi $dfs(root)$ trong hàm chính. Sau khi thực thi, đáp án nằm trong $ans$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node trong cây nhị phân.

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
    def maxSumBST(self, root: Optional[TreeNode]) -> int:
        def dfs(root: Optional[TreeNode]) -> tuple:
            if root is None:
                return 1, inf, -inf, 0
            lbst, lmi, lmx, ls = dfs(root.left)
            rbst, rmi, rmx, rs = dfs(root.right)
            if lbst and rbst and lmx < root.val < rmi:
                nonlocal ans
                s = ls + rs + root.val
                ans = max(ans, s)
                return 1, min(lmi, root.val), max(rmx, root.val), s
            return 0, 0, 0, 0

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
    private final int inf = 1 << 30;

    public int maxSumBST(TreeNode root) {
        dfs(root);
        return ans;
    }

    private int[] dfs(TreeNode root) {
        if (root == null) {
            return new int[] {1, inf, -inf, 0};
        }
        var l = dfs(root.left);
        var r = dfs(root.right);
        int v = root.val;
        if (l[0] == 1 && r[0] == 1 && l[2] < v && r[1] > v) {
            int s = v + l[3] + r[3];
            ans = Math.max(ans, s);
            return new int[] {1, Math.min(l[1], v), Math.max(r[2], v), s};
        }
        return new int[4];
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
    int maxSumBST(TreeNode* root) {
        int ans = 0;
        const int inf = 1 << 30;
        auto dfs = [&](this auto&& dfs, TreeNode* root) -> array<int, 4> {
            if (!root) {
                return {1, inf, -inf, 0};
            }
            auto l = dfs(root->left);
            auto r = dfs(root->right);
            int v = root->val;
            if (l[0] && r[0] && l[2] < v && v < r[1]) {
                int s = l[3] + r[3] + v;
                ans = max(ans, s);
                return {1, min(l[1], v), max(r[2], v), s};
            }
            return {0};
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
func maxSumBST(root *TreeNode) (ans int) {
	const inf = 1 << 30
	var dfs func(root *TreeNode) [4]int
	dfs = func(root *TreeNode) [4]int {
		if root == nil {
			return [4]int{1, inf, -inf, 0}
		}
		l, r := dfs(root.Left), dfs(root.Right)
		if l[0] == 1 && r[0] == 1 && l[2] < root.Val && root.Val < r[1] {
			s := l[3] + r[3] + root.Val
			ans = max(ans, s)
			return [4]int{1, min(l[1], root.Val), max(r[2], root.Val), s}
		}
		return [4]int{}
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

function maxSumBST(root: TreeNode | null): number {
    const inf = 1 << 30;
    let ans = 0;
    const dfs = (root: TreeNode | null): [boolean, number, number, number] => {
        if (!root) {
            return [true, inf, -inf, 0];
        }
        const [lbst, lmi, lmx, ls] = dfs(root.left);
        const [rbst, rmi, rmx, rs] = dfs(root.right);
        if (lbst && rbst && lmx < root.val && root.val < rmi) {
            const s = ls + rs + root.val;
            ans = Math.max(ans, s);
            return [true, Math.min(lmi, root.val), Math.max(rmx, root.val), s];
        }
        return [false, 0, 0, 0];
    };
    dfs(root);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
