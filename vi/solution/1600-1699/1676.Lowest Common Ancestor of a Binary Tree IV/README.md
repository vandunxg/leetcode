---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Hash Table
    - Binary Tree
    - Lowest Common Ancestor
    - Binary Lifting
---

<!-- problem:start -->

# [1676. Lowest Common Ancestor of a Binary Tree IV 🔒](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree-iv)

[中文文档](/solution/1600-1699/1676.Lowest%20Common%20Ancestor%20of%20a%20Binary%20Tree%20IV/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một cây nhị phân và mảng các đối tượng <code>TreeNode</code> <code>nodes</code>, hãy trả về <em>tổ tiên chung gần nhất (LCA) của <strong>tất cả node</strong> trong </em><code>nodes</code>. Tất cả node đều tồn tại trong cây và mọi giá trị node đều <strong>khác nhau</strong>.</p>

<p>Mở rộng từ <strong><a href="https://en.wikipedia.org/wiki/Lowest_common_ancestor" target="_blank">định nghĩa LCA trên Wikipedia</a></strong>: &quot;Tổ tiên chung gần nhất của <code>n</code> node <code>p<sub>1</sub></code>, <code>p<sub>2</sub></code>, ..., <code>p<sub>n</sub></code> trong cây nhị phân <code>T</code> là node thấp nhất có mọi <code>p<sub>i</sub></code> là <strong>hậu duệ</strong> (cho phép <b>một node là hậu duệ của chính nó</b>) với mọi <code>i</code> hợp lệ&quot;. <strong>Hậu duệ</strong> của node <code>x</code> là node <code>y</code> nằm trên đường đi từ node <code>x</code> đến một node lá.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1676.Lowest%20Common%20Ancestor%20of%20a%20Binary%20Tree%20IV/images/binarytree.png" />
<pre>
<strong>Input:</strong> root = [3,5,1,6,2,0,8,null,null,7,4], nodes = [4,7]
<strong>Output:</strong> 2
<strong>Giải thích:</strong> Tổ tiên chung gần nhất của node 4 và 7 là node 2.
</pre>

<p><strong class="example">Example 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1676.Lowest%20Common%20Ancestor%20of%20a%20Binary%20Tree%20IV/images/binarytree.png" />
<pre>
<strong>Input:</strong> root = [3,5,1,6,2,0,8,null,null,7,4], nodes = [1]
<strong>Output:</strong> 1
<strong>Giải thích:</strong> Tổ tiên chung gần nhất của một node duy nhất là chính node đó.

</pre>

<p><strong class="example">Example 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1676.Lowest%20Common%20Ancestor%20of%20a%20Binary%20Tree%20IV/images/binarytree.png" />
<pre>
<strong>Input:</strong> root = [3,5,1,6,2,0,8,null,null,7,4], nodes = [7,6,2,4]
<strong>Output:</strong> 5
<strong>Giải thích:</strong> Tổ tiên chung gần nhất của các node 7, 6, 2 và 4 là node 5.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 10<sup>4</sup>]</code>.</li>
	<li><code>-10<sup>9</sup> &lt;= Node.val &lt;= 10<sup>9</sup></code></li>
	<li>All <code>Node.val</code> are <strong>unique</strong>.</li>
	<li>Mọi <code>nodes[i]</code> đều tồn tại trong cây.</li>
	<li>All <code>nodes[i]</code> are distinct.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + DFS

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần LCA của một tập node, tất cả đều có trong cây. Sau khi đưa các node đích vào một set, đệ quy LCA thông thường vẫn hoạt động: một cây con trả về node đích hoặc tổ tiên đã tìm được.
>
> Trả về ngay nếu node hiện tại là node đích; nếu cả hai phía đều khác rỗng thì node hiện tại là tổ tiên, ngược lại truyền nhánh khác rỗng lên trên.

<!-- thinking:end -->

Ta dùng hash table $\textit{s}$ để lưu giá trị của mọi node trong mảng $\textit{nodes}$, sau đó dùng depth-first search. Nếu node đang duyệt là null hoặc giá trị của nó có trong hash table $\textit{s}$, ta trả về node hiện tại. Nếu không, ta đệ quy duyệt cây con trái và phải. Nếu kết quả trả về từ cả hai cây con đều khác null, node hiện tại là tổ tiên chung gần nhất. Ngược lại, trả về kết quả khác null của cây con.

Độ phức tạp thời gian là $O(n + m)$ và độ phức tạp không gian là $O(n + m)$, trong đó $n$ là số node trong cây nhị phân và $m$ là độ dài mảng $\textit{nodes}$.

<!-- tabs:start -->

#### Python3

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None


class Solution:
    def lowestCommonAncestor(
        self, root: 'TreeNode', nodes: 'List[TreeNode]'
    ) -> 'TreeNode':
        def dfs(root):
            if root is None or root.val in s:
                return root
            left, right = dfs(root.left), dfs(root.right)
            if left and right:
                return root
            return left or right

        s = {node.val for node in nodes}
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
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    private Set<Integer> s = new HashSet<>();

    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode[] nodes) {
        for (TreeNode node : nodes) {
            s.add(node.val);
        }
        return dfs(root);
    }

    private TreeNode dfs(TreeNode root) {
        if (root == null || s.contains(root.val)) {
            return root;
        }
        TreeNode left = dfs(root.left);
        TreeNode right = dfs(root.right);
        if (left == null) {
            return right;
        }
        if (right == null) {
            return left;
        }
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
    TreeNode* lowestCommonAncestor(TreeNode* root, vector<TreeNode*>& nodes) {
        unordered_set<int> s;
        for (auto node : nodes) {
            s.insert(node->val);
        }
        auto dfs = [&](this auto&& dfs, TreeNode* root) -> TreeNode* {
            if (!root || s.contains(root->val)) {
                return root;
            }
            auto left = dfs(root->left);
            auto right = dfs(root->right);
            if (!left) {
                return right;
            }
            if (!right) {
                return left;
            }
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
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @param {TreeNode[]} nodes
 * @return {TreeNode}
 */
var lowestCommonAncestor = function (root, nodes) {
    const s = new Set();
    for (const node of nodes) {
        s.add(node.val);
    }
    function dfs(root) {
        if (!root || s.has(root.val)) {
            return root;
        }
        const [left, right] = [dfs(root.left), dfs(root.right)];
        if (left && right) {
            return root;
        }
        return left || right;
    }
    return dfs(root);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
