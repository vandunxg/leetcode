---
comments: true
difficulty: Medium
rating: 1374
source: Weekly Contest 135 Q2
tags:
    - Tree
    - Depth-First Search
    - Binary Search Tree
    - Binary Tree
---

<!-- problem:start -->

# [1038. Binary Search Tree to Greater Sum Tree](https://leetcode.com/problems/binary-search-tree-to-greater-sum-tree)

[中文文档](/solution/1000-1099/1038.Binary%20Search%20Tree%20to%20Greater%20Sum%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một cây tìm kiếm nhị phân (BST), hãy chuyển đổi nó thành Greater Tree sao cho mỗi key ban đầu được thay bằng tổng của key đó và tất cả các key lớn hơn nó trong BST.</p>

<p>Nhắc lại, <em>cây tìm kiếm nhị phân</em> là cây thỏa mãn các ràng buộc sau:</p>

<ul>
	<li>Cây con trái của một node chỉ chứa các node có key <strong>nhỏ hơn</strong> key của node đó.</li>
	<li>Cây con phải của một node chỉ chứa các node có key <strong>lớn hơn</strong> key của node đó.</li>
	<li>Cả cây con trái và cây con phải cũng phải là cây tìm kiếm nhị phân.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1038.Binary%20Search%20Tree%20to%20Greater%20Sum%20Tree/images/tree.png" style="width: 400px; height: 273px;" />
<pre>
<strong>Đầu vào:</strong> root = [4,1,6,0,2,5,7,null,null,null,3,null,null,null,8]
<strong>Đầu ra:</strong> [30,36,21,36,35,26,15,null,null,null,33,null,null,null,8]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [0,null,1]
<strong>Đầu ra:</strong> [1,null,1]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 100]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 100</code></li>
	<li>Tất cả giá trị trong cây đều <strong>khác nhau</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Lưu ý:</strong> Bài này giống bài 538: <a href="https://leetcode.com/problems/convert-bst-to-greater-tree/" target="_blank">https://leetcode.com/problems/convert-bst-to-greater-tree/</a></p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi node cần được đổi thành tổng các key lớn hơn hoặc bằng key ban đầu của nó. Duyệt inorder ngược (phải, gốc, trái) sẽ thăm các node của BST từ lớn đến nhỏ, nên có thể dùng tổng tích lũy để cập nhật trực tiếp giá trị. Vì $n\le 100$, dùng đệ quy là phù hợp.
>
> Đệ quy trên cây con phải, cộng giá trị hiện tại vào $s$ rồi ghi lại vào node, sau đó đệ quy trên cây con trái.
>
> Chỉ cần duyệt cây một lần để chuyển đổi.

<!-- thinking:end -->

Duyệt cây tìm kiếm nhị phân theo thứ tự "phải-gốc-trái". Cộng dồn giá trị các node đã gặp vào $s$, rồi gán tổng tích lũy đó cho `node` tương ứng.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, với $n$ là số node trong cây tìm kiếm nhị phân.

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
    def bstToGst(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        def dfs(root: Optional[TreeNode]):
            if root is None:
                return
            dfs(root.right)
            nonlocal s
            s += root.val
            root.val = s
            dfs(root.left)

        s = 0
        dfs(root)
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
    private int s;

    public TreeNode bstToGst(TreeNode root) {
        dfs(root);
        return root;
    }

    private void dfs(TreeNode root) {
        if (root == null) {
            return;
        }
        dfs(root.right);
        s += root.val;
        root.val = s;
        dfs(root.left);
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
    TreeNode* bstToGst(TreeNode* root) {
        int s = 0;
        auto dfs = [&](this auto&& dfs, TreeNode* root) {
            if (!root) {
                return;
            }
            dfs(root->right);
            s += root->val;
            root->val = s;
            dfs(root->left);
        };
        dfs(root);
        return root;
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
func bstToGst(root *TreeNode) *TreeNode {
	s := 0
	var dfs func(*TreeNode)
	dfs = func(root *TreeNode) {
		if root == nil {
			return
		}
		dfs(root.Right)
		s += root.Val
		root.Val = s
		dfs(root.Left)
	}
	dfs(root)
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

function bstToGst(root: TreeNode | null): TreeNode | null {
    let s = 0;
    const dfs = (root: TreeNode | null) => {
        if (!root) {
            return;
        }
        dfs(root.right);
        s += root.val;
        root.val = s;
        dfs(root.left);
    };
    dfs(root);
    return root;
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
use std::rc::Rc;
use std::cell::RefCell;

impl Solution {
    pub fn bst_to_gst(root: Option<Rc<RefCell<TreeNode>>>) -> Option<Rc<RefCell<TreeNode>>> {
        let mut s = 0;
        Self::dfs(&root, &mut s);
        root
    }

    fn dfs(root: &Option<Rc<RefCell<TreeNode>>>, s: &mut i32) {
        if let Some(node) = root {
            let mut node = node.borrow_mut();
            Self::dfs(&node.right, s);
            *s += node.val;
            node.val = *s;
            Self::dfs(&node.left, s);
        }
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
 * @return {TreeNode}
 */
var bstToGst = function (root) {
    let s = 0;
    function dfs(root) {
        if (!root) {
            return;
        }
        dfs(root.right);
        s += root.val;
        root.val = s;
        dfs(root.left);
    }
    dfs(root);
    return root;
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

int dfs(struct TreeNode* root, int sum) {
    if (root) {
        sum = dfs(root->right, sum) + root->val;
        root->val = sum;
        sum = dfs(root->left, sum);
    }
    return sum;
}

struct TreeNode* bstToGst(struct TreeNode* root) {
    dfs(root, 0);
    return root;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Morris Traversal

<!-- thinking:start -->

> **Tư duy**
>
> Stack đệ quy tăng theo chiều cao cây, nhưng có thể dùng bộ nhớ phụ hằng số. Morris traversal tạo một liên kết tạm thời từ node ngoài cùng bên trái của cây con phải đến node kế nhiệm, rồi duyệt theo thứ tự inorder ngược tương tự mà không cần stack.
>
> Nếu node không có cây con phải: xử lý node đó rồi đi sang trái. Nếu có, tìm node kế nhiệm: nếu chưa có liên kết tạm thì tạo liên kết rồi đi sang phải; nếu đã có liên kết, xử lý node, gỡ liên kết rồi đi sang trái. Cách cộng dồn giống Lời giải 1.

<!-- thinking:end -->

Morris traversal không cần stack, có độ phức tạp thời gian $O(n)$ và độ phức tạp không gian $O(1)$. Ý tưởng chính như sau:

Đặt $s$ là tổng tích lũy các giá trị node trong cây tìm kiếm nhị phân. Duyệt các node của cây như sau:

1. Nếu cây con phải của node hiện tại `root` là null, **cộng giá trị node hiện tại vào $s$**, cập nhật giá trị node hiện tại thành $s$, rồi chuyển node hiện tại sang `root.left`.
2. Nếu cây con phải của node hiện tại `root` không null, tìm node ngoài cùng bên trái `next` trong cây con phải (tức node kế nhiệm của `root` khi duyệt inorder):
    - Nếu cây con trái của node kế nhiệm `next` là null, cho cây con trái của `next` trỏ đến node hiện tại `root`, rồi chuyển node hiện tại sang `root.right`.
    - Nếu cây con trái của node kế nhiệm `next` không null, **cộng giá trị node hiện tại vào $s$**, cập nhật giá trị node hiện tại thành $s$, sau đó đặt cây con trái của `next` thành null (tức gỡ liên kết giữa `next` và `root`), rồi chuyển node hiện tại sang `root.left`.
3. Lặp lại các bước trên cho đến khi node hiện tại là null; khi đó quá trình duyệt kết thúc.
4. Cuối cùng, trả về node gốc của cây tìm kiếm nhị phân.

> Morris traversal inorder ngược dùng cùng ý tưởng với Morris traversal inorder, chỉ thay đổi thứ tự duyệt từ "trái-gốc-phải" thành "phải-gốc-trái".

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
    def bstToGst(self, root: TreeNode) -> TreeNode:
        s = 0
        node = root
        while root:
            if root.right is None:
                s += root.val
                root.val = s
                root = root.left
            else:
                next = root.right
                while next.left and next.left != root:
                    next = next.left
                if next.left is None:
                    next.left = root
                    root = root.right
                else:
                    s += root.val
                    root.val = s
                    next.left = None
                    root = root.left
        return node
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
    public TreeNode bstToGst(TreeNode root) {
        int s = 0;
        TreeNode node = root;
        while (root != null) {
            if (root.right == null) {
                s += root.val;
                root.val = s;
                root = root.left;
            } else {
                TreeNode next = root.right;
                while (next.left != null && next.left != root) {
                    next = next.left;
                }
                if (next.left == null) {
                    next.left = root;
                    root = root.right;
                } else {
                    s += root.val;
                    root.val = s;
                    next.left = null;
                    root = root.left;
                }
            }
        }
        return node;
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
    TreeNode* bstToGst(TreeNode* root) {
        int s = 0;
        TreeNode* node = root;
        while (root) {
            if (root->right == nullptr) {
                s += root->val;
                root->val = s;
                root = root->left;
            } else {
                TreeNode* next = root->right;
                while (next->left && next->left != root) {
                    next = next->left;
                }
                if (next->left == nullptr) {
                    next->left = root;
                    root = root->right;
                } else {
                    s += root->val;
                    root->val = s;
                    next->left = nullptr;
                    root = root->left;
                }
            }
        }
        return node;
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
func bstToGst(root *TreeNode) *TreeNode {
	s := 0
	node := root
	for root != nil {
		if root.Right == nil {
			s += root.Val
			root.Val = s
			root = root.Left
		} else {
			next := root.Right
			for next.Left != nil && next.Left != root {
				next = next.Left
			}
			if next.Left == nil {
				next.Left = root
				root = root.Right
			} else {
				s += root.Val
				root.Val = s
				next.Left = nil
				root = root.Left
			}
		}
	}
	return node
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

function bstToGst(root: TreeNode | null): TreeNode | null {
    let cur = root;
    let sum = 0;
    while (cur != null) {
        const { val, left, right } = cur;
        if (right == null) {
            sum += val;
            cur.val = sum;
            cur = left;
        } else {
            let next = right;
            while (next.left != null && next.left != cur) {
                next = next.left;
            }
            if (next.left == null) {
                next.left = cur;
                cur = right;
            } else {
                next.left = null;
                sum += val;
                cur.val = sum;
                cur = left;
            }
        }
    }
    return root;
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

struct TreeNode* bstToGst(struct TreeNode* root) {
    struct TreeNode* cur = root;
    int sum = 0;
    while (cur) {
        if (!cur->right) {
            sum += cur->val;
            cur->val = sum;
            cur = cur->left;
        } else {
            struct TreeNode* next = cur->right;
            while (next->left && next->left != cur) {
                next = next->left;
            }
            if (!next->left) {
                next->left = cur;
                cur = cur->right;
            } else {
                next->left = NULL;
                sum += cur->val;
                cur->val = sum;
                cur = cur->left;
            }
        }
    }
    return root;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
