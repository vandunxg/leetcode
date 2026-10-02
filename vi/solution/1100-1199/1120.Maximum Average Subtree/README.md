---
comments: true
difficulty: Medium
rating: 1361
source: Biweekly Contest 4 Q3
tags:
    - Tree
    - Depth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [1120. Maximum Average Subtree 🔒](https://leetcode.com/problems/maximum-average-subtree)

[中文文档](/solution/1100-1199/1120.Maximum%20Average%20Subtree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một cây nhị phân, hãy trả về giá trị <em><strong>trung bình</strong> lớn nhất trong các <strong>subtree</strong> của cây</em>. Các đáp án sai lệch không quá <code>10<sup>-5</sup></code> so với đáp án chính xác được chấp nhận.</p>

<p><strong>Subtree</strong> của một cây gồm một node bất kỳ của cây đó cùng tất cả các hậu duệ của nó.</p>

<p>Giá trị <strong>trung bình</strong> của một cây bằng tổng giá trị các node chia cho số node.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1100-1199/1120.Maximum%20Average%20Subtree/images/1308_example_1.png" style="width: 132px; height: 123px;" />
<pre>
<strong>Đầu vào:</strong> root = [5,6,1]
<strong>Đầu ra:</strong> 6.00000
<strong>Giải thích:</strong> 
Với node có giá trị = 5, giá trị trung bình là (5 + 6 + 1) / 3 = 4.
Với node có giá trị = 6, giá trị trung bình là 6 / 1 = 6.
Với node có giá trị = 1, giá trị trung bình là 1 / 1 = 1.
Vậy đáp án là 6, giá trị lớn nhất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [0,null,1]
<strong>Đầu ra:</strong> 1.00000
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 10<sup>4</sup>]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Muốn tính trung bình của một subtree, ta cần tổng giá trị và số node của subtree đó; cả hai đại lượng đều được tổng hợp từ dưới lên. Nếu duyệt lại mọi subtree từ từng node thì độ phức tạp sẽ là bậc hai.
>
> Một lượt DFS hậu thứ tự trả về $(\textit{sum},\textit{count})$. Trung bình tại node hiện tại được dùng để cập nhật đáp án toàn cục, rồi hai giá trị này được trả về node cha. Mỗi node chỉ được duyệt một lần.

<!-- thinking:end -->

Ta có thể dùng phương pháp đệ quy. Với mỗi node, ta tính tổng và số node của subtree gốc tại node đó, sau đó tính giá trị trung bình, so sánh với giá trị lớn nhất hiện tại và cập nhật nếu cần.

Vì vậy, ta thiết kế hàm `dfs(root)` để biểu diễn tổng và số node trong subtree gốc tại `root`. Giá trị trả về là một mảng có độ dài 2: phần tử đầu là tổng giá trị các node, phần tử thứ hai là số node.

Quá trình đệ quy của hàm `dfs(root)` như sau:

- Nếu `root` là null, trả về `[0, 0]`;
- Nếu không, tính tổng và số node trong subtree trái của `root`, lần lượt ký hiệu là `[ls, ln]`; tính tổng và số node trong subtree phải của `root`, lần lượt ký hiệu là `[rs, rn]`. Tổng các node trong subtree gốc tại `root` là `root.val + ls + rs`, số node là `1 + ln + rn`. Tính giá trị trung bình, so sánh với giá trị lớn nhất hiện tại và cập nhật nếu cần;
- Trả về `[root.val + ls + rs, 1 + ln + rn]`.

Cuối cùng, trả về giá trị lớn nhất.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node trong cây nhị phân.

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
    def maximumAverageSubtree(self, root: Optional[TreeNode]) -> float:
        def dfs(root):
            if root is None:
                return 0, 0
            ls, ln = dfs(root.left)
            rs, rn = dfs(root.right)
            s = root.val + ls + rs
            n = 1 + ln + rn
            nonlocal ans
            ans = max(ans, s / n)
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
    private double ans;

    public double maximumAverageSubtree(TreeNode root) {
        dfs(root);
        return ans;
    }

    private int[] dfs(TreeNode root) {
        if (root == null) {
            return new int[2];
        }
        var l = dfs(root.left);
        var r = dfs(root.right);
        int s = root.val + l[0] + r[0];
        int n = 1 + l[1] + r[1];
        ans = Math.max(ans, s * 1.0 / n);
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
    double maximumAverageSubtree(TreeNode* root) {
        double ans = 0;
        auto dfs = [&](this auto&& dfs, TreeNode* root) -> pair<int, int> {
            if (!root) {
                return {0, 0};
            }
            auto [ls, ln] = dfs(root->left);
            auto [rs, rn] = dfs(root->right);
            int s = root->val + ls + rs;
            int n = 1 + ln + rn;
            ans = max(ans, s * 1.0 / n);
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
func maximumAverageSubtree(root *TreeNode) (ans float64) {
	var dfs func(*TreeNode) [2]int
	dfs = func(root *TreeNode) [2]int {
		if root == nil {
			return [2]int{}
		}
		l, r := dfs(root.Left), dfs(root.Right)
		s := root.Val + l[0] + r[0]
		n := 1 + l[1] + r[1]
		ans = math.Max(ans, float64(s)/float64(n))
		return [2]int{s, n}
	}
	dfs(root)
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
