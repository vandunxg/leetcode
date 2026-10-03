---
comments: true
difficulty: Medium
rating: 1643
source: Weekly Contest 283 Q3
tags:
    - Tree
    - Array
    - Hash Table
    - Binary Tree
---

<!-- problem:start -->

# [2196. Create Binary Tree From Descriptions](https://leetcode.com/problems/create-binary-tree-from-descriptions)

[中文文档](/solution/2100-2199/2196.Create%20Binary%20Tree%20From%20Descriptions/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên 2 chiều <code>descriptions</code>, trong đó <code>descriptions[i] = [parent<sub>i</sub>, child<sub>i</sub>, isLeft<sub>i</sub>]</code> cho biết <code>parent<sub>i</sub></code> là <strong>cha</strong> của <code>child<sub>i</sub></code> trong một cây <strong>nhị phân</strong> có các giá trị <strong>khác nhau</strong>. Ngoài ra,</p>

<ul>
	<li>Nếu <code>isLeft<sub>i</sub> == 1</code>, thì <code>child<sub>i</sub></code> là node con bên trái của <code>parent<sub>i</sub></code>.</li>
	<li>Nếu <code>isLeft<sub>i</sub> == 0</code>, thì <code>child<sub>i</sub></code> là node con bên phải của <code>parent<sub>i</sub></code>.</li>
</ul>

<p>Hãy xây dựng cây nhị phân được mô tả bởi <code>descriptions</code> và trả về <em><strong>root</strong> của cây</em>.</p>

<p>Các test case được tạo sao cho cây nhị phân luôn <strong>hợp lệ</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2196.Create%20Binary%20Tree%20From%20Descriptions/images/example1drawio.png" style="width: 300px; height: 236px;" />
<pre>
<strong>Đầu vào:</strong> descriptions = [[20,15,1],[20,17,0],[50,20,1],[50,80,0],[80,19,1]]
<strong>Đầu ra:</strong> [50,20,80,15,17,19]
<strong>Giải thích:</strong> Node root là node có giá trị 50 vì nó không có node cha.
Cây nhị phân thu được được minh họa trong hình.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2196.Create%20Binary%20Tree%20From%20Descriptions/images/example2drawio.png" style="width: 131px; height: 300px;" />
<pre>
<strong>Đầu vào:</strong> descriptions = [[1,2,1],[2,3,0],[3,4,1]]
<strong>Đầu ra:</strong> [1,2,null,null,3,4]
<strong>Giải thích:</strong> Node root là node có giá trị 1 vì nó không có node cha.
Cây nhị phân thu được được minh họa trong hình.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= descriptions.length &lt;= 10<sup>4</sup></code></li>
	<li><code>descriptions[i].length == 3</code></li>
	<li><code>1 &lt;= parent<sub>i</sub>, child<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= isLeft<sub>i</sub> &lt;= 1</code></li>
	<li>Cây nhị phân được mô tả bởi <code>descriptions</code> là hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bộ ba là một cạnh cha-con kèm cờ trái/phải. Các giá trị là duy nhất; node cha có thể xuất hiện sau node con, nên ta tạo node khi cần. Root là giá trị duy nhất không bao giờ xuất hiện dưới dạng node con.
>
> Một map lưu các node theo giá trị, còn một set ghi nhận các node con. Ta nối các cạnh trong một lần duyệt; root là tập key của map sau khi loại bỏ tập node con.
>
> Độ phức tạp thời gian và bộ nhớ là tuyến tính theo số lượng descriptions.

<!-- thinking:end -->

Ta có thể dùng một hash table $\textit{nodes}$ để lưu tất cả các node, trong đó key là giá trị của node và value là chính node đó. Ngoài ra, ta dùng một set $\textit{children}$ để lưu tất cả các node con.

Ta duyệt qua $\textit{descriptions}$, với mỗi description $[\textit{parent}, \textit{child}, \textit{isLeft}]$, nếu $\textit{parent}$ chưa có trong $\textit{nodes}$, ta thêm $\textit{parent}$ vào $\textit{nodes}$ và khởi tạo một node có giá trị $\textit{parent}$. Nếu $\textit{child}$ chưa có trong $\textit{nodes}$, ta thêm $\textit{child}$ vào $\textit{nodes}$ và khởi tạo một node có giá trị $\textit{child}$. Sau đó, ta thêm $\textit{child}$ vào $\textit{children}$.

Nếu $\textit{isLeft}$ là true, ta đặt $\textit{child}$ làm node con bên trái của $\textit{parent}$; ngược lại, ta đặt $\textit{child}$ làm node con bên phải của $\textit{parent}$.

Cuối cùng, ta duyệt qua $\textit{nodes}$, nếu giá trị của một node không nằm trong $\textit{children}$ thì node đó là root và ta trả về node này.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của $\textit{descriptions}$.

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
    def createBinaryTree(self, descriptions: List[List[int]]) -> Optional[TreeNode]:
        nodes = defaultdict(TreeNode)
        children = set()
        for parent, child, isLeft in descriptions:
            if parent not in nodes:
                nodes[parent] = TreeNode(parent)
            if child not in nodes:
                nodes[child] = TreeNode(child)
            children.add(child)
            if isLeft:
                nodes[parent].left = nodes[child]
            else:
                nodes[parent].right = nodes[child]
        root = (set(nodes.keys()) - children).pop()
        return nodes[root]
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
    public TreeNode createBinaryTree(int[][] descriptions) {
        Map<Integer, TreeNode> nodes = new HashMap<>();
        Set<Integer> children = new HashSet<>();
        for (var d : descriptions) {
            int parent = d[0], child = d[1], isLeft = d[2];
            if (!nodes.containsKey(parent)) {
                nodes.put(parent, new TreeNode(parent));
            }
            if (!nodes.containsKey(child)) {
                nodes.put(child, new TreeNode(child));
            }
            if (isLeft == 1) {
                nodes.get(parent).left = nodes.get(child);
            } else {
                nodes.get(parent).right = nodes.get(child);
            }
            children.add(child);
        }
        for (var e : nodes.entrySet()) {
            if (!children.contains(e.getKey())) {
                return e.getValue();
            }
        }
        return null;
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
    TreeNode* createBinaryTree(vector<vector<int>>& descriptions) {
        unordered_map<int, TreeNode*> nodes;
        unordered_set<int> children;
        for (const auto& d : descriptions) {
            int parent = d[0], child = d[1], isLeft = d[2];
            if (!nodes.contains(parent)) {
                nodes[parent] = new TreeNode(parent);
            }
            if (!nodes.contains(child)) {
                nodes[child] = new TreeNode(child);
            }
            if (isLeft) {
                nodes[parent]->left = nodes[child];
            } else {
                nodes[parent]->right = nodes[child];
            }
            children.insert(child);
        }
        for (const auto& [k, v] : nodes) {
            if (!children.contains(k)) {
                return v;
            }
        }
        return nullptr;
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
func createBinaryTree(descriptions [][]int) *TreeNode {
	nodes := map[int]*TreeNode{}
	children := map[int]bool{}
	for _, d := range descriptions {
		parent, child, isLeft := d[0], d[1], d[2]
		if _, ok := nodes[parent]; !ok {
			nodes[parent] = &TreeNode{Val: parent}
		}
		if _, ok := nodes[child]; !ok {
			nodes[child] = &TreeNode{Val: child}
		}
		if isLeft == 1 {
			nodes[parent].Left = nodes[child]
		} else {
			nodes[parent].Right = nodes[child]
		}
		children[child] = true
	}
	for k, v := range nodes {
		if _, ok := children[k]; !ok {
			return v
		}
	}
	return nil
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

function createBinaryTree(descriptions: number[][]): TreeNode | null {
    const nodes: Record<number, TreeNode> = {};
    const children = new Set<number>();
    for (const [parent, child, isLeft] of descriptions) {
        if (!nodes[parent]) {
            nodes[parent] = new TreeNode(parent);
        }
        if (!nodes[child]) {
            nodes[child] = new TreeNode(child);
        }
        if (isLeft) {
            nodes[parent].left = nodes[child];
        } else {
            nodes[parent].right = nodes[child];
        }
        children.add(child);
    }
    for (const [k, v] of Object.entries(nodes)) {
        if (!children.has(+k)) {
            return v;
        }
    }
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
use std::collections::{HashMap, HashSet};
use std::rc::Rc;
impl Solution {
    pub fn create_binary_tree(descriptions: Vec<Vec<i32>>) -> Option<Rc<RefCell<TreeNode>>> {
        let mut nodes = HashMap::new();
        let mut children = HashSet::new();

        for d in descriptions {
            let parent = d[0];
            let child = d[1];
            let is_left = d[2];

            nodes
                .entry(parent)
                .or_insert_with(|| Rc::new(RefCell::new(TreeNode::new(parent))));
            nodes
                .entry(child)
                .or_insert_with(|| Rc::new(RefCell::new(TreeNode::new(child))));

            if is_left == 1 {
                nodes.get(&parent).unwrap().borrow_mut().left =
                    Some(Rc::clone(nodes.get(&child).unwrap()));
            } else {
                nodes.get(&parent).unwrap().borrow_mut().right =
                    Some(Rc::clone(nodes.get(&child).unwrap()));
            }

            children.insert(child);
        }

        for (key, node) in &nodes {
            if !children.contains(key) {
                return Some(Rc::clone(node));
            }
        }

        None
    }
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
 * @param {number[][]} descriptions
 * @return {TreeNode}
 */
var createBinaryTree = function (descriptions) {
    const nodes = {};
    const children = new Set();
    for (const [parent, child, isLeft] of descriptions) {
        if (!nodes[parent]) {
            nodes[parent] = new TreeNode(parent);
        }
        if (!nodes[child]) {
            nodes[child] = new TreeNode(child);
        }
        if (isLeft) {
            nodes[parent].left = nodes[child];
        } else {
            nodes[parent].right = nodes[child];
        }
        children.add(child);
    }
    for (const [k, v] of Object.entries(nodes)) {
        if (!children.has(+k)) {
            return v;
        }
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
