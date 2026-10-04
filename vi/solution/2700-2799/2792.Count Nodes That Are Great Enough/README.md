---
comments: true
difficulty: Hard
tags:
    - Tree
    - Depth-First Search
    - Divide and Conquer
    - Binary Tree
---

<!-- problem:start -->

# [2792. Count Nodes That Are Great Enough 🔒](https://leetcode.com/problems/count-nodes-that-are-great-enough)

[中文文档](/solution/2700-2799/2792.Count%20Nodes%20That%20Are%20Great%20Enough/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho <code>root</code> của một cây nhị phân và một số nguyên <code>k</code>. Một node của cây này được gọi là <strong>đủ lớn</strong> nếu thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Cây con của nó có <strong>ít nhất</strong> <code>k</code> node.</li>
	<li>Giá trị của nó <b>lớn hơn</b> giá trị của <strong>ít nhất</strong> <code>k</code> node trong cây con của nó.</li>
</ul>

<p>Trả về <em>số node trong cây đủ lớn.</em></p>

<p>Node <code>u</code> nằm trong <strong>cây con</strong> của node&nbsp;<code>v</code> nếu <code><font face="monospace">u == v</font></code>&nbsp;hoặc&nbsp;<code>v</code> là tổ tiên của <code>u</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [7,6,5,4,3,2,1], k = 2
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Đánh số các node từ 1 đến 7.
Các giá trị trong cây con của node 1: {1,2,3,4,5,6,7}. Vì node.val == 7 nên có 6 node có giá trị nhỏ hơn giá trị của nó. Do đó, node này đủ lớn.
Các giá trị trong cây con của node 2: {3,4,6}. Vì node.val == 6 nên có 2 node có giá trị nhỏ hơn giá trị của nó. Do đó, node này đủ lớn.
Các giá trị trong cây con của node 3: {1,2,5}. Vì node.val == 5 nên có 2 node có giá trị nhỏ hơn giá trị của nó. Do đó, node này đủ lớn.
Có thể chứng minh rằng các node khác không đủ lớn.
Xem hình bên dưới để hiểu rõ hơn.</pre>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2700-2799/2792.Count%20Nodes%20That%20Are%20Great%20Enough/images/1.png" style="padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; width: 300px; height: 167px;" /></p>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [1,2,3], k = 1
<strong>Đầu ra:</strong> 0
<strong>Giải thích: </strong>Đánh số các node từ 1 đến 3.
Các giá trị trong cây con của node 1: {1,2,3}. Vì node.val == 1 nên không có node nào có giá trị nhỏ hơn giá trị của nó. Do đó, node này không đủ lớn.
Các giá trị trong cây con của node 2: {2}. Vì node.val == 2 nên không có node nào có giá trị nhỏ hơn giá trị của nó. Do đó, node này không đủ lớn.
Các giá trị trong cây con của node 3: {3}. Vì node.val == 3 nên không có node nào có giá trị nhỏ hơn giá trị của nó. Do đó, node này không đủ lớn.
Xem hình bên dưới để hiểu rõ hơn.</pre>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2700-2799/2792.Count%20Nodes%20That%20Are%20Great%20Enough/images/2.png" style="padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; width: 123px; height: 101px;" /></p>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [3,2,2], k = 2
<strong>Đầu ra:</strong> 1
<strong>Giải thích: </strong>Đánh số các node từ 1 đến 3.
Các giá trị trong cây con của node 1: {2,2,3}. Vì node.val == 3 nên có 2 node có giá trị nhỏ hơn giá trị của nó. Do đó, node này đủ lớn.
Các giá trị trong cây con của node 2: {2}. Vì node.val == 2 nên không có node nào có giá trị nhỏ hơn giá trị của nó. Do đó, node này không đủ lớn.
Các giá trị trong cây con của node 3: {2}. Vì node.val == 2 nên không có node nào có giá trị nhỏ hơn giá trị của nó. Do đó, node này không đủ lớn.
Xem hình bên dưới để hiểu rõ hơn.</pre>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2700-2799/2792.Count%20Nodes%20That%20Are%20Great%20Enough/images/3.png" style="padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; width: 123px; height: 101px;" /></p>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng&nbsp;<code>[1, 10<sup>4</sup>]</code>.<span style="display: none;">&nbsp;</span></li>
	<li><code>1 &lt;= Node.val &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một node đủ lớn khi và chỉ khi cây con của nó chứa ít nhất $k$ giá trị nhỏ hơn nó. Việc sắp xếp toàn bộ từng cây con sẽ tốn nhiều công sức khi $k$ nhỏ.
>
> Duyệt DFS hậu tự sẽ gộp tối đa $k$ giá trị nhỏ nhất của các node con vào một heap có kích thước $k$. Nếu heap đã đầy và phần tử đầu của nó (giá trị thứ $k$ nhỏ nhất sau khi đổi dấu) vẫn nhỏ hơn node hiện tại, ta tăng kết quả.

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
    def countGreatEnoughNodes(self, root: Optional[TreeNode], k: int) -> int:
        def push(pq, x):
            heappush(pq, x)
            if len(pq) > k:
                heappop(pq)

        def dfs(root):
            if root is None:
                return []
            l, r = dfs(root.left), dfs(root.right)
            for x in r:
                push(l, x)
            if len(l) == k and -l[0] < root.val:
                nonlocal ans
                ans += 1
            push(l, -root.val)
            return l

        ans = 0
        dfs(root)
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
    private int ans;
    private int k;

    public int countGreatEnoughNodes(TreeNode root, int k) {
        this.k = k;
        dfs(root);
        return ans;
    }

    private PriorityQueue<Integer> dfs(TreeNode root) {
        if (root == null) {
            return new PriorityQueue<>(Comparator.reverseOrder());
        }
        var l = dfs(root.left);
        var r = dfs(root.right);
        for (int x : r) {
            l.offer(x);
            if (l.size() > k) {
                l.poll();
            }
        }
        if (l.size() == k && l.peek() < root.val) {
            ++ans;
        }
        l.offer(root.val);
        if (l.size() > k) {
            l.poll();
        }
        return l;
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
    int countGreatEnoughNodes(TreeNode* root, int k) {
        int ans = 0;
        function<priority_queue<int>(TreeNode*)> dfs = [&](TreeNode* root) {
            if (!root) {
                return priority_queue<int>();
            }
            auto left = dfs(root->left);
            auto right = dfs(root->right);
            while (right.size()) {
                left.push(right.top());
                right.pop();
                if (left.size() > k) {
                    left.pop();
                }
            }
            if (left.size() == k && left.top() < root->val) {
                ++ans;
            }
            left.push(root->val);
            if (left.size() > k) {
                left.pop();
            }
            return left;
        };
        dfs(root);
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
func countGreatEnoughNodes(root *TreeNode, k int) (ans int) {
	var dfs func(*TreeNode) hp
	dfs = func(root *TreeNode) hp {
		if root == nil {
			return hp{}
		}
		l, r := dfs(root.Left), dfs(root.Right)
		for _, x := range r.IntSlice {
			l.push(x)
			if l.Len() > k {
				l.pop()
			}
		}
		if l.Len() == k && root.Val > l.IntSlice[0] {
			ans++
		}
		l.push(root.Val)
		if l.Len() > k {
			l.pop()
		}
		return l
	}
	dfs(root)
	return
}

type hp struct{ sort.IntSlice }

func (h hp) Less(i, j int) bool { return h.IntSlice[i] > h.IntSlice[j] }
func (h *hp) Push(v any)        { h.IntSlice = append(h.IntSlice, v.(int)) }
func (h *hp) Pop() any {
	a := h.IntSlice
	v := a[len(a)-1]
	h.IntSlice = a[:len(a)-1]
	return v
}
func (h *hp) push(v int) { heap.Push(h, v) }
func (h *hp) pop() int   { return heap.Pop(h).(int) }
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
