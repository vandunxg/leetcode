---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Hash Table
    - Binary Tree
---

<!-- problem:start -->

# [863. All Nodes Distance K in Binary Tree](https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree)

[中文文档](/solution/0800-0899/0863.All%20Nodes%20Distance%20K%20in%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của cây nhị phân, giá trị của node đích <code>target</code> và số nguyên <code>k</code>. Hãy trả về <em>mảng chứa giá trị của mọi node cách node đích một khoảng cách </em><code>k</code>.</p>

<p>Có thể trả đáp án theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0863.All%20Nodes%20Distance%20K%20in%20Binary%20Tree/images/sketch0.png" style="width: 500px; height: 429px;" />
<pre>
<strong>Đầu vào:</strong> root = [3,5,1,6,2,0,8,null,null,7,4], target = 5, k = 2
<strong>Đầu ra:</strong> [7,4,1]
Giải thích: Các node cách node đích (có giá trị 5) một khoảng cách 2 có giá trị lần lượt là 7, 4 và 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [1], target = 1, k = 3
<strong>Đầu ra:</strong> []
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 500]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 500</code></li>
	<li>Tất cả giá trị <code>Node.val</code> đều <strong>khác nhau</strong>.</li>
	<li><code>target</code> là giá trị của một node trong cây.</li>
	<li><code>0 &lt;= k &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS + hash table

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm các node cách $\textit{target}$ một khoảng cách $k$. Chỉ có con trỏ đến node con thì không thể đi lên node cha. Cây có tối đa $500$ node, nên ta lưu node cha trước rồi duyệt $k$ bước từ $\textit{target}$.
>
> Lượt DFS đầu tiên lưu node cha của từng node; lượt thứ hai duyệt sang trái, sang phải và lên cha, bỏ qua node vừa đi qua, rồi thu thập giá trị khi khoảng cách còn lại bằng $0$.

<!-- thinking:end -->

Trước tiên, dùng DFS duyệt toàn bộ cây và lưu node cha của mỗi node vào hash table $\textit{g}$.

Tiếp theo, chạy DFS lần nữa từ $\textit{target}$ để tìm các node cách nó $k$ bước theo cả hướng lên trên lẫn xuống dưới, rồi thêm chúng vào mảng kết quả.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node trong cây nhị phân.

<!-- tabs:start -->

#### Python3

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None


class Solution:
    def distanceK(self, root: TreeNode, target: TreeNode, k: int) -> List[int]:
        def dfs(root, fa):
            if root is None:
                return
            g[root] = fa
            dfs(root.left, root)
            dfs(root.right, root)

        def dfs2(root, fa, k):
            if root is None:
                return
            if k == 0:
                ans.append(root.val)
                return
            for nxt in (root.left, root.right, g[root]):
                if nxt != fa:
                    dfs2(nxt, root, k - 1)

        g = {}
        dfs(root, None)
        ans = []
        dfs2(target, None, k)
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
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    private Map<TreeNode, TreeNode> g = new HashMap<>();
    private List<Integer> ans = new ArrayList<>();

    public List<Integer> distanceK(TreeNode root, TreeNode target, int k) {
        dfs(root, null);
        dfs2(target, null, k);
        return ans;
    }

    private void dfs(TreeNode root, TreeNode fa) {
        if (root == null) {
            return;
        }
        g.put(root, fa);
        dfs(root.left, root);
        dfs(root.right, root);
    }

    private void dfs2(TreeNode root, TreeNode fa, int k) {
        if (root == null) {
            return;
        }
        if (k == 0) {
            ans.add(root.val);
            return;
        }
        for (TreeNode nxt : new TreeNode[] {root.left, root.right, g.get(root)}) {
            if (nxt != fa) {
                dfs2(nxt, root, k - 1);
            }
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
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> distanceK(TreeNode* root, TreeNode* target, int k) {
        unordered_map<TreeNode*, TreeNode*> g;
        vector<int> ans;

        auto dfs = [&](this auto&& dfs, TreeNode* node, TreeNode* fa) {
            if (!node) return;
            g[node] = fa;
            dfs(node->left, node);
            dfs(node->right, node);
        };

        auto dfs2 = [&](this auto&& dfs2, TreeNode* node, TreeNode* fa, int k) {
            if (!node) return;
            if (k == 0) {
                ans.push_back(node->val);
                return;
            }
            for (auto&& nxt : {node->left, node->right, g[node]}) {
                if (nxt != fa) {
                    dfs2(nxt, node, k - 1);
                }
            }
        };

        dfs(root, nullptr);
        dfs2(target, nullptr, k);
        return ans;
    }
};
```

#### Go

```go
func distanceK(root *TreeNode, target *TreeNode, k int) []int {
	g := make(map[*TreeNode]*TreeNode)
	ans := []int{}

	var dfs func(node, fa *TreeNode)
	dfs = func(node, fa *TreeNode) {
		if node == nil {
			return
		}
		g[node] = fa
		dfs(node.Left, node)
		dfs(node.Right, node)
	}

	var dfs2 func(node, fa *TreeNode, k int)
	dfs2 = func(node, fa *TreeNode, k int) {
		if node == nil {
			return
		}
		if k == 0 {
			ans = append(ans, node.Val)
			return
		}
		for _, nxt := range []*TreeNode{node.Left, node.Right, g[node]} {
			if nxt != fa {
				dfs2(nxt, node, k-1)
			}
		}
	}

	dfs(root, nil)
	dfs2(target, nil, k)

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

function distanceK(root: TreeNode | null, target: TreeNode | null, k: number): number[] {
    const g = new Map<TreeNode, TreeNode | null>();
    const ans: number[] = [];

    const dfs = (node: TreeNode | null, fa: TreeNode | null) => {
        if (!node) {
            return;
        }
        g.set(node, fa);
        dfs(node.left, node);
        dfs(node.right, node);
    };

    const dfs2 = (node: TreeNode | null, fa: TreeNode | null, k: number) => {
        if (!node) {
            return;
        }
        if (k === 0) {
            ans.push(node.val);
            return;
        }
        for (const nxt of [node.left, node.right, g.get(node) || null]) {
            if (nxt !== fa) {
                dfs2(nxt, node, k - 1);
            }
        }
    };

    dfs(root, null);
    dfs2(target, null, k);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
