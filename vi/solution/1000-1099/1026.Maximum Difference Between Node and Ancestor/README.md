---
comments: true
difficulty: Medium
rating: 1446
source: Weekly Contest 132 Q2
tags:
    - Tree
    - Depth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [1026. Maximum Difference Between Node and Ancestor](https://leetcode.com/problems/maximum-difference-between-node-and-ancestor)

[中文文档](/solution/1000-1099/1026.Maximum%20Difference%20Between%20Node%20and%20Ancestor/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một binary tree, hãy tìm giá trị lớn nhất <code>v</code> sao cho tồn tại hai node <strong>khác nhau</strong> <code>a</code> và <code>b</code> thỏa mãn <code>v = |a.val - b.val|</code>, trong đó <code>a</code> là ancestor của <code>b</code>.</p>

<p>Node <code>a</code> là ancestor của <code>b</code> nếu một node con bất kỳ của <code>a</code> chính là <code>b</code>&nbsp;hoặc một node con bất kỳ của <code>a</code> là ancestor của <code>b</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1026.Maximum%20Difference%20Between%20Node%20and%20Ancestor/images/tmp-tree.jpg" style="width: 400px; height: 390px;" />
<pre>
<strong>Đầu vào:</strong> root = [8,3,10,1,6,null,14,null,null,4,7,13]
<strong>Đầu ra:</strong> 7
<strong>Giải thích: </strong>Một số hiệu giữa ancestor và node như sau:
|8 - 3| = 5
|3 - 7| = 4
|8 - 1| = 7
|10 - 13| = 3
Trong tất cả các hiệu có thể có, giá trị lớn nhất là 7, đạt được với |8 - 1| = 7.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1026.Maximum%20Difference%20Between%20Node%20and%20Ancestor/images/tmp-tree-1.jpg" style="width: 250px; height: 349px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,null,2,null,0,3]
<strong>Đầu ra:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong đoạn <code>[2, 5000]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Thu thập tất cả ancestor tại mỗi node khiến ta lặp lại các đường đi, khá tốn kém khi $n\le 5000$. Ta chỉ cần quan tâm giá trị nhỏ nhất và lớn nhất trên đường đi.
>
> DFS truyền cặp $(\textit{mi},\textit{mx})$ xuống dưới, cập nhật đáp án bằng $|\textit{val}-\textit{mi}|$ và $|\textit{val}-\textit{mx}|$, rồi đưa giá trị hiện tại vào khoảng giá trị.
>
> Chỉ cần duyệt cây một lần.

<!-- thinking:end -->

Với mỗi node, để tìm hiệu lớn nhất với các ancestor, ta chỉ cần xét giá trị lớn nhất và nhỏ nhất trong số các ancestor đó. Đáp án là hiệu lớn nhất giữa mọi node và các ancestor của nó.

Vì vậy, ta xây dựng hàm $dfs(root, mi, mx)$, trong đó $root$ là node đang xét, $mx$ là giá trị lớn nhất của các ancestor và $mi$ là giá trị nhỏ nhất. Hàm này cập nhật hiệu lớn nhất $ans$.

Hàm $dfs(root, mi, mx)$ hoạt động như sau:

- Nếu $root$ là null, kết thúc hàm.
- Nếu không, cập nhật $ans = max(ans, |mi - root.val|, |mx - root.val|)$.
- Sau đó, cập nhật $mi = min(mi, root.val)$, $mx = max(mx, root.val)$ rồi đệ quy duyệt cây con trái và phải.

Trong hàm chính, ta gọi $dfs(root, root.val, root.val)$ rồi trả về $ans$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node trong binary tree.

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
    def maxAncestorDiff(self, root: Optional[TreeNode]) -> int:
        def dfs(root: Optional[TreeNode], mi: int, mx: int):
            if root is None:
                return
            nonlocal ans
            ans = max(ans, abs(mi - root.val), abs(mx - root.val))
            mi = min(mi, root.val)
            mx = max(mx, root.val)
            dfs(root.left, mi, mx)
            dfs(root.right, mi, mx)

        ans = 0
        dfs(root, root.val, root.val)
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

    public int maxAncestorDiff(TreeNode root) {
        dfs(root, root.val, root.val);
        return ans;
    }

    private void dfs(TreeNode root, int mi, int mx) {
        if (root == null) {
            return;
        }
        int x = Math.max(Math.abs(mi - root.val), Math.abs(mx - root.val));
        ans = Math.max(ans, x);
        mi = Math.min(mi, root.val);
        mx = Math.max(mx, root.val);
        dfs(root.left, mi, mx);
        dfs(root.right, mi, mx);
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
    int maxAncestorDiff(TreeNode* root) {
        int ans = 0;
        function<void(TreeNode*, int, int)> dfs = [&](TreeNode* root, int mi, int mx) {
            if (!root) {
                return;
            }
            ans = max({ans, abs(mi - root->val), abs(mx - root->val)});
            mi = min(mi, root->val);
            mx = max(mx, root->val);
            dfs(root->left, mi, mx);
            dfs(root->right, mi, mx);
        };
        dfs(root, root->val, root->val);
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
func maxAncestorDiff(root *TreeNode) (ans int) {
	var dfs func(*TreeNode, int, int)
	dfs = func(root *TreeNode, mi, mx int) {
		if root == nil {
			return
		}
		ans = max(ans, max(abs(mi-root.Val), abs(mx-root.Val)))
		mi = min(mi, root.Val)
		mx = max(mx, root.Val)
		dfs(root.Left, mi, mx)
		dfs(root.Right, mi, mx)
	}
	dfs(root, root.Val, root.Val)
	return
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
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

function maxAncestorDiff(root: TreeNode | null): number {
    const dfs = (root: TreeNode | null, mi: number, mx: number): void => {
        if (!root) {
            return;
        }
        ans = Math.max(ans, Math.abs(root.val - mi), Math.abs(root.val - mx));
        mi = Math.min(mi, root.val);
        mx = Math.max(mx, root.val);
        dfs(root.left, mi, mx);
        dfs(root.right, mi, mx);
    };
    let ans: number = 0;
    dfs(root, root.val, root.val);
    return ans;
}
```

#### JavaScript

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number}
 */
var maxAncestorDiff = function (root) {
    let ans = 0;
    const dfs = (root, mi, mx) => {
        if (!root) {
            return;
        }
        ans = Math.max(ans, Math.abs(mi - root.val), Math.abs(mx - root.val));
        mi = Math.min(mi, root.val);
        mx = Math.max(mx, root.val);
        dfs(root.left, mi, mx);
        dfs(root.right, mi, mx);
    };
    dfs(root, root.val, root.val);
    return ans;
};
```

#### C#

```cs
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left;
 *     public TreeNode right;
 *     public TreeNode(int val=0, TreeNode left=null, TreeNode right=null) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
public class Solution {
    private int ans;

    public int MaxAncestorDiff(TreeNode root) {
        dfs(root, root.val, root.val);
        return ans;
    }

    private void dfs(TreeNode root, int mi, int mx) {
        if (root == null) {
            return;
        }
        int x = Math.Max(Math.Abs(mi - root.val), Math.Abs(mx - root.val));
        ans = Math.Max(ans, x);
        mi = Math.Min(mi, root.val);
        mx = Math.Max(mx, root.val);
        dfs(root.left, mi, mx);
        dfs(root.right, mi, mx);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
