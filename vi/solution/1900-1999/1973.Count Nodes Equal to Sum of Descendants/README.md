---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [1973. Count Nodes Equal to Sum of Descendants 🔒](https://leetcode.com/problems/count-nodes-equal-to-sum-of-descendants)

[中文文档](/solution/1900-1999/1973.Count%20Nodes%20Equal%20to%20Sum%20of%20Descendants/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một cây nhị phân, hãy trả về <em>số node có giá trị bằng <strong>tổng</strong> giá trị của các node hậu duệ</em>.</p>

<p>Một <strong>node hậu duệ</strong> của node <code>x</code> là bất kỳ node nào nằm trên đường đi từ node <code>x</code> đến một node lá. Tổng được coi là <code>0</code> nếu node đó không có node hậu duệ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1973.Count%20Nodes%20Equal%20to%20Sum%20of%20Descendants/images/screenshot-2021-08-17-at-17-16-50-diagram-drawio-diagrams-net.png" style="width: 250px; height: 207px;" />
<pre>
<strong>Đầu vào:</strong> root = [10,3,4,2,1]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Với node có giá trị 10: Tổng các node hậu duệ của node này là 3+4+2+1 = 10.
Với node có giá trị 3: Tổng các node hậu duệ của node này là 2+1 = 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1973.Count%20Nodes%20Equal%20to%20Sum%20of%20Descendants/images/screenshot-2021-08-17-at-17-25-21-diagram-drawio-diagrams-net.png" style="height: 196px; width: 200px;" />
<pre>
<strong>Đầu vào:</strong> root = [2,3,null,2,null]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Không có node nào có giá trị bằng tổng các node hậu duệ của nó.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1973.Count%20Nodes%20Equal%20to%20Sum%20of%20Descendants/images/screenshot-2021-08-17-at-17-23-53-diagram-drawio-diagrams-net.png" style="width: 50px; height: 50px;" />
<pre>
<strong>Đầu vào:</strong> root = [0]
<strong>Đầu ra:</strong> 1
Với node có giá trị 0: Tổng các node hậu duệ của node này là 0 vì node này không có node hậu duệ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 10<sup>5</sup>]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi node, ta so sánh giá trị của node đó với tổng các node hậu duệ. Nếu duyệt lại cây cho từng node thì độ phức tạp sẽ là bậc hai.
>
> Một lần DFS hậu tự sẽ trả về tổng các giá trị trong cây con: nếu tổng của cây con trái và cây con phải bằng giá trị của node, ta tăng đáp án lên một; sau đó trả về tổng giá trị của node và hai cây con. Như vậy, chỉ cần duyệt cây một lần.

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
    def equalToDescendants(self, root: Optional[TreeNode]) -> int:
        def dfs(root):
            if root is None:
                return 0
            l, r = dfs(root.left), dfs(root.right)
            if l + r == root.val:
                nonlocal ans
                ans += 1
            return root.val + l + r

        ans = 0
        dfs(root)
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
    private int ans;

    public int equalToDescendants(TreeNode root) {
        dfs(root);
        return ans;
    }

    private int dfs(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int l = dfs(root.left);
        int r = dfs(root.right);
        if (l + r == root.val) {
            ++ans;
        }
        return root.val + l + r;
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
    int equalToDescendants(TreeNode* root) {
        int ans = 0;
        function<long long(TreeNode*)> dfs = [&](TreeNode* root) -> long long {
            if (!root) {
                return 0;
            }
            auto l = dfs(root->left);
            auto r = dfs(root->right);
            ans += l + r == root->val;
            return root->val + l + r;
        };
        dfs(root);
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
func equalToDescendants(root *TreeNode) (ans int) {
	var dfs func(*TreeNode) int
	dfs = func(root *TreeNode) int {
		if root == nil {
			return 0
		}
		l, r := dfs(root.Left), dfs(root.Right)
		if l+r == root.Val {
			ans++
		}
		return root.Val + l + r
	}
	dfs(root)
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
