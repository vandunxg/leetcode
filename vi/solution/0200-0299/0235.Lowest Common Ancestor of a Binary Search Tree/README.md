---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Binary Search Tree
    - Binary Tree
    - Lowest Common Ancestor
    - Binary Lifting
---

<!-- problem:start -->

# [235. Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree)

[中文文档](/solution/0200-0299/0235.Lowest%20Common%20Ancestor%20of%20a%20Binary%20Search%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một cây tìm kiếm nhị phân (BST), hãy tìm node tổ tiên chung thấp nhất (LCA) của hai node đã cho trong BST.</p>

<p>Theo <a href="https://en.wikipedia.org/wiki/Lowest_common_ancestor" target="_blank">định nghĩa về LCA trên Wikipedia</a>: &ldquo;Tổ tiên chung thấp nhất được định nghĩa giữa hai node <code>p</code> và <code>q</code> là node thấp nhất trong <code>T</code> có cả <code>p</code> và <code>q</code> là các node con (trong đó <strong>một node được xem là node con của chính nó</strong>).&rdquo;</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0200-0299/0235.Lowest%20Common%20Ancestor%20of%20a%20Binary%20Search%20Tree/images/binarysearchtree_improved.png" style="width: 200px; height: 190px;" />
<pre>
<strong>Đầu vào:</strong> root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> LCA của các node 2 và 8 là 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0200-0299/0235.Lowest%20Common%20Ancestor%20of%20a%20Binary%20Search%20Tree/images/binarysearchtree_improved.png" style="width: 200px; height: 190px;" />
<pre>
<strong>Đầu vào:</strong> root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> LCA của các node 2 và 4 là 2, vì theo định nghĩa LCA, một node có thể là node con của chính nó.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [2,1], p = 2, q = 1
<strong>Đầu ra:</strong> 2
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[2, 10<sup>5</sup>]</code>.</li>
	<li><code>-10<sup>9</sup> &lt;= Node.val &lt;= 10<sup>9</sup></code></li>
	<li>Tất cả <code>Node.val</code> đều <strong>khác nhau</strong>.</li>
	<li><code>p != q</code></li>
	<li><code>p</code> và <code>q</code> sẽ tồn tại trong BST.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt lặp

<!-- thinking:start -->

> **Tư duy**
>
> Trong một BST, LCA của $p$ và $q$ là điểm phân nhánh giữa các giá trị của chúng. Bắt đầu từ root: đi sang trái nếu cả hai đều nhỏ hơn node hiện tại, đi sang phải nếu cả hai đều lớn hơn, nếu không thì node hiện tại chính là điểm phân nhánh.
>
> Cách duyệt này dùng vòng lặp và không cần call stack.

<!-- thinking:end -->

Bắt đầu từ root, chúng ta duyệt cây. Nếu giá trị của node hiện tại nhỏ hơn cả giá trị của $\textit{p}$ và $\textit{q}$, điều đó có nghĩa là $\textit{p}$ và $\textit{q}$ phải nằm trong cây con bên phải của node hiện tại, nên chúng ta chuyển sang node con bên phải. Nếu giá trị của node hiện tại lớn hơn cả giá trị của $\textit{p}$ và $\textit{q}$, điều đó có nghĩa là $\textit{p}$ và $\textit{q}$ phải nằm trong cây con bên trái, nên chúng ta chuyển sang node con bên trái. Nếu không, node hiện tại chính là tổ tiên chung thấp nhất của $\textit{p}$ và $\textit{q}$, nên chúng ta trả về node hiện tại.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số node trong cây tìm kiếm nhị phân. Độ phức tạp không gian là $O(1)$.

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
        while 1:
            if root.val < min(p.val, q.val):
                root = root.right
            elif root.val > max(p.val, q.val):
                root = root.left
            else:
                return root
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
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        while (true) {
            if (root.val < Math.min(p.val, q.val)) {
                root = root.right;
            } else if (root.val > Math.max(p.val, q.val)) {
                root = root.left;
            } else {
                return root;
            }
        }
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
        while (1) {
            if (root->val < min(p->val, q->val)) {
                root = root->right;
            } else if (root->val > max(p->val, q->val)) {
                root = root->left;
            } else {
                return root;
            }
        }
    }
};
```

#### Go

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val   int
 *     Left  *TreeNode
 *     Right *TreeNode
 * }
 */

func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
	for {
		if root.Val < min(p.Val, q.Val) {
			root = root.Right
		} else if root.Val > max(p.Val, q.Val) {
			root = root.Left
		} else {
			return root
		}
	}
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

function lowestCommonAncestor(
    root: TreeNode | null,
    p: TreeNode | null,
    q: TreeNode | null,
): TreeNode | null {
    while (root) {
        if (root.val > p.val && root.val > q.val) {
            root = root.left;
        } else if (root.val < p.val && root.val < q.val) {
            root = root.right;
        } else {
            return root;
        }
    }
}
```

#### C#

```cs
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left;
 *     public TreeNode right;
 *     public TreeNode(int x) { val = x; }
 * }
 */

public class Solution {
    public TreeNode LowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        while (true) {
            if (root.val < Math.Min(p.val, q.val)) {
                root = root.right;
            } else if (root.val > Math.Max(p.val, q.val)) {
                root = root.left;
            } else {
                return root;
            }
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Quy tắc điểm phân nhánh tương tự cũng có thể được biểu diễn bằng đệ quy: đệ quy vào phía chứa cả hai node, nếu không thì trả về root hiện tại. Ý tưởng này giống với cách lặp nhưng có phần cài đặt ngắn hơn.

<!-- thinking:end -->

Ngoài ra, có thể dùng cách tiếp cận đệ quy để giải bài toán này.

Trước tiên, chúng ta kiểm tra xem giá trị của node hiện tại có nhỏ hơn cả giá trị của $\textit{p}$ và $\textit{q}$ hay không. Nếu có, chúng ta đệ quy duyệt cây con bên phải. Nếu giá trị của node hiện tại lớn hơn cả giá trị của $\textit{p}$ và $\textit{q}$, chúng ta đệ quy duyệt cây con bên trái. Nếu không, node hiện tại chính là tổ tiên chung thấp nhất của $\textit{p}$ và $\textit{q}$, nên chúng ta trả về node hiện tại.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó $n$ là số node trong cây tìm kiếm nhị phân.

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
        if root.val < min(p.val, q.val):
            return self.lowestCommonAncestor(root.right, p, q)
        if root.val > max(p.val, q.val):
            return self.lowestCommonAncestor(root.left, p, q)
        return root
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
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root.val < Math.min(p.val, q.val)) {
            return lowestCommonAncestor(root.right, p, q);
        }
        if (root.val > Math.max(p.val, q.val)) {
            return lowestCommonAncestor(root.left, p, q);
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
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */

class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root->val < min(p->val, q->val)) {
            return lowestCommonAncestor(root->right, p, q);
        }
        if (root->val > max(p->val, q->val)) {
            return lowestCommonAncestor(root->left, p, q);
        }
        return root;
    }
};
```

#### Go

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val   int
 *     Left  *TreeNode
 *     Right *TreeNode
 * }
 */

func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
	if root.Val < p.Val && root.Val < q.Val {
		return lowestCommonAncestor(root.Right, p, q)
	}
	if root.Val > p.Val && root.Val > q.Val {
		return lowestCommonAncestor(root.Left, p, q)
	}
	return root
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

function lowestCommonAncestor(
    root: TreeNode | null,
    p: TreeNode | null,
    q: TreeNode | null,
): TreeNode | null {
    if (root.val > p.val && root.val > q.val) {
        return lowestCommonAncestor(root.left, p, q);
    }
    if (root.val < p.val && root.val < q.val) {
        return lowestCommonAncestor(root.right, p, q);
    }
    return root;
}
```

#### C#

```cs
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left;
 *     public TreeNode right;
 *     public TreeNode(int x) { val = x; }
 * }
 */

public class Solution {
    public TreeNode LowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root.val < Math.Min(p.val, q.val)) {
            return LowestCommonAncestor(root.right, p, q);
        }
        if (root.val > Math.Max(p.val, q.val)) {
            return LowestCommonAncestor(root.left, p, q);
        }
        return root;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
