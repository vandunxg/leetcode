---
comments: true
difficulty: Easy
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [404. Sum of Left Leaves](https://leetcode.com/problems/sum-of-left-leaves)

[中文文档](/solution/0400-0499/0404.Sum%20of%20Left%20Leaves/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một binary tree, hãy trả về <em>tổng giá trị của tất cả left leaf.</em></p>

<p><strong>Leaf</strong> là node không có node con. <strong>Left leaf</strong> là leaf đồng thời là node con bên trái của một node khác.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0404.Sum%20of%20Left%20Leaves/images/leftsum-tree.jpg" style="width: 277px; height: 302px;" />
<pre>
<strong>Đầu vào:</strong> root = [3,9,20,null,null,15,7]
<strong>Đầu ra:</strong> 24
<strong>Giải thích:</strong> Trong binary tree có hai left leaf, lần lượt có giá trị 9 và 15.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [1]
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số lượng node trong cây nằm trong khoảng <code>[1, 1000]</code>.</li>
	<li><code>-1000 &lt;= Node.val &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Left leaf là node con bên trái không có node con, chứ không phải toàn bộ cây con bên trái. Duyệt lại cây con từ mỗi node sẽ khiến một số node bị xét nhiều lần.
>
> Chỉ cần một lượt đệ quy: luôn duyệt node con bên phải; nếu node con bên trái là leaf thì cộng giá trị của nó, nếu không thì đệ quy tiếp. Cây rỗng đóng góp $0$. Có thể nhận biết leaf khi cả hai node con đều null, không cần duyệt thêm.

<!-- thinking:end -->

Trước tiên, kiểm tra `root` có null không. Nếu có, trả về $0$.

Nếu không, đệ quy gọi hàm `sumOfLeftLeaves` để tính tổng các left leaf trong cây con bên phải của `root`, rồi gán kết quả cho biến $ans$. Tiếp theo, kiểm tra `root` có node con bên trái hay không. Nếu có, kiểm tra node đó có phải leaf không. Nếu đúng, cộng giá trị của nó vào $ans$. Nếu không, đệ quy gọi `sumOfLeftLeaves` để tính tổng các left leaf trong cây con bên trái của `root`, rồi cộng kết quả vào $ans$.

Cuối cùng, trả về kết quả.

Độ phức tạp thời gian và không gian đều là $O(n)$, trong đó $n$ là số node trong binary tree.

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
    def sumOfLeftLeaves(self, root: Optional[TreeNode]) -> int:
        if root is None:
            return 0
        ans = self.sumOfLeftLeaves(root.right)
        if root.left:
            if root.left.left == root.left.right:
                ans += root.left.val
            else:
                ans += self.sumOfLeftLeaves(root.left)
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
    public int sumOfLeftLeaves(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int ans = sumOfLeftLeaves(root.right);
        if (root.left != null) {
            if (root.left.left == root.left.right) {
                ans += root.left.val;
            } else {
                ans += sumOfLeftLeaves(root.left);
            }
        }
        return ans;
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
    int sumOfLeftLeaves(TreeNode* root) {
        if (!root) {
            return 0;
        }
        int ans = sumOfLeftLeaves(root->right);
        if (root->left) {
            if (!root->left->left && !root->left->right) {
                ans += root->left->val;
            } else {
                ans += sumOfLeftLeaves(root->left);
            }
        }
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
func sumOfLeftLeaves(root *TreeNode) int {
	if root == nil {
		return 0
	}
	ans := sumOfLeftLeaves(root.Right)
	if root.Left != nil {
		if root.Left.Left == root.Left.Right {
			ans += root.Left.Val
		} else {
			ans += sumOfLeftLeaves(root.Left)
		}
	}
	return ans
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

function sumOfLeftLeaves(root: TreeNode | null): number {
    if (!root) {
        return 0;
    }
    let ans = sumOfLeftLeaves(root.right);
    if (root.left) {
        if (root.left.left === root.left.right) {
            ans += root.left.val;
        } else {
            ans += sumOfLeftLeaves(root.left);
        }
    }
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
    fn dfs(root: &Option<Rc<RefCell<TreeNode>>>, is_left: bool) -> i32 {
        if root.is_none() {
            return 0;
        }
        let node = root.as_ref().unwrap().borrow();
        let left = &node.left;
        let right = &node.right;
        if left.is_none() && right.is_none() {
            if is_left {
                return node.val;
            }
            return 0;
        }
        Self::dfs(left, true) + Self::dfs(right, false)
    }

    pub fn sum_of_left_leaves(root: Option<Rc<RefCell<TreeNode>>>) -> i32 {
        Self::dfs(&root, false)
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

int sumOfLeftLeaves(struct TreeNode* root) {
    if (!root) {
        return 0;
    }
    int ans = sumOfLeftLeaves(root->right);
    if (root->left) {
        if (!root->left->left && !root->left->right) {
            ans += root->left->val;
        } else {
            ans += sumOfLeftLeaves(root->left);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Stack

<!-- thinking:start -->

> **Tư duy**
>
> Có thể áp dụng cách phân loại tương tự bằng stack tường minh: pop một node, cộng giá trị nếu node con trái là leaf hoặc push node con trái vào stack; nếu có node con phải thì cũng push vào stack.
>
> Độ phức tạp không đổi; ta dùng stack tường minh thay cho call stack.

<!-- thinking:end -->

Cũng có thể chuyển lời giải đệ quy ở Lời giải 1 thành vòng lặp, dùng stack để mô phỏng quá trình đệ quy.

Tương tự Lời giải 1, trước tiên kiểm tra `root` có null không. Nếu có, trả về $0$.

Nếu không, khởi tạo biến kết quả $ans$ bằng $0$, sau đó khởi tạo stack $stk$ và thêm `root` vào stack.

Khi stack chưa rỗng, pop phần tử trên cùng `root` ra. Nếu `root` có node con bên trái, kiểm tra node đó có phải leaf không. Nếu đúng, cộng giá trị của nó vào $ans$; nếu không, thêm node con bên trái vào stack. Sau đó, nếu `root` có node con bên phải thì thêm node đó vào stack.

Cuối cùng, trả về kết quả.

Độ phức tạp thời gian và không gian đều là $O(n)$, trong đó $n$ là số node trong binary tree.

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
    def sumOfLeftLeaves(self, root: Optional[TreeNode]) -> int:
        if root is None:
            return 0
        ans = 0
        stk = [root]
        while stk:
            root = stk.pop()
            if root.left:
                if root.left.left == root.left.right:
                    ans += root.left.val
                else:
                    stk.append(root.left)
            if root.right:
                stk.append(root.right)
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
    public int sumOfLeftLeaves(TreeNode root) {
        if (root == null) {
            return 0;
        }
        Deque<TreeNode> stk = new ArrayDeque<>();
        stk.push(root);
        int ans = 0;
        while (!stk.isEmpty()) {
            root = stk.pop();
            if (root.left != null) {
                if (root.left.left == root.left.right) {
                    ans += root.left.val;
                } else {
                    stk.push(root.left);
                }
            }
            if (root.right != null) {
                stk.push(root.right);
            }
        }
        return ans;
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
    int sumOfLeftLeaves(TreeNode* root) {
        if (!root) {
            return 0;
        }
        int ans = 0;
        stack<TreeNode*> stk{{root}};
        while (!stk.empty()) {
            root = stk.top(), stk.pop();
            if (root->left) {
                if (!root->left->left && !root->left->right) {
                    ans += root->left->val;
                } else {
                    stk.push(root->left);
                }
            }
            if (root->right) {
                stk.push(root->right);
            }
        }
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
func sumOfLeftLeaves(root *TreeNode) (ans int) {
	if root == nil {
		return 0
	}
	stk := []*TreeNode{root}
	for len(stk) > 0 {
		root = stk[len(stk)-1]
		stk = stk[:len(stk)-1]
		if root.Left != nil {
			if root.Left.Left == root.Left.Right {
				ans += root.Left.Val
			} else {
				stk = append(stk, root.Left)
			}
		}
		if root.Right != nil {
			stk = append(stk, root.Right)
		}
	}
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

function sumOfLeftLeaves(root: TreeNode | null): number {
    if (!root) {
        return 0;
    }
    let ans = 0;
    const stk: TreeNode[] = [root];
    while (stk.length) {
        const { left, right } = stk.pop()!;
        if (left) {
            if (left.left === left.right) {
                ans += left.val;
            } else {
                stk.push(left);
            }
        }
        if (right) {
            stk.push(right);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
