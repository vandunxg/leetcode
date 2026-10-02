---
comments: true
difficulty: Hard
tags:
    - Tree
    - Depth-First Search
    - Dynamic Programming
    - Binary Tree
    - Tree DP
---

<!-- problem:start -->

# [968. Binary Tree Cameras](https://leetcode.com/problems/binary-tree-cameras)

[中文文档](/solution/0900-0999/0968.Binary%20Tree%20Cameras/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một cây nhị phân. Ta đặt camera tại các node; mỗi camera có thể giám sát node cha, chính node đó và các node con trực tiếp.</p>

<p>Trả về <em>số camera ít nhất cần thiết để giám sát mọi node trong cây</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0968.Binary%20Tree%20Cameras/images/bst_cameras_01.png" style="width: 138px; height: 163px;" />
<pre>
<strong>Đầu vào:</strong> root = [0,0,null,0,0]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Chỉ cần một camera để giám sát tất cả node nếu đặt như hình.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0968.Binary%20Tree%20Cameras/images/bst_cameras_02.png" style="width: 139px; height: 312px;" />
<pre>
<strong>Đầu vào:</strong> root = [0,0,null,0,null,0,null,null,0]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Cần ít nhất hai camera để giám sát tất cả node trong cây. Hình trên minh họa một cách đặt camera hợp lệ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong đoạn <code>[1, 1000]</code>.</li>
	<li><code>Node.val == 0</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động (Tree DP)

<!-- thinking:start -->

> **Tư duy**
>
> Camera giám sát node đặt nó, node cha và các node con; ta muốn dùng ít camera nhất có thể. Đáp án tối ưu tại một node phụ thuộc vào các node con, nên ta xét ba trạng thái: “có camera / được node con giám sát / chưa được giám sát”. Tree DP tính ba giá trị nhỏ nhất theo thứ tự từ dưới lên; node gốc không được để ở trạng thái chưa được giám sát.

<!-- thinking:end -->

Với mỗi node, ta định nghĩa ba trạng thái:

- `a`: Node hiện tại có camera
- `b`: Node hiện tại không có camera nhưng được các node con giám sát
- `c`: Node hiện tại không có camera và cũng không được các node con giám sát

Tiếp theo, ta định nghĩa hàm $dfs(root)$ trả về một mảng độ dài 3, lần lượt biểu diễn số camera ít nhất trong cây con có gốc tại `root` ứng với ba trạng thái. Đáp án là $\min(dfs(root)[0], dfs(root)[1])$.

Hàm $dfs(root)$ được tính như sau:

Nếu `root` là null, trả về $[inf, 0, 0]$, trong đó `inf` là một số rất lớn dùng để biểu thị trạng thái không thể xảy ra.

Nếu không, ta đệ quy tính các cây con trái và phải của `root`, lần lượt thu được $[la, lb, lc]$ và $[ra, rb, rc]$.

- Nếu node hiện tại có camera, thì node con trái và phải có thể ở bất kỳ trạng thái nào vì camera tại node hiện tại sẽ giám sát chúng. Do đó, $a = \min(la, lb, lc) + \min(ra, rb, rc) + 1$.
- Nếu node hiện tại không có camera nhưng được các node con giám sát, thì ít nhất một node con phải có camera. Do đó, $b = \min(la + rb, lb + ra, la + ra)$.
- Nếu node hiện tại không có camera và cũng không được các node con giám sát, thì các node con phải được các node con của chúng giám sát. Do đó, $c = lb + rb$.

Cuối cùng, ta trả về $[a, b, c]$.

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
    def minCameraCover(self, root: Optional[TreeNode]) -> int:
        def dfs(root):
            if root is None:
                return inf, 0, 0
            la, lb, lc = dfs(root.left)
            ra, rb, rc = dfs(root.right)
            a = min(la, lb, lc) + min(ra, rb, rc) + 1
            b = min(la + rb, lb + ra, la + ra)
            c = lb + rb
            return a, b, c

        a, b, _ = dfs(root)
        return min(a, b)
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
    public int minCameraCover(TreeNode root) {
        int[] ans = dfs(root);
        return Math.min(ans[0], ans[1]);
    }

    private int[] dfs(TreeNode root) {
        if (root == null) {
            return new int[] {1 << 29, 0, 0};
        }
        var l = dfs(root.left);
        var r = dfs(root.right);
        int a = 1 + Math.min(Math.min(l[0], l[1]), l[2]) + Math.min(Math.min(r[0], r[1]), r[2]);
        int b = Math.min(Math.min(l[0] + r[1], l[1] + r[0]), l[0] + r[0]);
        int c = l[1] + r[1];
        return new int[] {a, b, c};
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
struct Status {
    int a, b, c;
};

class Solution {
public:
    int minCameraCover(TreeNode* root) {
        auto [a, b, _] = dfs(root);
        return min(a, b);
    }

    Status dfs(TreeNode* root) {
        if (!root) {
            return {1 << 29, 0, 0};
        }
        auto [la, lb, lc] = dfs(root->left);
        auto [ra, rb, rc] = dfs(root->right);
        int a = 1 + min({la, lb, lc}) + min({ra, rb, rc});
        int b = min({la + ra, la + rb, lb + ra});
        int c = lb + rb;
        return {a, b, c};
    };
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
func minCameraCover(root *TreeNode) int {
	var dfs func(*TreeNode) (int, int, int)
	dfs = func(root *TreeNode) (int, int, int) {
		if root == nil {
			return 1 << 29, 0, 0
		}
		la, lb, lc := dfs(root.Left)
		ra, rb, rc := dfs(root.Right)
		a := 1 + min(la, min(lb, lc)) + min(ra, min(rb, rc))
		b := min(la+ra, min(la+rb, lb+ra))
		c := lb + rb
		return a, b, c
	}
	a, b, _ := dfs(root)
	return min(a, b)
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

function minCameraCover(root: TreeNode | null): number {
    const dfs = (root: TreeNode | null): number[] => {
        if (!root) {
            return [1 << 29, 0, 0];
        }
        const [la, lb, lc] = dfs(root.left);
        const [ra, rb, rc] = dfs(root.right);
        const a = 1 + Math.min(la, lb, lc) + Math.min(ra, rb, rc);
        const b = Math.min(la + ra, la + rb, lb + ra);
        const c = lb + rb;
        return [a, b, c];
    };
    const [a, b, _] = dfs(root);
    return Math.min(a, b);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
