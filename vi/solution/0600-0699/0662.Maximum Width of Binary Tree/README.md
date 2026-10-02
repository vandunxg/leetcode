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

# [662. Maximum Width of Binary Tree](https://leetcode.com/problems/maximum-width-of-binary-tree)

[中文文档](/solution/0600-0699/0662.Maximum%20Width%20of%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một binary tree, hãy trả về <em><strong>độ rộng lớn nhất</strong> của cây</em>.</p>

<p><strong>Độ rộng lớn nhất</strong> của cây là <strong>độ rộng</strong> lớn nhất trong tất cả các level.</p>

<p><strong>Độ rộng</strong> của một level được tính bằng khoảng cách giữa hai node ở hai đầu (node khác null ngoài cùng bên trái và ngoài cùng bên phải). Các node null nằm giữa hai node đầu mút, vốn xuất hiện trong một complete binary tree kéo dài đến level đó, cũng được tính vào độ dài.</p>

<p>Đảm bảo kết quả nằm trong phạm vi số nguyên có dấu <strong>32-bit</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0662.Maximum%20Width%20of%20Binary%20Tree/images/width1-tree.jpg" style="width: 359px; height: 302px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,3,2,5,3,null,9]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Độ rộng lớn nhất nằm ở level thứ ba, có độ dài 4 (5,3,null,9).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0662.Maximum%20Width%20of%20Binary%20Tree/images/maximum-width-of-binary-tree-v3.jpg" style="width: 442px; height: 422px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,3,2,5,null,null,9,6,null,7]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Độ rộng lớn nhất nằm ở level thứ tư, có độ dài 7 (6,null,null,null,null,null,7).
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0662.Maximum%20Width%20of%20Binary%20Tree/images/width3-tree.jpg" style="width: 289px; height: 299px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,3,2,5]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Độ rộng lớn nhất nằm ở level thứ hai, có độ dài 2 (3,2).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 3000]</code>.</li>
	<li><code>-100 &lt;= Node.val &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Độ rộng là khoảng cách giữa vị trí ngoài cùng bên trái và bên phải trên một level, tính cả các vị trí trống. Chỉ lưu node sẽ không thể hiện được các vị trí trống này.
>
> Đánh số root là $1$, node trái là $i\ll 1$, node phải là $i\ll 1|1$. Khi BFS, độ rộng bằng chỉ số cuối trừ chỉ số đầu rồi cộng một; lấy giá trị lớn nhất.

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
    def widthOfBinaryTree(self, root: Optional[TreeNode]) -> int:
        ans = 0
        q = deque([(root, 1)])
        while q:
            ans = max(ans, q[-1][1] - q[0][1] + 1)
            for _ in range(len(q)):
                root, i = q.popleft()
                if root.left:
                    q.append((root.left, i << 1))
                if root.right:
                    q.append((root.right, i << 1 | 1))
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
    public int widthOfBinaryTree(TreeNode root) {
        Deque<Pair<TreeNode, Integer>> q = new ArrayDeque<>();
        q.offer(new Pair<>(root, 1));
        int ans = 0;
        while (!q.isEmpty()) {
            ans = Math.max(ans, q.peekLast().getValue() - q.peekFirst().getValue() + 1);
            for (int n = q.size(); n > 0; --n) {
                var p = q.pollFirst();
                root = p.getKey();
                int i = p.getValue();
                if (root.left != null) {
                    q.offer(new Pair<>(root.left, i << 1));
                }
                if (root.right != null) {
                    q.offer(new Pair<>(root.right, i << 1 | 1));
                }
            }
        }
        return ans;
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
    int widthOfBinaryTree(TreeNode* root) {
        queue<pair<TreeNode*, int>> q;
        q.push({root, 1});
        int ans = 0;
        while (!q.empty()) {
            ans = max(ans, q.back().second - q.front().second + 1);
            int i = q.front().second;
            for (int n = q.size(); n; --n) {
                auto p = q.front();
                q.pop();
                root = p.first;
                int j = p.second;
                if (root->left) q.push({root->left, (j << 1) - (i << 1)});
                if (root->right) q.push({root->right, (j << 1 | 1) - (i << 1)});
            }
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
func widthOfBinaryTree(root *TreeNode) int {
	q := []pair{{root, 1}}
	ans := 0
	for len(q) > 0 {
		ans = max(ans, q[len(q)-1].i-q[0].i+1)
		for n := len(q); n > 0; n-- {
			p := q[0]
			q = q[1:]
			root = p.node
			if root.Left != nil {
				q = append(q, pair{root.Left, p.i << 1})
			}
			if root.Right != nil {
				q = append(q, pair{root.Right, p.i<<1 | 1})
			}
		}
	}
	return ans
}

type pair struct {
	node *TreeNode
	i    int
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> BFS cần queue. DFS lưu chỉ số đầu tiên gặp ở mỗi depth và cập nhật độ rộng theo công thức $i - t[depth] + 1$, vẫn dùng cách đánh số như trên.

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
    def widthOfBinaryTree(self, root: Optional[TreeNode]) -> int:
        def dfs(root, depth, i):
            if root is None:
                return
            if len(t) == depth:
                t.append(i)
            else:
                nonlocal ans
                ans = max(ans, i - t[depth] + 1)
            dfs(root.left, depth + 1, i << 1)
            dfs(root.right, depth + 1, i << 1 | 1)

        ans = 1
        t = []
        dfs(root, 0, 1)
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
    private int ans = 1;
    private List<Integer> t = new ArrayList<>();

    public int widthOfBinaryTree(TreeNode root) {
        dfs(root, 0, 1);
        return ans;
    }

    private void dfs(TreeNode root, int depth, int i) {
        if (root == null) {
            return;
        }
        if (t.size() == depth) {
            t.add(i);
        } else {
            ans = Math.max(ans, i - t.get(depth) + 1);
        }
        dfs(root.left, depth + 1, i << 1);
        dfs(root.right, depth + 1, i << 1 | 1);
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
    vector<int> t;
    int ans = 1;
    using ull = unsigned long long;

    int widthOfBinaryTree(TreeNode* root) {
        dfs(root, 0, 1);
        return ans;
    }

    void dfs(TreeNode* root, int depth, ull i) {
        if (!root) return;
        if (t.size() == depth) {
            t.push_back(i);
        } else {
            ans = max(ans, (int) (i - t[depth] + 1));
        }
        dfs(root->left, depth + 1, i << 1);
        dfs(root->right, depth + 1, i << 1 | 1);
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
func widthOfBinaryTree(root *TreeNode) int {
	ans := 1
	t := []int{}
	var dfs func(root *TreeNode, depth, i int)
	dfs = func(root *TreeNode, depth, i int) {
		if root == nil {
			return
		}
		if len(t) == depth {
			t = append(t, i)
		} else {
			ans = max(ans, i-t[depth]+1)
		}
		dfs(root.Left, depth+1, i<<1)
		dfs(root.Right, depth+1, i<<1|1)
	}
	dfs(root, 0, 1)
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
