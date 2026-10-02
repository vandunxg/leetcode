---
comments: true
difficulty: Easy
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [993. Cousins in Binary Tree](https://leetcode.com/problems/cousins-in-binary-tree)

[中文文档](/solution/0900-0999/0993.Cousins%20in%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một binary tree có các giá trị duy nhất, cùng giá trị <code>x</code> và <code>y</code> của hai node khác nhau trong cây. Trả về <code>true</code> <em>nếu hai node có giá trị </em><code>x</code><em> và </em><code>y</code><em> là </em><strong>anh em họ</strong><em>, nếu không thì trả về </em><code>false</code>.</p>

<p>Hai node trong binary tree là <strong>anh em họ</strong> nếu chúng có cùng độ sâu nhưng khác node cha.</p>

<p>Lưu ý, root node của binary tree có độ sâu <code>0</code>, còn node con của mỗi node ở độ sâu <code>k</code> nằm tại độ sâu <code>k + 1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0993.Cousins%20in%20Binary%20Tree/images/q1248-01.png" style="width: 304px; height: 270px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,2,3,4], x = 4, y = 3
<strong>Đầu ra:</strong> false
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0993.Cousins%20in%20Binary%20Tree/images/q1248-02.png" style="width: 334px; height: 266px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,2,3,null,4,null,5], x = 5, y = 4
<strong>Đầu ra:</strong> true
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0993.Cousins%20in%20Binary%20Tree/images/q1248-03.png" style="width: 267px; height: 258px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,2,3,null,4], x = 2, y = 3
<strong>Đầu ra:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[2, 100]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 100</code></li>
	<li>Mỗi node có giá trị <strong>duy nhất</strong>.</li>
	<li><code>x != y</code></li>
	<li><code>x</code> và <code>y</code> đều có trong cây.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Hai anh em họ có cùng độ sâu nhưng khác cha. Duyệt cây theo từng tầng và lưu node cha; khi gặp $x$ hoặc $y$, ghi lại node cha và độ sâu, rồi so sánh sau khi duyệt.

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
    def isCousins(self, root: Optional[TreeNode], x: int, y: int) -> bool:
        q = deque([(root, None)])
        depth = 0
        p1 = p2 = None
        d1 = d2 = None
        while q:
            for _ in range(len(q)):
                node, parent = q.popleft()
                if node.val == x:
                    p1, d1 = parent, depth
                elif node.val == y:
                    p2, d2 = parent, depth
                if node.left:
                    q.append((node.left, node))
                if node.right:
                    q.append((node.right, node))
            depth += 1
        return p1 != p2 and d1 == d2
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
    public boolean isCousins(TreeNode root, int x, int y) {
        Deque<TreeNode[]> q = new ArrayDeque<>();
        q.offer(new TreeNode[] {root, null});
        int d1 = 0, d2 = 0;
        TreeNode p1 = null, p2 = null;
        for (int depth = 0; !q.isEmpty(); ++depth) {
            for (int n = q.size(); n > 0; --n) {
                TreeNode[] t = q.poll();
                TreeNode node = t[0], parent = t[1];
                if (node.val == x) {
                    d1 = depth;
                    p1 = parent;
                } else if (node.val == y) {
                    d2 = depth;
                    p2 = parent;
                }
                if (node.left != null) {
                    q.offer(new TreeNode[] {node.left, node});
                }
                if (node.right != null) {
                    q.offer(new TreeNode[] {node.right, node});
                }
            }
        }
        return p1 != p2 && d1 == d2;
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
    bool isCousins(TreeNode* root, int x, int y) {
        queue<pair<TreeNode*, TreeNode*>> q;
        q.push({root, nullptr});
        int d1 = 0, d2 = 0;
        TreeNode *p1 = nullptr, *p2 = nullptr;
        for (int depth = 0; q.size(); ++depth) {
            for (int n = q.size(); n; --n) {
                auto [node, parent] = q.front();
                q.pop();
                if (node->val == x) {
                    d1 = depth;
                    p1 = parent;
                } else if (node->val == y) {
                    d2 = depth;
                    p2 = parent;
                }
                if (node->left) {
                    q.push({node->left, node});
                }
                if (node->right) {
                    q.push({node->right, node});
                }
            }
        }
        return d1 == d2 && p1 != p2;
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
func isCousins(root *TreeNode, x int, y int) bool {
	type pair struct{ node, parent *TreeNode }
	var d1, d2 int
	var p1, p2 *TreeNode
	q := []pair{{root, nil}}
	for depth := 0; len(q) > 0; depth++ {
		for n := len(q); n > 0; n-- {
			node, parent := q[0].node, q[0].parent
			q = q[1:]
			if node.Val == x {
				d1, p1 = depth, parent
			} else if node.Val == y {
				d2, p2 = depth, parent
			}
			if node.Left != nil {
				q = append(q, pair{node.Left, node})
			}
			if node.Right != nil {
				q = append(q, pair{node.Right, node})
			}
		}
	}
	return d1 == d2 && p1 != p2
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

function isCousins(root: TreeNode | null, x: number, y: number): boolean {
    let [d1, d2] = [0, 0];
    let [p1, p2] = [null, null];
    const q: [TreeNode, TreeNode][] = [[root, null]];
    for (let depth = 0; q.length > 0; ++depth) {
        const t: [TreeNode, TreeNode][] = [];
        for (const [node, parent] of q) {
            if (node.val === x) {
                [d1, p1] = [depth, parent];
            } else if (node.val === y) {
                [d2, p2] = [depth, parent];
            }
            if (node.left) {
                t.push([node.left, node]);
            }
            if (node.right) {
                t.push([node.right, node]);
            }
        }
        q.splice(0, q.length, ...t);
    }
    return d1 === d2 && p1 !== p2;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> BFS duyệt cây lần lượt theo từng tầng. DFS cũng dùng được: truyền node cha và độ sâu, ghi lại thông tin cho $x$ và $y$, rồi kiểm tra điều kiện anh em họ.

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
    def isCousins(self, root: Optional[TreeNode], x: int, y: int) -> bool:
        def dfs(root, parent, depth):
            if root is None:
                return
            if root.val == x:
                st[0] = (parent, depth)
            elif root.val == y:
                st[1] = (parent, depth)
            dfs(root.left, root, depth + 1)
            dfs(root.right, root, depth + 1)

        st = [None, None]
        dfs(root, None, 0)
        return st[0][0] != st[1][0] and st[0][1] == st[1][1]
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
    private int x, y;
    private int d1, d2;
    private TreeNode p1, p2;

    public boolean isCousins(TreeNode root, int x, int y) {
        this.x = x;
        this.y = y;
        dfs(root, null, 0);
        return p1 != p2 && d1 == d2;
    }

    private void dfs(TreeNode root, TreeNode parent, int depth) {
        if (root == null) {
            return;
        }
        if (root.val == x) {
            d1 = depth;
            p1 = parent;
        } else if (root.val == y) {
            d2 = depth;
            p2 = parent;
        }
        dfs(root.left, root, depth + 1);
        dfs(root.right, root, depth + 1);
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
    bool isCousins(TreeNode* root, int x, int y) {
        int d1, d2;
        TreeNode* p1;
        TreeNode* p2;
        function<void(TreeNode*, TreeNode*, int)> dfs = [&](TreeNode* root, TreeNode* parent, int depth) {
            if (!root) {
                return;
            }
            if (root->val == x) {
                d1 = depth;
                p1 = parent;
            } else if (root->val == y) {
                d2 = depth;
                p2 = parent;
            }
            dfs(root->left, root, depth + 1);
            dfs(root->right, root, depth + 1);
        };
        dfs(root, nullptr, 0);
        return p1 != p2 && d1 == d2;
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
func isCousins(root *TreeNode, x int, y int) bool {
	var d1, d2 int
	var p1, p2 *TreeNode
	var dfs func(root, parent *TreeNode, depth int)
	dfs = func(root, parent *TreeNode, depth int) {
		if root == nil {
			return
		}
		if root.Val == x {
			d1, p1 = depth, parent
		} else if root.Val == y {
			d2, p2 = depth, parent
		}
		dfs(root.Left, root, depth+1)
		dfs(root.Right, root, depth+1)
	}
	dfs(root, nil, 0)
	return d1 == d2 && p1 != p2
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

function isCousins(root: TreeNode | null, x: number, y: number): boolean {
    let [d1, d2, p1, p2] = [0, 0, null, null];
    const dfs = (root: TreeNode | null, parent: TreeNode | null, depth: number) => {
        if (!root) {
            return;
        }
        if (root.val === x) {
            [d1, p1] = [depth, parent];
        } else if (root.val === y) {
            [d2, p2] = [depth, parent];
        }
        dfs(root.left, root, depth + 1);
        dfs(root.right, root, depth + 1);
    };
    dfs(root, null, 0);
    return d1 === d2 && p1 !== p2;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
