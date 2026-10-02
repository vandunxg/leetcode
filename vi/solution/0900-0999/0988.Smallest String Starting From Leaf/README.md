---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - String
    - Backtracking
    - Binary Tree
---

<!-- problem:start -->

# [988. Smallest String Starting From Leaf](https://leetcode.com/problems/smallest-string-starting-from-leaf)

[中文文档](/solution/0900-0999/0988.Smallest%20String%20Starting%20From%20Leaf/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của cây nhị phân, trong đó mỗi node có giá trị thuộc đoạn <code>[0, 25]</code>, tương ứng với các chữ cái từ <code>&#39;a&#39;</code> đến <code>&#39;z&#39;</code>.</p>

<p>Trả về <em>chuỗi <strong>nhỏ nhất theo thứ tự từ điển</strong> bắt đầu tại một lá của cây và kết thúc ở node gốc</em>.</p>

<p>Nhắc lại, một tiền tố ngắn hơn của chuỗi luôn <strong>nhỏ hơn theo thứ tự từ điển</strong>.</p>

<ul>
	<li>Ví dụ, <code>&quot;ab&quot;</code> nhỏ hơn <code>&quot;aba&quot;</code> theo thứ tự từ điển.</li>
</ul>

<p>Lá là node không có node con.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0988.Smallest%20String%20Starting%20From%20Leaf/images/tree1.png" style="width: 534px; height: 358px;" />
<pre>
<strong>Đầu vào:</strong> root = [0,1,2,3,4,3,4]
<strong>Đầu ra:</strong> &quot;dba&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0988.Smallest%20String%20Starting%20From%20Leaf/images/tree2.png" style="width: 534px; height: 358px;" />
<pre>
<strong>Đầu vào:</strong> root = [25,1,3,1,3,0,2]
<strong>Đầu ra:</strong> &quot;adz&quot;
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0988.Smallest%20String%20Starting%20From%20Leaf/images/tree3.png" style="height: 490px; width: 468px;" />
<pre>
<strong>Đầu vào:</strong> root = [2,2,1,null,1,0,null,0]
<strong>Đầu ra:</strong> &quot;abc&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong đoạn <code>[1, 8500]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 25</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đường đi từ lá đến gốc tạo thành một chuỗi; ta cần tìm chuỗi nhỏ nhất theo thứ tự từ điển. Cây có tối đa $8500$ node nên có thể duyệt mọi đường đi từ gốc đến lá. DFS thêm chữ cái vào path; khi đến lá, đảo ngược path rồi so sánh với đáp án, sau đó bỏ chữ cái vừa thêm ra.

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
    def smallestFromLeaf(self, root: TreeNode) -> str:
        ans = chr(ord('z') + 1)

        def dfs(root, path):
            nonlocal ans
            if root:
                path.append(chr(ord('a') + root.val))
                if root.left is None and root.right is None:
                    ans = min(ans, ''.join(reversed(path)))
                dfs(root.left, path)
                dfs(root.right, path)
                path.pop()

        dfs(root, [])
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
    private StringBuilder path;
    private String ans;

    public String smallestFromLeaf(TreeNode root) {
        path = new StringBuilder();
        ans = String.valueOf((char) ('z' + 1));
        dfs(root, path);
        return ans;
    }

    private void dfs(TreeNode root, StringBuilder path) {
        if (root != null) {
            path.append((char) ('a' + root.val));
            if (root.left == null && root.right == null) {
                String t = path.reverse().toString();
                if (t.compareTo(ans) < 0) {
                    ans = t;
                }
                path.reverse();
            }
            dfs(root.left, path);
            dfs(root.right, path);
            path.deleteCharAt(path.length() - 1);
        }
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
    string ans = "";

    string smallestFromLeaf(TreeNode* root) {
        string path = "";
        dfs(root, path);
        return ans;
    }

    void dfs(TreeNode* root, string& path) {
        if (!root) return;
        path += 'a' + root->val;
        if (!root->left && !root->right) {
            string t = path;
            reverse(t.begin(), t.end());
            if (ans == "" || t < ans) ans = t;
        }
        dfs(root->left, path);
        dfs(root->right, path);
        path.pop_back();
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
func smallestFromLeaf(root *TreeNode) string {
	ans := ""
	var dfs func(root *TreeNode, path string)
	dfs = func(root *TreeNode, path string) {
		if root == nil {
			return
		}
		path = string('a'+root.Val) + path
		if root.Left == nil && root.Right == nil {
			if ans == "" || path < ans {
				ans = path
			}
			return
		}
		dfs(root.Left, path)
		dfs(root.Right, path)
	}

	dfs(root, "")
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
