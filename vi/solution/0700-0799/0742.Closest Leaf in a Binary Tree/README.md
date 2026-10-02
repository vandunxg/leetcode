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

# [742. Closest Leaf in a Binary Tree 🔒](https://leetcode.com/problems/closest-leaf-in-a-binary-tree)

[中文文档](/solution/0700-0799/0742.Closest%20Leaf%20in%20a%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một binary tree, trong đó mỗi node có <strong>giá trị riêng biệt</strong>, cùng số nguyên mục tiêu <code>k</code>. Hãy trả về <em>giá trị của <strong>node lá gần nhất</strong> với </em><code>k</code><em> trong cây</em>.</p>

<p><strong>Gần nhất với lá</strong> nghĩa là cần đi qua ít cạnh nhất trong binary tree để tới một node lá bất kỳ. Node không có node con được gọi là node lá.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0742.Closest%20Leaf%20in%20a%20Binary%20Tree/images/closest1-tree.jpg" style="width: 224px; height: 145px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,3,2], k = 1
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Node lá 2 hoặc 3 đều gần node mục tiêu 1 nhất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0742.Closest%20Leaf%20in%20a%20Binary%20Tree/images/closest2-tree.jpg" style="width: 64px; height: 65px;" />
<pre>
<strong>Đầu vào:</strong> root = [1], k = 1
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Node lá gần nhất chính là root.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0742.Closest%20Leaf%20in%20a%20Binary%20Tree/images/closest3-tree.jpg" style="width: 464px; height: 384px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,2,3,4,null,null,null,5,null,6], k = 2
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Node lá có giá trị 3 (không phải node lá có giá trị 6) gần node có giá trị 2 nhất.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 1000]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 1000</code></li>
	<li>Tất cả giá trị trong cây đều <strong>khác nhau</strong>.</li>
	<li>Tồn tại node trong cây sao cho <code>Node.val == k</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS + BFS

<!-- thinking:start -->

> **Tư duy**
>
> Tìm node lá gần nhất với node có giá trị $k$. Node lá có thể nằm trong subtree hoặc cần đi ngược lên parent trước; chỉ tìm xuống dưới sẽ bỏ sót trường hợp này.
>
> Trong đồ thị vô hướng biểu diễn cây, node lá đầu tiên BFS gặp từ $k$ chính là node gần nhất. Các giá trị đều riêng biệt nên dễ tìm node bắt đầu.
>
> DFS ghi lại cạnh giữa parent và child theo cả hai chiều, sau đó BFS từ $k$ và trả về node đầu tiên không có node con. Độ phức tạp $O(n)$.

<!-- thinking:end -->

Trước tiên, dùng depth-first search để tạo đồ thị vô hướng $g$, trong đó $g[node]$ chứa các node kề với $node$. Sau đó, bắt đầu breadth-first search từ node $k$ cho đến khi tìm thấy node lá; đó là đáp án.

Độ phức tạp thời gian và không gian đều là $O(n)$, trong đó $n$ là số node trong binary tree.

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
    def findClosestLeaf(self, root: Optional[TreeNode], k: int) -> int:
        def dfs(root: Optional[TreeNode], fa: Optional[TreeNode]):
            if root:
                g[root].append(fa)
                g[fa].append(root)
                dfs(root.left, root)
                dfs(root.right, root)

        g = defaultdict(list)
        dfs(root, None)
        q = deque(node for node in g if node and node.val == k)
        vis = set(q)
        while 1:
            node = q.popleft()
            if node:
                if node.left == node.right:
                    return node.val
                for nxt in g[node]:
                    if nxt not in vis:
                        vis.add(nxt)
                        q.append(nxt)
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
    private Map<TreeNode, List<TreeNode>> g = new HashMap<>();

    public int findClosestLeaf(TreeNode root, int k) {
        dfs(root, null);
        Deque<TreeNode> q = new LinkedList<>();
        Set<TreeNode> vis = new HashSet<>(q.size());
        for (TreeNode node : g.keySet()) {
            if (node != null && node.val == k) {
                vis.add(node);
                q.offer(node);
                break;
            }
        }
        while (true) {
            TreeNode node = q.poll();
            if (node != null) {
                if (node.left == node.right) {
                    return node.val;
                }
                for (TreeNode nxt : g.get(node)) {
                    if (vis.add(nxt)) {
                        q.offer(nxt);
                    }
                }
            }
        }
    }

    private void dfs(TreeNode root, TreeNode fa) {
        if (root != null) {
            g.computeIfAbsent(root, k -> new ArrayList<>()).add(fa);
            g.computeIfAbsent(fa, k -> new ArrayList<>()).add(root);
            dfs(root.left, root);
            dfs(root.right, root);
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
    int findClosestLeaf(TreeNode* root, int k) {
        unordered_map<TreeNode*, vector<TreeNode*>> g;
        function<void(TreeNode*, TreeNode*)> dfs = [&](TreeNode* root, TreeNode* fa) {
            if (root) {
                g[root].push_back(fa);
                g[fa].push_back(root);
                dfs(root->left, root);
                dfs(root->right, root);
            }
        };
        dfs(root, nullptr);
        queue<TreeNode*> q;
        unordered_set<TreeNode*> vis;
        for (auto& [node, _] : g) {
            if (node && node->val == k) {
                q.push(node);
                vis.insert(node);
            }
        }
        while (1) {
            auto node = q.front();
            q.pop();
            if (node) {
                if (node->left == node->right) {
                    return node->val;
                }
                for (auto& nxt : g[node]) {
                    if (vis.count(nxt)) {
                        continue;
                    }
                    q.push(nxt);
                    vis.insert(nxt);
                }
            }
        }
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
func findClosestLeaf(root *TreeNode, k int) int {
	g := map[*TreeNode][]*TreeNode{}
	var dfs func(*TreeNode, *TreeNode)
	dfs = func(root, fa *TreeNode) {
		if root != nil {
			g[root] = append(g[root], fa)
			g[fa] = append(g[fa], root)
			dfs(root.Left, root)
			dfs(root.Right, root)
		}
	}
	dfs(root, nil)
	q := []*TreeNode{}
	vis := map[*TreeNode]bool{}
	for node := range g {
		if node != nil && node.Val == k {
			q = append(q, node)
			vis[node] = true
			break
		}
	}
	for {
		node := q[0]
		q = q[1:]
		if node != nil {
			if node.Left == node.Right {
				return node.Val
			}
			for _, nxt := range g[node] {
				if !vis[nxt] {
					vis[nxt] = true
					q = append(q, nxt)
				}
			}
		}
	}
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
