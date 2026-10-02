---
comments: true
difficulty: Medium
rating: 1562
source: Weekly Contest 127 Q4
tags:
    - Stack
    - Tree
    - Binary Search Tree
    - Array
    - Binary Tree
    - Monotonic Stack
---

<!-- problem:start -->

# [1008. Construct Binary Search Tree from Preorder Traversal](https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal)

[中文文档](/solution/1000-1099/1008.Construct%20Binary%20Search%20Tree%20from%20Preorder%20Traversal/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên preorder biểu diễn <strong>phép duyệt preorder</strong> của một BST (tức <strong>binary search tree</strong>), hãy dựng cây và trả về <em>root</em> của cây.</p>

<p>Đề bài <strong>đảm bảo</strong> luôn tồn tại một binary search tree thỏa mãn các yêu cầu đã cho trong những test case này.</p>

<p><strong>Binary search tree</strong> là binary tree mà với mỗi node, mọi node con cháu thuộc <code>Node.left</code> đều có giá trị <strong>nhỏ hơn nghiêm ngặt</strong> <code>Node.val</code>, còn mọi node con cháu thuộc <code>Node.right</code> đều có giá trị <strong>lớn hơn nghiêm ngặt</strong> <code>Node.val</code>.</p>

<p>Phép <strong>duyệt preorder</strong> của binary tree ghi nhận giá trị của node trước, sau đó duyệt <code>Node.left</code> rồi duyệt <code>Node.right</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1008.Construct%20Binary%20Search%20Tree%20from%20Preorder%20Traversal/images/1266.png" style="height: 386px; width: 590px;" />
<pre>
<strong>Đầu vào:</strong> preorder = [8,5,1,7,10,12]
<strong>Đầu ra:</strong> [8,5,10,1,7,null,12]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> preorder = [1,3]
<strong>Đầu ra:</strong> [1,null,3]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= preorder.length &lt;= 100</code></li>
	<li><code>1 &lt;= preorder[i] &lt;= 1000</code></li>
	<li>Tất cả giá trị trong <code>preorder</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS + tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Giá trị đầu tiên trong preorder là root; duyệt tuyến tính để tìm giá trị lớn hơn đầu tiên rồi chia thành cây con trái và phải. Vì $n\le 100$, độ phức tạp $O(n^2)$ vẫn chấp nhận được, nhưng mỗi lần chia lại phải duyệt qua các đoạn đã xét. 
>
> Trong một đoạn preorder của BST, mọi key thuộc cây con trái đều nhỏ hơn root và mọi key thuộc cây con phải đều lớn hơn root. Vì vậy, có thể dùng tìm kiếm nhị phân để tìm chỉ số đầu tiên có giá trị lớn hơn root.
>
> $\textit{dfs}(i,j)$ tạo node từ $\textit{preorder}[i]$, tìm điểm bắt đầu của cây con phải bằng tìm kiếm nhị phân rồi đệ quy cho hai phía, đạt $O(n\log n)$.

<!-- thinking:end -->

Ta xây dựng hàm $\textit{dfs}(i, j)$ để tạo binary search tree từ các node $\textit{preorder}[i]$ đến $\textit{preorder}[j]$. Kết quả là $\textit{dfs}(0, n - 1)$.

Trong $\textit{dfs}(i, j)$, trước tiên ta tạo root node có giá trị $\textit{preorder}[i]$. Sau đó, dùng tìm kiếm nhị phân để tìm node đầu tiên có giá trị lớn hơn $\textit{preorder}[i]$ và lấy chỉ số của node đó là $\textit{mid}$. Ta gán $\textit{dfs}(i + 1, \textit{mid} - 1)$ làm cây con trái và $\textit{dfs}(\textit{mid}, j)$ làm cây con phải của root.

Cuối cùng, ta trả về root node.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\textit{preorder}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def bstFromPreorder(self, preorder: List[int]) -> Optional[TreeNode]:
        def dfs(i: int, j: int) -> Optional[TreeNode]:
            if i > j:
                return None
            root = TreeNode(preorder[i])
            l, r = i + 1, j + 1
            while l < r:
                mid = (l + r) >> 1
                if preorder[mid] > preorder[i]:
                    r = mid
                else:
                    l = mid + 1
            root.left = dfs(i + 1, l - 1)
            root.right = dfs(l, j)
            return root

        return dfs(0, len(preorder) - 1)
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
    private int[] preorder;

    public TreeNode bstFromPreorder(int[] preorder) {
        this.preorder = preorder;
        return dfs(0, preorder.length - 1);
    }

    private TreeNode dfs(int i, int j) {
        if (i > j) {
            return null;
        }
        TreeNode root = new TreeNode(preorder[i]);
        int l = i + 1, r = j + 1;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (preorder[mid] > preorder[i]) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        root.left = dfs(i + 1, l - 1);
        root.right = dfs(l, j);
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
    TreeNode* bstFromPreorder(vector<int>& preorder) {
        auto dfs = [&](this auto&& dfs, int i, int j) -> TreeNode* {
            if (i > j) {
                return nullptr;
            }
            TreeNode* root = new TreeNode(preorder[i]);
            int l = i + 1, r = j + 1;
            while (l < r) {
                int mid = (l + r) >> 1;
                if (preorder[mid] > preorder[i]) {
                    r = mid;
                } else {
                    l = mid + 1;
                }
            }
            root->left = dfs(i + 1, l - 1);
            root->right = dfs(l, j);
            return root;
        };
        return dfs(0, preorder.size() - 1);
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
func bstFromPreorder(preorder []int) *TreeNode {
	var dfs func(i, j int) *TreeNode
	dfs = func(i, j int) *TreeNode {
		if i > j {
			return nil
		}
		root := &TreeNode{Val: preorder[i]}
		l, r := i+1, j+1
		for l < r {
			mid := (l + r) >> 1
			if preorder[mid] > preorder[i] {
				r = mid
			} else {
				l = mid + 1
			}
		}
		root.Left = dfs(i+1, l-1)
		root.Right = dfs(l, j)
		return root
	}
	return dfs(0, len(preorder)-1)
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

function bstFromPreorder(preorder: number[]): TreeNode | null {
    const dfs = (i: number, j: number): TreeNode | null => {
        if (i > j) {
            return null;
        }
        const root = new TreeNode(preorder[i]);
        let [l, r] = [i + 1, j + 1];
        while (l < r) {
            const mid = (l + r) >> 1;
            if (preorder[mid] > preorder[i]) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        root.left = dfs(i + 1, l - 1);
        root.right = dfs(l, j);
        return root;
    };
    return dfs(0, preorder.length - 1);
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
    pub fn bst_from_preorder(preorder: Vec<i32>) -> Option<Rc<RefCell<TreeNode>>> {
        fn dfs(preorder: &Vec<i32>, i: usize, j: usize) -> Option<Rc<RefCell<TreeNode>>> {
            if i > j {
                return None;
            }
            let root = Rc::new(RefCell::new(TreeNode::new(preorder[i])));
            let mut l = i + 1;
            let mut r = j + 1;
            while l < r {
                let mid = (l + r) >> 1;
                if preorder[mid] > preorder[i] {
                    r = mid;
                } else {
                    l = mid + 1;
                }
            }
            let mut root_ref = root.borrow_mut();
            root_ref.left = dfs(preorder, i + 1, l - 1);
            root_ref.right = dfs(preorder, l, j);
            Some(root.clone())
        }

        dfs(&preorder, 0, preorder.len() - 1)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
