---
comments: true
difficulty: Easy
tags:
    - Tree
    - Depth-First Search
    - String
    - Backtracking
    - Binary Tree
---

<!-- problem:start -->

# [257. Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths)

[中文文档](/solution/0200-0299/0257.Binary%20Tree%20Paths/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một cây nhị phân, hãy trả về <em>tất cả đường đi từ gốc đến lá theo <strong>bất kỳ thứ tự nào</strong></em>.</p>

<p><strong>Lá</strong> là node không có node con.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0200-0299/0257.Binary%20Tree%20Paths/images/paths-tree.jpg" style="width: 207px; height: 293px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,2,3,null,5]
<strong>Đầu ra:</strong> [&quot;1-&gt;2-&gt;5&quot;,&quot;1-&gt;3&quot;]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [1]
<strong>Đầu ra:</strong> [&quot;1&quot;]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 100]</code>.</li>
	<li><code>-100 &lt;= Node.val &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Ta ghi lại đường đi từ gốc đến lá trong lúc duyệt. DFS thêm giá trị node hiện tại vào đường đi, ghép các giá trị thành một chuỗi khi tới node lá, rồi xóa giá trị đó khi quay lui.

<!-- thinking:end -->

Ta có thể dùng DFS để duyệt toàn bộ cây nhị phân. Mỗi lần đến một node, ta thêm node đó vào đường đi. Nếu node hiện tại là node lá, ta thêm toàn bộ đường đi vào kết quả. Nếu không, ta tiếp tục đệ quy duyệt các node con. Cuối cùng, khi lời gọi đệ quy kết thúc và quay lại node hiện tại, ta cần xóa node đó khỏi đường đi.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node trong cây nhị phân.

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
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        def dfs(root: Optional[TreeNode]):
            if root is None:
                return
            t.append(str(root.val))
            if root.left is None and root.right is None:
                ans.append("->".join(t))
            else:
                dfs(root.left)
                dfs(root.right)
            t.pop()

        ans = []
        t = []
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
    private List<String> ans = new ArrayList<>();
    private List<String> t = new ArrayList<>();

    public List<String> binaryTreePaths(TreeNode root) {
        dfs(root);
        return ans;
    }

    private void dfs(TreeNode root) {
        if (root == null) {
            return;
        }
        t.add(root.val + "");
        if (root.left == null && root.right == null) {
            ans.add(String.join("->", t));
        } else {
            dfs(root.left);
            dfs(root.right);
        }
        t.remove(t.size() - 1);
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
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> ans;
        vector<string> t;
        function<void(TreeNode*)> dfs = [&](TreeNode* root) {
            if (!root) {
                return;
            }
            t.push_back(to_string(root->val));
            if (!root->left && !root->right) {
                ans.push_back(join(t));
            } else {
                dfs(root->left);
                dfs(root->right);
            }
            t.pop_back();
        };
        dfs(root);
        return ans;
    }

    string join(vector<string>& t, string sep = "->") {
        string ans;
        for (int i = 0; i < t.size(); ++i) {
            if (i > 0) {
                ans += sep;
            }
            ans += t[i];
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
func binaryTreePaths(root *TreeNode) (ans []string) {
	t := []string{}
	var dfs func(*TreeNode)
	dfs = func(root *TreeNode) {
		if root == nil {
			return
		}
		t = append(t, strconv.Itoa(root.Val))
		if root.Left == nil && root.Right == nil {
			ans = append(ans, strings.Join(t, "->"))
		} else {
			dfs(root.Left)
			dfs(root.Right)
		}
		t = t[:len(t)-1]
	}
	dfs(root)
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

function binaryTreePaths(root: TreeNode | null): string[] {
    const ans: string[] = [];
    const t: number[] = [];
    const dfs = (root: TreeNode | null) => {
        if (!root) {
            return;
        }
        t.push(root.val);
        if (!root.left && !root.right) {
            ans.push(t.join('->'));
        } else {
            dfs(root.left);
            dfs(root.right);
        }
        t.pop();
    };
    dfs(root);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
