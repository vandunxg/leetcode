---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [2773. Height of Special Binary Tree 🔒](https://leetcode.com/problems/height-of-special-binary-tree)

[中文文档](/solution/2700-2799/2773.Height%20of%20Special%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một <code>root</code>, là gốc của một cây nhị phân <strong>đặc biệt</strong> có <code>n</code> node. Các node của cây nhị phân đặc biệt được đánh số từ <code>1</code> đến <code>n</code>. Giả sử cây có <code>k</code> node lá theo thứ tự: <code>b<sub>1 </sub>&lt;<sub> </sub>b<sub>2 </sub>&lt; ... &lt; b<sub>k</sub></code>.</p>

<p>Các lá của cây này có một tính chất <strong>đặc biệt</strong>! Cụ thể, với mọi lá <code>b<sub>i</sub></code>, các điều kiện sau được thỏa mãn:</p>

<ul>
	<li>Con phải của <code>b<sub>i</sub></code> là <code>b<sub>i + 1</sub></code> nếu <code>i &lt; k</code>, và là <code>b<sub>1</sub></code> trong trường hợp ngược lại.</li>
	<li>Con trái của <code>b<sub>i</sub></code> là <code>b<sub>i - 1</sub></code> nếu <code>i &gt; 1</code>, và là <code>b<sub>k</sub></code> trong trường hợp ngược lại.</li>
</ul>

<p>Hãy trả về <em>chiều cao của cây đã cho</em>.</p>

<p><strong>Lưu ý:</strong> Chiều cao của một cây nhị phân là độ dài của <strong>đường đi dài nhất</strong> từ gốc đến bất kỳ node nào khác.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [1,2,3,null,null,4,5]
<strong>Đầu ra:</strong> 2
<strong>Giải thích: </strong>Cây đã cho được minh họa trong hình dưới đây. Con trái của mỗi lá là chiếc lá bên trái nó (được biểu diễn bằng các cạnh màu xanh dương). Con phải của mỗi lá là chiếc lá bên phải nó (được biểu diễn bằng các cạnh màu đỏ). Ta thấy đồ thị có chiều cao bằng 2.
</pre>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2700-2799/2773.Height%20of%20Special%20Binary%20Tree/images/1.png" style="padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; width: 200px; height: 200px;" /></p>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [1,2]
<strong>Đầu ra:</strong> 1
<strong>Giải thích: </strong>Cây đã cho được minh họa trong hình dưới đây. Cây chỉ có một lá nên lá đó không có con trái hay con phải. Ta thấy đồ thị có chiều cao bằng 1.
</pre>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2700-2799/2773.Height%20of%20Special%20Binary%20Tree/images/2.png" style="padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; width: 95px; height: 122px;" /></p>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [1,2,3,null,null,4,null,5,6]
<strong>Đầu ra:</strong> 3
<strong>Giải thích: </strong>Cây đã cho được minh họa trong hình dưới đây. Con trái của mỗi lá là chiếc lá bên trái nó (được biểu diễn bằng các cạnh màu xanh dương). Con phải của mỗi lá là chiếc lá bên phải nó (được biểu diễn bằng các cạnh màu đỏ). Ta thấy đồ thị có chiều cao bằng 3.
</pre>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2700-2799/2773.Height%20of%20Special%20Binary%20Tree/images/3.png" style="padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; width: 200px; height: 280px;" /></p>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == number of nodes in the tree</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= node.val &lt;= n</code></li>
	<li>Đầu vào được tạo sao cho mỗi <code>node.val</code> là duy nhất.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Trong cây đặc biệt này, hai node con của một lá trỏ tới nhau; ta cần tìm chiều cao. DFS thông thường sẽ rơi vào chu kỳ tại một lá.
>
> Chỉ đi xuống node con trái khi $left.right$ không phải là node hiện tại, và chỉ đi xuống node con phải khi $right.left$ không phải là node hiện tại. Như vậy, ta bỏ qua các cạnh nối chéo tại các lá, đồng thời cập nhật độ sâu để tìm chiều cao.

<!-- thinking:end -->

Điểm mấu chốt của bài toán là xác định một node có phải là node lá hay không. Ta xây dựng hàm $dfs(root, d)$, trong đó $root$ là node hiện tại và $d$ là độ sâu của node hiện tại. Mỗi lần tìm kiếm, ta cập nhật đáp án $ans = \max(ans, d)$, sau đó xác định node hiện tại có phải là node lá hay không. Nếu node hiện tại có node con trái và node con phải của node con trái không phải là node hiện tại, ta gọi đệ quy $dfs(root.left, d + 1)$. Nếu node hiện tại có node con phải và node con trái của node con phải không phải là node hiện tại, ta gọi đệ quy $dfs(root.right, d + 1)$.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số node trong cây nhị phân.

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
    def heightOfTree(self, root: Optional[TreeNode]) -> int:
        def dfs(root: Optional[TreeNode], d: int):
            nonlocal ans
            ans = max(ans, d)
            if root.left and root.left.right != root:
                dfs(root.left, d + 1)
            if root.right and root.right.left != root:
                dfs(root.right, d + 1)

        ans = 0
        dfs(root, 0)
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

    public int heightOfTree(TreeNode root) {
        dfs(root, 0);
        return ans;
    }

    private void dfs(TreeNode root, int d) {
        ans = Math.max(ans, d++);
        if (root.left != null && root.left.right != root) {
            dfs(root.left, d);
        }
        if (root.right != null && root.right.left != root) {
            dfs(root.right, d);
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
    int heightOfTree(TreeNode* root) {
        int ans = 0;
        function<void(TreeNode*, int)> dfs = [&](TreeNode* root, int d) {
            ans = max(ans, d++);
            if (root->left && root->left->right != root) {
                dfs(root->left, d);
            }
            if (root->right && root->right->left != root) {
                dfs(root->right, d);
            }
        };
        dfs(root, 0);
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
func heightOfTree(root *TreeNode) (ans int) {
	var dfs func(*TreeNode, int)
	dfs = func(root *TreeNode, d int) {
		if ans < d {
			ans = d
		}
		d++
		if root.Left != nil && root.Left.Right != root {
			dfs(root.Left, d)
		}
		if root.Right != nil && root.Right.Left != root {
			dfs(root.Right, d)
		}
	}
	dfs(root, 0)
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

function heightOfTree(root: TreeNode | null): number {
    let ans = 0;
    const dfs = (root: TreeNode | null, d: number) => {
        ans = Math.max(ans, d++);
        if (root.left && root.left.right !== root) {
            dfs(root.left, d);
        }
        if (root.right && root.right.left !== root) {
            dfs(root.right, d);
        }
    };
    dfs(root, 0);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
