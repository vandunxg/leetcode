---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Binary Search Tree
    - Binary Tree
---

<!-- problem:start -->

# [3831. Median of a Binary Search Tree Level 🔒](https://leetcode.com/problems/median-of-a-binary-search-tree-level)

[中文文档](/solution/3800-3899/3831.Median%20of%20a%20Binary%20Search%20Tree%20Level/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho <code>root</code> của một <strong>Cây tìm kiếm nhị phân (BST)</strong> và một số nguyên <code>level</code>.</p>

<p>Nút gốc nằm ở level 0. Mỗi level biểu thị khoảng cách từ nút gốc.</p>

<p>Hãy trả về <strong>giá trị trung vị</strong> của tất cả các giá trị nút xuất hiện ở <code>level</code> đã cho. Nếu level không tồn tại hoặc không chứa nút nào, hãy trả về -1.</p>

<p><strong>Trung vị</strong> được định nghĩa là phần tử ở giữa sau khi sắp xếp các giá trị ở level theo thứ tự <strong>không giảm</strong>. Nếu số lượng giá trị ở level là số chẵn, hãy trả về <strong>trung vị trên</strong> (phần tử lớn hơn trong hai phần tử ở giữa sau khi sắp xếp).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3800-3899/3831.Median%20of%20a%20Binary%20Search%20Tree%20Level/images/screenshot-2026-01-27-at-20801pm.png" style="width: 180px; height: 182px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">root = [4,null,5,null,7], level = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các nút ở <code>level = 2</code> là <code>[7]</code>. Giá trị trung vị là 7.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3800-3899/3831.Median%20of%20a%20Binary%20Search%20Tree%20Level/images/screenshot-2026-01-27-at-20926pm.png" style="width: 200px; height: 169px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">root = [6,3,8], level = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các nút ở <code>level = 1</code> là <code>[3, 8]</code>. Có hai giá trị trung vị có thể chọn, nên giá trị lớn hơn là 8 được chọn làm đáp án.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><strong class="example">​​​​​​​​​​​​​​</strong><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3800-3899/3831.Median%20of%20a%20Binary%20Search%20Tree%20Level/images/screenshot-2026-01-27-at-21001pm.png" style="width: 150px; height: 193px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">root = [2,1], level = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có nút nào ở <code>level = 2</code>​​​​​​​, nên đáp án là -1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số lượng nút trong cây nằm trong khoảng <code>[1, 2 * 10<sup>5</sup>]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 10<sup>6</sup></code></li>
	<li><code>0 &lt;= level &lt;= 2 * 10<sup>​​​​​​​5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm trung vị của một level cụ thể trong BST, lấy phần tử ở giữa phía trên khi số lượng phần tử là số chẵn. Một level có thể chứa $O(n)$ nút với $n \le 2 \times 10^5$.
>
> Việc thu thập rồi sắp xếp là không cần thiết: phép duyệt inorder của BST vốn đã cho kết quả được sắp xếp, nên dãy con chỉ gồm các nút ở level đó cũng đã có thứ tự.
>
> Một DFS inorder chỉ thêm giá trị khi depth bằng $\textit{level}$.
>
> Trung vị là $\textit{nums}[\lfloor |\textit{nums}|/2 \rfloor]$; nếu level rỗng thì kết quả là $-1$.

<!-- thinking:end -->

Ta nhận thấy bài toán yêu cầu tìm trung vị của các giá trị nút ở một level nhất định trong cây tìm kiếm nhị phân. Vì định nghĩa của trung vị là sắp xếp các giá trị nút rồi lấy giá trị ở giữa, mà phép duyệt inorder của cây tìm kiếm nhị phân vốn đã có thứ tự tăng dần, ta có thể thu thập các giá trị nút ở level cần tìm thông qua phép duyệt inorder.

Ta định nghĩa hàm hỗ trợ $\text{dfs}(root, i)$, trong đó $root$ là nút hiện tại và $i$ là level của nút hiện tại. Trong hàm này, nếu nút hiện tại rỗng, ta trả về ngay. Nếu không, ta lần lượt duyệt đệ quy cây con trái, kiểm tra xem level của nút hiện tại có bằng level mục tiêu hay không; nếu có thì thêm giá trị của nút hiện tại vào danh sách kết quả, rồi cuối cùng duyệt đệ quy cây con phải.

Ta khởi tạo một danh sách rỗng $\text{nums}$ để lưu các giá trị nút ở level cần tìm, rồi gọi $\text{dfs}(root, 0)$ để bắt đầu duyệt. Cuối cùng, ta kiểm tra xem $\text{nums}$ có rỗng hay không; nếu rỗng thì trả về -1, ngược lại trả về giá trị ở vị trí giữa của $\text{nums}$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số lượng nút trong cây.

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
    def levelMedian(self, root: Optional[TreeNode], level: int) -> int:
        def dfs(root: Optional[TreeNode], i: int):
            if root is None:
                return
            dfs(root.left, i + 1)
            if i == level:
                nums.append(root.val)
            dfs(root.right, i + 1)

        nums = []
        dfs(root, 0)
        return nums[len(nums) // 2] if nums else -1
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
    private int level;

    public int levelMedian(TreeNode root, int level) {
        this.level = level;
        dfs(root, 0);
        return nums.isEmpty() ? -1 : nums.get(nums.size() / 2);
    }

    private void dfs(TreeNode root, int i) {
        if (root == null) {
            return;
        }
        dfs(root.left, i + 1);
        if (i == level) {
            nums.add(root.val);
        }
        dfs(root.right, i + 1);
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
    int levelMedian(TreeNode* root, int level) {
        vector<int> nums;

        auto dfs = [&](this auto&& dfs, TreeNode* node, int i) -> void {
            if (!node) {
                return;
            }
            dfs(node->left, i + 1);
            if (i == level) {
                nums.push_back(node->val);
            }
            dfs(node->right, i + 1);
        };

        dfs(root, 0);
        return nums.empty() ? -1 : nums[nums.size() / 2];
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
func levelMedian(root *TreeNode, level int) int {
	nums := make([]int, 0)

	var dfs func(*TreeNode, int)
	dfs = func(node *TreeNode, i int) {
		if node == nil {
			return
		}
		dfs(node.Left, i+1)
		if i == level {
			nums = append(nums, node.Val)
		}
		dfs(node.Right, i+1)
	}

	dfs(root, 0)
	if len(nums) == 0 {
		return -1
	}
	return nums[len(nums)/2]
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
function levelMedian(root: TreeNode | null, level: number): number {
    const nums: number[] = [];

    const dfs = (node: TreeNode | null, i: number): void => {
        if (node === null) {
            return;
        }
        dfs(node.left, i + 1);
        if (i === level) {
            nums.push(node.val);
        }
        dfs(node.right, i + 1);
    };

    dfs(root, 0);
    if (nums.length === 0) {
        return -1;
    }
    return nums[nums.length >> 1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
