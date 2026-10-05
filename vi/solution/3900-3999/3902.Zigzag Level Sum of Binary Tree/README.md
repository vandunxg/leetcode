---
comments: true
difficulty: Medium
tags:
    - Tree
    - Breadth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [3902. Zigzag Level Sum of Binary Tree 🔒](https://leetcode.com/problems/zigzag-level-sum-of-binary-tree)

[中文文档](/solution/3900-3999/3902.Zigzag%20Level%20Sum%20of%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho <code>root</code> của một <strong>cây nhị phân</strong>.</p>

<p>Hãy duyệt cây theo từng level bằng mẫu zigzag:</p>

<ul>
	<li>Ở các level được đánh số <strong>lẻ</strong> (tính từ 1), duyệt các node từ <strong>trái sang phải</strong>.</li>
	<li>Ở các level được đánh số <strong>chẵn</strong>, duyệt các node từ <strong>phải sang trái</strong>.</li>
</ul>

<p>Khi duyệt một level theo hướng được chỉ định, hãy xử lý các node theo thứ tự và <strong>dừng</strong> ngay trước node đầu tiên vi phạm điều kiện:</p>

<ul>
	<li>Ở các level <strong>lẻ</strong>: node không có child <strong>trái</strong>.</li>
	<li>Ở các level <strong>chẵn</strong>: node không có child <strong>phải</strong>.</li>
</ul>

<p>Chỉ các node được xử lý trước điều kiện dừng mới được tính vào tổng của level.</p>

<p>Hãy trả về một mảng số nguyên <code>ans</code>, trong đó <code>ans[i]</code> là <strong>tổng</strong> các giá trị node được xử lý ở level <code>i + 1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">root = [5,2,8,1,null,9,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[5,8,0]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3902.Zigzag%20Level%20Sum%20of%20Binary%20Tree/images/screenshot-2026-04-13-at-22054am.png" style="height: 240px; width: 300px;" />​​​​​​​</p>

<ul>
	<li>Ở level 1, các node được xử lý từ trái sang phải. Node 5 được tính, nên <code>ans[0] = 5</code>.</li>
	<li>Ở level 2, các node được xử lý từ phải sang trái. Node 8 được tính, nhưng node 2 không có child phải, nên quá trình xử lý dừng lại và <code>ans[1] = 8</code>.</li>
	<li>Ở level 3, các node được xử lý từ trái sang phải. Node đầu tiên là 1 không có child trái, nên không có node nào được tính và <code>ans[2] = 0</code>.</li>
	<li>Vậy <code>ans = [5, 8, 0]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">root = [1,2,3,4,5,null,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,5,0]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3902.Zigzag%20Level%20Sum%20of%20Binary%20Tree/images/screenshot-2026-04-13-at-22232am.png" style="height: 254px; width: 300px;" /></p>

<ul>
	<li>Ở level 1, các node được xử lý từ trái sang phải. Node 1 được tính, nên <code>ans[0] = 1</code>.</li>
	<li>Ở level 2, các node được xử lý từ phải sang trái. Các node 3 và 2 đều được tính vì cả hai đều có child phải, nên <code>ans[1] = 3 + 2 = 5</code>.</li>
	<li>Ở level 3, các node được xử lý từ trái sang phải. Node đầu tiên là 4 không có child trái, nên không có node nào được tính và <code>ans[2] = 0</code>.</li>
	<li>Vậy <code>ans = [1, 5, 0]</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số lượng node trong cây nằm trong khoảng <code>[1, 10<sup>5</sup>]</code>.</li>
	<li><code>-10<sup>5</sup> &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Việc thu thập từng level rồi tính tổng một prefix theo thứ tự zigzag phức tạp hơn cần thiết. Tổng cần tính dừng ở node đầu tiên không có child tương ứng với hướng hiện tại; đó không phải tổng của toàn bộ level.
>
> BFS vốn trả về các node theo từng level. Một cờ $\textit{left}$ ghi nhận hướng: trước tiên đưa level tiếp theo vào queue, sau đó duyệt level hiện tại từ trái hoặc phải, chỉ cộng một node khi child tương ứng của nó tồn tại.
>
> Đảo cờ và thay queue sau mỗi level để một lần BFS tạo ra toàn bộ đáp án.

<!-- thinking:end -->

Ta sử dụng queue $q$ để thực hiện duyệt theo level, đồng thời định nghĩa một biến boolean $\textit{left}$ để biểu thị hướng duyệt của level hiện tại. Với mỗi level, trước tiên ta thêm các node của level tiếp theo vào queue $nq$, sau đó tính tổng các giá trị node của level hiện tại, ký hiệu là $s$, tùy theo giá trị của $\textit{left}$, rồi thêm $s$ vào mảng kết quả. Cuối cùng, ta cập nhật giá trị của $\textit{left}$ và gán $nq$ cho $q$ để tiếp tục duyệt level tiếp theo.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số lượng node trong cây nhị phân.

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
    def zigzagLevelSum(self, root: TreeNode | None) -> list[int]:
        q = [root]
        ans = []
        left = True
        while q:
            nq = []
            for node in q:
                if node.left:
                    nq.append(node.left)
                if node.right:
                    nq.append(node.right)
            m = len(q)
            s = 0
            for i in range(m):
                node = q[i] if left else q[m - i - 1]
                child = node.left if left else node.right
                if not child:
                    break
                s += node.val
            ans.append(s)
            left = not left
            q = nq
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
    public List<Long> zigzagLevelSum(TreeNode root) {
        List<Long> ans = new ArrayList<>();
        List<TreeNode> q = new ArrayList<>();
        q.add(root);
        boolean left = true;
        while (!q.isEmpty()) {
            List<TreeNode> nq = new ArrayList<>();
            for (TreeNode node : q) {
                if (node.left != null) {
                    nq.add(node.left);
                }
                if (node.right != null) {
                    nq.add(node.right);
                }
            }
            int m = q.size();
            long s = 0;
            for (int i = 0; i < m; i++) {
                TreeNode node = left ? q.get(i) : q.get(m - i - 1);
                TreeNode child = left ? node.left : node.right;
                if (child == null) {
                    break;
                }
                s += node.val;
            }
            ans.add(s);
            left = !left;
            q = nq;
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
    vector<long long> zigzagLevelSum(TreeNode* root) {
        vector<long long> ans;
        vector<TreeNode*> q = {root};
        bool left = true;
        while (!q.empty()) {
            vector<TreeNode*> nq;
            for (TreeNode* node : q) {
                if (node->left != nullptr) {
                    nq.push_back(node->left);
                }
                if (node->right != nullptr) {
                    nq.push_back(node->right);
                }
            }
            int m = q.size();
            long long s = 0;
            for (int i = 0; i < m; i++) {
                TreeNode* node = left ? q[i] : q[m - i - 1];
                TreeNode* child = left ? node->left : node->right;
                if (child == nullptr) {
                    break;
                }
                s += node->val;
            }
            ans.push_back(s);
            left = !left;
            q = nq;
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
func zigzagLevelSum(root *TreeNode) []int64 {
	ans := []int64{}
	q := []*TreeNode{root}
	left := true
	for len(q) > 0 {
		nq := []*TreeNode{}
		for _, node := range q {
			if node.Left != nil {
				nq = append(nq, node.Left)
			}
			if node.Right != nil {
				nq = append(nq, node.Right)
			}
		}
		m := len(q)
		var s int64 = 0
		for i := 0; i < m; i++ {
			var node *TreeNode
			if left {
				node = q[i]
			} else {
				node = q[m-i-1]
			}
			var child *TreeNode
			if left {
				child = node.Left
			} else {
				child = node.Right
			}
			if child == nil {
				break
			}
			s += int64(node.Val)
		}
		ans = append(ans, s)
		left = !left
		q = nq
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
function zigzagLevelSum(root: TreeNode | null): number[] {
    let q: TreeNode[] = [root];
    const ans: number[] = [];
    let left = true;
    while (q.length > 0) {
        const nq: TreeNode[] = [];
        for (const { left, right } of q) {
            if (left !== null) {
                nq.push(left);
            }
            if (right !== null) {
                nq.push(right);
            }
        }
        const m = q.length;
        let s = 0;
        for (let i = 0; i < m; i++) {
            const node = left ? q[i] : q[m - i - 1];
            const child = left ? node.left : node.right;
            if (child === null) {
                break;
            }
            s += node.val;
        }
        ans.push(s);
        left = !left;
        q = nq;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
