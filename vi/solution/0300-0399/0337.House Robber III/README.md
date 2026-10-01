---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Dynamic Programming
    - Binary Tree
    - Tree DP
---

<!-- problem:start -->

# [337. House Robber III](https://leetcode.com/problems/house-robber-iii)

[中文文档](/solution/0300-0399/0337.House%20Robber%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Tên trộm lại tìm được một khu vực mới để trộm cắp. Khu vực này chỉ có một lối vào, gọi là <code>root</code>.</p>

<p>Ngoại trừ <code>root</code>, mỗi ngôi nhà đều có đúng một nhà cha. Sau khi khảo sát, tên trộm nhận ra tất cả ngôi nhà trong khu vực tạo thành một cây nhị phân. Hệ thống sẽ tự động báo cảnh sát nếu <strong>hai ngôi nhà nối trực tiếp bị đột nhập trong cùng một đêm</strong>.</p>

<p>Cho <code>root</code> của cây nhị phân, hãy trả về <em>số tiền tối đa tên trộm có thể lấy <strong>mà không báo động cảnh sát</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0337.House%20Robber%20III/images/rob1-tree.jpg" style="width: 277px; height: 293px;" />
<pre>
<strong>Đầu vào:</strong> root = [3,2,3,null,3,null,1]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Số tiền tối đa tên trộm có thể lấy = 3 + 3 + 1 = 7.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0337.House%20Robber%20III/images/rob2-tree.jpg" style="width: 357px; height: 293px;" />
<pre>
<strong>Đầu vào:</strong> root = [3,4,5,1,3,null,1]
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Số tiền tối đa tên trộm có thể lấy = 4 + 5 = 9.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 10<sup>4</sup>]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Không thể lấy tiền ở hai node kề nhau. Nếu tính riêng hai trạng thái lấy/bỏ rồi duyệt mỗi subtree hai lần thì sẽ lặp lại công việc.
>
> Duyệt hậu tự trả về cặp (lấy tiền ở root, bỏ qua root). Nếu lấy tiền ở root thì buộc phải bỏ qua cả hai node con; nếu bỏ qua root thì với mỗi node con, chọn trạng thái có kết quả tốt hơn. Đáp án là giá trị lớn nhất tại root. Mỗi node chỉ được duyệt một lần.

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
    def rob(self, root: Optional[TreeNode]) -> int:
        def dfs(root: Optional[TreeNode]) -> (int, int):
            if root is None:
                return 0, 0
            la, lb = dfs(root.left)
            ra, rb = dfs(root.right)
            return root.val + lb + rb, max(la, lb) + max(ra, rb)

        return max(dfs(root))
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
    public int rob(TreeNode root) {
        int[] ans = dfs(root);
        return Math.max(ans[0], ans[1]);
    }

    private int[] dfs(TreeNode root) {
        if (root == null) {
            return new int[2];
        }
        int[] l = dfs(root.left);
        int[] r = dfs(root.right);
        return new int[] {root.val + l[1] + r[1], Math.max(l[0], l[1]) + Math.max(r[0], r[1])};
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
    int rob(TreeNode* root) {
        function<pair<int, int>(TreeNode*)> dfs = [&](TreeNode* root) -> pair<int, int> {
            if (!root) {
                return make_pair(0, 0);
            }
            auto [la, lb] = dfs(root->left);
            auto [ra, rb] = dfs(root->right);
            return make_pair(root->val + lb + rb, max(la, lb) + max(ra, rb));
        };
        auto [a, b] = dfs(root);
        return max(a, b);
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
func rob(root *TreeNode) int {
	var dfs func(*TreeNode) (int, int)
	dfs = func(root *TreeNode) (int, int) {
		if root == nil {
			return 0, 0
		}
		la, lb := dfs(root.Left)
		ra, rb := dfs(root.Right)
		return root.Val + lb + rb, max(la, lb) + max(ra, rb)
	}
	a, b := dfs(root)
	return max(a, b)
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

function rob(root: TreeNode | null): number {
    const dfs = (root: TreeNode | null): [number, number] => {
        if (!root) {
            return [0, 0];
        }
        const [la, lb] = dfs(root.left);
        const [ra, rb] = dfs(root.right);
        return [root.val + lb + rb, Math.max(la, lb) + Math.max(ra, rb)];
    };
    return Math.max(...dfs(root));
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
