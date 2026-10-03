---
comments: true
difficulty: Medium
rating: 1431
source: Weekly Contest 311 Q3
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [2415. Reverse Odd Levels of Binary Tree](https://leetcode.com/problems/reverse-odd-levels-of-binary-tree)

[Tài liệu tiếng Trung](/solution/2400-2499/2415.Reverse%20Odd%20Levels%20of%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một cây nhị phân <strong>hoàn hảo</strong>, hãy đảo ngược giá trị của các node ở mỗi level <strong>lẻ</strong> trong cây.</p>

<ul>
	<li>Ví dụ, giả sử các giá trị của node ở level 3 là <code>[2,1,3,4,7,11,29,18]</code>, thì chúng sẽ trở thành <code>[18,29,11,7,4,3,1,2]</code>.</li>
</ul>

<p>Trả về <em>root của cây sau khi đảo ngược</em>.</p>

<p>Một cây nhị phân là <strong>hoàn hảo</strong> nếu mọi node cha đều có hai node con và tất cả node lá đều nằm trên cùng một level.</p>

<p><strong>Level</strong> của một node là số cạnh trên đường đi giữa node đó và node gốc.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2415.Reverse%20Odd%20Levels%20of%20Binary%20Tree/images/first_case1.png" style="width: 626px; height: 191px;" />
<pre>
<strong>Đầu vào:</strong> root = [2,3,5,8,13,21,34]
<strong>Đầu ra:</strong> [2,5,3,8,13,21,34]
<strong>Giải thích:</strong>
Cây chỉ có một level lẻ.
Các node ở level 1 lần lượt là 3, 5; sau khi đảo ngược, chúng trở thành 5, 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2415.Reverse%20Odd%20Levels%20of%20Binary%20Tree/images/second_case3.png" style="width: 591px; height: 111px;" />
<pre>
<strong>Đầu vào:</strong> root = [7,13,11]
<strong>Đầu ra:</strong> [7,11,13]
<strong>Giải thích:</strong>
Các node ở level 1 là 13, 11; sau khi đảo ngược, chúng trở thành 11, 13.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [0,1,2,0,0,0,0,1,1,1,1,2,2,2,2]
<strong>Đầu ra:</strong> [0,2,1,0,0,0,0,2,2,2,2,1,1,1,1]
<strong>Giải thích:</strong>
Các level lẻ có giá trị khác không.
Các node ở level 1 là 1, 2, sau khi đảo ngược sẽ là 2, 1.
Các node ở level 3 là 1, 1, 1, 1, 2, 2, 2, 2, sau khi đảo ngược sẽ là 2, 2, 2, 2, 1, 1, 1, 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 2<sup>14</sup>]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
	<li><code>root</code> là một cây nhị phân <strong>hoàn hảo</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Cây perfect có các node liên tiếp trên mỗi level, vì vậy đảo ngược một level lẻ chỉ là hoán đổi các giá trị từ hai đầu. Duyệt theo level đã tạo sẵn mỗi level thành một dãy; không cần di chuyển các con trỏ.
>
> Với BFS và một queue: ở level lẻ, hoán đổi giá trị $\textit{val}$ từ hai đầu queue, sau đó đưa các node con vào queue. Độ phức tạp thời gian là tuyến tính theo số node.

<!-- thinking:end -->

Ta có thể sử dụng phương pháp Breadth-First Search (BFS), dùng queue $q$ để lưu các node của từng level và một biến $i$ để ghi nhận level hiện tại. Nếu $i$ là số lẻ, ta đảo ngược các giá trị của các node ở level hiện tại.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số node trong cây nhị phân.

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
    def reverseOddLevels(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        q = deque([root])
        i = 0
        while q:
            if i & 1:
                l, r = 0, len(q) - 1
                while l < r:
                    q[l].val, q[r].val = q[r].val, q[l].val
                    l, r = l + 1, r - 1
            for _ in range(len(q)):
                node = q.popleft()
                if node.left:
                    q.append(node.left)
                    q.append(node.right)
            i += 1
        return root
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
    public TreeNode reverseOddLevels(TreeNode root) {
        Deque<TreeNode> q = new ArrayDeque<>();
        q.offer(root);
        for (int i = 0; !q.isEmpty(); ++i) {
            List<TreeNode> t = new ArrayList<>();
            for (int k = q.size(); k > 0; --k) {
                var node = q.poll();
                if (i % 2 == 1) {
                    t.add(node);
                }
                if (node.left != null) {
                    q.offer(node.left);
                    q.offer(node.right);
                }
            }
            for (int l = 0, r = t.size() - 1; l < r; ++l, --r) {
                var x = t.get(l).val;
                t.get(l).val = t.get(r).val;
                t.get(r).val = x;
            }
        }
        return root;
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
    TreeNode* reverseOddLevels(TreeNode* root) {
        queue<TreeNode*> q{{root}};
        for (int i = 0; q.size(); ++i) {
            vector<TreeNode*> t;
            for (int k = q.size(); k; --k) {
                TreeNode* node = q.front();
                q.pop();
                if (i & 1) {
                    t.push_back(node);
                }
                if (node->left) {
                    q.push(node->left);
                    q.push(node->right);
                }
            }
            for (int l = 0, r = t.size() - 1; l < r; ++l, --r) {
                swap(t[l]->val, t[r]->val);
            }
        }
        return root;
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
func reverseOddLevels(root *TreeNode) *TreeNode {
	q := []*TreeNode{root}
	for i := 0; len(q) > 0; i++ {
		t := []*TreeNode{}
		for k := len(q); k > 0; k-- {
			node := q[0]
			q = q[1:]
			if i%2 == 1 {
				t = append(t, node)
			}
			if node.Left != nil {
				q = append(q, node.Left)
				q = append(q, node.Right)
			}
		}
		for l, r := 0, len(t)-1; l < r; l, r = l+1, r-1 {
			t[l].Val, t[r].Val = t[r].Val, t[l].Val
		}
	}
	return root
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

function reverseOddLevels(root: TreeNode | null): TreeNode | null {
    const q: TreeNode[] = [root];
    for (let i = 0; q.length > 0; ++i) {
        if (i % 2) {
            for (let l = 0, r = q.length - 1; l < r; ++l, --r) {
                [q[l].val, q[r].val] = [q[r].val, q[l].val];
            }
        }
        const nq: TreeNode[] = [];
        for (const { left, right } of q) {
            if (left) {
                nq.push(left);
                nq.push(right);
            }
        }
        q.splice(0, q.length, ...nq);
    }
    return root;
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
    fn create_tree(vals: &Vec<Vec<i32>>, i: usize, j: usize) -> Option<Rc<RefCell<TreeNode>>> {
        if i == vals.len() {
            return None;
        }
        Some(Rc::new(RefCell::new(TreeNode {
            val: vals[i][j],
            left: Self::create_tree(vals, i + 1, j * 2),
            right: Self::create_tree(vals, i + 1, j * 2 + 1),
        })))
    }

    pub fn reverse_odd_levels(
        root: Option<Rc<RefCell<TreeNode>>>,
    ) -> Option<Rc<RefCell<TreeNode>>> {
        let mut queue = VecDeque::new();
        queue.push_back(root);
        let mut d = 0;
        let mut vals = Vec::new();
        while !queue.is_empty() {
            let mut val = Vec::new();
            for _ in 0..queue.len() {
                let mut node = queue.pop_front().unwrap();
                let mut node = node.as_mut().unwrap().borrow_mut();
                val.push(node.val);
                if node.left.is_some() {
                    queue.push_back(node.left.take());
                }
                if node.right.is_some() {
                    queue.push_back(node.right.take());
                }
            }
            if d % 2 == 1 {
                val.reverse();
            }
            vals.push(val);
            d += 1;
        }
        Self::create_tree(&vals, 0, 0)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
