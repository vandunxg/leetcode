---
comments: true
difficulty: Medium
rating: 1649
source: Weekly Contest 178 Q3
tags:
    - Tree
    - Depth-First Search
    - Linked List
    - Binary Tree
---

<!-- problem:start -->

# [1367. Linked List in Binary Tree](https://leetcode.com/problems/linked-list-in-binary-tree)

[中文文档](/solution/1300-1399/1367.Linked%20List%20in%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho cây nhị phân <code>root</code> và một&nbsp;linked list có&nbsp;<code>head</code>&nbsp;là node đầu tiên.&nbsp;</p>

<p>Trả về true nếu tất cả phần tử trong linked list bắt đầu từ <code>head</code> khớp với một <em>đường đi hướng xuống</em> liên tiếp nào đó trong cây nhị phân;&nbsp;nếu không thì trả về false.</p>

<p>Trong bài này, đường đi hướng xuống là đường đi bắt đầu từ một node bất kỳ và đi xuống dưới.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1367.Linked%20List%20in%20Binary%20Tree/images/sample_1_1720.png" style="width: 220px; height: 280px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> head = [4,2,8], root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Các node màu xanh tạo thành một đường đi con trong cây nhị phân.  
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1367.Linked%20List%20in%20Binary%20Tree/images/sample_2_1720.png" style="width: 220px; height: 280px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> head = [1,4,2,6], root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]
<strong>Đầu ra:</strong> true
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> head = [1,4,2,6,8], root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không có đường đi nào trong cây nhị phân chứa toàn bộ phần tử của linked list bắt đầu từ <code>head</code>.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 2500]</code>.</li>
	<li>Số node trong danh sách nằm trong khoảng <code>[1, 100]</code>.</li>
	<li><code>1 &lt;= Node.val&nbsp;&lt;= 100</code>&nbsp;đối với mỗi node trong linked list và cây nhị phân.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Kiểm tra xem danh sách có xuất hiện như một đường đi hướng xuống hay không. Cả hai cấu trúc đều đủ nhỏ để thử khớp từ mọi node của cây. $dfs(\textit{head},\textit{root})$ kiểm tra một đường đi liên tiếp có các giá trị tương ứng bằng nhau; hàm bao sẽ thử lại từ từng node thông qua hai node con. Danh sách rỗng thì khớp; cây rỗng hoặc giá trị không khớp thì thất bại.

<!-- thinking:end -->

Ta định nghĩa hàm đệ quy $dfs(head, root)$ để kiểm tra linked list $head$ có khớp với một đường đi con bắt đầu tại $root$ trong cây nhị phân hay không. Hàm $dfs(head, root)$ hoạt động như sau:

- Nếu linked list $head$ rỗng, nghĩa là đã duyệt hết danh sách; trả về `true`;
- Nếu cây nhị phân $root$ rỗng, nghĩa là đã đi hết nhánh cây nhưng vẫn chưa duyệt hết linked list; trả về `false`;
- Nếu giá trị tại $root$ không bằng giá trị tại $head$, trả về `false`;
- Ngược lại, trả về $dfs(head.next, root.left)$ hoặc $dfs(head.next, root.right)$.

Trong hàm chính, ta gọi $dfs(head, root)$ tại từng node của cây nhị phân. Nếu có lần gọi nào trả về `true`, linked list là một đường đi con trong cây và ta trả về `true`; nếu mọi node đều cho kết quả `false`, linked list không phải đường đi con trong cây nên trả về `false`.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$, với $n$ là số node trong cây nhị phân.

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSubPath(self, head: Optional[ListNode], root: Optional[TreeNode]) -> bool:
        def dfs(head, root):
            if head is None:
                return True
            if root is None or root.val != head.val:
                return False
            return dfs(head.next, root.left) or dfs(head.next, root.right)

        if root is None:
            return False
        return (
            dfs(head, root)
            or self.isSubPath(head, root.left)
            or self.isSubPath(head, root.right)
        )
```

#### Java

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
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
    public boolean isSubPath(ListNode head, TreeNode root) {
        if (root == null) {
            return false;
        }
        return dfs(head, root) || isSubPath(head, root.left) || isSubPath(head, root.right);
    }

    private boolean dfs(ListNode head, TreeNode root) {
        if (head == null) {
            return true;
        }
        if (root == null || head.val != root.val) {
            return false;
        }
        return dfs(head.next, root.left) || dfs(head.next, root.right);
    }
}
```

#### C++

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
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
    bool isSubPath(ListNode* head, TreeNode* root) {
        if (!root) {
            return false;
        }
        return dfs(head, root) || isSubPath(head, root->left) || isSubPath(head, root->right);
    }

    bool dfs(ListNode* head, TreeNode* root) {
        if (!head) {
            return true;
        }
        if (!root || head->val != root->val) {
            return false;
        }
        return dfs(head->next, root->left) || dfs(head->next, root->right);
    }
};
```

#### Go

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func isSubPath(head *ListNode, root *TreeNode) bool {
	if root == nil {
		return false
	}
	return dfs(head, root) || isSubPath(head, root.Left) || isSubPath(head, root.Right)
}

func dfs(head *ListNode, root *TreeNode) bool {
	if head == nil {
		return true
	}
	if root == nil || head.Val != root.Val {
		return false
	}
	return dfs(head.Next, root.Left) || dfs(head.Next, root.Right)
}
```

#### TypeScript

```ts
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     val: number
 *     next: ListNode | null
 *     constructor(val?: number, next?: ListNode | null) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.next = (next===undefined ? null : next)
 *     }
 * }
 */

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

const dfs = (head: ListNode | null, root: TreeNode | null) => {
    if (head == null) {
        return true;
    }
    if (root == null || head.val !== root.val) {
        return false;
    }
    return dfs(head.next, root.left) || dfs(head.next, root.right);
};

function isSubPath(head: ListNode | null, root: TreeNode | null): boolean {
    if (root == null) {
        return false;
    }
    return dfs(head, root) || isSubPath(head, root.left) || isSubPath(head, root.right);
}
```

#### Rust

```rust
// Definition for singly-linked list.
// #[derive(PartialEq, Eq, Clone, Debug)]
// pub struct ListNode {
//   pub val: i32,
//   pub next: Option<Box<ListNode>>
// }
//
// impl ListNode {
//   #[inline]
//   fn new(val: i32) -> Self {
//     ListNode {
//       next: None,
//       val
//     }
//   }
// }
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
    fn dfs(head: &Option<Box<ListNode>>, root: &Option<Rc<RefCell<TreeNode>>>) -> bool {
        if head.is_none() {
            return true;
        }
        if root.is_none() {
            return false;
        }
        let node = head.as_ref().unwrap();
        let root = root.as_ref().unwrap().borrow();
        if node.val != root.val {
            return false;
        }
        Self::dfs(&node.next, &root.left) || Self::dfs(&node.next, &root.right)
    }

    fn my_is_sub_path(head: &Option<Box<ListNode>>, root: &Option<Rc<RefCell<TreeNode>>>) -> bool {
        if root.is_none() {
            return false;
        }
        let node = root.as_ref().unwrap().borrow();
        Self::dfs(head, root)
            || Self::my_is_sub_path(head, &node.left)
            || Self::my_is_sub_path(head, &node.right)
    }

    pub fn is_sub_path(head: Option<Box<ListNode>>, root: Option<Rc<RefCell<TreeNode>>>) -> bool {
        Self::my_is_sub_path(&head, &root)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
