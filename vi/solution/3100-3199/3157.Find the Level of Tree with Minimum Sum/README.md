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

# [3157. Find the Level of Tree with Minimum Sum 🔒](https://leetcode.com/problems/find-the-level-of-tree-with-minimum-sum)

[中文文档](/solution/3100-3199/3157.Find%20the%20Level%20of%20Tree%20with%20Minimum%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho root của một cây nhị phân <code>root</code>, trong đó mỗi node có một giá trị, hãy trả về tầng của cây có tổng giá trị <strong>nhỏ nhất</strong> trong tất cả các tầng (nếu có nhiều tầng cùng tổng, trả về tầng có số thứ tự <strong>nhỏ nhất</strong>).</p>

<p><strong>Lưu ý</strong> rằng root của cây nằm ở tầng 1, và tầng của bất kỳ node nào khác bằng khoảng cách từ node đó đến root + 1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">root = [50,6,2,30,80,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3157.Find%20the%20Level%20of%20Tree%20with%20Minimum%20Sum/images/image_2024-05-17_16-15-46.png" style="padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; width: 265px; height: 129px;" /></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">root = [36,17,10,null,null,24]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3157.Find%20the%20Level%20of%20Tree%20with%20Minimum%20Sum/images/image_2024-05-17_16-14-18.png" style="padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; width: 170px; height: 135px;" /></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">root = [5,null,5,null,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3157.Find%20the%20Level%20of%20Tree%20with%20Minimum%20Sum/images/image_2024-05-19_19-07-20.png" style="padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; width: 170px; height: 135px;" /></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số lượng node trong cây nằm trong khoảng <code>[1, 10<sup>5</sup>]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Tìm tầng nông nhất có tổng giá trị nhỏ nhất. DFS cần bookkeeping bổ sung; BFS phù hợp với thứ tự theo tầng.
>
> Chỉ biết tổng một tầng sau khi đọc hết tầng đó, lúc ấy có thể cập nhật kết quả tốt nhất hiện tại.
>
> Mở rộng queue theo từng tầng và thay thế đáp án khi tổng nhỏ hơn. Mỗi node được đưa vào queue đúng một lần.

<!-- thinking:end -->

Ta có thể sử dụng Breadth-First Search (BFS) để duyệt cây nhị phân theo từng tầng, ghi lại tổng giá trị của các node ở mỗi tầng, tìm tầng có tổng giá trị node nhỏ nhất, rồi trả về số thứ tự của tầng đó.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó $n$ là số node trong cây nhị phân.

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
    def minimumLevel(self, root: Optional[TreeNode]) -> int:
        q = deque([root])
        ans = 0
        level, s = 1, inf
        while q:
            t = 0
            for _ in range(len(q)):
                node = q.popleft()
                t += node.val
                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)
            if s > t:
                s = t
                ans = level
            level += 1
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
    public int minimumLevel(TreeNode root) {
        Deque<TreeNode> q = new ArrayDeque<>();
        q.offer(root);
        int ans = 0;
        long s = Long.MAX_VALUE;
        for (int level = 1; !q.isEmpty(); ++level) {
            long t = 0;
            for (int m = q.size(); m > 0; --m) {
                TreeNode node = q.poll();
                t += node.val;
                if (node.left != null) {
                    q.offer(node.left);
                }
                if (node.right != null) {
                    q.offer(node.right);
                }
            }
            if (s > t) {
                s = t;
                ans = level;
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
    int minimumLevel(TreeNode* root) {
        queue<TreeNode*> q{{root}};
        int ans = 0;
        long long s = 1LL << 60;
        for (int level = 1; q.size(); ++level) {
            long long t = 0;
            for (int m = q.size(); m; --m) {
                TreeNode* node = q.front();
                q.pop();
                t += node->val;
                if (node->left) {
                    q.push(node->left);
                }
                if (node->right) {
                    q.push(node->right);
                }
            }
            if (s > t) {
                s = t;
                ans = level;
            }
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
func minimumLevel(root *TreeNode) (ans int) {
	q := []*TreeNode{root}
	s := math.MaxInt64
	for level := 1; len(q) > 0; level++ {
		t := 0
		for m := len(q); m > 0; m-- {
			node := q[0]
			q = q[1:]
			t += node.Val
			if node.Left != nil {
				q = append(q, node.Left)
			}
			if node.Right != nil {
				q = append(q, node.Right)
			}
		}
		if s > t {
			s = t
			ans = level
		}
	}
	return
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

function minimumLevel(root: TreeNode | null): number {
    const q: TreeNode[] = [root];
    let s = Infinity;
    let ans = 0;
    for (let level = 1; q.length; ++level) {
        const qq: TreeNode[] = [];
        let t = 0;
        for (const { val, left, right } of q) {
            t += val;
            left && qq.push(left);
            right && qq.push(right);
        }
        if (s > t) {
            s = t;
            ans = level;
        }
        q.splice(0, q.length, ...qq);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
