---
comments: true
difficulty: Medium
rating: 1405
source: Weekly Contest 190 Q3
tags:
    - Bit Manipulation
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [1457. Pseudo-Palindromic Paths in a Binary Tree](https://leetcode.com/problems/pseudo-palindromic-paths-in-a-binary-tree)

[中文文档](/solution/1400-1499/1457.Pseudo-Palindromic%20Paths%20in%20a%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một cây nhị phân có giá trị các node là các chữ số từ 1 đến 9. Một đường đi trong cây nhị phân được gọi là <strong>pseudo-palindromic</strong> nếu có ít nhất một hoán vị của các giá trị node trên đường đi tạo thành một palindrome.</p>

<p><em>Hãy trả về số đường đi <strong>pseudo-palindromic</strong> từ node gốc đến các node lá.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1457.Pseudo-Palindromic%20Paths%20in%20a%20Binary%20Tree/images/palindromic_paths_1.png" style="width: 300px; height: 201px;" /></p>

<pre>
<strong>Đầu vào:</strong> root = [2,3,1,3,1,null,1]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Hình trên biểu diễn cây nhị phân đã cho. Có ba đường đi từ node gốc đến các node lá: đường màu đỏ [2,3,3], đường màu xanh lá [2,1,1] và đường [2,3,1]. Trong số các đường đi này, chỉ đường màu đỏ và đường màu xanh lá là các đường pseudo-palindromic, vì đường màu đỏ [2,3,3] có thể được sắp xếp lại thành [3,2,3] (palindrome), còn đường màu xanh lá [2,1,1] có thể được sắp xếp lại thành [1,2,1] (palindrome).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1457.Pseudo-Palindromic%20Paths%20in%20a%20Binary%20Tree/images/palindromic_paths_2.png" style="width: 300px; height: 314px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> root = [2,1,1,1,3,null,null,null,null,null,1]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Hình trên biểu diễn cây nhị phân đã cho. Có ba đường đi từ node gốc đến các node lá: đường màu xanh lá [2,1,1], đường [2,1,3,1] và đường [2,1]. Trong số các đường đi này, chỉ đường màu xanh lá là pseudo-palindromic, vì [2,1,1] có thể được sắp xếp lại thành [1,2,1] (palindrome).
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [9]
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 10<sup>5</sup>]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS + Bit Manipulation

<!-- thinking:start -->

> **Tư duy**
>
> Một đường đi là pseudo-palindromic khi và chỉ khi có nhiều nhất một giá trị xuất hiện số lần lẻ. Các giá trị nằm trong khoảng $1$– $9$ và $n\le 10^5$, nên ta XOR một mask 10 bit trên đường đi.
>
> Tại node lá, đếm đường đi nếu $mask$ có nhiều nhất một bit được bật. Tiếp tục đệ quy với mask đã cập nhật và cộng kết quả của hai node con.

<!-- thinking:end -->

Một đường đi là pseudo-palindromic khi và chỉ khi số node xuất hiện số lần lẻ trên đường đi là $0$ hoặc $1$.

Vì miền giá trị của các node trong cây nhị phân là từ $1$ đến $9$, với mỗi đường đi từ node gốc đến node lá, ta có thể dùng một số nhị phân $10$ bit $mask$ để biểu diễn trạng thái xuất hiện của các giá trị node trên đường đi hiện tại. Bit thứ $i$ của $mask$ là $1$ nếu giá trị node $i$ xuất hiện số lần lẻ trên đường đi hiện tại, và là $0$ nếu xuất hiện số lần chẵn. Do đó, một đường đi là pseudo-palindromic khi và chỉ khi $mask \&(mask - 1) = 0$, trong đó $\&$ là phép AND bit.

Dựa trên phân tích trên, ta có thể dùng phương pháp tìm kiếm theo chiều sâu để tính số đường đi. Ta định nghĩa hàm $dfs(root, mask)$, biểu diễn số đường đi pseudo-palindromic bắt đầu từ node $root$ hiện tại với trạng thái $mask$ hiện tại. Đáp án là $dfs(root, 0)$.

Logic thực thi của hàm $dfs(root, mask)$ như sau:

Nếu $root$ là null, trả về $0$;

Ngược lại, đặt $mask = mask \oplus 2^{root.val}$, trong đó $\oplus$ là phép XOR bit.

Nếu $root$ là node lá, trả về $1$ nếu $mask \&(mask - 1) = 0$, ngược lại trả về $0$;

Nếu $root$ không phải node lá, trả về $dfs(root.left, mask) + dfs(root.right, mask)$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số node trong cây nhị phân.

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
    def pseudoPalindromicPaths(self, root: Optional[TreeNode]) -> int:
        def dfs(root: Optional[TreeNode], mask: int):
            if root is None:
                return 0
            mask ^= 1 << root.val
            if root.left is None and root.right is None:
                return int((mask & (mask - 1)) == 0)
            return dfs(root.left, mask) + dfs(root.right, mask)

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
    public int pseudoPalindromicPaths(TreeNode root) {
        return dfs(root, 0);
    }

    private int dfs(TreeNode root, int mask) {
        if (root == null) {
            return 0;
        }
        mask ^= 1 << root.val;
        if (root.left == null && root.right == null) {
            return (mask & (mask - 1)) == 0 ? 1 : 0;
        }
        return dfs(root.left, mask) + dfs(root.right, mask);
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
    int pseudoPalindromicPaths(TreeNode* root) {
        function<int(TreeNode*, int)> dfs = [&](TreeNode* root, int mask) {
            if (!root) {
                return 0;
            }
            mask ^= 1 << root->val;
            if (!root->left && !root->right) {
                return (mask & (mask - 1)) == 0 ? 1 : 0;
            }
            return dfs(root->left, mask) + dfs(root->right, mask);
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
func pseudoPalindromicPaths(root *TreeNode) int {
	var dfs func(*TreeNode, int) int
	dfs = func(root *TreeNode, mask int) int {
		if root == nil {
			return 0
		}
		mask ^= 1 << root.Val
		if root.Left == nil && root.Right == nil {
			if mask&(mask-1) == 0 {
				return 1
			}
			return 0
		}
		return dfs(root.Left, mask) + dfs(root.Right, mask)
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

function pseudoPalindromicPaths(root: TreeNode | null): number {
    const dfs = (root: TreeNode | null, mask: number): number => {
        if (!root) {
            return 0;
        }
        mask ^= 1 << root.val;
        if (!root.left && !root.right) {
            return (mask & (mask - 1)) === 0 ? 1 : 0;
        }
        return dfs(root.left, mask) + dfs(root.right, mask);
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
    pub fn pseudo_palindromic_paths(root: Option<Rc<RefCell<TreeNode>>>) -> i32 {
        fn dfs(root: Option<Rc<RefCell<TreeNode>>>, mask: i32) -> i32 {
            if let Some(node) = root {
                let mut mask = mask;
                let val = node.borrow().val;
                mask ^= 1 << val;

                if node.borrow().left.is_none() && node.borrow().right.is_none() {
                    return if (mask & (mask - 1)) == 0 { 1 } else { 0 };
                }

                return (dfs(node.borrow().left.clone(), mask)
                    + dfs(node.borrow().right.clone(), mask));
            }
            0
        }

        dfs(root, 0)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
