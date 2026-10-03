---
comments: true
difficulty: Medium
rating: 1711
source: Weekly Contest 307 Q3
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Hash Table
    - Binary Tree
---

<!-- problem:start -->

# [2385. Amount of Time for Binary Tree to Be Infected](https://leetcode.com/problems/amount-of-time-for-binary-tree-to-be-infected)

[中文文档](/solution/2300-2399/2385.Amount%20of%20Time%20for%20Binary%20Tree%20to%20Be%20Infected/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho <code>root</code> của một cây nhị phân có các giá trị <strong>không trùng nhau</strong>, và một số nguyên <code>start</code>. Tại phút <code>0</code>, <strong>sự lây nhiễm</strong> bắt đầu từ nút có giá trị <code>start</code>.</p>

<p>Mỗi phút, một nút bị nhiễm nếu:</p>

<ul>
	<li>Nút đó hiện chưa bị nhiễm.</li>
	<li>Nút đó kề với một nút đã bị nhiễm.</li>
</ul>

<p>Hãy trả về <em>số phút cần thiết để toàn bộ cây bị nhiễm.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2385.Amount%20of%20Time%20for%20Binary%20Tree%20to%20Be%20Infected/images/image-20220625231744-1.png" style="width: 400px; height: 306px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,5,3,null,4,10,6,9,2], start = 3
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Các nút bị nhiễm trong những phút sau:
- Phút 0: Nút 3
- Phút 1: Các nút 1, 10 và 6
- Phút 2: Nút 5
- Phút 3: Nút 4
- Phút 4: Các nút 9 và 2
Cần 4 phút để toàn bộ cây bị nhiễm, nên ta trả về 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2385.Amount%20of%20Time%20for%20Binary%20Tree%20to%20Be%20Infected/images/image-20220625231812-2.png" style="width: 75px; height: 66px;" />
<pre>
<strong>Đầu vào:</strong> root = [1], start = 1
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Tại phút 0, nút duy nhất trong cây đã bị nhiễm, nên ta trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số lượng nút trong cây nằm trong khoảng <code>[1, 10<sup>5</sup>]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
	<li>Mỗi nút có một giá trị <strong>duy nhất</strong>.</li>
	<li>Nút có giá trị <code>start</code> tồn tại trong cây.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai DFS

<!-- thinking:start -->

> **Tư duy**
>
> Sự lây nhiễm lan dọc theo các cạnh của cây; thời gian chính là khoảng cách lớn nhất từ $start$ đến một nút bất kỳ trong cây. Với tối đa $10^5$ nút, cần biểu diễn tường minh các liên kết đến nút cha.
>
> DFS đầu tiên xây dựng danh sách kề vô hướng; DFS thứ hai bắt đầu từ $start$ và trả về độ sâu xa nhất. Cả hai đều có độ phức tạp tuyến tính.

<!-- thinking:end -->

Trước tiên, ta dùng một DFS để xây dựng đồ thị và thu được danh sách kề $g$, trong đó $g[node]$ biểu diễn tất cả các nút được nối với nút $node$.

Sau đó, ta dùng $start$ làm điểm bắt đầu và duyệt toàn bộ cây bằng DFS để tìm khoảng cách xa nhất, chính là đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số lượng nút trong cây nhị phân.

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
    def amountOfTime(self, root: Optional[TreeNode], start: int) -> int:
        def dfs(node: Optional[TreeNode], fa: Optional[TreeNode]):
            if node is None:
                return
            if fa:
                g[node.val].append(fa.val)
                g[fa.val].append(node.val)
            dfs(node.left, node)
            dfs(node.right, node)

        def dfs2(node: int, fa: int) -> int:
            ans = 0
            for nxt in g[node]:
                if nxt != fa:
                    ans = max(ans, 1 + dfs2(nxt, node))
            return ans

        g = defaultdict(list)
        dfs(root, None)
        return dfs2(start, -1)
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
    private Map<Integer, List<Integer>> g = new HashMap<>();

    public int amountOfTime(TreeNode root, int start) {
        dfs(root, null);
        return dfs2(start, -1);
    }

    private void dfs(TreeNode node, TreeNode fa) {
        if (node == null) {
            return;
        }
        if (fa != null) {
            g.computeIfAbsent(node.val, k -> new ArrayList<>()).add(fa.val);
            g.computeIfAbsent(fa.val, k -> new ArrayList<>()).add(node.val);
        }
        dfs(node.left, node);
        dfs(node.right, node);
    }

    private int dfs2(int node, int fa) {
        int ans = 0;
        for (int nxt : g.getOrDefault(node, List.of())) {
            if (nxt != fa) {
                ans = Math.max(ans, 1 + dfs2(nxt, node));
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
    int amountOfTime(TreeNode* root, int start) {
        unordered_map<int, vector<int>> g;
        function<void(TreeNode*, TreeNode*)> dfs = [&](TreeNode* node, TreeNode* fa) {
            if (!node) {
                return;
            }
            if (fa) {
                g[node->val].push_back(fa->val);
                g[fa->val].push_back(node->val);
            }
            dfs(node->left, node);
            dfs(node->right, node);
        };
        function<int(int, int)> dfs2 = [&](int node, int fa) -> int {
            int ans = 0;
            for (int nxt : g[node]) {
                if (nxt != fa) {
                    ans = max(ans, 1 + dfs2(nxt, node));
                }
            }
            return ans;
        };
        dfs(root, nullptr);
        return dfs2(start, -1);
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
func amountOfTime(root *TreeNode, start int) int {
	g := map[int][]int{}
	var dfs func(*TreeNode, *TreeNode)
	dfs = func(node, fa *TreeNode) {
		if node == nil {
			return
		}
		if fa != nil {
			g[node.Val] = append(g[node.Val], fa.Val)
			g[fa.Val] = append(g[fa.Val], node.Val)
		}
		dfs(node.Left, node)
		dfs(node.Right, node)
	}
	var dfs2 func(int, int) int
	dfs2 = func(node, fa int) (ans int) {
		for _, nxt := range g[node] {
			if nxt != fa {
				ans = max(ans, 1+dfs2(nxt, node))
			}
		}
		return
	}
	dfs(root, nil)
	return dfs2(start, -1)
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

function amountOfTime(root: TreeNode | null, start: number): number {
    const g: Map<number, number[]> = new Map();
    const dfs = (node: TreeNode | null, fa: TreeNode | null) => {
        if (!node) {
            return;
        }
        if (fa) {
            if (!g.has(node.val)) {
                g.set(node.val, []);
            }
            g.get(node.val)!.push(fa.val);
            if (!g.has(fa.val)) {
                g.set(fa.val, []);
            }
            g.get(fa.val)!.push(node.val);
        }
        dfs(node.left, node);
        dfs(node.right, node);
    };
    const dfs2 = (node: number, fa: number): number => {
        let ans = 0;
        for (const nxt of g.get(node) || []) {
            if (nxt !== fa) {
                ans = Math.max(ans, 1 + dfs2(nxt, node));
            }
        }
        return ans;
    };
    dfs(root, null);
    return dfs2(start, -1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
