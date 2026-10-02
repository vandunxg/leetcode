---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - String
    - Binary Tree
---

<!-- problem:start -->

# [606. Construct String from Binary Tree](https://leetcode.com/problems/construct-string-from-binary-tree)

[中文文档](/solution/0600-0699/0606.Construct%20String%20from%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho node <code>root</code> của một cây nhị phân, hãy tạo chuỗi biểu diễn cây theo một số quy tắc định dạng cụ thể. Biểu diễn này dựa trên cách duyệt preorder và phải tuân theo các hướng dẫn sau:</p>

<ul>
	<li>
	<p><strong>Biểu diễn node</strong>: Mỗi node trong cây được biểu diễn bằng giá trị số nguyên của nó.</p>
	</li>
	<li>
	<p><strong>Dùng ngoặc cho node con</strong>: Nếu node có ít nhất một node con (trái hoặc phải), các node con phải được biểu diễn trong dấu ngoặc. Cụ thể:</p>

    <ul>
    	<li>Nếu node có con trái, giá trị của con trái phải được đặt trong ngoặc ngay sau giá trị của node.</li>
    	<li>Nếu node có con phải, giá trị của con phải cũng phải được đặt trong ngoặc. Cặp ngoặc của con phải đặt sau cặp ngoặc của con trái.</li>
    </ul>
    </li>
    <li>
    <p><strong>Bỏ cặp ngoặc rỗng</strong>: Các cặp ngoặc rỗng (tức <code>()</code>) phải được bỏ khỏi chuỗi biểu diễn cuối cùng, ngoại trừ trường hợp node có con phải nhưng không có con trái. Khi đó, cần giữ một cặp ngoặc rỗng để biểu thị rằng node không có con trái. Quy tắc này đảm bảo chuỗi biểu diễn tương ứng duy nhất với cấu trúc cây nhị phân ban đầu.</p>

    <p>Tóm lại, bỏ cặp ngoặc rỗng khi node chỉ có con trái hoặc không có con nào. Tuy nhiên, nếu node có con phải nhưng không có con trái, phải đặt một cặp ngoặc rỗng trước phần biểu diễn con phải để thể hiện đúng cấu trúc cây.</p>
    </li>

</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0606.Construct%20String%20from%20Binary%20Tree/images/cons1-tree.jpg" style="padding: 10px; background: #fff; border-radius: .5rem;" />
<pre>
<strong>Đầu vào:</strong> root = [1,2,3,4]
<strong>Đầu ra:</strong> &quot;1(2(4))(3)&quot;
<strong>Giải thích:</strong> Ban đầu, chuỗi cần có dạng &quot;1(2(4)())(3()())&quot;, nhưng cần bỏ tất cả các cặp ngoặc rỗng. Kết quả là &quot;1(2(4))(3)&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0606.Construct%20String%20from%20Binary%20Tree/images/cons2-tree.jpg" style="padding: 10px; background: #fff; border-radius: .5rem;" />
<pre>
<strong>Đầu vào:</strong> root = [1,2,3,null,4]
<strong>Đầu ra:</strong> &quot;1(2()(4))(3)&quot;
<strong>Giải thích:</strong> Gần giống ví dụ đầu tiên, ngoại trừ cặp <code>()</code> sau <code>2</code> là cần thiết để biểu thị node <code>2</code> không có con trái nhưng có con phải.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 10<sup>4</sup>]</code>.</li>
	<li><code>-1000 &lt;= Node.val &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Có thể khôi phục cây bằng cách duyệt preorder và thêm ngoặc, nhưng quy tắc về ngoặc rỗng không đối xứng: thiếu con phải thì có thể bỏ `()`, còn thiếu con trái thì không.
>
> Đệ quy theo ba trường hợp: node lá chỉ cần giá trị; nếu không có con phải thì chỉ bọc phần con trái; còn lại bọc cả hai. Cách này tuân theo quy tắc bỏ ngoặc rỗng.

<!-- thinking:end -->

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
    def tree2str(self, root: Optional[TreeNode]) -> str:
        def dfs(root):
            if root is None:
                return ''
            if root.left is None and root.right is None:
                return str(root.val)
            if root.right is None:
                return f'{root.val}({dfs(root.left)})'
            return f'{root.val}({dfs(root.left)})({dfs(root.right)})'

        return dfs(root)
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
    public String tree2str(TreeNode root) {
        if (root == null) {
            return "";
        }
        if (root.left == null && root.right == null) {
            return root.val + "";
        }
        if (root.right == null) {
            return root.val + "(" + tree2str(root.left) + ")";
        }
        return root.val + "(" + tree2str(root.left) + ")(" + tree2str(root.right) + ")";
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
    string tree2str(TreeNode* root) {
        if (!root) return "";
        if (!root->left && !root->right) return to_string(root->val);
        if (!root->right) return to_string(root->val) + "(" + tree2str(root->left) + ")";
        return to_string(root->val) + "(" + tree2str(root->left) + ")(" + tree2str(root->right) + ")";
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
func tree2str(root *TreeNode) string {
	if root == nil {
		return ""
	}
	if root.Left == nil && root.Right == nil {
		return strconv.Itoa(root.Val)
	}
	if root.Right == nil {
		return strconv.Itoa(root.Val) + "(" + tree2str(root.Left) + ")"
	}
	return strconv.Itoa(root.Val) + "(" + tree2str(root.Left) + ")(" + tree2str(root.Right) + ")"
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

function tree2str(root: TreeNode | null): string {
    if (root == null) {
        return '';
    }
    if (root.left == null && root.right == null) {
        return `${root.val}`;
    }
    return `${root.val}(${root.left ? tree2str(root.left) : ''})${
        root.right ? `(${tree2str(root.right)})` : ''
    }`;
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
    fn dfs(root: &Option<Rc<RefCell<TreeNode>>>, res: &mut String) {
        if let Some(node) = root {
            let node = node.borrow();
            res.push_str(node.val.to_string().as_str());

            if node.left.is_none() && node.right.is_none() {
                return;
            }
            res.push('(');
            if node.left.is_some() {
                Self::dfs(&node.left, res);
            }
            res.push(')');
            if node.right.is_some() {
                res.push('(');
                Self::dfs(&node.right, res);
                res.push(')');
            }
        }
    }

    pub fn tree2str(root: Option<Rc<RefCell<TreeNode>>>) -> String {
        let mut res = String::new();
        Self::dfs(&root, &mut res);
        res
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
