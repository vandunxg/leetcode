---
comments: true
difficulty: Medium
rating: 1676
source: Biweekly Contest 102 Q3
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Hash Table
    - Binary Tree
---

<!-- problem:start -->

# [2641. Cousins in Binary Tree II](https://leetcode.com/problems/cousins-in-binary-tree-ii)

[中文文档](/solution/2600-2699/2641.Cousins%20in%20Binary%20Tree%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một cây nhị phân, thay giá trị của mỗi node trong cây bằng <strong>tổng giá trị của tất cả các node cousin của nó</strong>.</p>

<p>Hai node trong một cây nhị phân là <strong>cousin</strong> nếu chúng có cùng độ sâu nhưng khác cha.</p>

<p>Trả về <em>cây có </em><code>root</code><em> đã được thay đổi</em>.</p>

<p><strong>Lưu ý</strong> rằng độ sâu của một node là số cạnh trên đường đi từ node gốc đến node đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2641.Cousins%20in%20Binary%20Tree%20II/images/example11.png" style="width: 571px; height: 151px;" />
<pre>
<strong>Đầu vào:</strong> root = [5,4,9,1,10,null,7]
<strong>Đầu ra:</strong> [0,0,0,7,7,null,11]
<strong>Giải thích:</strong> Hình minh họa ở trên cho thấy cây nhị phân ban đầu và cây nhị phân sau khi thay đổi giá trị của mỗi node.
- Node có giá trị 5 không có cousin nào nên tổng của nó là 0.
- Node có giá trị 4 không có cousin nào nên tổng của nó là 0.
- Node có giá trị 9 không có cousin nào nên tổng của nó là 0.
- Node có giá trị 1 có một cousin với giá trị 7 nên tổng của nó là 7.
- Node có giá trị 10 có một cousin với giá trị 7 nên tổng của nó là 7.
- Node có giá trị 7 có các cousin với giá trị 1 và 10 nên tổng của nó là 11.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2641.Cousins%20in%20Binary%20Tree%20II/images/diagram33.png" style="width: 481px; height: 91px;" />
<pre>
<strong>Đầu vào:</strong> root = [3,1,2]
<strong>Đầu ra:</strong> [0,0,0]
<strong>Giải thích:</strong> Hình minh họa ở trên cho thấy cây nhị phân ban đầu và cây nhị phân sau khi thay đổi giá trị của mỗi node.
- Node có giá trị 3 không có cousin nào nên tổng của nó là 0.
- Node có giá trị 1 không có cousin nào nên tổng của nó là 0.
- Node có giá trị 2 không có cousin nào nên tổng của nó là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 10<sup>5</sup>]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai lượt duyệt DFS

<!-- thinking:start -->

> **Tư duy**
>
> Giá trị mới của mỗi node là tổng các node ở cùng level, trừ chính nó và sibling của nó. Việc duyệt lại một level cho từng node là quá chậm khi $n \le 10^5$. Tổng của mỗi level chỉ phụ thuộc vào độ sâu, nên có thể tính một lần.
>
> DFS đầu tiên điền $s[depth]$; DFS thứ hai cập nhật tại node cha bằng tổng của level chứa các node con trừ đi hai sibling. Root không có cousin nên được gán bằng $0$.

<!-- thinking:end -->

Ta tạo một mảng $s$ để lưu tổng giá trị của các node ở mỗi level của cây nhị phân, trong đó $s[depth]$ biểu thị tổng giá trị của các node ở level thứ $depth$ (node gốc nằm ở level $0$).

Tiếp theo, ta thực hiện một lượt DFS để tính các giá trị trong mảng $s$. Sau đó, ta thực hiện thêm một lượt DFS để cập nhật giá trị của các node con. Giá trị của một node con bằng tổng giá trị của các node ở level của nó trừ đi giá trị của node con và các node sibling của nó.

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
    def replaceValueInTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        def dfs1(root: Optional[TreeNode], depth: int):
            if root is None:
                return
            if len(s) <= depth:
                s.append(0)
            s[depth] += root.val
            dfs1(root.left, depth + 1)
            dfs1(root.right, depth + 1)

        def dfs2(root: Optional[TreeNode], depth: int):
            sub = (root.left.val if root.left else 0) + (
                root.right.val if root.right else 0
            )
            depth += 1
            if root.left:
                root.left.val = s[depth] - sub
                dfs2(root.left, depth)
            if root.right:
                root.right.val = s[depth] - sub
                dfs2(root.right, depth)

        s = []
        dfs1(root, 0)
        root.val = 0
        dfs2(root, 0)
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
    private List<Integer> s = new ArrayList<>();

    public TreeNode replaceValueInTree(TreeNode root) {
        dfs1(root, 0);
        root.val = 0;
        dfs2(root, 0);
        return root;
    }

    private void dfs1(TreeNode root, int depth) {
        if (root == null) {
            return;
        }
        if (s.size() <= depth) {
            s.add(0);
        }
        s.set(depth, s.get(depth) + root.val);
        dfs1(root.left, depth + 1);
        dfs1(root.right, depth + 1);
    }

    private void dfs2(TreeNode root, int depth) {
        int l = root.left == null ? 0 : root.left.val;
        int r = root.right == null ? 0 : root.right.val;
        int sub = l + r;
        ++depth;
        if (root.left != null) {
            root.left.val = s.get(depth) - sub;
            dfs2(root.left, depth);
        }
        if (root.right != null) {
            root.right.val = s.get(depth) - sub;
            dfs2(root.right, depth);
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
    TreeNode* replaceValueInTree(TreeNode* root) {
        memset(s, 0, sizeof(s));
        dfs1(root, 0);
        root->val = 0;
        dfs2(root, 0);
        return root;
    }

private:
    int s[100010];
    void dfs1(TreeNode* root, int depth) {
        if (!root) {
            return;
        }
        s[depth] += root->val;
        dfs1(root->left, depth + 1);
        dfs1(root->right, depth + 1);
    };

    void dfs2(TreeNode* root, int depth) {
        int l = root->left ? root->left->val : 0;
        int r = root->right ? root->right->val : 0;
        int sub = l + r;
        ++depth;
        if (root->left) {
            root->left->val = s[depth] - sub;
            dfs2(root->left, depth);
        }
        if (root->right) {
            root->right->val = s[depth] - sub;
            dfs2(root->right, depth);
        }
    };
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
func replaceValueInTree(root *TreeNode) *TreeNode {
	s := []int{}
	var dfs1 func(*TreeNode, int)
	dfs1 = func(root *TreeNode, depth int) {
		if root == nil {
			return
		}
		if len(s) <= depth {
			s = append(s, 0)
		}
		s[depth] += root.Val
		dfs1(root.Left, depth+1)
		dfs1(root.Right, depth+1)
	}
	var dfs2 func(*TreeNode, int)
	dfs2 = func(root *TreeNode, depth int) {
		l, r := 0, 0
		if root.Left != nil {
			l = root.Left.Val
		}
		if root.Right != nil {
			r = root.Right.Val
		}
		sub := l + r
		depth++
		if root.Left != nil {
			root.Left.Val = s[depth] - sub
			dfs2(root.Left, depth)
		}
		if root.Right != nil {
			root.Right.Val = s[depth] - sub
			dfs2(root.Right, depth)
		}
	}
	dfs1(root, 0)
	root.Val = 0
	dfs2(root, 0)
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

function replaceValueInTree(root: TreeNode | null): TreeNode | null {
    const s: number[] = [];
    const dfs1 = (root: TreeNode | null, depth: number) => {
        if (!root) {
            return;
        }
        if (s.length <= depth) {
            s.push(0);
        }
        s[depth] += root.val;
        dfs1(root.left, depth + 1);
        dfs1(root.right, depth + 1);
    };
    const dfs2 = (root: TreeNode | null, depth: number) => {
        const sub = (root.left?.val || 0) + (root.right?.val || 0);
        ++depth;
        if (root.left) {
            root.left.val = s[depth] - sub;
            dfs2(root.left, depth);
        }
        if (root.right) {
            root.right.val = s[depth] - sub;
            dfs2(root.right, depth);
        }
    };
    dfs1(root, 0);
    root.val = 0;
    dfs2(root, 0);
    return root;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Breadth-First Search (BFS)

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 sử dụng hai lượt đệ quy và lưu tổng của mọi level. Với BFS, layer hiện tại đã nằm trong queue, nên ta có thể tính tổng của layer tiếp theo rồi trừ các cặp sibling tại mỗi node cha, nhờ đó bỏ được mảng bổ sung và lượt DFS thứ hai.

<!-- thinking:end -->

Trước tiên, ta cập nhật giá trị của node gốc thành $0$ và dùng một queue $q$ để lưu tất cả các node ở mỗi level, ban đầu đưa node gốc vào queue.

Sau đó, ta duyệt queue, tính tổng $s$ của giá trị tất cả các node con ở mỗi level, rồi tính tổng $sub$ của giá trị mỗi node con và các node sibling của nó, sau đó cập nhật giá trị của mỗi node con thành $s - sub$.

Sau khi kết thúc việc duyệt, ta trả về node gốc.

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
    def replaceValueInTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        root.val = 0
        q = [root]
        while q:
            t = []
            s = 0
            for node in q:
                if node.left:
                    t.append(node.left)
                    s += node.left.val
                if node.right:
                    t.append(node.right)
                    s += node.right.val
            for node in q:
                sub = (node.left.val if node.left else 0) + (
                    node.right.val if node.right else 0
                )
                if node.left:
                    node.left.val = s - sub
                if node.right:
                    node.right.val = s - sub
            q = t
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
    public TreeNode replaceValueInTree(TreeNode root) {
        root.val = 0;
        List<TreeNode> q = List.of(root);
        while (!q.isEmpty()) {
            List<TreeNode> t = new ArrayList<>();
            int s = 0;
            for (TreeNode node : q) {
                if (node.left != null) {
                    t.add(node.left);
                    s += node.left.val;
                }
                if (node.right != null) {
                    t.add(node.right);
                    s += node.right.val;
                }
            }
            for (TreeNode node : q) {
                int sub = (node.left == null ? 0 : node.left.val)
                    + (node.right == null ? 0 : node.right.val);
                if (node.left != null) {
                    node.left.val = s - sub;
                }
                if (node.right != null) {
                    node.right.val = s - sub;
                }
            }
            q = t;
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
    TreeNode* replaceValueInTree(TreeNode* root) {
        root->val = 0;
        vector<TreeNode*> q = {root};
        while (!q.empty()) {
            vector<TreeNode*> t;
            int s = 0;
            for (TreeNode* node : q) {
                if (node->left) {
                    t.emplace_back(node->left);
                    s += node->left->val;
                }
                if (node->right) {
                    t.emplace_back(node->right);
                    s += node->right->val;
                }
            }
            for (TreeNode* node : q) {
                int sub = (node->left ? node->left->val : 0) + (node->right ? node->right->val : 0);
                if (node->left) {
                    node->left->val = s - sub;
                }
                if (node->right) {
                    node->right->val = s - sub;
                }
            }
            q = move(t);
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
func replaceValueInTree(root *TreeNode) *TreeNode {
	root.Val = 0
	q := []*TreeNode{root}
	for len(q) > 0 {
		t := []*TreeNode{}
		s := 0
		for _, node := range q {
			if node.Left != nil {
				t = append(t, node.Left)
				s += node.Left.Val
			}
			if node.Right != nil {
				t = append(t, node.Right)
				s += node.Right.Val
			}
		}
		for _, node := range q {
			sub := 0
			if node.Left != nil {
				sub += node.Left.Val
			}
			if node.Right != nil {
				sub += node.Right.Val
			}
			if node.Left != nil {
				node.Left.Val = s - sub
			}
			if node.Right != nil {
				node.Right.Val = s - sub
			}
		}
		q = t
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

function replaceValueInTree(root: TreeNode | null): TreeNode | null {
    let q = [root];
    let [sum, nextSum] = [0, root.val];

    while (q.length) {
        const qNext: TreeNode[] = [];
        [sum, nextSum] = [nextSum, 0];

        for (const node of q) {
            const x = (node.left?.val ?? 0) + (node.right?.val ?? 0);
            node.val = sum - node.val;
            nextSum += x;

            if (node.left) {
                node.left.val = x;
                qNext.push(node.left);
            }

            if (node.right) {
                node.right.val = x;
                qNext.push(node.right);
            }
        }

        q = qNext;
    }

    return root;
}
```

#### JavaScript

```js
function replaceValueInTree(root) {
    let q = [root];
    let [sum, nextSum] = [0, root.val];

    while (q.length) {
        const qNext = [];
        [sum, nextSum] = [nextSum, 0];

        for (const node of q) {
            const x = (node.left?.val ?? 0) + (node.right?.val ?? 0);
            node.val = sum - node.val;
            nextSum += x;

            if (node.left) {
                node.left.val = x;
                qNext.push(node.left);
            }

            if (node.right) {
                node.right.val = x;
                qNext.push(node.right);
            }
        }

        q = qNext;
    }

    return root;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
