---
comments: true
difficulty: Medium
rating: 1472
source: Weekly Contest 292 Q2
tags:
    - Tree
    - Depth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [2265. Count Nodes Equal to Average of Subtree](https://leetcode.com/problems/count-nodes-equal-to-average-of-subtree)

[中文文档](/solution/2200-2299/2265.Count%20Nodes%20Equal%20to%20Average%20of%20Subtree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một cây nhị phân, hãy trả về <em>số node có giá trị bằng <strong>average</strong> của các giá trị trong <strong>cây con</strong> của node đó</em>.</p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li><strong>Average</strong> của <code>n</code> phần tử là <strong>tổng</strong> của <code>n</code> phần tử chia cho <code>n</code> và <strong>làm tròn xuống</strong> đến số nguyên gần nhất.</li>
	<li><strong>Cây con</strong> của <code>root</code> là một cây gồm <code>root</code> và tất cả các hậu duệ của nó.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2265.Count%20Nodes%20Equal%20to%20Average%20of%20Subtree/images/image-20220315203925-1.png" style="width: 300px; height: 212px;" />
<pre>
<strong>Đầu vào:</strong> root = [4,8,5,0,1,null,6]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong>
Với node có giá trị 4: Average của cây con là (4 + 8 + 5 + 0 + 1 + 6) / 6 = 24 / 6 = 4.
Với node có giá trị 5: Average của cây con là (5 + 6) / 2 = 11 / 2 = 5.
Với node có giá trị 0: Average của cây con là 0 / 1 = 0.
Với node có giá trị 1: Average của cây con là 1 / 1 = 1.
Với node có giá trị 6: Average của cây con là 6 / 1 = 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2265.Count%20Nodes%20Equal%20to%20Average%20of%20Subtree/images/image-20220326133920-1.png" style="width: 80px; height: 76px;" />
<pre>
<strong>Đầu vào:</strong> root = [1]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Với node có giá trị 1: Average của cây con là 1 / 1 = 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 1000]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các node có giá trị bằng average làm tròn xuống của cây con tương ứng. Với $n \le 10^3$, ta có thể duyệt lại cây cho từng node, nhưng cách đó lặp lại nhiều công việc. Một lần duyệt post-order có thể trả về tổng và số node của cây con.
>
> $\textit{dfs}$ trả về $(s, n)$; so sánh $\lfloor s/n \rfloor$ với giá trị của node rồi tăng biến đếm. Cây rỗng trả về $(0,0)$.

<!-- thinking:end -->

Ta xây dựng hàm $\textit{dfs}$ để tính tổng và số node của cây con có gốc là node hiện tại.

Quá trình thực thi hàm $\textit{dfs}$ như sau:

- Nếu node hiện tại là null, trả về $(0, 0)$.
- Nếu không, ta đệ quy tính tổng và số node của cây con bên trái và bên phải, lần lượt ký hiệu là $(\textit{ls}, \textit{ln})$ và $(\textit{rs}, \textit{rn})$. Khi đó, tổng $\textit{s}$ và số node $\textit{n}$ của cây con có gốc tại node hiện tại lần lượt là $\textit{ls} + \textit{rs} + \textit{root.val}$ và $\textit{ln} + \textit{rn} + 1$. Nếu $\textit{s} / \textit{n} = \textit{root.val}$, node hiện tại thỏa mãn yêu cầu của bài toán, do đó ta tăng đáp án $\textit{ans}$ lên $1$.
- Cuối cùng, hàm $\textit{dfs}$ trả về $\textit{s}$ và $\textit{n}$.

Ta khởi tạo đáp án $\textit{ans}$ bằng $0$, sau đó gọi hàm $\textit{dfs}$ và cuối cùng trả về đáp án $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số node trong cây nhị phân.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def averageOfSubtree(self, root: TreeNode) -> int:
        def dfs(root) -> tuple:
            if not root:
                return 0, 0
            ls, ln = dfs(root.left)
            rs, rn = dfs(root.right)
            s = ls + rs + root.val
            n = ln + rn + 1
            nonlocal ans
            ans += int(s // n == root.val)
            return s, n

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

    public int averageOfSubtree(TreeNode root) {
        dfs(root);
        return ans;
    }

    private int[] dfs(TreeNode root) {
        if (root == null) {
            return new int[2];
        }
        var l = dfs(root.left);
        var r = dfs(root.right);
        int s = l[0] + r[0] + root.val;
        int n = l[1] + r[1] + 1;
        if (s / n == root.val) {
            ++ans;
        }
        return new int[] {s, n};
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
    int averageOfSubtree(TreeNode* root) {
        int ans = 0;
        auto dfs = [&](this auto&& dfs, TreeNode* root) -> pair<int, int> {
            if (!root) {
                return {0, 0};
            }
            auto [ls, ln] = dfs(root->left);
            auto [rs, rn] = dfs(root->right);
            int s = ls + rs + root->val;
            int n = ln + rn + 1;
            if (s / n == root->val) {
                ++ans;
            }
            return {s, n};
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
func averageOfSubtree(root *TreeNode) (ans int) {
	var dfs func(root *TreeNode) (int, int)
	dfs = func(root *TreeNode) (int, int) {
		if root == nil {
			return 0, 0
		}
		ls, ln := dfs(root.Left)
		rs, rn := dfs(root.Right)
		s, n := ls+rs+root.Val, ln+rn+1
		if s/n == root.Val {
			ans++
		}
		return s, n
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

function averageOfSubtree(root: TreeNode | null): number {
    let ans: number = 0;
    const dfs = (root: TreeNode | null): [number, number] => {
        if (!root) {
            return [0, 0];
        }
        const [ls, ln] = dfs(root.left);
        const [rs, rn] = dfs(root.right);
        const s = ls + rs + root.val;
        const n = ln + rn + 1;
        if (Math.floor(s / n) === root.val) {
            ++ans;
        }
        return [s, n];
    };
    dfs(root);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
