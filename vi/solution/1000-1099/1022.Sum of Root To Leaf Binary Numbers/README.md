---
comments: true
difficulty: Easy
rating: 1462
source: Weekly Contest 131 Q2
tags:
    - Tree
    - Depth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [1022. Sum of Root To Leaf Binary Numbers](https://leetcode.com/problems/sum-of-root-to-leaf-binary-numbers)

[中文文档](/solution/1000-1099/1022.Sum%20of%20Root%20To%20Leaf%20Binary%20Numbers/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một binary tree, trong đó mỗi node có giá trị <code>0</code> hoặc <code>1</code>. Mỗi đường đi từ root đến leaf biểu diễn một số nhị phân, bắt đầu bằng bit có trọng số lớn nhất.</p>

<ul>
	<li>Ví dụ, nếu đường đi là <code>0 -&gt; 1 -&gt; 1 -&gt; 0 -&gt; 1</code>, thì nó biểu diễn số nhị phân <code>01101</code>, tức là <code>13</code>.</li>
</ul>

<p>Với mỗi leaf trong cây, xét số được biểu diễn bởi đường đi từ root đến leaf đó. Trả về <em>tổng các số này</em>.</p>

<p>Các test case được tạo sao cho đáp án nằm trong phạm vi của số nguyên <strong>32 bit</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1022.Sum%20of%20Root%20To%20Leaf%20Binary%20Numbers/images/sum-of-root-to-leaf-binary-numbers.png" style="width: 400px; height: 263px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,0,1,0,1,0,1]
<strong>Đầu ra:</strong> 22
<strong>Giải thích: </strong>(100) + (101) + (110) + (111) = 4 + 5 + 6 + 7 = 22
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [0]
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong đoạn <code>[1, 1000]</code>.</li>
	<li><code>Node.val</code> bằng <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Liệt kê mọi đường đi từ root đến leaf rồi chuyển các bit thành số vẫn khả thi với $n\le 1000$, nhưng cần thêm bộ nhớ để lưu các đường đi. Khi duyệt xuống cây, giá trị đường đi được cập nhật theo $t\leftarrow 2t+\textit{val}$ và hoàn chỉnh khi đến leaf.
>
> Node null đóng góp $0$. Node không có con là leaf và trả về $t$ hiện tại; nếu không, ta truyền cùng giá trị $t$ cho hai cây con rồi cộng kết quả.
>
> DFS mang theo giá trị của đường đi và chỉ duyệt mỗi node một lần.

<!-- thinking:end -->

Ta xây dựng hàm đệ quy $\text{dfs}(root, t)$ với hai tham số: node hiện tại $root$ và số nhị phân $t$ tương ứng với node cha của node hiện tại. Hàm trả về tổng các số nhị phân được biểu diễn bởi những đường đi từ node hiện tại đến các leaf. Đáp án là $\textrm{dfs}(root, 0)$.

Hàm đệ quy hoạt động như sau:

- Nếu node hiện tại $root$ là null, trả về $0$; nếu không, tính số nhị phân $t$ tương ứng với node hiện tại theo công thức $t = t \ll 1 | root.val$.
- Nếu node hiện tại là leaf, trả về $t$; nếu không, trả về tổng $\textrm{dfs}(root.left, t)$ và $\textrm{dfs}(root.right, t)$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node trong binary tree. Mỗi node được duyệt một lần; call stack của đệ quy cần $O(n)$ không gian.

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
    def sumRootToLeaf(self, root: Optional[TreeNode]) -> int:
        def dfs(root: Optional[TreeNode], x: int) -> int:
            if root is None:
                return 0
            x = x << 1 | root.val
            if root.left == root.right:
                return x
            return dfs(root.left, x) + dfs(root.right, x)

        return dfs(root, 0)
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
    public int sumRootToLeaf(TreeNode root) {
        return dfs(root, 0);
    }

    private int dfs(TreeNode root, int x) {
        if (root == null) {
            return 0;
        }
        x = x << 1 | root.val;
        if (root.left == root.right) {
            return x;
        }
        return dfs(root.left, x) + dfs(root.right, x);
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
    int sumRootToLeaf(TreeNode* root) {
        auto dfs = [&](this auto&& dfs, TreeNode* root, int x) -> int {
            if (!root) {
                return 0;
            }
            x = x << 1 | root->val;
            if (root->left == root->right) {
                return x;
            }
            return dfs(root->left, x) + dfs(root->right, x);
        };
        return dfs(root, 0);
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
func sumRootToLeaf(root *TreeNode) int {
	var dfs func(*TreeNode, int) int
	dfs = func(root *TreeNode, x int) int {
		if root == nil {
			return 0
		}
		x = x<<1 | root.Val
		if root.Left == root.Right {
			return x
		}
		return dfs(root.Left, x) + dfs(root.Right, x)
	}
	return dfs(root, 0)
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

function sumRootToLeaf(root: TreeNode | null): number {
    const dfs = (node: TreeNode | null, x: number): number => {
        if (node === null) {
            return 0;
        }

        x = (x << 1) | node.val;

        if (node.left === null && node.right === null) {
            return x;
        }

        return dfs(node.left, x) + dfs(node.right, x);
    };

    return dfs(root, 0);
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
    pub fn sum_root_to_leaf(root: Option<Rc<RefCell<TreeNode>>>) -> i32 {
        fn dfs(node: Option<Rc<RefCell<TreeNode>>>, x: i32) -> i32 {
            if let Some(n) = node {
                let n_ref = n.borrow();
                let x = (x << 1) | n_ref.val;

                if n_ref.left.is_none() && n_ref.right.is_none() {
                    return x;
                }

                dfs(n_ref.left.clone(), x) + dfs(n_ref.right.clone(), x)
            } else {
                0
            }
        }

        dfs(root, 0)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
