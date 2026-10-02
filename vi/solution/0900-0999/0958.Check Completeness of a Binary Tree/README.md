---
comments: true
difficulty: Medium
tags:
    - Tree
    - Breadth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [958. Check Completeness of a Binary Tree](https://leetcode.com/problems/check-completeness-of-a-binary-tree)

[中文文档](/solution/0900-0999/0958.Check%20Completeness%20of%20a%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một binary tree, hãy xác định xem đó có phải là <em>cây nhị phân hoàn chỉnh</em> hay không.</p>

<p>Trong <strong><a href="http://en.wikipedia.org/wiki/Binary_tree#Types_of_binary_trees" target="_blank">cây nhị phân hoàn chỉnh</a></strong>, mọi level trừ level cuối (nếu có) đều được lấp đầy, còn các node ở level cuối được dồn về bên trái nhiều nhất có thể. Level cuối <code>h</code> có từ <code>1</code> đến <code>2<sup>h</sup></code> node, tính cả hai đầu mút.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0958.Check%20Completeness%20of%20a%20Binary%20Tree/images/complete-binary-tree-1.png" style="width: 180px; height: 145px;" />
<pre>
<strong>Input:</strong> root = [1,2,3,4,5,6]
<strong>Output:</strong> true
<strong>Giải thích:</strong> Mọi level trước level cuối đều đầy (tức các level có giá trị node {1} và {2, 3}), và các node ở level cuối ({4, 5, 6}) được dồn về bên trái nhiều nhất có thể.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0958.Check%20Completeness%20of%20a%20Binary%20Tree/images/complete-binary-tree-2.png" style="width: 200px; height: 145px;" />
<pre>
<strong>Input:</strong> root = [1,2,3,4,5,null,7]
<strong>Output:</strong> false
<strong>Giải thích:</strong> Node có giá trị 7 không nằm ở vị trí trái nhất có thể.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 100]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Trong cây nhị phân hoàn chỉnh, khi duyệt level order thì sau node null không được có node khác null. Đưa cả các node con null vào queue; sau khi lấy node null đầu tiên ra, mọi phần tử còn lại trong queue cũng phải là null. Cây có tối đa $100$ node nên chỉ cần một lượt BFS.

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
    def isCompleteTree(self, root: TreeNode) -> bool:
        q = deque([root])
        while q:
            node = q.popleft()
            if node is None:
                break
            q.append(node.left)
            q.append(node.right)
        return all(node is None for node in q)
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
    public boolean isCompleteTree(TreeNode root) {
        Deque<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while (q.peek() != null) {
            TreeNode node = q.poll();
            q.offer(node.left);
            q.offer(node.right);
        }
        while (!q.isEmpty() && q.peek() == null) {
            q.poll();
        }
        return q.isEmpty();
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
    bool isCompleteTree(TreeNode* root) {
        queue<TreeNode*> q{{root}};
        while (q.front()) {
            root = q.front();
            q.pop();
            q.push(root->left);
            q.push(root->right);
        }
        while (!q.empty() && !q.front()) q.pop();
        return q.empty();
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
func isCompleteTree(root *TreeNode) bool {
	q := []*TreeNode{root}
	for q[0] != nil {
		root = q[0]
		q = q[1:]
		q = append(q, root.Left)
		q = append(q, root.Right)
	}
	for len(q) > 0 && q[0] == nil {
		q = q[1:]
	}
	return len(q) == 0
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
