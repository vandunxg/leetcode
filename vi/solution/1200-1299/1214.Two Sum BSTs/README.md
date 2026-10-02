---
comments: true
difficulty: Medium
rating: 1389
source: Biweekly Contest 10 Q2
tags:
    - Stack
    - Tree
    - Depth-First Search
    - Binary Search Tree
    - Two Pointers
    - Binary Search
    - Binary Tree
---

<!-- problem:start -->

# [1214. Two Sum BSTs 🔒](https://leetcode.com/problems/two-sum-bsts)

[中文文档](/solution/1200-1299/1214.Two%20Sum%20BSTs/README.md)

## Mô tả

<!-- description:start -->

<p>Cho root của hai cây tìm kiếm nhị phân là <code>root1</code> và <code>root2</code>. Trả về <code>true</code> khi và chỉ khi có một node trong cây thứ nhất và một node trong cây thứ hai sao cho tổng giá trị của chúng bằng số nguyên <code>target</code> đã cho.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1214.Two%20Sum%20BSTs/images/ex1.png" style="width: 369px; height: 169px;" />
<pre>
<strong>Input:</strong> root1 = [2,1,4], root2 = [1,0,3], target = 5
<strong>Output:</strong> true
<strong>Giải thích: </strong>2 và 3 có tổng bằng 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1214.Two%20Sum%20BSTs/images/ex2.png" style="width: 453px; height: 290px;" />
<pre>
<strong>Input:</strong> root1 = [0,-10,10], root2 = [5,1,7,0,2], target = 18
<strong>Output:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong mỗi cây nằm trong khoảng <code>[1, 5000]</code>.</li>
	<li><code>-10<sup>9</sup> &lt;= Node.val, target &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt inorder + Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Tìm $target-x$ trong một BST có độ phức tạp $O(n\log n)$ và vẫn chấp nhận được với $5000$ node, nhưng ta cần ghép các giá trị từ hai cây. Duyệt inorder tạo ra hai mảng đã sắp xếp, đưa bài toán về tìm two-sum trên hai danh sách đã sắp xếp.
>
> Đặt hai con trỏ ở đầu $nums[0]$ và cuối $nums[1]$: nếu tổng nhỏ hơn target thì tăng con trỏ trái, nếu tổng lớn hơn thì giảm con trỏ phải. Duyệt inorder tạo thứ tự đã sắp xếp; two pointers tìm cặp trong thời gian tuyến tính.

<!-- thinking:end -->

Ta duyệt inorder riêng từng cây để thu được hai mảng đã sắp xếp $nums[0]$ và $nums[1]$. Sau đó, dùng phương pháp two pointers để kiểm tra có hai số nào có tổng bằng target hay không. Cách làm như sau:

Khởi tạo hai con trỏ $i$ và $j$, lần lượt trỏ đến đầu mảng $nums[0]$ và cuối mảng $nums[1]$.

Mỗi lần, so sánh tổng $x = nums[0][i] + nums[1][j]$ với target. Nếu $x = target$, trả về `true`; nếu $x \lt target$, dịch $i$ sang phải một vị trí; còn nếu $x \gt target$, dịch $j$ sang trái một vị trí.

Độ phức tạp thời gian là $O(m + n)$ và độ phức tạp không gian là $O(m + n)$, trong đó $m$ và $n$ lần lượt là số node trong hai cây.

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
    def twoSumBSTs(
        self, root1: Optional[TreeNode], root2: Optional[TreeNode], target: int
    ) -> bool:
        def dfs(root: Optional[TreeNode], i: int):
            if root is None:
                return
            dfs(root.left, i)
            nums[i].append(root.val)
            dfs(root.right, i)

        nums = [[], []]
        dfs(root1, 0)
        dfs(root2, 1)
        i, j = 0, len(nums[1]) - 1
        while i < len(nums[0]) and ~j:
            x = nums[0][i] + nums[1][j]
            if x == target:
                return True
            if x < target:
                i += 1
            else:
                j -= 1
        return False
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
    private List<Integer>[] nums = new List[2];

    public boolean twoSumBSTs(TreeNode root1, TreeNode root2, int target) {
        Arrays.setAll(nums, k -> new ArrayList<>());
        dfs(root1, 0);
        dfs(root2, 1);
        int i = 0, j = nums[1].size() - 1;
        while (i < nums[0].size() && j >= 0) {
            int x = nums[0].get(i) + nums[1].get(j);
            if (x == target) {
                return true;
            }
            if (x < target) {
                ++i;
            } else {
                --j;
            }
        }
        return false;
    }

    private void dfs(TreeNode root, int i) {
        if (root == null) {
            return;
        }
        dfs(root.left, i);
        nums[i].add(root.val);
        dfs(root.right, i);
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
    bool twoSumBSTs(TreeNode* root1, TreeNode* root2, int target) {
        vector<int> nums[2];
        function<void(TreeNode*, int)> dfs = [&](TreeNode* root, int i) {
            if (!root) {
                return;
            }
            dfs(root->left, i);
            nums[i].push_back(root->val);
            dfs(root->right, i);
        };
        dfs(root1, 0);
        dfs(root2, 1);
        int i = 0, j = nums[1].size() - 1;
        while (i < nums[0].size() && j >= 0) {
            int x = nums[0][i] + nums[1][j];
            if (x == target) {
                return true;
            }
            if (x < target) {
                ++i;
            } else {
                --j;
            }
        }
        return false;
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
func twoSumBSTs(root1 *TreeNode, root2 *TreeNode, target int) bool {
	nums := [2][]int{}
	var dfs func(*TreeNode, int)
	dfs = func(root *TreeNode, i int) {
		if root == nil {
			return
		}
		dfs(root.Left, i)
		nums[i] = append(nums[i], root.Val)
		dfs(root.Right, i)
	}
	dfs(root1, 0)
	dfs(root2, 1)
	i, j := 0, len(nums[1])-1
	for i < len(nums[0]) && j >= 0 {
		x := nums[0][i] + nums[1][j]
		if x == target {
			return true
		}
		if x < target {
			i++
		} else {
			j--
		}
	}
	return false
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

function twoSumBSTs(root1: TreeNode | null, root2: TreeNode | null, target: number): boolean {
    const nums: number[][] = Array(2)
        .fill(0)
        .map(() => []);
    const dfs = (root: TreeNode | null, i: number) => {
        if (!root) {
            return;
        }
        dfs(root.left, i);
        nums[i].push(root.val);
        dfs(root.right, i);
    };
    dfs(root1, 0);
    dfs(root2, 1);
    let i = 0;
    let j = nums[1].length - 1;
    while (i < nums[0].length && j >= 0) {
        const x = nums[0][i] + nums[1][j];
        if (x === target) {
            return true;
        }
        if (x < target) {
            ++i;
        } else {
            --j;
        }
    }
    return false;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
