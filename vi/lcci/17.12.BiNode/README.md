---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [17.12. BiNode](https://leetcode.cn/problems/binode-lcci)

[中文文档](/lcci/17.12.BiNode/README.md)

## Mô tả

<!-- description:start -->

<p>Cấu trúc dữ liệu&nbsp;<code>TreeNode</code>&nbsp;được dùng cho cây nhị phân, nhưng cũng có thể được dùng để biểu diễn một linked list đơn (trong đó left là null và right là node tiếp theo trong list). Hãy cài đặt một method để chuyển đổi một binary search tree (được cài đặt bằng <code>TreeNode</code>) thành một linked list đơn. Thứ tự các giá trị phải được giữ nguyên và thao tác phải được thực hiện in place (tức là trên cấu trúc dữ liệu ban đầu).</p>

<p>Trả về node đầu của linked list sau khi chuyển đổi.</p>

<p><b>Lưu ý:&nbsp;</b>Bài toán này hơi khác so với bài toán gốc trong sách.</p>

<p>&nbsp;</p>

<p><strong>Ví dụ: </strong></p>

<pre>

<strong>Đầu vào: </strong> [4,2,5,1,3,null,6,0]

<strong>Đầu ra: </strong> [0,null,1,null,2,null,3,null,4,null,5,null,6]

</pre>

<p><strong>Lưu ý: </strong></p>

<ul>
	<li>Số lượng node không vượt quá&nbsp;100000.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chuyển BST thành một linked list chỉ đi sang phải theo thứ tự tăng dần. Dùng một mảng inorder rồi duyệt lần hai sẽ tốn thêm không gian tuyến tính.
>
> Thứ tự inorder vốn đã là thứ tự của list: khi thăm node, nối node đó vào $prev.right$ và xóa node con trái.
>
> Một dummy đứng đầu list và $prev$ theo dõi đuôi. Đệ quy sang trái/phải rồi trả về `dummy.right`.

<!-- thinking:end -->

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
    def convertBiNode(self, root: TreeNode) -> TreeNode:
        def dfs(root):
            if root is None:
                return
            nonlocal prev
            dfs(root.left)
            prev.right = root
            root.left = None
            prev = root
            dfs(root.right)

        dummy = TreeNode(val=0, right=root)
        prev = dummy
        dfs(root)
        return dummy.right
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
    private TreeNode prev;

    public TreeNode convertBiNode(TreeNode root) {
        TreeNode dummy = new TreeNode(0, null, root);
        prev = dummy;
        dfs(root);
        return dummy.right;
    }

    private void dfs(TreeNode root) {
        if (root == null) {
            return;
        }
        dfs(root.left);
        prev.right = root;
        root.left = null;
        prev = root;
        dfs(root.right);
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
    TreeNode* prev;

    TreeNode* convertBiNode(TreeNode* root) {
        TreeNode* dummy = new TreeNode(0, nullptr, root);
        prev = dummy;
        dfs(root);
        return dummy->right;
    }

    void dfs(TreeNode* root) {
        if (!root) return;
        dfs(root->left);
        prev->right = root;
        root->left = nullptr;
        prev = root;
        dfs(root->right);
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
func convertBiNode(root *TreeNode) *TreeNode {
	dummy := &TreeNode{Val: 0, Right: root}
	prev := dummy
	var dfs func(root *TreeNode)
	dfs = func(root *TreeNode) {
		if root == nil {
			return
		}
		dfs(root.Left)
		prev.Right = root
		root.Left = nil
		prev = root
		dfs(root.Right)
	}
	dfs(root)
	return dummy.Right
}
```

#### JavaScript

```js
const convertBiNode = root => {
    const dfs = root => {
        if (!root) {
            return;
        }
        dfs(root.left);
        prev.right = root;
        root.left = null;
        prev = root;
        dfs(root.right);
    };
    const dummy = new TreeNode(0);
    let prev = dummy;
    dfs(root);
    return dummy.right;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
