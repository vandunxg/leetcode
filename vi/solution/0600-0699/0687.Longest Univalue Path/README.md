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

# [687. Longest Univalue Path](https://leetcode.com/problems/longest-univalue-path)

[中文文档](/solution/0600-0699/0687.Longest%20Univalue%20Path/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một binary tree, hãy trả về <em>độ dài của đường đi dài nhất mà mọi node trên đường đi đều có cùng giá trị</em>. Đường đi này có thể đi qua hoặc không đi qua root.</p>

<p><strong>Độ dài đường đi</strong> giữa hai node được tính bằng số cạnh nối chúng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0687.Longest%20Univalue%20Path/images/ex1.jpg" style="width: 450px; height: 238px;" />
<pre>
<strong>Đầu vào:</strong> root = [5,4,5,1,1,null,5]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Hình minh họa cho thấy đường đi dài nhất gồm các node có cùng giá trị là 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0687.Longest%20Univalue%20Path/images/ex2.jpg" style="width: 450px; height: 238px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,4,5,4,4,null,5]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Hình minh họa cho thấy đường đi dài nhất gồm các node có cùng giá trị là 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[0, 10<sup>4</sup>]</code>.</li>
	<li><code>-1000 &lt;= Node.val &lt;= 1000</code></li>
	<li>Độ sâu của cây không vượt quá <code>1000</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Đường đi gồm các node cùng giá trị có thể rẽ qua một node và được tính theo số cạnh. Chỉ xét đường đi từ root đến leaf sẽ bỏ sót trường hợp ghép nhánh trái với nhánh phải.
>
> DFS trả về độ dài lớn nhất của nhánh đi xuống mà các node có cùng giá trị; chỉ tăng độ dài nếu node con có cùng giá trị với node hiện tại. Cập nhật đáp án bằng $l+r$ rồi trả về $\max(l,r)$.

<!-- thinking:end -->

Ta định nghĩa hàm $\textit{dfs}(root)$ trả về độ dài đường đi gồm các node cùng giá trị dài nhất đi xuống từ node $\textit{root}$, trong đó $\textit{root}$ là một đầu mút của đường đi.

Trong $\textit{dfs}(root)$, trước tiên ta gọi đệ quy $\textit{dfs}(root.\textit{left})$ và $\textit{dfs}(root.\textit{right})$ để nhận hai giá trị trả về $\textit{l}$ và $\textit{r}$. Chúng lần lượt biểu thị độ dài đường đi gồm các node cùng giá trị dài nhất đi xuống từ cây con trái và cây con phải của $\textit{root}$.

Nếu $\textit{root}$ có node con trái và $\textit{root}.\textit{val} = \textit{root}.\textit{left}.\textit{val}$, độ dài nhánh cùng giá trị đi xuống qua node con trái là $\textit{l} + 1$; nếu không thì độ dài bằng $0$. Tương tự, nếu $\textit{root}$ có node con phải và $\textit{root}.\textit{val} = \textit{root}.\textit{right}.\textit{val}$, độ dài nhánh qua node con phải là $\textit{r} + 1$; nếu không thì bằng $0$.

Sau khi gọi đệ quy cho cây con trái và phải, ta cập nhật đáp án thành $\max(\textit{ans}, \textit{l} + \textit{r})$. Giá trị này biểu thị độ dài đường đi cùng giá trị dài nhất đi qua $\textit{root}$, với $\textit{root}$ là điểm nối hai nhánh.

Cuối cùng, hàm $\textit{dfs}(root)$ trả về độ dài nhánh cùng giá trị dài nhất đi xuống từ $\textit{root}$, bằng $\max(\textit{l}, \textit{r})$.

Trong hàm chính, ta gọi $\textit{dfs}(root)$ để tính đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, với $n$ là số node trong binary tree.

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
    def longestUnivaluePath(self, root: Optional[TreeNode]) -> int:
        def dfs(root: Optional[TreeNode]) -> int:
            if root is None:
                return 0
            l, r = dfs(root.left), dfs(root.right)
            l = l + 1 if root.left and root.left.val == root.val else 0
            r = r + 1 if root.right and root.right.val == root.val else 0
            nonlocal ans
            ans = max(ans, l + r)
            return max(l, r)

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

    public int longestUnivaluePath(TreeNode root) {
        dfs(root);
        return ans;
    }

    private int dfs(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int l = dfs(root.left);
        int r = dfs(root.right);
        l = root.left != null && root.left.val == root.val ? l + 1 : 0;
        r = root.right != null && root.right.val == root.val ? r + 1 : 0;
        ans = Math.max(ans, l + r);
        return Math.max(l, r);
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
    int longestUnivaluePath(TreeNode* root) {
        int ans = 0;
        auto dfs = [&](this auto&& dfs, TreeNode* root) -> int {
            if (!root) {
                return 0;
            }
            int l = dfs(root->left);
            int r = dfs(root->right);
            l = root->left && root->left->val == root->val ? l + 1 : 0;
            r = root->right && root->right->val == root->val ? r + 1 : 0;
            ans = max(ans, l + r);
            return max(l, r);
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
func longestUnivaluePath(root *TreeNode) (ans int) {
	var dfs func(*TreeNode) int
	dfs = func(root *TreeNode) int {
		if root == nil {
			return 0
		}
		l, r := dfs(root.Left), dfs(root.Right)
		if root.Left != nil && root.Left.Val == root.Val {
			l++
		} else {
			l = 0
		}
		if root.Right != nil && root.Right.Val == root.Val {
			r++
		} else {
			r = 0
		}
		ans = max(ans, l+r)
		return max(l, r)
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

function longestUnivaluePath(root: TreeNode | null): number {
    let ans: number = 0;
    const dfs = (root: TreeNode | null): number => {
        if (!root) {
            return 0;
        }
        let [l, r] = [dfs(root.left), dfs(root.right)];
        l = root.left && root.left.val === root.val ? l + 1 : 0;
        r = root.right && root.right.val === root.val ? r + 1 : 0;
        ans = Math.max(ans, l + r);
        return Math.max(l, r);
    };
    dfs(root);
    return ans;
}
```

#### Rust

```rust
// Definition for a binary tree node.
// #[derive(Debug, PartialEq, Eq)]
// pub struct TreeNode {
//   pub val: i32,
//   pub left: Option<Rc<RefCell<TreeNode>>>,
//   pub right: Option<Rc<RefCell<TreeNode>>>,
// }
//
// impl TreeNode {
//   #[inline]
//   pub fn new(val: i32) -> Self {
//     TreeNode {
//       val,
//       left: None,
//       right: None
//     }
//   }
// }
use std::cell::RefCell;
use std::rc::Rc;

impl Solution {
    fn dfs(root: &Option<Rc<RefCell<TreeNode>>>, target: i32, ans: &mut i32) -> i32 {
        if root.is_none() {
            return 0;
        }

        let root = root.as_ref().unwrap().borrow();
        let left = Self::dfs(&root.left, root.val, ans);
        let right = Self::dfs(&root.right, root.val, ans);

        *ans = (*ans).max(left + right);

        if root.val == target {
            return left.max(right) + 1;
        }

        0
    }

    pub fn longest_univalue_path(root: Option<Rc<RefCell<TreeNode>>>) -> i32 {
        if root.is_none() {
            return 0;
        }

        let mut ans = 0;
        Self::dfs(&root, root.as_ref().unwrap().borrow().val, &mut ans);
        ans
    }
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
 * @return {number}
 */
var longestUnivaluePath = function (root) {
    let ans = 0;
    const dfs = root => {
        if (!root) {
            return 0;
        }
        let [l, r] = [dfs(root.left), dfs(root.right)];
        l = root.left && root.left.val === root.val ? l + 1 : 0;
        r = root.right && root.right.val === root.val ? r + 1 : 0;
        ans = Math.max(ans, l + r);
        return Math.max(l, r);
    };
    dfs(root);
    return ans;
};
```

#### C

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */
#define max(a, b) (((a) > (b)) ? (a) : (b))

int dfs(struct TreeNode* root, int* ans) {
    if (!root) {
        return 0;
    }
    int l = dfs(root->left, ans);
    int r = dfs(root->right, ans);
    l = root->left && root->left->val == root->val ? l + 1 : 0;
    r = root->right && root->right->val == root->val ? r + 1 : 0;
    *ans = max(*ans, l + r);
    return max(l, r);
}

int longestUnivaluePath(struct TreeNode* root) {
    int ans = 0;
    dfs(root, &ans);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
