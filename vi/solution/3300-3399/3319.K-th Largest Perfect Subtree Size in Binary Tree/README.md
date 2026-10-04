---
comments: true
difficulty: Medium
rating: 1603
source: Weekly Contest 419 Q2
tags:
    - Tree
    - Depth-First Search
    - Binary Tree
    - Sorting
---

<!-- problem:start -->

# [3319. K-th Largest Perfect Subtree Size in Binary Tree](https://leetcode.com/problems/k-th-largest-perfect-subtree-size-in-binary-tree)

[中文文档](/solution/3300-3399/3319.K-th%20Largest%20Perfect%20Subtree%20Size%20in%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho <code>root</code> của một <strong>cây nhị phân</strong> và một số nguyên <code>k</code>.</p>

<p>Trả về một số nguyên biểu thị kích thước của <code>k<sup>th</sup></code> <strong>cây con<em> </em>nhị phân hoàn hảo</strong><em> </em><span data-keyword="subtree">lớn nhất</span>, hoặc <code>-1</code> nếu cây con đó không tồn tại.</p>

<p><strong>Cây nhị phân hoàn hảo</strong> là một cây trong đó tất cả các nút lá nằm trên cùng một level và mỗi nút cha có hai nút con.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">root = [5,3,6,5,2,5,7,1,8,null,null,6,8], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3319.K-th%20Largest%20Perfect%20Subtree%20Size%20in%20Binary%20Tree/images/tmpresl95rp-1.png" style="width: 400px; height: 173px;" /></p>

<p>Các nút gốc của những cây con nhị phân hoàn hảo được đánh dấu màu đen. Kích thước của chúng theo thứ tự không tăng là <code>[3, 3, 1, 1, 1, 1, 1, 1]</code>.<br />
Kích thước lớn thứ <code>2<sup>nd</sup></code> là 3.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">root = [1,2,3,4,5,6,7], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3319.K-th%20Largest%20Perfect%20Subtree%20Size%20in%20Binary%20Tree/images/tmp_s508x9e-1.png" style="width: 300px; height: 189px;" /></p>

<p>Kích thước của các cây con nhị phân hoàn hảo theo thứ tự không tăng là <code>[7, 3, 3, 1, 1, 1, 1]</code>. Kích thước của cây con nhị phân hoàn hảo lớn nhất là 7.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">root = [1,2,3,null,4], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3319.K-th%20Largest%20Perfect%20Subtree%20Size%20in%20Binary%20Tree/images/tmp74xnmpj4-1.png" style="width: 250px; height: 225px;" /></p>

<p>Kích thước của các cây con nhị phân hoàn hảo theo thứ tự không tăng là <code>[1, 1]</code>. Có ít hơn 3 cây con nhị phân hoàn hảo.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số lượng nút trong cây nằm trong khoảng <code>[1, 2000]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 2000</code></li>
	<li><code>1 &lt;= k &lt;= 1024</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Một cây con nhị phân hoàn hảo cần hai cây con hoàn hảo có cùng kích thước. Với $n \le 2000$, một lần duyệt post-order có thể thu thập mọi kích thước hợp lệ.
>
> Cây rỗng có kích thước $0$. Nếu hai cây con trả về cùng một kích thước không âm, cây hiện tại là cây hoàn hảo và ta ghi nhận $l+r+1$; nếu không, ta trả về $-1$.
>
> Sau khi sắp xếp các kích thước theo thứ tự giảm dần, ta trả về phần tử thứ $k$ hoặc $-1$ nếu có ít hơn $k$ phần tử.

<!-- thinking:end -->

Ta định nghĩa một hàm $\textit{dfs}$ để tính kích thước của cây con nhị phân hoàn hảo có gốc là nút hiện tại, đồng thời sử dụng một mảng $\textit{nums}$ để lưu kích thước của tất cả các cây con nhị phân hoàn hảo. Nếu cây con có gốc là nút hiện tại không phải là cây con nhị phân hoàn hảo, hàm trả về $-1$.

Quy trình thực hiện của hàm $\textit{dfs}$ như sau:

1. Nếu nút hiện tại là null, trả về $0$;
2. Đệ quy tính kích thước của các cây con nhị phân hoàn hảo bên trái và bên phải, lần lượt ký hiệu là $l$ và $r$;
3. Nếu kích thước của hai cây con trái và phải không bằng nhau, hoặc nếu kích thước của chúng nhỏ hơn $0$, trả về $-1$;
4. Tính kích thước của cây con nhị phân hoàn hảo có gốc là nút hiện tại $\textit{cnt} = l + r + 1$, rồi thêm $\textit{cnt}$ vào mảng $\textit{nums}$;
5. Trả về $\textit{cnt}$.

Ta gọi hàm $\textit{dfs}$ để tính kích thước của tất cả các cây con nhị phân hoàn hảo. Nếu độ dài của mảng $\textit{nums}$ nhỏ hơn $k$, trả về $-1$. Ngược lại, sắp xếp mảng $\textit{nums}$ theo thứ tự giảm dần rồi trả về kích thước của cây con nhị phân hoàn hảo lớn thứ $k$.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số lượng nút trong cây nhị phân.

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
    def kthLargestPerfectSubtree(self, root: Optional[TreeNode], k: int) -> int:
        def dfs(root: Optional[TreeNode]) -> int:
            if root is None:
                return 0
            l, r = dfs(root.left), dfs(root.right)
            if l < 0 or l != r:
                return -1
            cnt = l + r + 1
            nums.append(cnt)
            return cnt

        nums = []
        dfs(root)
        if len(nums) < k:
            return -1
        nums.sort(reverse=True)
        return nums[k - 1]
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

    public int kthLargestPerfectSubtree(TreeNode root, int k) {
        dfs(root);
        if (nums.size() < k) {
            return -1;
        }
        nums.sort(Comparator.reverseOrder());
        return nums.get(k - 1);
    }

    private int dfs(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int l = dfs(root.left);
        int r = dfs(root.right);
        if (l < 0 || l != r) {
            return -1;
        }
        int cnt = l + r + 1;
        nums.add(cnt);
        return cnt;
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
    int kthLargestPerfectSubtree(TreeNode* root, int k) {
        vector<int> nums;
        auto dfs = [&](this auto&& dfs, TreeNode* root) -> int {
            if (!root) {
                return 0;
            }
            int l = dfs(root->left);
            int r = dfs(root->right);
            if (l < 0 || l != r) {
                return -1;
            }
            int cnt = l + r + 1;
            nums.push_back(cnt);
            return cnt;
        };
        dfs(root);
        if (nums.size() < k) {
            return -1;
        }
        ranges::sort(nums, greater<int>());
        return nums[k - 1];
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
func kthLargestPerfectSubtree(root *TreeNode, k int) int {
	nums := []int{}
	var dfs func(*TreeNode) int
	dfs = func(root *TreeNode) int {
		if root == nil {
			return 0
		}
		l, r := dfs(root.Left), dfs(root.Right)
		if l < 0 || l != r {
			return -1
		}
		cnt := l + r + 1
		nums = append(nums, cnt)
		return cnt
	}
	dfs(root)
	if len(nums) < k {
		return -1
	}
	sort.Sort(sort.Reverse(sort.IntSlice(nums)))
	return nums[k-1]
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

function kthLargestPerfectSubtree(root: TreeNode | null, k: number): number {
    const nums: number[] = [];
    const dfs = (root: TreeNode | null): number => {
        if (!root) {
            return 0;
        }
        const l = dfs(root.left);
        const r = dfs(root.right);
        if (l < 0 || l !== r) {
            return -1;
        }
        const cnt = l + r + 1;
        nums.push(cnt);
        return cnt;
    };
    dfs(root);
    if (nums.length < k) {
        return -1;
    }
    return nums.sort((a, b) => b - a)[k - 1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
