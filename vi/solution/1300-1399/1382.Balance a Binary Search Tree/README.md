---
comments: true
difficulty: Medium
rating: 1540
source: Weekly Contest 180 Q3
tags:
    - Greedy
    - Tree
    - Depth-First Search
    - Binary Search Tree
    - Divide and Conquer
    - Binary Tree
---

<!-- problem:start -->

# [1382. Balance a Binary Search Tree](https://leetcode.com/problems/balance-a-binary-search-tree)

[中文文档](/solution/1300-1399/1382.Balance%20a%20Binary%20Search%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một binary search tree, hãy trả về <em>một binary search tree <strong>cân bằng</strong> có cùng giá trị node</em>. Nếu có nhiều đáp án, hãy trả về <strong>bất kỳ đáp án nào</strong>.</p>

<p>Binary search tree được gọi là <strong>cân bằng</strong> nếu độ sâu hai cây con của mỗi node chênh lệch không quá <code>1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1382.Balance%20a%20Binary%20Search%20Tree/images/balance1-tree.jpg" style="width: 500px; height: 319px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,null,2,null,3,null,4,null,null]
<strong>Đầu ra:</strong> [2,1,3,null,null,null,4]
<b>Giải thích:</b> Đây không phải đáp án đúng duy nhất; [3,1,4,null,2] cũng là đáp án hợp lệ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1382.Balance%20a%20Binary%20Search%20Tree/images/balanced2-tree.jpg" style="width: 224px; height: 145px;" />
<pre>
<strong>Đầu vào:</strong> root = [2,1,3]
<strong>Đầu ra:</strong> [2,1,3]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 10<sup>4</sup>]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt inorder + Xây dựng binary search tree cân bằng

<!-- thinking:start -->

> **Tư duy**
>
> Cân bằng lại BST mà không thay đổi thứ tự inorder. Duyệt inorder sẽ tạo ra mảng đã sắp xếp; đệ quy chọn phần tử ở giữa làm root sẽ xây dựng cây có chiều cao $\log n$ và vẫn là BST.

<!-- thinking:end -->

Vì cây ban đầu là binary search tree, ta có thể lưu kết quả duyệt inorder vào mảng $nums$. Sau đó, ta thiết kế hàm $build(i, j)$ để xây dựng binary search tree cân bằng từ đoạn chỉ số $[i, j]$ trong $nums$. Đáp án là $build(0, |nums| - 1)$.

Hàm $build(i, j)$ hoạt động như sau:

- Nếu $i > j$, binary search tree cân bằng là cây rỗng, nên trả về node rỗng;
- Nếu không, chọn $mid = (i + j) / 2$ làm root, sau đó đệ quy xây dựng cây con trái và phải rồi trả về root.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node trong binary search tree.

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
    def balanceBST(self, root: TreeNode) -> TreeNode:
        def dfs(root: TreeNode):
            if root is None:
                return
            dfs(root.left)
            nums.append(root.val)
            dfs(root.right)

        def build(i: int, j: int) -> TreeNode:
            if i > j:
                return None
            mid = (i + j) >> 1
            left = build(i, mid - 1)
            right = build(mid + 1, j)
            return TreeNode(nums[mid], left, right)

        nums = []
        dfs(root)
        return build(0, len(nums) - 1)
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
    private List<Integer> nums = new ArrayList<>();

    public TreeNode balanceBST(TreeNode root) {
        dfs(root);
        return build(0, nums.size() - 1);
    }

    private void dfs(TreeNode root) {
        if (root == null) {
            return;
        }
        dfs(root.left);
        nums.add(root.val);
        dfs(root.right);
    }

    private TreeNode build(int i, int j) {
        if (i > j) {
            return null;
        }
        int mid = (i + j) >> 1;
        TreeNode left = build(i, mid - 1);
        TreeNode right = build(mid + 1, j);
        return new TreeNode(nums.get(mid), left, right);
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
    TreeNode* balanceBST(TreeNode* root) {
        dfs(root);
        return build(0, nums.size() - 1);
    }

private:
    vector<int> nums;

    void dfs(TreeNode* root) {
        if (!root) {
            return;
        }
        dfs(root->left);
        nums.push_back(root->val);
        dfs(root->right);
    }

    TreeNode* build(int i, int j) {
        if (i > j) {
            return nullptr;
        }
        int mid = (i + j) >> 1;
        TreeNode* left = build(i, mid - 1);
        TreeNode* right = build(mid + 1, j);
        return new TreeNode(nums[mid], left, right);
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
func balanceBST(root *TreeNode) *TreeNode {
	ans := []int{}
	var dfs func(*TreeNode)
	dfs = func(root *TreeNode) {
		if root == nil {
			return
		}
		dfs(root.Left)
		ans = append(ans, root.Val)
		dfs(root.Right)
	}
	var build func(i, j int) *TreeNode
	build = func(i, j int) *TreeNode {
		if i > j {
			return nil
		}
		mid := (i + j) >> 1
		left := build(i, mid-1)
		right := build(mid+1, j)
		return &TreeNode{Val: ans[mid], Left: left, Right: right}
	}
	dfs(root)
	return build(0, len(ans)-1)
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

function balanceBST(root: TreeNode | null): TreeNode | null {
    const nums: number[] = [];
    const dfs = (root: TreeNode | null): void => {
        if (root == null) {
            return;
        }
        dfs(root.left);
        nums.push(root.val);
        dfs(root.right);
    };
    const build = (i: number, j: number): TreeNode | null => {
        if (i > j) {
            return null;
        }
        const mid: number = (i + j) >> 1;
        const left: TreeNode | null = build(i, mid - 1);
        const right: TreeNode | null = build(mid + 1, j);
        return new TreeNode(nums[mid], left, right);
    };
    dfs(root);
    return build(0, nums.length - 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
