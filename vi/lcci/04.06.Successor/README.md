---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [04.06. Successor](https://leetcode.cn/problems/successor-lcci)

[Tài liệu tiếng Trung](/lcci/04.06.Successor/README.md)

## Mô tả

<!-- description:start -->

<p>Viết một thuật toán để tìm node &quot;tiếp theo&quot; (tức là node kế tiếp trong thứ tự trung tố) của một node đã cho trong cây tìm kiếm nhị phân.</p>

<p>Trả về <code>null</code> nếu node đã cho không có node &quot;tiếp theo&quot;.</p>

<p><strong>Ví dụ 1:</strong></p>

<pre>

<strong>Đầu vào:</strong> root = <code>[2,1,3], p = 1



  2
 / \
1   3

</code>

<strong>Đầu ra:</strong> 2</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>

<strong>Đầu vào:</strong> root = <code>[5,3,6,2,4,null,null,1], p = 6



      5
     / \
    3   6
   / \
  2   4
 /   
1

</code>

<strong>Đầu ra:</strong> null</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Node kế tiếp trong thứ tự trung tố là key đứng ngay sau $p$. Duyệt trung tố toàn bộ có độ phức tạp tuyến tính; BST có thể làm tốt hơn.
>
> Node kế tiếp là key nhỏ nhất lớn hơn $p.val$. Nếu $root.val > p.val$, $root$ là một ứng viên và ta tiếp tục tìm bên trái; ngược lại, chỉ có cây con bên phải mới có thể cho đáp án.
>
> Vòng lặp cập nhật $ans$ rồi đi sang trái hoặc phải trong $O(h)$ thời gian mà không cần parent pointer.

<!-- thinking:end -->

Phép duyệt trung tố của cây tìm kiếm nhị phân tạo thành một dãy tăng dần, vì vậy ta có thể sử dụng phương pháp tìm kiếm nhị phân.

Node kế tiếp trong thứ tự trung tố của một node cây tìm kiếm nhị phân $p$ thỏa mãn:

1. Giá trị của node kế tiếp trong thứ tự trung tố lớn hơn giá trị của node $p$.
2. Node kế tiếp trong thứ tự trung tố là node có giá trị nhỏ nhất trong tất cả các node lớn hơn $p$.

Do đó, với node hiện tại $root$, nếu $root.val > p.val$ thì $root$ có thể là node kế tiếp trong thứ tự trung tố của $p$. Ta lưu $root$ vào $ans$, sau đó tìm trong cây con bên trái, tức là $root = root.left$. Nếu $root.val \leq p.val$ thì $root$ không thể là node kế tiếp trong thứ tự trung tố của $p$, và ta tìm trong cây con bên phải, tức là $root = root.right$.

Độ phức tạp thời gian là $O(h)$, trong đó $h$ là chiều cao của cây tìm kiếm nhị phân. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None


class Solution:
    def inorderSuccessor(self, root: TreeNode, p: TreeNode) -> Optional[TreeNode]:
        ans = None
        while root:
            if root.val > p.val:
                ans = root
                root = root.left
            else:
                root = root.right
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
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        TreeNode ans = null;
        while (root != null) {
            if (root.val > p.val) {
                ans = root;
                root = root.left;
            } else {
                root = root.right;
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
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
        TreeNode* ans = nullptr;
        while (root) {
            if (root->val > p->val) {
                ans = root;
                root = root->left;
            } else {
                root = root->right;
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
func inorderSuccessor(root *TreeNode, p *TreeNode) (ans *TreeNode) {
	for root != nil {
		if root.Val > p.Val {
			ans = root
			root = root.Left
		} else {
			root = root.Right
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

function inorderSuccessor(root: TreeNode | null, p: TreeNode | null): TreeNode | null {
    let ans: TreeNode | null = null;
    while (root) {
        if (root.val > p.val) {
            ans = root;
            root = root.left;
        } else {
            root = root.right;
        }
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @param {TreeNode} p
 * @return {TreeNode}
 */
var inorderSuccessor = function (root, p) {
    let ans = null;
    while (root) {
        if (root.val > p.val) {
            ans = root;
            root = root.left;
        } else {
            root = root.right;
        }
    }
    return ans;
};
```

#### Swift

```swift
/* class TreeNode {
*    var val: Int
*    var left: TreeNode?
*    var right: TreeNode?
*
*    init(_ val: Int) {
*        self.val = val
*        self.left = nil
*        self.right = nil
*    }
* }
*/

class Solution {
    func inorderSuccessor(_ root: TreeNode?, _ p: TreeNode?) -> TreeNode? {
        var current = root
        var successor: TreeNode? = nil

        while let node = current {
            if node.val > p!.val {
                successor = node
                current = node.left
            } else {
                current = node.right
            }
        }
        return successor
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
