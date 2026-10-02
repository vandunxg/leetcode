---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Hash Table
    - Binary Tree
---

<!-- problem:start -->

# [1660. Correct a Binary Tree 🔒](https://leetcode.com/problems/correct-a-binary-tree)

[中文文档](/solution/1600-1699/1660.Correct%20a%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có một cây nhị phân bị lỗi nhỏ. Có <strong>chính xác một</strong> node không hợp lệ, trong đó con phải của nó trỏ nhầm đến một node khác ở <strong>cùng độ sâu</strong> và nằm về <b>bên phải của node không hợp lệ</b>.</p>

<p>Cho root <code>root</code> của cây nhị phân bị lỗi, hãy trả về <em>root của cây nhị phân sau khi <strong>xóa</strong> node không hợp lệ này <strong>và mọi node bên dưới nó</strong> (trừ node mà nó trỏ nhầm đến).</em></p>

<p><strong>Kiểm thử tùy chỉnh:</strong></p>

<p>Input kiểm thử được đọc thành 3 dòng:</p>

<ul>
	<li><code>TreeNode root</code></li>
	<li><code>int fromNode</code> (<strong>not available to </strong><code>correctBinaryTree</code>)</li>
	<li><code>int toNode</code> (<strong>not available to </strong><code>correctBinaryTree</code>)</li>
</ul>

<p>Sau khi cây nhị phân có root là <code>root</code> được phân tích, con trỏ con phải của <code>TreeNode</code> có giá trị <code>fromNode</code> sẽ trỏ đến <code>TreeNode</code> có giá trị <code>toNode</code>. Sau đó, <code>root</code> được truyền vào <code>correctBinaryTree</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1660.Correct%20a%20Binary%20Tree/images/ex1v2.png" style="width: 250px; height: 177px;" /></strong></p>

<pre>
<strong>Input:</strong> root = [1,2,3], fromNode = 2, toNode = 3
<strong>Output:</strong> [1,null,3]
<strong>Explanation:</strong> Node có giá trị 2 không hợp lệ nên xóa node này.
</pre>

<p><strong class="example">Example 2:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1660.Correct%20a%20Binary%20Tree/images/ex2v3.png" style="width: 350px; height: 255px;" /></strong></p>

<pre>
<strong>Input:</strong> root = [8,3,1,7,null,9,4,2,null,null,null,5,6], fromNode = 7, toNode = 4
<strong>Output:</strong> [8,3,1,null,null,9,4,null,null,5,6]
<strong>Explanation:</strong> Node có giá trị 7 không hợp lệ nên xóa node này cùng node bên dưới nó là node 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[3, 10<sup>4</sup>]</code>.</li>
	<li><code>-10<sup>9</sup> &lt;= Node.val &lt;= 10<sup>9</sup></code></li>
	<li>All <code>Node.val</code> are <strong>unique</strong>.</li>
	<li><code>fromNode != toNode</code></li>
	<li><code>fromNode</code> and <code>toNode</code> will exist in the tree and will be on the same depth.</li>
	<li><code>toNode</code> is to the <strong>right</strong> of <code>fromNode</code>.</li>
	<li><code>fromNode.right</code> is <code>null</code> in the initial tree from the test data.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Có đúng một con trỏ phải trỏ nhầm đến node cùng mức ở bên phải; ta xóa node lỗi đó và toàn bộ cây con của nó. Khi duyệt theo thứ tự phải trước rồi trái, node đích của cạnh lỗi được thăm trước node nguồn.
>
> Ta duyệt preorder theo thứ tự phải rồi trái: nếu con phải hiện tại đã nằm trong tập các node đã thăm, node hiện tại là node lỗi và ta trả về null để cắt bỏ nó.
>
> Một set $\textit{vis}$ lưu các node đã thăm; đệ quy cập nhật lại các con, có thể là null.

<!-- thinking:end -->

Ta xây dựng hàm `dfs(root)` để xử lý cây con có `root` làm gốc. Nếu `root` là `null` hoặc `root.right` đã được thăm, `root` là node không hợp lệ nên ta trả về `null`. Ngược lại, ta lần lượt đệ quy xử lý `root.right` và `root.left`, rồi trả về `root`.

Cuối cùng, trả về `dfs(root)`.

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
    def correctBinaryTree(self, root: TreeNode) -> TreeNode:
        def dfs(root):
            if root is None or root.right in vis:
                return None
            vis.add(root)
            root.right = dfs(root.right)
            root.left = dfs(root.left)
            return root

        vis = set()
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
    private Set<TreeNode> vis = new HashSet<>();

    public TreeNode correctBinaryTree(TreeNode root) {
        return dfs(root);
    }

    private TreeNode dfs(TreeNode root) {
        if (root == null || vis.contains(root.right)) {
            return null;
        }
        vis.add(root);
        root.right = dfs(root.right);
        root.left = dfs(root.left);
        return root;
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
    TreeNode* correctBinaryTree(TreeNode* root) {
        unordered_set<TreeNode*> vis;
        function<TreeNode*(TreeNode*)> dfs = [&](TreeNode* root) -> TreeNode* {
            if (!root || vis.count(root->right)) {
                return nullptr;
            }
            vis.insert(root);
            root->right = dfs(root->right);
            root->left = dfs(root->left);
            return root;
        };
        return dfs(root);
    }
};
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
 * @param {number} from
 * @param {number} to
 * @return {TreeNode}
 */
var correctBinaryTree = function (root) {
    const dfs = root => {
        if (!root || vis.has(root.right)) {
            return null;
        }
        vis.add(root);
        root.right = dfs(root.right);
        root.left = dfs(root.left);
        return root;
    };
    const vis = new Set();
    return dfs(root);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
