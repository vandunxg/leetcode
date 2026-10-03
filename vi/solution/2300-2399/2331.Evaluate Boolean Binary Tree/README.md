---
comments: true
difficulty: Easy
rating: 1303
source: Biweekly Contest 82 Q1
tags:
    - Tree
    - Depth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [2331. Evaluate Boolean Binary Tree](https://leetcode.com/problems/evaluate-boolean-binary-tree)

[中文文档](/solution/2300-2399/2331.Evaluate%20Boolean%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho <code>root</code> của một <strong>cây nhị phân đầy đủ</strong> với các tính chất sau:</p>

<ul>
	<li><strong>Nút lá</strong> có giá trị <code>0</code> hoặc <code>1</code>, trong đó <code>0</code> biểu thị <code>False</code> và <code>1</code> biểu thị <code>True</code>.</li>
	<li><strong>Nút không phải lá</strong> có giá trị <code>2</code> hoặc <code>3</code>, trong đó <code>2</code> biểu thị phép toán boolean <code>OR</code> và <code>3</code> biểu thị phép toán boolean <code>AND</code>.</li>
</ul>

<p>Việc <strong>đánh giá</strong> một nút được thực hiện như sau:</p>

<ul>
	<li>Nếu nút là nút lá, kết quả đánh giá là <strong>giá trị</strong> của nút, tức <code>True</code> hoặc <code>False</code>.</li>
	<li>Ngược lại, hãy <strong>đánh giá</strong> hai nút con của nút đó và <strong>áp dụng</strong> phép toán boolean tương ứng với giá trị của nó lên kết quả đánh giá của hai nút con.</li>
</ul>

<p>Hãy trả về<em> kết quả boolean của việc <strong>đánh giá</strong> nút </em><code>root</code><em>.</em></p>

<p><strong>Cây nhị phân đầy đủ</strong> là cây nhị phân trong đó mỗi nút có <code>0</code> hoặc <code>2</code> nút con.</p>

<p><strong>Nút lá</strong> là nút không có nút con.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2331.Evaluate%20Boolean%20Binary%20Tree/images/example1drawio1.png" style="width: 700px; height: 252px;" />
<pre>
<strong>Đầu vào:</strong> root = [2,1,3,null,null,0,1]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Sơ đồ trên minh họa quá trình đánh giá.
Nút AND được đánh giá là False AND True = False.
Nút OR được đánh giá là True OR False = True.
Nút root được đánh giá là True, vì vậy ta trả về true.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [0]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Nút root là nút lá và được đánh giá là false, vì vậy ta trả về false.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số lượng nút trong cây nằm trong khoảng <code>[1, 1000]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 3</code></li>
	<li>Mỗi nút có <code>0</code> hoặc <code>2</code> nút con.</li>
	<li>Các nút lá có giá trị <code>0</code> hoặc <code>1</code>.</li>
	<li>Các nút không phải lá có giá trị <code>2</code> hoặc <code>3</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Các nút lá là boolean; các nút bên trong là OR hoặc AND. Vì có nhiều nhất $1000$ nút, ta có thể đánh giá theo đúng định nghĩa.
>
> Một nút con trái bị thiếu nghĩa là nút hiện tại là nút lá. Nếu không, ta đệ quy trên cả hai nút con rồi áp dụng $or$ hoặc $and$ dựa trên giá trị của nút. Mỗi nút được duyệt đúng một lần.

<!-- thinking:end -->

Ta có thể sử dụng đệ quy để giải bài toán này.

Với nút hiện tại $\textit{root}$:

- Nếu nút con trái của nó là null, điều đó có nghĩa nút hiện tại là một nút lá. Nếu giá trị của nút hiện tại là $1$, trả về $\textit{true}$; ngược lại, trả về $\textit{false}$;
- Nếu giá trị của nút hiện tại là $2$, trả về phép OR logic của kết quả đệ quy trên nút con trái và nút con phải; ngược lại, trả về phép AND logic của kết quả đệ quy trên nút con trái và nút con phải.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số lượng nút trong cây nhị phân.

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
    def evaluateTree(self, root: Optional[TreeNode]) -> bool:
        if root.left is None:
            return bool(root.val)
        op = or_ if root.val == 2 else and_
        return op(self.evaluateTree(root.left), self.evaluateTree(root.right))
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
    public boolean evaluateTree(TreeNode root) {
        if (root.left == null) {
            return root.val == 1;
        }
        if (root.val == 2) {
            return evaluateTree(root.left) || evaluateTree(root.right);
        }
        return evaluateTree(root.left) && evaluateTree(root.right);
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
    bool evaluateTree(TreeNode* root) {
        if (!root->left) {
            return root->val;
        }
        if (root->val == 2) {
            return evaluateTree(root->left) || evaluateTree(root->right);
        }
        return evaluateTree(root->left) && evaluateTree(root->right);
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
func evaluateTree(root *TreeNode) bool {
	if root.Left == nil {
		return root.Val == 1
	}
	if root.Val == 2 {
		return evaluateTree(root.Left) || evaluateTree(root.Right)
	} else {
		return evaluateTree(root.Left) && evaluateTree(root.Right)
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

function evaluateTree(root: TreeNode | null): boolean {
    const { val, left, right } = root;
    if (left === null) {
        return val === 1;
    }
    if (val === 2) {
        return evaluateTree(left) || evaluateTree(right);
    }
    return evaluateTree(left) && evaluateTree(right);
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
    pub fn evaluate_tree(root: Option<Rc<RefCell<TreeNode>>>) -> bool {
        match root {
            Some(node) => {
                let node = node.borrow();
                if node.left.is_none() {
                    return node.val == 1;
                }
                if node.val == 2 {
                    return Self::evaluate_tree(node.left.clone())
                        || Self::evaluate_tree(node.right.clone());
                }
                Self::evaluate_tree(node.left.clone()) && Self::evaluate_tree(node.right.clone())
            }
            None => false,
        }
    }
}
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
bool evaluateTree(struct TreeNode* root) {
    if (!root->left) {
        return root->val == 1;
    }
    if (root->val == 2) {
        return evaluateTree(root->left) || evaluateTree(root->right);
    }
    return evaluateTree(root->left) && evaluateTree(root->right);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
