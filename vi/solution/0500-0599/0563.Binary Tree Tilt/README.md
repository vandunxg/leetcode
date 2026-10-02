---
comments: true
difficulty: Easy
tags:
    - Tree
    - Depth-First Search
    - Binary Tree
    - Tree DP
---

<!-- problem:start -->

# [563. Binary Tree Tilt](https://leetcode.com/problems/binary-tree-tilt)

[中文文档](/solution/0500-0599/0563.Binary%20Tree%20Tilt/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một cây nhị phân, hãy trả về <em>tổng <strong>độ nghiêng</strong> của mọi node trong cây.</em></p>

<p><strong>Độ nghiêng</strong> của một node là <strong>giá trị tuyệt đối của hiệu</strong> giữa tổng <strong>giá trị</strong> các node trong cây con trái và tổng <strong>giá trị</strong> các node trong cây con phải. Nếu node không có con trái, tổng <strong>giá trị</strong> của cây con trái được xem là <code>0</code>. Quy tắc tương tự áp dụng nếu node không có con phải.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0563.Binary%20Tree%20Tilt/images/tilt1.jpg" style="width: 712px; height: 182px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,2,3]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> 
Độ nghiêng của node 2: |0-0| = 0 (không có node con)
Độ nghiêng của node 3: |0-0| = 0 (không có node con)
Độ nghiêng của node 1: |2-3| = 1 (cây con trái chỉ gồm node con trái nên tổng là 2; cây con phải chỉ gồm node con phải nên tổng là 3)
Tổng độ nghiêng của các node: 0 + 0 + 1 = 1
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0563.Binary%20Tree%20Tilt/images/tilt2.jpg" style="width: 800px; height: 203px;" />
<pre>
<strong>Đầu vào:</strong> root = [4,2,9,3,5,null,7]
<strong>Đầu ra:</strong> 15
<strong>Giải thích:</strong> 
Độ nghiêng của node 3: |0-0| = 0 (không có node con)
Độ nghiêng của node 5: |0-0| = 0 (không có node con)
Độ nghiêng của node 7: |0-0| = 0 (không có node con)
Độ nghiêng của node 2: |3-5| = 2 (cây con trái chỉ gồm node con trái nên tổng là 3; cây con phải chỉ gồm node con phải nên tổng là 5)
Độ nghiêng của node 9: |0-7| = 7 (không có con trái nên tổng là 0; cây con phải chỉ gồm node con phải nên tổng là 7)
Độ nghiêng của node 4: |(3+5+2)-(9+7)| = |10-16| = 6 (các giá trị trong cây con trái là 3, 5 và 2, có tổng bằng 10; các giá trị trong cây con phải là 9 và 7, có tổng bằng 16)
Tổng độ nghiêng của các node: 0 + 0 + 0 + 2 + 7 + 6 = 15
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0563.Binary%20Tree%20Tilt/images/tilt3.jpg" style="width: 800px; height: 293px;" />
<pre>
<strong>Đầu vào:</strong> root = [21,7,14,1,1,2,2,3,3]
<strong>Đầu ra:</strong> 9
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số lượng node trong cây nằm trong khoảng <code>[0, 10<sup>4</sup>]</code>.</li>
	<li><code>-1000 &lt;= Node.val &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Độ nghiêng là giá trị tuyệt đối của hiệu hai tổng cây con; độ nghiêng của cây là tổng độ nghiêng của các node. Mỗi node cần tổng của cả hai cây con, nên duyệt hậu tự là phù hợp.
>
> DFS trả về chiều cao và khi quay lui cập nhật đáp án toàn cục bằng $|l-r|$, sau đó trả về $l+r+\textit{val}$. Một lượt duyệt tính được độ nghiêng của mọi node.

<!-- thinking:end -->

Ta xây dựng hàm $\text{dfs}$ để tính tổng giá trị các node trong cây con bắt đầu từ node hiện tại. Trong hàm $\text{dfs}$, trước tiên kiểm tra node hiện tại có null hay không; nếu có thì trả về 0. Sau đó, gọi đệ quy hàm $\text{dfs}$ để tính tổng giá trị cây con trái $l$ và cây con phải $r$. Tiếp theo, tính độ nghiêng của node hiện tại là $|l - r|$ rồi cộng vào đáp án. Cuối cùng, trả về tổng giá trị các node trong cây con hiện tại là $l + r + \textit{root.val}$.

Trong hàm chính, khởi tạo đáp án bằng 0, sau đó gọi hàm $\text{dfs}$ để tính độ nghiêng của toàn cây và trả về đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node.

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
    def findTilt(self, root: Optional[TreeNode]) -> int:
        def dfs(root: Optional[TreeNode]) -> int:
            if root is None:
                return 0
            l, r = dfs(root.left), dfs(root.right)
            nonlocal ans
            ans += abs(l - r)
            return l + r + root.val

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

    public int findTilt(TreeNode root) {
        dfs(root);
        return ans;
    }

    private int dfs(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int l = dfs(root.left), r = dfs(root.right);
        ans += Math.abs(l - r);
        return l + r + root.val;
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
    int findTilt(TreeNode* root) {
        int ans = 0;
        auto dfs = [&](this auto&& dfs, TreeNode* root) -> int {
            if (!root) {
                return 0;
            }
            int l = dfs(root->left), r = dfs(root->right);
            ans += abs(l - r);
            return l + r + root->val;
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
func findTilt(root *TreeNode) (ans int) {
	var dfs func(*TreeNode) int
	dfs = func(root *TreeNode) int {
		if root == nil {
			return 0
		}
		l, r := dfs(root.Left), dfs(root.Right)
		ans += abs(l - r)
		return l + r + root.Val
	}
	dfs(root)
	return
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
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

function findTilt(root: TreeNode | null): number {
    let ans: number = 0;
    const dfs = (root: TreeNode | null): number => {
        if (!root) {
            return 0;
        }
        const [l, r] = [dfs(root.left), dfs(root.right)];
        ans += Math.abs(l - r);
        return l + r + root.val;
    };
    dfs(root);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
