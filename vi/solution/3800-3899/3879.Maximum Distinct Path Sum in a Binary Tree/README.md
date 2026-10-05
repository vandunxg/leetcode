---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Hash Table
    - Binary Tree
---

<!-- problem:start -->

# [3879. Maximum Distinct Path Sum in a Binary Tree 🔒](https://leetcode.com/problems/maximum-distinct-path-sum-in-a-binary-tree)

[中文文档](/solution/3800-3899/3879.Maximum%20Distinct%20Path%20Sum%20in%20a%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho <code>root</code> của một <strong>cây nhị phân</strong>, trong đó mỗi node chứa một giá trị nguyên.</p>

<p>Một <strong>đường đi hợp lệ</strong> trong cây là một dãy các node <strong>liền kề</strong> sao cho:</p>

<ul>
	<li>Đường đi có thể bắt đầu và kết thúc tại <strong>bất kỳ node nào</strong> trong cây.</li>
	<li>Đường đi <strong>không nhất thiết</strong> phải đi qua root.</li>
	<li>Tất cả giá trị node trên đường đi đều <strong>phân biệt</strong>.</li>
</ul>

<p>Trả về một số nguyên biểu thị <strong>tổng</strong> lớn nhất có thể của các giá trị node trong mọi đường đi hợp lệ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3800-3899/3879.Maximum%20Distinct%20Path%20Sum%20in%20a%20Binary%20Tree/images/screenshot-2026-01-29-at-12940am.png" style="width: 200px; height: 175px;" /></p>

<p><strong>Đầu vào:</strong> <span class="example-io">root = [2,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Đường đi <code>2 &rarr; 2</code> không hợp lệ vì giá trị 2 không phân biệt.</li>
	<li>Đường đi hợp lệ có tổng lớn nhất là <code>2 &rarr; 1</code>, với tổng = <code>2 + 1 = 3</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3800-3899/3879.Maximum%20Distinct%20Path%20Sum%20in%20a%20Binary%20Tree/images/screenshot-2026-01-29-at-15149am.png" style="width: 200px; height: 204px;" /></p>

<p><strong>Đầu vào:</strong> <span class="example-io">root = [1,-2,5,null,null,3,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Đường đi <code>3 &rarr; 5 &rarr; 5</code> không hợp lệ vì giá trị 5 bị lặp.</li>
	<li>Đường đi hợp lệ có tổng lớn nhất là <code>1 &rarr; 5 &rarr; 3</code>, với tổng = <code>1 + 5 + 3 = 9</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3800-3899/3879.Maximum%20Distinct%20Path%20Sum%20in%20a%20Binary%20Tree/images/screenshot-2026-01-29-at-15555am.png" style="width: 180px; height: 217px;" />​​​​​​​</p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">root = [4,6,6,null,null,null,9]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">19</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Đường đi <code>6 &rarr; 4 &rarr; 6 &rarr; 9</code> không hợp lệ vì giá trị 6 xuất hiện nhiều hơn một lần.</li>
	<li>Đường đi hợp lệ có tổng lớn nhất là <code>4 &rarr; 6 &rarr; 9</code>, với tổng = <code>4 + 6 + 9 = 19</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số lượng node trong cây nằm trong khoảng <code>[1, 1000]</code>.</li>
	<li><code>-1000 &lt;= Node.val &lt;= 1000​​​​​​​</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Một đường đi có thể bắt đầu và kết thúc ở bất kỳ đâu, đồng thời phải có các giá trị phân biệt; mục tiêu là tìm tổng giá trị lớn nhất. Cây có nhiều nhất $1000$ node và các giá trị có thể lặp lại.
>
> Đường đi không nhất thiết đi qua root, nên ta xem cây như một đồ thị vô hướng. Ta thực hiện DFS từ mỗi điểm bắt đầu và dùng một set để duy trì tính duy nhất của các giá trị.
>
> Ta xây dựng adjacency (node cha, node con trái, node con phải), tìm kiếm từ mỗi node và hoàn tác trạng thái đã thăm khi backtrack.
>
> Có thể chấp nhận $O(n^2)$ điểm bắt đầu khi $n \le 1000$.

<!-- thinking:end -->

Ta có thể xem cây như một đồ thị vô hướng, sử dụng hash table $g$ để lưu các node kề với mỗi node, trong đó $g[node]$ chứa node cha, node con trái và node con phải của $node$.

Ta sử dụng depth-first search để duyệt cây và xây dựng hash table $g$. Với mỗi node, ta thêm node cha, node con trái và node con phải của node đó vào $g[node]$.

Tiếp theo, ta sử dụng một depth-first search khác để tính tổng đường đi lớn nhất bắt đầu từ mỗi node. Trong quá trình này, ta dùng hash set $vis$ để ghi lại các giá trị node đã đi qua trên đường đi hiện tại, đảm bảo tất cả giá trị node trên đường đi đều phân biệt. Với mỗi node, trước tiên ta kiểm tra xem node đó đã có trong $vis$ hay chưa; nếu có, ta trả về $0$. Nếu chưa, ta thêm giá trị node vào $vis$ và tính tổng đường đi bắt đầu từ node đó. Ta duyệt các node kề trong $g[node]$, đệ quy tính tổng đường đi bắt đầu từ từng node kề và cập nhật kết quả tốt nhất hiện tại. Cuối cùng, ta xóa giá trị node hiện tại khỏi $vis$ và trả về giá trị node hiện tại cộng với tổng đường đi tốt nhất.

Ta thực hiện phép tính trên với mọi node trong cây và theo dõi tổng đường đi lớn nhất. Đáp án cuối cùng là tổng đường đi lớn nhất.

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là số lượng node trong cây.

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
    def maxSum(self, root: Optional[TreeNode]) -> int:
        def dfs(node, p):
            if node is None:
                return
            g[node].append(p)
            g[node].append(node.left)
            g[node].append(node.right)
            dfs(node.left, node)
            dfs(node.right, node)

        def dfs2(node):
            if node is None or node.val in vis:
                return 0
            vis.add(node.val)
            res = node.val
            best = 0
            for nxt in g[node]:
                best = max(best, dfs2(nxt))
            vis.remove(node.val)
            res += best
            return res

        g = defaultdict(list)
        dfs(root, None)
        vis = set()
        ans = -inf
        for node in g:
            ans = max(ans, dfs2(node))
            vis.clear()
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
    Map<TreeNode, List<TreeNode>> g = new HashMap<>();
    Set<Integer> vis = new HashSet<>();

    public int maxSum(TreeNode root) {
        dfs(root, null);

        int ans = Integer.MIN_VALUE;
        for (TreeNode node : g.keySet()) {
            ans = Math.max(ans, dfs2(node));
            vis.clear();
        }
        return ans;
    }

    private void dfs(TreeNode node, TreeNode p) {
        if (node == null) {
            return;
        }
        g.computeIfAbsent(node, k -> new ArrayList<>());
        g.get(node).add(p);
        g.get(node).add(node.left);
        g.get(node).add(node.right);

        dfs(node.left, node);
        dfs(node.right, node);
    }

    private int dfs2(TreeNode node) {
        if (node == null || vis.contains(node.val)) {
            return 0;
        }
        vis.add(node.val);
        int res = node.val;
        int best = 0;
        for (TreeNode nxt : g.getOrDefault(node, Collections.emptyList())) {
            best = Math.max(best, dfs2(nxt));
        }
        vis.remove(node.val);
        return res + best;
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
    int maxSum(TreeNode* root) {
        unordered_map<TreeNode*, vector<TreeNode*>> g;
        unordered_set<int> vis;

        auto dfs = [&](this auto&& dfs, TreeNode* node, TreeNode* p) -> void {
            if (!node) return;
            g[node].push_back(p);
            g[node].push_back(node->left);
            g[node].push_back(node->right);
            dfs(node->left, node);
            dfs(node->right, node);
        };

        auto dfs2 = [&](this auto&& dfs2, TreeNode* node) -> int {
            if (!node || vis.count(node->val)) return 0;
            vis.insert(node->val);
            int res = node->val;
            int best = 0;
            for (auto nxt : g[node]) {
                best = max(best, dfs2(nxt));
            }
            vis.erase(node->val);
            return res + best;
        };

        dfs(root, nullptr);

        int ans = INT_MIN;
        for (auto& [node, _] : g) {
            ans = max(ans, dfs2(node));
            vis.clear();
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
func maxSum(root *TreeNode) int {
	g := map[*TreeNode][]*TreeNode{}

	var dfs func(node, p *TreeNode)
	dfs = func(node, p *TreeNode) {
		if node == nil {
			return
		}
		g[node] = append(g[node], p, node.Left, node.Right)
		dfs(node.Left, node)
		dfs(node.Right, node)
	}

	vis := map[int]bool{}

	var dfs2 func(node *TreeNode) int
	dfs2 = func(node *TreeNode) int {
		if node == nil || vis[node.Val] {
			return 0
		}
		vis[node.Val] = true
		res := node.Val
		best := 0
		for _, nxt := range g[node] {
			if v := dfs2(nxt); v > best {
				best = v
			}
		}
		vis[node.Val] = false
		return res + best
	}

	dfs(root, nil)

	ans := math.MinInt
	for node := range g {
		ans = max(ans, dfs2(node))
		clear(vis)
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
function maxSum(root: TreeNode | null): number {
    const g = new Map<TreeNode, (TreeNode | null)[]>();

    function dfs(node: TreeNode | null, p: TreeNode | null): void {
        if (!node) return;
        if (!g.has(node)) g.set(node, []);
        g.get(node)!.push(p, node.left, node.right);
        dfs(node.left, node);
        dfs(node.right, node);
    }

    const vis = new Set<number>();

    function dfs2(node: TreeNode | null): number {
        if (!node || vis.has(node.val)) return 0;
        vis.add(node.val);
        let res = node.val;
        let best = 0;
        for (const nxt of g.get(node) || []) {
            best = Math.max(best, dfs2(nxt));
        }
        vis.delete(node.val);
        return res + best;
    }

    dfs(root, null);

    let ans = -Infinity;
    for (const node of g.keys()) {
        ans = Math.max(ans, dfs2(node));
        vis.clear();
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
