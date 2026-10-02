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

# [1379. Find a Corresponding Node of a Binary Tree in a Clone of That Tree](https://leetcode.com/problems/find-a-corresponding-node-of-a-binary-tree-in-a-clone-of-that-tree)

[中文文档](/solution/1300-1399/1379.Find%20a%20Corresponding%20Node%20of%20a%20Binary%20Tree%20in%20a%20Clone%20of%20That%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai binary tree <code>original</code> và <code>cloned</code>, cùng tham chiếu đến một node <code>target</code> trong cây gốc.</p>

<p>Cây <code>cloned</code> là một <strong>bản sao của</strong> cây <code>original</code>.</p>

<p>Trả về <em>tham chiếu đến node tương ứng</em> trong cây <code>cloned</code>.</p>

<p><strong>Lưu ý</strong> rằng bạn <strong>không được phép</strong> thay đổi hai cây hoặc node <code>target</code>, và đáp án <strong>phải là</strong> tham chiếu đến một node trong cây <code>cloned</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1379.Find%20a%20Corresponding%20Node%20of%20a%20Binary%20Tree%20in%20a%20Clone%20of%20That%20Tree/images/e1.png" style="width: 544px; height: 426px;" />
<pre>
<strong>Đầu vào:</strong> tree = [7,4,3,null,null,6,19], target = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Tất cả ví dụ đều minh họa cây gốc và cây clone. Node target là node màu xanh lá trong cây gốc. Đáp án là node màu vàng trong cây clone.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1379.Find%20a%20Corresponding%20Node%20of%20a%20Binary%20Tree%20in%20a%20Clone%20of%20That%20Tree/images/e2.png" style="width: 221px; height: 159px;" />
<pre>
<strong>Đầu vào:</strong> tree = [7], target =  7
<strong>Đầu ra:</strong> 7
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1379.Find%20a%20Corresponding%20Node%20of%20a%20Binary%20Tree%20in%20a%20Clone%20of%20That%20Tree/images/e3.png" style="width: 459px; height: 486px;" />
<pre>
<strong>Đầu vào:</strong> tree = [8,null,6,null,5,null,4,null,3,null,2,null,1], target = 4
<strong>Đầu ra:</strong> 4
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong <code>tree</code> nằm trong khoảng <code>[1, 10<sup>4</sup>]</code>.</li>
	<li>Giá trị các node trong <code>tree</code> là duy nhất.</li>
	<li>Node <code>target</code> thuộc cây <code>original</code> và không phải <code>null</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể giải bài toán nếu cây cho phép các node có giá trị trùng nhau không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Tìm node trong cây clone tương ứng với $\textit{target}$. Chỉ có thể xác định node bằng giá trị khi các giá trị là duy nhất; câu hỏi mở rộng cho phép giá trị trùng nhau. DFS đồng thời trên hai cây và khi gặp $\textit{target}$ trong cây gốc thì trả về node hiện tại trong cây clone.

<!-- thinking:end -->

Ta thiết kế hàm $dfs(root1, root2)$ để duyệt DFS đồng thời trên hai cây $root1$ và $root2$. Khi đến một node, nếu đó là $target$ thì trả về node tương ứng trong $root2$. Nếu không, ta đệ quy tìm $target$ trong cây con trái và phải của $root1$ và $root2$, rồi trả về kết quả khác rỗng.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, với $n$ là số node trong cây.

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
    def getTargetCopy(
        self, original: TreeNode, cloned: TreeNode, target: TreeNode
    ) -> TreeNode:
        def dfs(root1: TreeNode, root2: TreeNode) -> TreeNode:
            if root1 is None:
                return None
            if root1 == target:
                return root2
            return dfs(root1.left, root2.left) or dfs(root1.right, root2.right)

        return dfs(original, cloned)
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
    private TreeNode target;

    public final TreeNode getTargetCopy(
        final TreeNode original, final TreeNode cloned, final TreeNode target) {
        this.target = target;
        return dfs(original, cloned);
    }

    private TreeNode dfs(TreeNode root1, TreeNode root2) {
        if (root1 == null) {
            return null;
        }
        if (root1 == target) {
            return root2;
        }
        TreeNode res = dfs(root1.left, root2.left);
        return res == null ? dfs(root1.right, root2.right) : res;
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
    TreeNode* getTargetCopy(TreeNode* original, TreeNode* cloned, TreeNode* target) {
        function<TreeNode*(TreeNode*, TreeNode*)> dfs = [&](TreeNode* root1, TreeNode* root2) -> TreeNode* {
            if (root1 == nullptr) {
                return nullptr;
            }
            if (root1 == target) {
                return root2;
            }
            TreeNode* left = dfs(root1->left, root2->left);
            return left == nullptr ? dfs(root1->right, root2->right) : left;
        };
        return dfs(original, cloned);
    }
};
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

function getTargetCopy(
    original: TreeNode | null,
    cloned: TreeNode | null,
    target: TreeNode | null,
): TreeNode | null {
    const dfs = (root1: TreeNode | null, root2: TreeNode | null): TreeNode | null => {
        if (!root1) {
            return null;
        }
        if (root1 === target) {
            return root2;
        }
        return dfs(root1.left, root2.left) || dfs(root1.right, root2.right);
    };
    return dfs(original, cloned);
}
```

#### C#

```cs
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left;
 *     public TreeNode right;
 *     public TreeNode(int x) { val = x; }
 * }
 */

public class Solution {
    private TreeNode target;

    public TreeNode GetTargetCopy(TreeNode original, TreeNode cloned, TreeNode target) {
        this.target = target;
        return dfs(original, cloned);
    }

    private TreeNode dfs(TreeNode original, TreeNode cloned) {
        if (original == null) {
            return null;
        }
        if (original == target) {
            return cloned;
        }
        TreeNode left = dfs(original.left, cloned.left);
        return left == null ? dfs(original.right, cloned.right) : left;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
