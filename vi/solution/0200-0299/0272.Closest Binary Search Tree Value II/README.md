---
comments: true
difficulty: Hard
tags:
    - Stack
    - Tree
    - Depth-First Search
    - Binary Search Tree
    - Two Pointers
    - Binary Tree
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [272. Closest Binary Search Tree Value II 🔒](https://leetcode.com/problems/closest-binary-search-tree-value-ii)

[中文文档](/solution/0200-0299/0272.Closest%20Binary%20Search%20Tree%20Value%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một binary search tree, giá trị <code>target</code> và số nguyên <code>k</code>, hãy trả về <em><code>k</code> giá trị trong BST gần với </em><code>target</code><em> nhất</em>. Bạn có thể trả về đáp án theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Đảm bảo rằng chỉ có duy nhất một tập hợp gồm <code>k</code> giá trị trong BST gần với <code>target</code> nhất.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0200-0299/0272.Closest%20Binary%20Search%20Tree%20Value%20II/images/closest1-1-tree.jpg" style="width: 292px; height: 302px;" />
<pre>
<strong>Đầu vào:</strong> root = [4,2,5,1,3], target = 3.714286, k = 2
<strong>Đầu ra:</strong> [4,3]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [1], target = 0.000000, k = 1
<strong>Đầu ra:</strong> [1]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây là <code>n</code>.</li>
	<li><code>1 &lt;= k &lt;= n &lt;= 10<sup>4</sup></code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 10<sup>9</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= target &lt;= 10<sup>9</sup></code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Giả sử BST cân bằng. Bạn có thể giải bài này với thời gian chạy nhỏ hơn <code>O(n)</code> (trong đó <code>n = total nodes</code>) không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Duyệt inorder BST sẽ cho các giá trị theo thứ tự tăng dần, nên $k$ giá trị gần nhất tạo thành một đoạn liên tiếp trong dãy đó. Dùng một deque và giữ tối đa $k$ giá trị trong lúc duyệt inorder.
>
> Khi deque đã đủ $k$ phần tử mà giá trị hiện tại không gần hơn, các giá trị bên phải sẽ chỉ càng xa hơn nên có thể dừng.

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
    def closestKValues(self, root: TreeNode, target: float, k: int) -> List[int]:
        def dfs(root):
            if root is None:
                return
            dfs(root.left)
            if len(q) < k:
                q.append(root.val)
            else:
                if abs(root.val - target) >= abs(q[0] - target):
                    return
                q.popleft()
                q.append(root.val)
            dfs(root.right)

        q = deque()
        dfs(root)
        return list(q)
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

    private List<Integer> ans;
    private double target;
    private int k;

    public List<Integer> closestKValues(TreeNode root, double target, int k) {
        ans = new LinkedList<>();
        this.target = target;
        this.k = k;
        dfs(root);
        return ans;
    }

    private void dfs(TreeNode root) {
        if (root == null) {
            return;
        }
        dfs(root.left);
        if (ans.size() < k) {
            ans.add(root.val);
        } else {
            if (Math.abs(root.val - target) >= Math.abs(ans.get(0) - target)) {
                return;
            }
            ans.remove(0);
            ans.add(root.val);
        }
        dfs(root.right);
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
    queue<int> q;
    double target;
    int k;

    vector<int> closestKValues(TreeNode* root, double target, int k) {
        this->target = target;
        this->k = k;
        dfs(root);
        vector<int> ans;
        while (!q.empty()) {
            ans.push_back(q.front());
            q.pop();
        }
        return ans;
    }

    void dfs(TreeNode* root) {
        if (!root) return;
        dfs(root->left);
        if (q.size() < k)
            q.push(root->val);
        else {
            if (abs(root->val - target) >= abs(q.front() - target)) return;
            q.pop();
            q.push(root->val);
        }
        dfs(root->right);
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
func closestKValues(root *TreeNode, target float64, k int) []int {
	var ans []int
	var dfs func(root *TreeNode)
	dfs = func(root *TreeNode) {
		if root == nil {
			return
		}
		dfs(root.Left)
		if len(ans) < k {
			ans = append(ans, root.Val)
		} else {
			if math.Abs(float64(root.Val)-target) >= math.Abs(float64(ans[0])-target) {
				return
			}
			ans = ans[1:]
			ans = append(ans, root.Val)
		}
		dfs(root.Right)
	}
	dfs(root)
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
