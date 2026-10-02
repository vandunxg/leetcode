---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Binary Tree
    - Tree DP
---

<!-- problem:start -->

# [979. Distribute Coins in Binary Tree](https://leetcode.com/problems/distribute-coins-in-binary-tree)

[中文文档](/solution/0900-0999/0979.Distribute%20Coins%20in%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của cây nhị phân có <code>n</code> node, trong đó mỗi <code>node</code> có <code>node.val</code> đồng xu. Tổng cộng toàn cây có <code>n</code> đồng xu.</p>

<p>Trong một thao tác, ta có thể chọn hai node kề nhau và chuyển một đồng xu từ node này sang node kia. Có thể chuyển từ cha xuống con hoặc từ con lên cha.</p>

<p>Trả về <em><strong>số thao tác ít nhất</strong> cần thực hiện để mỗi node có <strong>đúng</strong> một đồng xu</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0979.Distribute%20Coins%20in%20Binary%20Tree/images/tree1.png" style="width: 250px; height: 236px;" />
<pre>
<strong>Đầu vào:</strong> root = [3,0,0]
<strong>Đầu ra:</strong> 2
<strong>Giải thích: </strong>Từ node gốc, ta chuyển một đồng xu sang node con trái và một đồng sang node con phải.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0979.Distribute%20Coins%20in%20Binary%20Tree/images/tree2.png" style="width: 250px; height: 236px;" />
<pre>
<strong>Đầu vào:</strong> root = [0,3,0]
<strong>Đầu ra:</strong> 3
<strong>Giải thích: </strong>Từ node con trái, ta chuyển hai đồng xu lên node gốc [cần hai thao tác]. Sau đó, chuyển một đồng từ node gốc sang node con phải.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây là <code>n</code>.</li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>0 &lt;= Node.val &lt;= n</code></li>
	<li>Tổng tất cả <code>Node.val</code> bằng <code>n</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần chuyển một đồng xu qua một cạnh được tính là một bước, cho đến khi mỗi node có đúng một đồng. Việc xác định lộ trình trên toàn cây khá phức tạp, nhưng phần dư hoặc thiếu của một cây con buộc phải đi qua cạnh nối với node cha: số dư $=\text{coins}-\text{nodes}$, và giá trị tuyệt đối của nó là số đồng xu đi qua cạnh đó. DFS cộng giá trị tuyệt đối của phần dư hoặc thiếu ở hai cây con, rồi trả về tổng phần dư hoặc thiếu.

<!-- thinking:end -->

Ta định nghĩa hàm $\textit{dfs(node)}$ biểu diễn số đồng xu dư trong cây con có gốc tại $\textit{node}$, tức là số đồng xu trừ đi số node. Nếu $\textit{dfs(node)}$ dương, cây con có nhiều đồng xu hơn số node và cần chuyển phần dư ra ngoài; nếu $\textit{dfs(node)}$ âm, cây con có ít đồng xu hơn số node và cần chuyển thêm đồng xu vào.

Trong hàm $\textit{dfs(node)}$, trước tiên ta duyệt cây con trái và phải để lấy số đồng xu dư của chúng, lần lượt là $\textit{left}$ và $\textit{right}$. Sau đó, cộng $|\textit{left}| + |\textit{right}|$ vào số thao tác hiện tại; đây là số đồng xu cần chuyển giữa hai cây con và node hiện tại. Tiếp theo, trả về số đồng xu dư của toàn bộ cây con, bằng $\textit{left} + \textit{right} + \textit{node.val} - 1$.

Cuối cùng, ta trả về số thao tác.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(h)$, trong đó $n$ là số node và $h$ là chiều cao của cây nhị phân.

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
    def distributeCoins(self, root: Optional[TreeNode]) -> int:
        def dfs(root):
            if root is None:
                return 0
            left, right = dfs(root.left), dfs(root.right)
            nonlocal ans
            ans += abs(left) + abs(right)
            return left + right + root.val - 1

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

    public int distributeCoins(TreeNode root) {
        dfs(root);
        return ans;
    }

    private int dfs(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int left = dfs(root.left);
        int right = dfs(root.right);
        ans += Math.abs(left) + Math.abs(right);
        return left + right + root.val - 1;
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
    int distributeCoins(TreeNode* root) {
        int ans = 0;
        function<int(TreeNode*)> dfs = [&](TreeNode* root) -> int {
            if (!root) {
                return 0;
            }
            int left = dfs(root->left);
            int right = dfs(root->right);
            ans += abs(left) + abs(right);
            return left + right + root->val - 1;
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
func distributeCoins(root *TreeNode) (ans int) {
	var dfs func(*TreeNode) int
	dfs = func(root *TreeNode) int {
		if root == nil {
			return 0
		}
		left, right := dfs(root.Left), dfs(root.Right)
		ans += abs(left) + abs(right)
		return left + right + root.Val - 1
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

function distributeCoins(root: TreeNode | null): number {
    let ans = 0;
    const dfs = (root: TreeNode | null) => {
        if (!root) {
            return 0;
        }
        const left = dfs(root.left);
        const right = dfs(root.right);
        ans += Math.abs(left) + Math.abs(right);
        return left + right + root.val - 1;
    };
    dfs(root);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
