---
comments: true
difficulty: Medium
rating: 1249
source: Weekly Contest 150 Q2
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [1161. Maximum Level Sum of a Binary Tree](https://leetcode.com/problems/maximum-level-sum-of-a-binary-tree)

[中文文档](/solution/1100-1199/1161.Maximum%20Level%20Sum%20of%20a%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của cây nhị phân. Level của root là <code>1</code>, level của các node con là <code>2</code>, và cứ tiếp tục như vậy.</p>

<p>Hãy trả về level <strong>nhỏ nhất</strong> <code>x</code> sao cho tổng giá trị các node ở level <code>x</code> là <strong>lớn nhất</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1100-1199/1161.Maximum%20Level%20Sum%20of%20a%20Binary%20Tree/images/capture.jpg" style="width: 200px; height: 175px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,7,0,7,-8,null,null]
<strong>Đầu ra:</strong> 2
<strong>Giải thích: </strong>
Tổng ở level 1 = 1.
Tổng ở level 2 = 7 + 0 = 7.
Tổng ở level 3 = 7 + -8 = -1.
Vì vậy, ta trả về level có tổng lớn nhất, tức level 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [989,null,10250,98693,-89388,null,null,null,-32127]
<strong>Đầu ra:</strong> 2
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 10<sup>4</sup>]</code>.</li>
	<li><code>-10<sup>5</sup> &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ số level và tổng của level phù hợp với cách duyệt theo thứ tự từng level. BFS tính tổng các node trong queue hiện tại, cập nhật level khi tổng tăng nghiêm ngặt; nếu hòa thì giữ nguyên để chọn level nhỏ hơn. Mỗi node chỉ được duyệt một lần.

<!-- thinking:end -->

Ta dùng BFS để duyệt từng level, tính tổng giá trị các node ở mỗi level và tìm level có tổng lớn nhất. Nếu nhiều level cùng có tổng lớn nhất, trả về level có số thứ tự nhỏ nhất.

Cụ thể, ta dùng queue $q$ để lưu các node ở level hiện tại. Trong mỗi lượt duyệt, tính tổng node của level hiện tại là $s$, sau đó đưa tất cả node con vào queue để chuẩn bị xử lý level tiếp theo. Biến $mx$ lưu tổng lớn nhất hiện tại, còn $ans$ lưu số thứ tự level tương ứng. Sau khi tính tổng từng level, nếu $s > mx$, ta cập nhật $mx$ và $ans$. Cuối cùng, trả về $ans$.

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
    def maxLevelSum(self, root: Optional[TreeNode]) -> int:
        q = deque([root])
        mx = -inf
        i = 0
        while q:
            i += 1
            s = 0
            for _ in range(len(q)):
                node = q.popleft()
                s += node.val
                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)
            if mx < s:
                mx = s
                ans = i
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
    public int maxLevelSum(TreeNode root) {
        Deque<TreeNode> q = new ArrayDeque<>();
        q.offer(root);
        int mx = Integer.MIN_VALUE;
        int i = 0;
        int ans = 0;
        while (!q.isEmpty()) {
            ++i;
            int s = 0;
            for (int n = q.size(); n > 0; --n) {
                TreeNode node = q.pollFirst();
                s += node.val;
                if (node.left != null) {
                    q.offer(node.left);
                }
                if (node.right != null) {
                    q.offer(node.right);
                }
            }
            if (mx < s) {
                mx = s;
                ans = i;
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
    int maxLevelSum(TreeNode* root) {
        queue<TreeNode*> q{{root}};
        int mx = INT_MIN;
        int ans = 0;
        int i = 0;
        while (!q.empty()) {
            ++i;
            int s = 0;
            for (int n = q.size(); n; --n) {
                root = q.front();
                q.pop();
                s += root->val;
                if (root->left) {
                    q.push(root->left);
                }
                if (root->right) {
                    q.push(root->right);
                }
            }
            if (mx < s) {
                mx = s;
                ans = i;
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
func maxLevelSum(root *TreeNode) int {
	q := []*TreeNode{root}
	mx := -0x3f3f3f3f
	i := 0
	ans := 0
	for len(q) > 0 {
		i++
		s := 0
		for n := len(q); n > 0; n-- {
			root = q[0]
			q = q[1:]
			s += root.Val
			if root.Left != nil {
				q = append(q, root.Left)
			}
			if root.Right != nil {
				q = append(q, root.Right)
			}
		}
		if mx < s {
			mx = s
			ans = i
		}
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

function maxLevelSum(root: TreeNode | null): number {
    let q = [root];
    let i = 0;
    let mx = -Infinity;
    let ans = 0;
    while (q.length) {
        ++i;
        const nq = [];
        let s = 0;
        for (const { val, left, right } of q) {
            s += val;
            left && nq.push(left);
            right && nq.push(right);
        }
        if (mx < s) {
            mx = s;
            ans = i;
        }
        q = nq;
    }
    return ans;
}
```

#### Rust

```rust
// Definition for a binary tree node.
// #[derive(Debug, PartialEq, Eq)]
// pub struct TreeNode {
//   pub val: i32,
//   pub left: Option<Rc<RefCell<TreeNode>>>,
//   pub right: Option<Rc<RefCell<TreeNode>>>,
// }
//
// impl TreeNode {
//   #[inline]
//   pub fn new(val: i32) -> Self {
//     TreeNode {
//       val,
//       left: None,
//       right: None
//     }
//   }
// }
use std::cell::RefCell;
use std::collections::VecDeque;
use std::rc::Rc;
impl Solution {
    pub fn max_level_sum(root: Option<Rc<RefCell<TreeNode>>>) -> i32 {
        let mut q = VecDeque::new();
        if let Some(r) = root {
            q.push_back(r);
        }

        let mut i = 0;
        let mut mx = i32::MIN;
        let mut ans = 0;

        while !q.is_empty() {
            i += 1;
            let mut s = 0;
            let sz = q.len();

            for _ in 0..sz {
                let node = q.pop_front().unwrap();
                let node = node.borrow();

                s += node.val;

                if let Some(left) = node.left.clone() {
                    q.push_back(left);
                }
                if let Some(right) = node.right.clone() {
                    q.push_back(right);
                }
            }

            if s > mx {
                mx = s;
                ans = i;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Cách 1 lưu toàn bộ một level trong queue. DFS cộng giá trị mỗi node vào $s[i]$ theo độ sâu; stack sử dụng bộ nhớ theo chiều cao cây. Chỉ số nhỏ nhất có giá trị lớn nhất trong $s$ chính là đáp án.

<!-- thinking:end -->

Ta cũng có thể giải bài này bằng DFS. Dùng mảng $s$ để lưu tổng giá trị node ở mỗi level: chỉ số mảng biểu thị level, còn giá trị tại đó là tổng các node. DFS duyệt cây nhị phân và cộng giá trị mỗi node vào tổng của level tương ứng. Cuối cùng, trả về chỉ số ứng với giá trị lớn nhất trong $s$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số node trong cây nhị phân.

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
    def maxLevelSum(self, root: Optional[TreeNode]) -> int:
        def dfs(node, i):
            if node is None:
                return
            if i == len(s):
                s.append(node.val)
            else:
                s[i] += node.val
            dfs(node.left, i + 1)
            dfs(node.right, i + 1)

        s = []
        dfs(root, 0)
        return s.index(max(s)) + 1
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
    private List<Integer> s = new ArrayList<>();

    public int maxLevelSum(TreeNode root) {
        dfs(root, 0);
        int mx = Integer.MIN_VALUE;
        int ans = 0;
        for (int i = 0; i < s.size(); ++i) {
            if (mx < s.get(i)) {
                mx = s.get(i);
                ans = i + 1;
            }
        }
        return ans;
    }

    private void dfs(TreeNode root, int i) {
        if (root == null) {
            return;
        }
        if (i == s.size()) {
            s.add(root.val);
        } else {
            s.set(i, s.get(i) + root.val);
        }
        dfs(root.left, i + 1);
        dfs(root.right, i + 1);
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
    int maxLevelSum(TreeNode* root) {
        vector<int> s;
        dfs(root, 0, s);
        int mx = INT_MIN;
        int ans = 0;
        for (int i = 0; i < s.size(); ++i)
            if (mx < s[i]) mx = s[i], ans = i + 1;
        return ans;
    }

    void dfs(TreeNode* root, int i, vector<int>& s) {
        if (!root) return;
        if (s.size() == i)
            s.push_back(root->val);
        else
            s[i] += root->val;
        dfs(root->left, i + 1, s);
        dfs(root->right, i + 1, s);
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
func maxLevelSum(root *TreeNode) int {
	s := []int{}
	var dfs func(*TreeNode, int)
	dfs = func(root *TreeNode, i int) {
		if root == nil {
			return
		}
		if len(s) == i {
			s = append(s, root.Val)
		} else {
			s[i] += root.Val
		}
		dfs(root.Left, i+1)
		dfs(root.Right, i+1)
	}
	dfs(root, 0)
	ans, mx := 0, -0x3f3f3f3f
	for i, v := range s {
		if mx < v {
			mx = v
			ans = i + 1
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
