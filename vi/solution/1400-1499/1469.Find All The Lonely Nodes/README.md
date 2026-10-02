---
comments: true
difficulty: Easy
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [1469. Find All The Lonely Nodes 🔒](https://leetcode.com/problems/find-all-the-lonely-nodes)

[中文文档](/solution/1400-1499/1469.Find%20All%20The%20Lonely%20Nodes/README.md)

## Mô tả

<!-- description:start -->

<p>Trong một binary tree, một node <strong>cô độc</strong> là node duy nhất là con của node cha. Root của tree không cô độc vì nó không có node cha.</p>

<p>Cho <code>root</code> của một binary tree, hãy trả về <em>một mảng chứa giá trị của tất cả các node cô độc</em> trong tree. Trả về danh sách <strong>theo bất kỳ thứ tự nào</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1469.Find%20All%20The%20Lonely%20Nodes/images/e1.png" style="width: 203px; height: 202px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,2,3,null,4]
<strong>Đầu ra:</strong> [4]
<strong>Giải thích:</strong> Node màu xanh nhạt là node cô độc duy nhất.
Node 1 là root và không cô độc.
Node 2 và 3 có cùng node cha nên không cô độc.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1469.Find%20All%20The%20Lonely%20Nodes/images/e2.png" style="width: 442px; height: 282px;" />
<pre>
<strong>Đầu vào:</strong> root = [7,1,4,6,null,5,3,null,null,null,null,null,2]
<strong>Đầu ra:</strong> [6,2]
<strong>Giải thích:</strong> Các node màu xanh nhạt là những node cô độc.
Hãy nhớ rằng thứ tự không quan trọng, [2,6] cũng là một đáp án hợp lệ.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1469.Find%20All%20The%20Lonely%20Nodes/images/tree.png" style="width: 363px; height: 202px;" />
<pre>
<strong>
Đầu vào:</strong> root = [11,99,88,77,null,null,66,55,null,null,44,33,null,null,22]
<strong>Đầu ra:</strong> [77,55,33,66,44,22]
<strong>Giải thích:</strong> Node 99 và 88 có cùng node cha. Node 11 là root.
Tất cả các node còn lại đều cô độc.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong <code>tree</code> nằm trong khoảng <code>[1, 1000].</code></li>
	<li><code>1 &lt;= Node.val &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Node cô độc là node duy nhất của cha. $n\le 1000$. Trong DFS, nếu tồn tại chính xác một node con thì ghi lại giá trị của node đó, sau đó tiếp tục đệ quy. Các node lá và null sẽ trả về ngay.

<!-- thinking:end -->

Ta có thể sử dụng Depth-First Search (DFS) để duyệt toàn bộ tree. Ta thiết kế một hàm $\textit{dfs}$ để duyệt từng node trong tree. Nếu node hiện tại là một node con cô độc, ta thêm giá trị của nó vào mảng kết quả. Quy trình thực thi của hàm $\textit{dfs}$ như sau:

1. Nếu node hiện tại là null hoặc là node lá (tức là cả node con trái và node con phải của node hiện tại đều là null), thì trả về ngay.
2. Nếu node con trái của node hiện tại là null, thì node con phải là node con cô độc, và ta thêm giá trị của nó vào mảng kết quả.
3. Nếu node con phải của node hiện tại là null, thì node con trái là node con cô độc, và ta thêm giá trị của nó vào mảng kết quả.
4. Đệ quy duyệt node con trái và node con phải của node hiện tại.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số node trong binary tree.

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
    def getLonelyNodes(self, root: Optional[TreeNode]) -> List[int]:
        def dfs(root: Optional[TreeNode]):
            if root is None or root.left == root.right:
                return
            if root.left is None:
                ans.append(root.right.val)
            if root.right is None:
                ans.append(root.left.val)
            dfs(root.left)
            dfs(root.right)

        ans = []
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
    private List<Integer> ans = new ArrayList<>();

    public List<Integer> getLonelyNodes(TreeNode root) {
        dfs(root);
        return ans;
    }

    private void dfs(TreeNode root) {
        if (root == null || (root.left == root.right)) {
            return;
        }
        if (root.left == null) {
            ans.add(root.right.val);
        }
        if (root.right == null) {
            ans.add(root.left.val);
        }
        dfs(root.left);
        dfs(root.right);
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
    vector<int> getLonelyNodes(TreeNode* root) {
        vector<int> ans;
        auto dfs = [&](this auto&& dfs, TreeNode* root) {
            if (!root || (root->left == root->right)) {
                return;
            }
            if (!root->left) {
                ans.push_back(root->right->val);
            }
            if (!root->right) {
                ans.push_back(root->left->val);
            }
            dfs(root->left);
            dfs(root->right);
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
 func getLonelyNodes(root *TreeNode) (ans []int) {
	var dfs func(*TreeNode)
	dfs = func(root *TreeNode) {
		if root == nil || (root.Left == root.Right) {
			return
		}
		if root.Left == nil {
			ans = append(ans, root.Right.Val)
		}
		if root.Right == nil {
			ans = append(ans, root.Left.Val)
		}
		dfs(root.Left)
		dfs(root.Right)
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

function getLonelyNodes(root: TreeNode | null): number[] {
    const ans: number[] = [];
    const dfs = (root: TreeNode | null) => {
        if (!root || root.left === root.right) {
            return;
        }
        if (!root.left) {
            ans.push(root.right.val);
        }
        if (!root.right) {
            ans.push(root.left.val);
        }
        dfs(root.left);
        dfs(root.right);
    };
    dfs(root);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
