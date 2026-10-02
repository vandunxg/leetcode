---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Binary Tree
    - Lowest Common Ancestor
    - Binary Lifting
---

<!-- problem:start -->

# [1644. Lowest Common Ancestor of a Binary Tree II 🔒](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree-ii)

[中文文档](/solution/1600-1699/1644.Lowest%20Common%20Ancestor%20of%20a%20Binary%20Tree%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một cây nhị phân, hãy trả về <em>tổ tiên chung thấp nhất (LCA) của hai nút </em><code>p</code><em> và </em><code>q</code>. Nếu nút <code>p</code> hoặc <code>q</code> <strong>không tồn tại</strong> trong cây, trả về <code>null</code>. Mọi giá trị nút trong cây là <strong>duy nhất</strong>.</p>

<p>Theo <strong><a href="https://en.wikipedia.org/wiki/Lowest_common_ancestor" target="_blank">định nghĩa LCA trên Wikipedia</a></strong>: &quot;Tổ tiên chung thấp nhất của hai nút <code>p</code> và <code>q</code> trong cây nhị phân <code>T</code> là nút thấp nhất có cả <code>p</code> và <code>q</code> là <strong>hậu duệ</strong> (trong đó <b>một nút được xem là hậu duệ của chính nó</b>)&quot;. <strong>Hậu duệ</strong> của nút <code>x</code> là nút <code>y</code> nằm trên đường đi từ nút <code>x</code> đến một nút lá.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1644.Lowest%20Common%20Ancestor%20of%20a%20Binary%20Tree%20II/images/binarytree.png" />
<pre>
<strong>Input:</strong> root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
<strong>Output:</strong> 3
<strong>Explanation:</strong> LCA của các nút 5 và 1 là 3.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1644.Lowest%20Common%20Ancestor%20of%20a%20Binary%20Tree%20II/images/binarytree.png" /></p>

<pre>
<strong>Input:</strong> root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
<strong>Output:</strong> 5
<strong>Explanation:</strong> LCA của các nút 5 và 4 là 5. Theo định nghĩa LCA, một nút có thể là hậu duệ của chính nó.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1644.Lowest%20Common%20Ancestor%20of%20a%20Binary%20Tree%20II/images/binarytree.png" /></p>

<pre>
<strong>Input:</strong> root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 10
<strong>Output:</strong> null
<strong>Explanation:</strong> Nút 10 không tồn tại trong cây, nên trả về null.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số nút trong cây nằm trong khoảng <code>[1, 10<sup>4</sup>]</code>.</li>
	<li><code>-10<sup>9</sup> &lt;= Node.val &lt;= 10<sup>9</sup></code></li>
	<li>Mọi <code>Node.val</code> đều <strong>khác nhau</strong>.</li>
	<li><code>p != q</code></li>
</ul>

<p>&nbsp;</p>
<strong>Câu hỏi mở rộng:</strong>&nbsp;Bạn có thể tìm LCA bằng cách duyệt cây mà không kiểm tra sự tồn tại của các nút không?

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Không giống LCA thông thường, $p$ hoặc $q$ có thể vắng mặt, nên chỉ được trả về một nút sau khi đã tìm thấy cả hai.
>
> Duyệt hậu tự cho biết một cây con có chứa nút đích hay không. Nếu cả hai nhánh đều chứa nút đích, hoặc một nhánh chứa và nút hiện tại là nút đích còn lại, nút hiện tại là LCA.
>
> Hàm đệ quy trả về boolean; đáp án thật được lưu trong $\textit{ans}$, nhờ đó việc “tìm thấy một nút” không bị nhầm là đã tìm thấy tổ tiên.

<!-- thinking:end -->

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
        self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode'
    ) -> 'TreeNode':
        def dfs(root, p, q):
            if root is None:
                return False
            l = dfs(root.left, p, q)
            r = dfs(root.right, p, q)
            nonlocal ans
            if l and r:
                ans = root
            if (l or r) and (root.val == p.val or root.val == q.val):
                ans = root
            return l or r or root.val == p.val or root.val == q.val

        ans = None
        dfs(root, p, q)
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
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    private TreeNode ans;

    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        dfs(root, p, q);
        return ans;
    }

    private boolean dfs(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null) {
            return false;
        }
        boolean l = dfs(root.left, p, q);
        boolean r = dfs(root.right, p, q);
        if (l && r) {
            ans = root;
        }
        if ((l || r) && (root.val == p.val || root.val == q.val)) {
            ans = root;
        }
        return l || r || root.val == p.val || root.val == q.val;
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
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        dfs(root, p, q);
        return ans;
    }

private:
    TreeNode* ans = nullptr;

    bool dfs(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (!root) {
            return false;
        }
        bool l = dfs(root->left, p, q);
        bool r = dfs(root->right, p, q);
        if (l && r) {
            ans = root;
        }
        if ((l || r) && (root->val == p->val || root->val == q->val)) {
            ans = root;
        }
        return l || r || root->val == p->val || root->val == q->val;
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
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {TreeNode}
 */
var lowestCommonAncestor = function (root, p, q) {
    const dfs = root => {
        if (!root) {
            return false;
        }
        const l = dfs(root.left);
        const r = dfs(root.right);
        if (l && r) {
            ans = root;
        }
        if ((l || r) && (root.val === p.val || root.val === q.val)) {
            ans = root;
        }
        return l || r || root.val === p.val || root.val === q.val;
    };
    let ans = null;
    dfs(root);
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
