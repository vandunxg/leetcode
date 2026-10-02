---
comments: true
difficulty: Medium
tags:
    - Tree
    - Binary Tree
---

<!-- problem:start -->

# [998. Maximum Binary Tree II](https://leetcode.com/problems/maximum-binary-tree-ii)

[中文文档](/solution/0900-0999/0998.Maximum%20Binary%20Tree%20II/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Cây cực đại</strong> là cây mà mỗi node có giá trị lớn hơn giá trị của mọi node khác trong cây con của nó.</p>

<p>Cho <code>root</code> của một cây nhị phân cực đại và số nguyên <code>val</code>.</p>

<p>Tương tự như <a href="https://leetcode.com/problems/maximum-binary-tree/" target="_blank">bài toán trước</a>, cây đã cho được xây dựng đệ quy từ danh sách <code>a</code> (<code>root = Construct(a)</code>) bằng quy trình <code>Construct(a)</code> sau:</p>

<ul>
	<li>Nếu <code>a</code> rỗng, trả về <code>null</code>.</li>
	<li>Nếu không, gọi <code>a[i]</code> là phần tử lớn nhất trong <code>a</code>. Tạo node <code>root</code> có giá trị <code>a[i]</code>.</li>
	<li>Node con trái của <code>root</code> là <code>Construct([a[0], a[1], ..., a[i - 1]])</code>.</li>
	<li>Node con phải của <code>root</code> là <code>Construct([a[i + 1], a[i + 2], ..., a[a.length - 1]])</code>.</li>
	<li>Trả về <code>root</code>.</li>
</ul>

<p>Lưu ý, ta không được cung cấp trực tiếp <code>a</code>, mà chỉ có node gốc <code>root = Construct(a)</code>.</p>

<p>Gọi <code>b</code> là bản sao của <code>a</code> được nối thêm giá trị <code>val</code>. Đảm bảo các giá trị trong <code>b</code> đều phân biệt.</p>

<p>Trả về <code>Construct(b)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0998.Maximum%20Binary%20Tree%20II/images/maxtree1.jpg" style="width: 376px; height: 235px;" />
<pre>
<strong>Đầu vào:</strong> root = [4,1,3,null,null,2], val = 5
<strong>Đầu ra:</strong> [5,4,null,1,3,null,null,2]
<strong>Giải thích:</strong> a = [1,4,2,3], b = [1,4,2,3,5]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0998.Maximum%20Binary%20Tree%20II/images/maxtree21.jpg" style="width: 358px; height: 156px;" />
<pre>
<strong>Đầu vào:</strong> root = [5,2,4,null,1], val = 3
<strong>Đầu ra:</strong> [5,2,4,null,1,null,3]
<strong>Giải thích:</strong> a = [2,1,5,4], b = [2,1,5,4,3]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0998.Maximum%20Binary%20Tree%20II/images/maxtree3.jpg" style="width: 404px; height: 180px;" />
<pre>
<strong>Đầu vào:</strong> root = [5,2,3,null,1], val = 4
<strong>Đầu ra:</strong> [5,2,4,null,1,3]
<strong>Giải thích:</strong> a = [2,1,5,3], b = [2,1,5,3,4]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong đoạn <code>[1, 100]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 100</code></li>
	<li>Tất cả giá trị trong cây đều <strong>phân biệt</strong>.</li>
	<li><code>1 &lt;= val &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Nối thêm $val$ vào mảng đã tạo nên cây cực đại này rồi xây dựng lại cây. Nếu $val$ lớn hơn root, nó trở thành root mới và cây cũ là cây con trái; nếu không, $val$ chỉ có thể nằm trên nhánh phải. Đệ quy xuống node con phải.

<!-- thinking:end -->

Nếu $val$ là số lớn nhất, ta tạo node mới có giá trị $val$ làm root và đặt $root$ làm cây con trái của node mới.

Nếu $val$ không phải số lớn nhất, vì $val$ là số được nối vào cuối mảng nên nó phải nằm bên phải $root$. Do đó, ta có thể chèn $val$ thành node mới trong cây con phải của $root$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node trong cây.

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
    def insertIntoMaxTree(
        self, root: Optional[TreeNode], val: int
    ) -> Optional[TreeNode]:
        if root is None or root.val < val:
            return TreeNode(val, root)
        root.right = self.insertIntoMaxTree(root.right, val)
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
    public TreeNode insertIntoMaxTree(TreeNode root, int val) {
        if (root == null || root.val < val) {
            return new TreeNode(val, root, null);
        }
        root.right = insertIntoMaxTree(root.right, val);
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
    TreeNode* insertIntoMaxTree(TreeNode* root, int val) {
        if (!root || root->val < val) return new TreeNode(val, root, nullptr);
        root->right = insertIntoMaxTree(root->right, val);
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
func insertIntoMaxTree(root *TreeNode, val int) *TreeNode {
	if root == nil || root.Val < val {
		return &TreeNode{val, root, nil}
	}
	root.Right = insertIntoMaxTree(root.Right, val)
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

function insertIntoMaxTree(root: TreeNode | null, val: number): TreeNode | null {
    if (!root || root.val < val) {
        return new TreeNode(val, root);
    }
    root.right = insertIntoMaxTree(root.right, val);
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
use std::cell::RefCell;
use std::rc::Rc;
impl Solution {
    pub fn insert_into_max_tree(
        mut root: Option<Rc<RefCell<TreeNode>>>,
        val: i32,
    ) -> Option<Rc<RefCell<TreeNode>>> {
        if root.is_none() || root.as_ref().unwrap().as_ref().borrow().val < val {
            return Some(Rc::new(RefCell::new(TreeNode {
                val,
                left: root.take(),
                right: None,
            })));
        }
        {
            let mut root = root.as_ref().unwrap().as_ref().borrow_mut();
            root.right = Self::insert_into_max_tree(root.right.take(), val);
        }
        root
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

struct TreeNode* insertIntoMaxTree(struct TreeNode* root, int val) {
    if (!root || root->val < val) {
        struct TreeNode* res = (struct TreeNode*) malloc(sizeof(struct TreeNode));
        res->val = val;
        res->left = root;
        res->right = NULL;
        return res;
    }
    root->right = insertIntoMaxTree(root->right, val);
    return root;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Lặp

<!-- thinking:start -->

> **Tư duy**
>
> Đệ quy có thể đi hết chiều cao của cây. Duyệt lặp theo nhánh phải, dừng tại node đầu tiên có node con phải nhỏ hơn $val$, rồi gắn cây con phải cũ vào bên trái node mới để tránh dùng call stack.

<!-- thinking:end -->

Tìm trong cây con phải node thỏa $curr.val \gt val \gt curr.right.val$, sau đó tạo node mới $node$, trỏ $node.left$ đến $curr.right$ rồi trỏ $curr.right$ đến $node$.

Cuối cùng, trả về $root$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số node trong cây. Độ phức tạp không gian là $O(1)$.

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
    def insertIntoMaxTree(
        self, root: Optional[TreeNode], val: int
    ) -> Optional[TreeNode]:
        if root.val < val:
            return TreeNode(val, root)
        curr = root
        node = TreeNode(val)
        while curr.right and curr.right.val > val:
            curr = curr.right
        node.left = curr.right
        curr.right = node
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
    public TreeNode insertIntoMaxTree(TreeNode root, int val) {
        if (root.val < val) {
            return new TreeNode(val, root, null);
        }
        TreeNode curr = root;
        TreeNode node = new TreeNode(val);
        while (curr.right != null && curr.right.val > val) {
            curr = curr.right;
        }
        node.left = curr.right;
        curr.right = node;
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
    TreeNode* insertIntoMaxTree(TreeNode* root, int val) {
        if (root->val < val) return new TreeNode(val, root, nullptr);
        TreeNode* curr = root;
        TreeNode* node = new TreeNode(val);
        while (curr->right && curr->right->val > val) curr = curr->right;
        node->left = curr->right;
        curr->right = node;
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
func insertIntoMaxTree(root *TreeNode, val int) *TreeNode {
	if root.Val < val {
		return &TreeNode{val, root, nil}
	}
	node := &TreeNode{Val: val}
	curr := root
	for curr.Right != nil && curr.Right.Val > val {
		curr = curr.Right
	}
	node.Left = curr.Right
	curr.Right = node
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

function insertIntoMaxTree(root: TreeNode | null, val: number): TreeNode | null {
    if (root.val < val) {
        return new TreeNode(val, root);
    }
    const node = new TreeNode(val);
    let curr = root;
    while (curr.right && curr.right.val > val) {
        curr = curr.right;
    }
    node.left = curr.right;
    curr.right = node;
    return root;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
