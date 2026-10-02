---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Hash Table
    - Binary Tree
    - Lowest Common Ancestor
    - Tree DP
    - Binary Lifting
---

<!-- problem:start -->

# [865. Smallest Subtree with all the Deepest Nodes](https://leetcode.com/problems/smallest-subtree-with-all-the-deepest-nodes)

[中文文档](/solution/0800-0899/0865.Smallest%20Subtree%20with%20all%20the%20Deepest%20Nodes/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một cây nhị phân. Độ sâu của mỗi node là <strong>khoảng cách ngắn nhất đến root</strong>.</p>

<p>Hãy trả về <em>cây con nhỏ nhất</em> chứa <strong>tất cả node sâu nhất</strong> trong cây ban đầu.</p>

<p>Node được gọi là <strong>sâu nhất</strong> nếu có độ sâu lớn nhất trong số tất cả node của cây.</p>

<p><strong>Cây con</strong> của một node là cây gồm node đó cùng với toàn bộ hậu duệ của nó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0865.Smallest%20Subtree%20with%20all%20the%20Deepest%20Nodes/images/sketch1.png" style="width: 600px; height: 510px;" />
<pre>
<strong>Đầu vào:</strong> root = [3,5,1,6,2,0,8,null,null,7,4]
<strong>Đầu ra:</strong> [2,7,4]
<strong>Giải thích:</strong> Ta trả về node có giá trị 2, được tô màu vàng trong hình.
Các node được tô màu xanh dương là những node sâu nhất trong cây.
Lưu ý, các node 5, 3 và 2 đều có cây con chứa các node sâu nhất, nhưng cây con có gốc là node 2 là nhỏ nhất, nên ta trả về node này.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [1]
<strong>Đầu ra:</strong> [1]
<strong>Giải thích:</strong> Node gốc là node sâu nhất trong cây.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [0,1,3,null,2]
<strong>Đầu ra:</strong> [2]
<strong>Giải thích:</strong> Node sâu nhất trong cây là 2. Các cây con hợp lệ là cây con có gốc tại node 2, 1 và 0, nhưng cây con của node 2 là nhỏ nhất.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 500]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 500</code></li>
	<li>Giá trị các node trong cây là <strong>duy nhất</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Lưu ý:</strong> Bài này giống với bài 1123: <a href="https://leetcode.com/problems/lowest-common-ancestor-of-deepest-leaves/" target="_blank">https://leetcode.com/problems/lowest-common-ancestor-of-deepest-leaves/</a></p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Cây con nhỏ nhất chứa tất cả lá sâu nhất chính là LCA của chúng. Tính độ sâu rồi tìm LCA cần hai lượt duyệt; một lượt hậu tự có thể trả về cả node gốc tiềm năng lẫn độ sâu.
>
> Nếu một node con sâu hơn, đáp án nằm ở nhánh đó; nếu độ sâu bằng nhau, node hiện tại bao phủ cả hai nhánh. Node null có độ sâu $0$.

<!-- thinking:end -->

Ta thiết kế hàm $\textit{dfs}(\textit{root})$ trả về cây con nhỏ nhất chứa tất cả node sâu nhất trong cây con có gốc $\textit{root}$, đồng thời trả về độ sâu của cây con có gốc $\textit{root}$.

Hàm $\textit{dfs}(\textit{root})$ hoạt động như sau:

- Nếu $\textit{root}$ là null, trả về $\text{null}$ và $0$.
- Nếu không, đệ quy tính cây con nhỏ nhất và độ sâu của cây con trái, cây con phải của $\textit{root}$; lần lượt ký hiệu là $l$, $l_d$ và $r$, $r_d$. Nếu $l_d > r_d$, cây con nhỏ nhất chứa mọi node sâu nhất ở nhánh trái là $l$, với độ sâu $l_d + 1$. Nếu $l_d < r_d$, cây con nhỏ nhất chứa mọi node sâu nhất ở nhánh phải là $r$, với độ sâu $r_d + 1$. Nếu $l_d = r_d$, thì $\textit{root}$ là gốc của cây con nhỏ nhất chứa tất cả node sâu nhất, với độ sâu $l_d + 1$.

Cuối cùng, trả về phần tử đầu tiên trong kết quả của $\textit{dfs}(\textit{root})$.

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
    def subtreeWithAllDeepest(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        def dfs(root: Optional[TreeNode]) -> Tuple[Optional[TreeNode], int]:
            if root is None:
                return None, 0
            l, ld = dfs(root.left)
            r, rd = dfs(root.right)
            if ld > rd:
                return l, ld + 1
            if ld < rd:
                return r, rd + 1
            return root, ld + 1

        return dfs(root)[0]
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
    public TreeNode subtreeWithAllDeepest(TreeNode root) {
        return dfs(root).getKey();
    }

    private Pair<TreeNode, Integer> dfs(TreeNode root) {
        if (root == null) {
            return new Pair<>(null, 0);
        }
        var l = dfs(root.left);
        var r = dfs(root.right);
        int ld = l.getValue(), rd = r.getValue();
        if (ld > rd) {
            return new Pair<>(l.getKey(), ld + 1);
        }
        if (ld < rd) {
            return new Pair<>(r.getKey(), rd + 1);
        }
        return new Pair<>(root, ld + 1);
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
    TreeNode* subtreeWithAllDeepest(TreeNode* root) {
        using pti = pair<TreeNode*, int>;
        auto dfs = [&](this auto&& dfs, TreeNode* root) -> pti {
            if (!root) {
                return {nullptr, 0};
            }
            auto [l, ld] = dfs(root->left);
            auto [r, rd] = dfs(root->right);
            if (ld > rd) {
                return {l, ld + 1};
            }
            if (ld < rd) {
                return {r, rd + 1};
            }
            return {root, ld + 1};
        };
        return dfs(root).first;
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
func subtreeWithAllDeepest(root *TreeNode) *TreeNode {
	type pair struct {
		node  *TreeNode
		depth int
	}
	var dfs func(*TreeNode) pair
	dfs = func(root *TreeNode) pair {
		if root == nil {
			return pair{nil, 0}
		}
		l, r := dfs(root.Left), dfs(root.Right)
		ld, rd := l.depth, r.depth
		if ld > rd {
			return pair{l.node, ld + 1}
		}
		if ld < rd {
			return pair{r.node, rd + 1}
		}
		return pair{root, ld + 1}
	}
	return dfs(root).node
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

function subtreeWithAllDeepest(root: TreeNode | null): TreeNode | null {
    const dfs = (root: TreeNode | null): [TreeNode, number] => {
        if (!root) {
            return [null, 0];
        }
        const [l, ld] = dfs(root.left);
        const [r, rd] = dfs(root.right);
        if (ld > rd) {
            return [l, ld + 1];
        }
        if (ld < rd) {
            return [r, rd + 1];
        }
        return [root, ld + 1];
    };
    return dfs(root)[0];
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
    pub fn subtree_with_all_deepest(
        root: Option<Rc<RefCell<TreeNode>>>,
    ) -> Option<Rc<RefCell<TreeNode>>> {
        fn dfs(root: Option<Rc<RefCell<TreeNode>>>) -> (Option<Rc<RefCell<TreeNode>>>, i32) {
            if let Some(node) = root {
                let left = node.borrow().left.clone();
                let right = node.borrow().right.clone();

                let (l, ld) = dfs(left);
                let (r, rd) = dfs(right);

                if ld > rd {
                    (l, ld + 1)
                } else if ld < rd {
                    (r, rd + 1)
                } else {
                    (Some(node), ld + 1)
                }
            } else {
                (None, 0)
            }
        }

        dfs(root).0
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
