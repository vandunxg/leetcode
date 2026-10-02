---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [971. Flip Binary Tree To Match Preorder Traversal](https://leetcode.com/problems/flip-binary-tree-to-match-preorder-traversal)

[中文文档](/solution/0900-0999/0971.Flip%20Binary%20Tree%20To%20Match%20Preorder%20Traversal/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của cây nhị phân có <code>n</code> node; mỗi node có giá trị duy nhất từ <code>1</code> đến <code>n</code>. Bạn cũng được cho dãy gồm <code>n</code> giá trị <code>voyage</code>, là thứ tự <a href="https://en.wikipedia.org/wiki/Tree_traversal#Pre-order" target="_blank"><strong>duyệt preorder</strong></a> <strong>mong muốn</strong> của cây.</p>

<p>Bạn có thể <strong>lật</strong> bất kỳ node nào trong cây nhị phân bằng cách hoán đổi cây con trái và cây con phải. Ví dụ, lật node 1 sẽ tạo ra kết quả sau:</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0971.Flip%20Binary%20Tree%20To%20Match%20Preorder%20Traversal/images/fliptree.jpg" style="width: 400px; height: 187px;" />
<p>Hãy lật <strong>ít node nhất</strong> sao cho thứ tự <strong>duyệt preorder</strong> của cây <strong>khớp với</strong> <code>voyage</code>.</p>

<p>Hãy trả về <em>danh sách giá trị của tất cả node được </em><strong>lật</strong><em>. Có thể trả về đáp án theo </em><strong>bất kỳ thứ tự nào</strong><em>. Nếu </em><strong>không thể</strong><em> lật các node để thứ tự duyệt preorder của cây khớp với </em><code>voyage</code><em>, hãy trả về danh sách </em><code>[-1]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0971.Flip%20Binary%20Tree%20To%20Match%20Preorder%20Traversal/images/1219-01.png" style="width: 150px; height: 205px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,2], voyage = [2,1]
<strong>Đầu ra:</strong> [-1]
<strong>Giải thích:</strong> Không thể lật các node sao cho thứ tự duyệt preorder khớp với voyage.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0971.Flip%20Binary%20Tree%20To%20Match%20Preorder%20Traversal/images/1219-02.png" style="width: 150px; height: 142px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,2,3], voyage = [1,3,2]
<strong>Đầu ra:</strong> [1]
<strong>Giải thích:</strong> Lật node 1 sẽ hoán đổi node 2 và 3, nhờ đó thứ tự duyệt preorder khớp với voyage.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0971.Flip%20Binary%20Tree%20To%20Match%20Preorder%20Traversal/images/1219-02.png" style="width: 150px; height: 142px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,2,3], voyage = [1,2,3]
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> Thứ tự duyệt preorder của cây đã khớp với voyage, nên không cần lật node nào.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>Cây có <code>n</code> node.</li>
	<li><code>n == voyage.length</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= Node.val, voyage[i] &lt;= n</code></li>
	<li>Tất cả giá trị trong cây đều <strong>khác nhau</strong>.</li>
	<li>Tất cả giá trị trong <code>voyage</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Sau một số lần lật cây con, thứ tự preorder cần khớp với $\textit{voyage}$. Vì các giá trị là duy nhất, node hiện tại phải khớp với $\textit{voyage}[i]$. Nếu có node con trái nhưng giá trị của nó không phải giá trị cần gặp tiếp theo, ta phải lật node hiện tại và duyệt cây con phải trước. Một lượt DFS sẽ cập nhật chỉ số; nếu thất bại, trả về $[-1]$.

<!-- thinking:end -->

Ta có thể duyệt toàn bộ cây bằng depth-first search, dùng chỉ số $i$ để theo dõi vị trí hiện tại trong mảng $\textit{voyage}$. Nếu giá trị của node hiện tại không bằng $\textit{voyage}[i]$, nghĩa là không thể đạt thứ tự mong muốn; đặt $\textit{ok}$ thành `false` rồi dừng ngay. Nếu khớp, tăng $i$ thêm $1$, sau đó kiểm tra node hiện tại có node con trái hay không. Nếu không có, hoặc giá trị node con trái bằng $\textit{voyage}[i]$, đệ quy duyệt cây con trái rồi cây con phải; nếu không, cần lật node hiện tại rồi đệ quy duyệt cây con phải trước, cây con trái sau.

Sau khi tìm kiếm, nếu $\textit{ok}$ là `true`, nghĩa là có thể đạt thứ tự mong muốn sau khi lật; trả về mảng đáp án $\textit{ans}$. Nếu không, trả về $[-1]$.

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
    def flipMatchVoyage(self, root: Optional[TreeNode], voyage: List[int]) -> List[int]:
        def dfs(root):
            nonlocal i, ok
            if root is None or not ok:
                return
            if root.val != voyage[i]:
                ok = False
                return
            i += 1
            if root.left is None or root.left.val == voyage[i]:
                dfs(root.left)
                dfs(root.right)
            else:
                ans.append(root.val)
                dfs(root.right)
                dfs(root.left)

        ans = []
        i = 0
        ok = True
        dfs(root)
        return ans if ok else [-1]
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
    private int i;
    private boolean ok;
    private int[] voyage;
    private List<Integer> ans = new ArrayList<>();

    public List<Integer> flipMatchVoyage(TreeNode root, int[] voyage) {
        this.voyage = voyage;
        ok = true;
        dfs(root);
        return ok ? ans : List.of(-1);
    }

    private void dfs(TreeNode root) {
        if (root == null || !ok) {
            return;
        }
        if (root.val != voyage[i]) {
            ok = false;
            return;
        }
        ++i;
        if (root.left == null || root.left.val == voyage[i]) {
            dfs(root.left);
            dfs(root.right);
        } else {
            ans.add(root.val);
            dfs(root.right);
            dfs(root.left);
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
    vector<int> flipMatchVoyage(TreeNode* root, vector<int>& voyage) {
        bool ok = true;
        int i = 0;
        vector<int> ans;
        function<void(TreeNode*)> dfs = [&](TreeNode* root) {
            if (!root || !ok) {
                return;
            }
            if (root->val != voyage[i]) {
                ok = false;
                return;
            }
            ++i;
            if (!root->left || root->left->val == voyage[i]) {
                dfs(root->left);
                dfs(root->right);
            } else {
                ans.push_back(root->val);
                dfs(root->right);
                dfs(root->left);
            }
        };
        dfs(root);
        return ok ? ans : vector<int>{-1};
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
func flipMatchVoyage(root *TreeNode, voyage []int) []int {
	i := 0
	ok := true
	ans := []int{}
	var dfs func(*TreeNode)
	dfs = func(root *TreeNode) {
		if root == nil || !ok {
			return
		}
		if root.Val != voyage[i] {
			ok = false
			return
		}
		i++
		if root.Left == nil || root.Left.Val == voyage[i] {
			dfs(root.Left)
			dfs(root.Right)
		} else {
			ans = append(ans, root.Val)
			dfs(root.Right)
			dfs(root.Left)
		}
	}
	dfs(root)
	if !ok {
		return []int{-1}
	}
	return ans
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

function flipMatchVoyage(root: TreeNode | null, voyage: number[]): number[] {
    let ok = true;
    let i = 0;
    const ans: number[] = [];
    const dfs = (root: TreeNode | null): void => {
        if (!root || !ok) {
            return;
        }
        if (root.val !== voyage[i++]) {
            ok = false;
            return;
        }
        if (!root.left || root.left.val === voyage[i]) {
            dfs(root.left);
            dfs(root.right);
        } else {
            ans.push(root.val);
            dfs(root.right);
            dfs(root.left);
        }
    };
    dfs(root);
    return ok ? ans : [-1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
