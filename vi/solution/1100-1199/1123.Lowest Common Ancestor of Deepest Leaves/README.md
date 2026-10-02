---
comments: true
difficulty: Medium
rating: 1607
source: Weekly Contest 145 Q2
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Hash Table
    - Binary Tree
    - Lowest Common Ancestor
    - Tree DP
    - Binary Lifting
---

<!-- problem:start -->

# [1123. Lowest Common Ancestor of Deepest Leaves](https://leetcode.com/problems/lowest-common-ancestor-of-deepest-leaves)

[中文文档](/solution/1100-1199/1123.Lowest%20Common%20Ancestor%20of%20Deepest%20Leaves/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một cây nhị phân, hãy trả về <em>tổ tiên chung thấp nhất của các lá sâu nhất</em>.</p>

<p>Nhắc lại:</p>

<ul>
	<li>Node trong cây nhị phân là lá khi và chỉ khi nó không có node con.</li>
	<li>Độ sâu của root là <code>0</code>. Nếu độ sâu của một node là <code>d</code>, độ sâu của mỗi node con là <code>d + 1</code>.</li>
	<li>Tổ tiên chung thấp nhất của tập node <code>S</code> là node <code>A</code> có độ sâu lớn nhất sao cho mọi node trong <code>S</code> đều nằm trong cây con có root là <code>A</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1100-1199/1123.Lowest%20Common%20Ancestor%20of%20Deepest%20Leaves/images/sketch1.png" style="width: 600px; height: 510px;" />
<pre>
<strong>Input:</strong> root = [3,5,1,6,2,0,8,null,null,7,4]
<strong>Output:</strong> [2,7,4]
<strong>Giải thích:</strong> Ta trả về node có giá trị 2, được tô màu vàng trong hình.
Các node được tô màu xanh dương là những lá sâu nhất của cây.
Lưu ý, các node 6, 0 và 8 cũng là lá, nhưng độ sâu của chúng là 2, trong khi độ sâu của các node 7 và 4 là 3.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> root = [1]
<strong>Output:</strong> [1]
<strong>Giải thích:</strong> Root là node sâu nhất trong cây, và chính nó là LCA của tập chỉ gồm nó.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> root = [0,1,3,null,2]
<strong>Output:</strong> [2]
<strong>Giải thích:</strong> Lá sâu nhất trong cây là node 2; LCA của một node chính là node đó.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 1000]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 1000</code></li>
	<li>Giá trị của các node trong cây đều <strong>khác nhau</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Lưu ý:</strong> Bài này giống bài 865: <a href="https://leetcode.com/problems/smallest-subtree-with-all-the-deepest-nodes/" target="_blank">https://leetcode.com/problems/smallest-subtree-with-all-the-deepest-nodes/</a></p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> LCA của các lá sâu nhất phụ thuộc vào độ sâu của hai cây con: đáp án thuộc nhánh sâu hơn, hoặc là node hiện tại nếu hai độ sâu bằng nhau. Một lượt DFS hậu tự trả về cả LCA lẫn độ sâu, nên không cần thu thập các lá sâu nhất rồi chạy thêm một lượt tìm LCA.

<!-- thinking:end -->

Ta thiết kế hàm `dfs(root)` trả về bộ `(l, d)`, trong đó `l` là tổ tiên chung thấp nhất của các lá sâu nhất trong cây con có root là `root`, còn `d` là độ sâu của `root`. Hàm `dfs(root)` hoạt động như sau:

- Nếu `root` là null, trả về bộ `(None, 0)`;
- Nếu không, gọi đệ quy `dfs(root.left)` và `dfs(root.right)`, nhận được các bộ `(l, d1)` và `(r, d2)`. Nếu `d1 > d2`, tổ tiên chung thấp nhất của `root` là `l` và độ sâu là `d1 + 1`; nếu `d1 < d2`, tổ tiên chung thấp nhất là `r` và độ sâu là `d2 + 1`; nếu `d1 = d2`, tổ tiên chung thấp nhất là `root` và độ sâu là `d1 + 1`.

Trong hàm chính, gọi `dfs(root)` rồi trả về phần tử đầu tiên của kết quả để lấy node tổ tiên chung thấp nhất.

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
    def lcaDeepestLeaves(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        def dfs(root):
            if root is None:
                return None, 0
            l, d1 = dfs(root.left)
            r, d2 = dfs(root.right)
            if d1 > d2:
                return l, d1 + 1
            if d1 < d2:
                return r, d2 + 1
            return root, d1 + 1

        return dfs(root)[0]
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
    public TreeNode lcaDeepestLeaves(TreeNode root) {
        return dfs(root).getKey();
    }

    private Pair<TreeNode, Integer> dfs(TreeNode root) {
        if (root == null) {
            return new Pair<>(null, 0);
        }
        Pair<TreeNode, Integer> l = dfs(root.left);
        Pair<TreeNode, Integer> r = dfs(root.right);
        int d1 = l.getValue(), d2 = r.getValue();
        if (d1 > d2) {
            return new Pair<>(l.getKey(), d1 + 1);
        }
        if (d1 < d2) {
            return new Pair<>(r.getKey(), d2 + 1);
        }
        return new Pair<>(root, d1 + 1);
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
    TreeNode* lcaDeepestLeaves(TreeNode* root) {
        auto dfs = [&](this auto&& dfs, TreeNode* root) -> pair<TreeNode*, int> {
            if (!root) {
                return {nullptr, 0};
            }
            auto [l, d1] = dfs(root->left);
            auto [r, d2] = dfs(root->right);
            if (d1 > d2) {
                return {l, d1 + 1};
            }
            if (d1 < d2) {
                return {r, d2 + 1};
            }
            return {root, d1 + 1};
        };
        return dfs(root).first;
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
type pair struct {
	first  *TreeNode
	second int
}

func lcaDeepestLeaves(root *TreeNode) *TreeNode {
	var dfs func(root *TreeNode) pair
	dfs = func(root *TreeNode) pair {
		if root == nil {
			return pair{nil, 0}
		}
		l, r := dfs(root.Left), dfs(root.Right)
		d1, d2 := l.second, r.second
		if d1 > d2 {
			return pair{l.first, d1 + 1}
		}
		if d1 < d2 {
			return pair{r.first, d2 + 1}
		}
		return pair{root, d1 + 1}
	}
	return dfs(root).first
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

function lcaDeepestLeaves(root: TreeNode | null): TreeNode | null {
    const dfs = (root: TreeNode | null): [TreeNode | null, number] => {
        if (root === null) {
            return [null, 0];
        }
        const [l, d1] = dfs(root.left);
        const [r, d2] = dfs(root.right);
        if (d1 > d2) {
            return [l, d1 + 1];
        }
        if (d1 < d2) {
            return [r, d2 + 1];
        }
        return [root, d1 + 1];
    };
    return dfs(root)[0];
}
```

#### C#

```cs
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left;
 *     public TreeNode right;
 *     public TreeNode(int val = 0, TreeNode left = null, TreeNode right = null) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
public class Solution {
    public TreeNode LcaDeepestLeaves(TreeNode root) {
        (TreeNode, int) Dfs(TreeNode root) {
            if (root == null) {
                return (null, 0);
            }

            var l = Dfs(root.left);
            var r = Dfs(root.right);
            int d1 = l.Item2;
            int d2 = r.Item2;

            if (d1 > d2) {
                return (l.Item1, d1 + 1);
            }
            if (d1 < d2) {
                return (r.Item1, d2 + 1);
            }
            return (root, d1 + 1);
        }

        return Dfs(root).Item1;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
