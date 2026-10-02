---
comments: true
difficulty: Medium
rating: 1741
source: Weekly Contest 148 Q2
tags:
    - Tree
    - Depth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [1145. Binary Tree Coloring Game](https://leetcode.com/problems/binary-tree-coloring-game)

[中文文档](/solution/1100-1199/1145.Binary%20Tree%20Coloring%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Hai người chơi chơi một trò chơi theo lượt trên cây nhị phân. Cho <code>root</code> của cây và số node <code>n</code>. <code>n</code> là số lẻ và mỗi node có một giá trị riêng biệt từ <code>1</code> đến <code>n</code>.</p>

<p>Ban đầu, người chơi thứ nhất chọn giá trị <code>x</code> sao cho <code>1 &lt;= x &lt;= n</code>, còn người chơi thứ hai chọn giá trị <code>y</code> sao cho <code>1 &lt;= y &lt;= n</code> và <code>y != x</code>. Người chơi thứ nhất tô đỏ node có giá trị <code>x</code>, còn người chơi thứ hai tô xanh node có giá trị <code>y</code>.</p>

<p>Sau đó, hai người chơi lần lượt đi, bắt đầu từ người chơi thứ nhất. Ở mỗi lượt, người chơi chọn một node mang màu của mình (đỏ với người chơi 1, xanh với người chơi 2), rồi tô màu một node <strong>chưa được tô</strong> kề với node đã chọn (là con trái, con phải hoặc cha của node đó).</p>

<p>Một người chơi phải bỏ lượt khi và chỉ khi họ không thể chọn được node nào theo cách trên. Nếu cả hai người chơi đều bỏ lượt, trò chơi kết thúc; người tô được nhiều node hơn sẽ thắng.</p>

<p>Bạn là người chơi thứ hai. Nếu có thể chọn <code>y</code> để đảm bảo thắng trò chơi, hãy trả về <code>true</code>; nếu không thể, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1100-1199/1145.Binary%20Tree%20Coloring%20Game/images/1480-binary-tree-coloring-game.png" style="width: 500px; height: 310px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,2,3,4,5,6,7,8,9,10,11], n = 11, x = 3
<strong>Đầu ra:</strong> true
<strong>Giải thích: </strong>Người chơi thứ hai có thể chọn node có giá trị 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [1,2,3], n = 3, x = 1
<strong>Đầu ra:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây là <code>n</code>.</li>
	<li><code>1 &lt;= x &lt;= n &lt;= 100</code></li>
	<li><code>n</code> là số lẻ.</li>
	<li>1 &lt;= Node.val &lt;= n</li>
	<li>Tất cả giá trị trong cây đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi người chơi thứ nhất chọn $x$, cây được chia thành ba vùng: cây con trái, cây con phải và phần cây phía cha. Người chơi thứ hai tô node gốc của vùng lớn nhất trong ba vùng này để chiếm vùng đó. Nếu vùng chiếm được có hơn $n/2$ node thì người chơi thứ hai thắng. Tìm node $x$ rồi đếm hai cây con sẽ cho kích thước của cả ba vùng.

<!-- thinking:end -->

Trước tiên, dùng DFS để tìm node có giá trị $x$ mà người chơi thứ nhất đã tô, ký hiệu là $node$.

Tiếp theo, đếm số node trong cây con trái và phải của $node$, lần lượt ký hiệu là $l$ và $r$. Số node ở phía node cha của $node$ là $n - l - r - 1$. Nếu $\max(l, r, n - l - r - 1) > \frac{n}{2}$ thì người chơi thứ hai có chiến lược thắng.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là tổng số node trong cây.

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
    def btreeGameWinningMove(self, root: Optional[TreeNode], n: int, x: int) -> bool:
        def dfs(root):
            if root is None or root.val == x:
                return root
            return dfs(root.left) or dfs(root.right)

        def count(root):
            if root is None:
                return 0
            return 1 + count(root.left) + count(root.right)

        node = dfs(root)
        l, r = count(node.left), count(node.right)
        return max(l, r, n - l - r - 1) > n // 2
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
    public boolean btreeGameWinningMove(TreeNode root, int n, int x) {
        TreeNode node = dfs(root, x);
        int l = count(node.left);
        int r = count(node.right);
        return Math.max(Math.max(l, r), n - l - r - 1) > n / 2;
    }

    private TreeNode dfs(TreeNode root, int x) {
        if (root == null || root.val == x) {
            return root;
        }
        TreeNode node = dfs(root.left, x);
        return node == null ? dfs(root.right, x) : node;
    }

    private int count(TreeNode root) {
        if (root == null) {
            return 0;
        }
        return 1 + count(root.left) + count(root.right);
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
    bool btreeGameWinningMove(TreeNode* root, int n, int x) {
        auto node = dfs(root, x);
        int l = count(node->left), r = count(node->right);
        return max({l, r, n - l - r - 1}) > n / 2;
    }

    TreeNode* dfs(TreeNode* root, int x) {
        if (!root || root->val == x) {
            return root;
        }
        auto node = dfs(root->left, x);
        return node ? node : dfs(root->right, x);
    }

    int count(TreeNode* root) {
        if (!root) {
            return 0;
        }
        return 1 + count(root->left) + count(root->right);
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
func btreeGameWinningMove(root *TreeNode, n int, x int) bool {
	var dfs func(*TreeNode) *TreeNode
	dfs = func(root *TreeNode) *TreeNode {
		if root == nil || root.Val == x {
			return root
		}
		node := dfs(root.Left)
		if node != nil {
			return node
		}
		return dfs(root.Right)
	}

	var count func(*TreeNode) int
	count = func(root *TreeNode) int {
		if root == nil {
			return 0
		}
		return 1 + count(root.Left) + count(root.Right)
	}

	node := dfs(root)
	l, r := count(node.Left), count(node.Right)
	return max(max(l, r), n-l-r-1) > n/2
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

function btreeGameWinningMove(root: TreeNode | null, n: number, x: number): boolean {
    const dfs = (root: TreeNode | null): TreeNode | null => {
        if (!root || root.val === x) {
            return root;
        }
        return dfs(root.left) || dfs(root.right);
    };

    const count = (root: TreeNode | null): number => {
        if (!root) {
            return 0;
        }
        return 1 + count(root.left) + count(root.right);
    };

    const node = dfs(root);
    const l = count(node.left);
    const r = count(node.right);
    return Math.max(l, r, n - l - r - 1) > n / 2;
}
```

#### JavaScript

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @param {number} n
 * @param {number} x
 * @return {boolean}
 */
var btreeGameWinningMove = function (root, n, x) {
    const dfs = root => {
        if (!root || root.val === x) {
            return root;
        }
        return dfs(root.left) || dfs(root.right);
    };

    const count = root => {
        if (!root) {
            return 0;
        }
        return 1 + count(root.left) + count(root.right);
    };

    const node = dfs(root);
    const l = count(node.left);
    const r = count(node.right);
    return Math.max(l, r, n - l - r - 1) > n / 2;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
