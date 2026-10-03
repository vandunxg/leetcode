---
comments: true
difficulty: Medium
rating: 1596
source: Weekly Contest 320 Q2
tags:
    - Tree
    - Depth-First Search
    - Binary Search Tree
    - Array
    - Binary Search
    - Binary Tree
---

<!-- problem:start -->

# [2476. Closest Nodes Queries in a Binary Search Tree](https://leetcode.com/problems/closest-nodes-queries-in-a-binary-search-tree)

[中文文档](/solution/2400-2499/2476.Closest%20Nodes%20Queries%20in%20a%20Binary%20Search%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho <code>root</code> của một <strong>cây tìm kiếm nhị phân</strong> và một mảng <code>queries</code> có kích thước <code>n</code>, gồm các số nguyên dương.</p>

<p>Hãy tìm một mảng <strong>2 chiều</strong> <code>answer</code> có kích thước <code>n</code> sao cho <code>answer[i] = [min<sub>i</sub>, max<sub>i</sub>]</code>:</p>

<ul>
	<li><code>min<sub>i</sub></code> là giá trị <strong>lớn nhất</strong> trong cây nhỏ hơn hoặc bằng <code>queries[i]</code>. Nếu không tồn tại giá trị như vậy, hãy thay bằng <code>-1</code>.</li>
	<li><code>max<sub>i</sub></code> là giá trị <strong>nhỏ nhất</strong> trong cây lớn hơn hoặc bằng <code>queries[i]</code>. Nếu không tồn tại giá trị như vậy, hãy thay bằng <code>-1</code>.</li>
</ul>

<p>Trả về <em>mảng</em> <code>answer</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2476.Closest%20Nodes%20Queries%20in%20a%20Binary%20Search%20Tree/images/bstreeedrawioo.png" style="width: 261px; height: 281px;" />
<pre>
<strong>Đầu vào:</strong> root = [6,2,13,1,4,9,15,null,null,null,null,null,null,14], queries = [2,5,16]
<strong>Đầu ra:</strong> [[2,2],[4,6],[15,-1]]
<strong>Giải thích:</strong> Ta trả lời các truy vấn như sau:
- Số lớn nhất nhỏ hơn hoặc bằng 2 trong cây là 2, còn số nhỏ nhất lớn hơn hoặc bằng 2 cũng là 2. Vì vậy, câu trả lời cho truy vấn đầu tiên là [2,2].
- Số lớn nhất nhỏ hơn hoặc bằng 5 trong cây là 4, còn số nhỏ nhất lớn hơn hoặc bằng 5 là 6. Vì vậy, câu trả lời cho truy vấn thứ hai là [4,6].
- Số lớn nhất nhỏ hơn hoặc bằng 16 trong cây là 15, còn không tồn tại số nhỏ nhất lớn hơn hoặc bằng 16. Vì vậy, câu trả lời cho truy vấn thứ ba là [15,-1].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2476.Closest%20Nodes%20Queries%20in%20a%20Binary%20Search%20Tree/images/bstttreee.png" style="width: 101px; height: 121px;" />
<pre>
<strong>Đầu vào:</strong> root = [4,null,9], queries = [3]
<strong>Đầu ra:</strong> [[-1,4]]
<strong>Giải thích:</strong> Không tồn tại số lớn nhất nhỏ hơn hoặc bằng 3 trong cây, còn số nhỏ nhất lớn hơn hoặc bằng 3 là 4. Vì vậy, câu trả lời cho truy vấn là [-1,4].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[2, 10<sup>5</sup>]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 10<sup>6</sup></code></li>
	<li><code>n == queries.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= queries[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt inorder + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Duyệt inorder trên BST cho một mảng đã sắp xếp. Với $n,q\le 10^5$, ta tìm kiếm nhị phân cho từng truy vấn để tìm giá trị lớn nhất $\le x$ và nhỏ nhất $\ge x$, dùng $-1$ nếu không tồn tại.

<!-- thinking:end -->

Vì đề bài cho một cây tìm kiếm nhị phân, ta có thể thu được một mảng đã sắp xếp bằng cách duyệt inorder. Sau đó, với mỗi truy vấn, ta dùng tìm kiếm nhị phân để tìm giá trị lớn nhất nhỏ hơn hoặc bằng giá trị truy vấn và giá trị nhỏ nhất lớn hơn hoặc bằng giá trị truy vấn.

Độ phức tạp thời gian là $O(n + m \times \log n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ và $m$ lần lượt là số node trong cây tìm kiếm nhị phân và số truy vấn.

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
    def closestNodes(
        self, root: Optional[TreeNode], queries: List[int]
    ) -> List[List[int]]:
        def dfs(root: Optional[TreeNode]):
            if root is None:
                return
            dfs(root.left)
            nums.append(root.val)
            dfs(root.right)

        nums = []
        dfs(root)
        ans = []
        for x in queries:
            i = bisect_left(nums, x + 1) - 1
            j = bisect_left(nums, x)
            mi = nums[i] if 0 <= i < len(nums) else -1
            mx = nums[j] if 0 <= j < len(nums) else -1
            ans.append([mi, mx])
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
    private List<Integer> nums = new ArrayList<>();

    public List<List<Integer>> closestNodes(TreeNode root, List<Integer> queries) {
        dfs(root);
        List<List<Integer>> ans = new ArrayList<>();
        for (int x : queries) {
            int i = Collections.binarySearch(nums, x + 1);
            int j = Collections.binarySearch(nums, x);
            i = i < 0 ? -i - 2 : i - 1;
            j = j < 0 ? -j - 1 : j;
            int mi = i >= 0 && i < nums.size() ? nums.get(i) : -1;
            int mx = j >= 0 && j < nums.size() ? nums.get(j) : -1;
            ans.add(List.of(mi, mx));
        }
        return ans;
    }

    private void dfs(TreeNode root) {
        if (root == null) {
            return;
        }
        dfs(root.left);
        nums.add(root.val);
        dfs(root.right);
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
    vector<vector<int>> closestNodes(TreeNode* root, vector<int>& queries) {
        vector<int> nums;
        function<void(TreeNode*)> dfs = [&](TreeNode* root) {
            if (!root) {
                return;
            }
            dfs(root->left);
            nums.push_back(root->val);
            dfs(root->right);
        };
        dfs(root);
        vector<vector<int>> ans;
        int n = nums.size();
        for (int& x : queries) {
            int i = lower_bound(nums.begin(), nums.end(), x + 1) - nums.begin() - 1;
            int j = lower_bound(nums.begin(), nums.end(), x) - nums.begin();
            int mi = i >= 0 && i < n ? nums[i] : -1;
            int mx = j >= 0 && j < n ? nums[j] : -1;
            ans.push_back({mi, mx});
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
func closestNodes(root *TreeNode, queries []int) (ans [][]int) {
	nums := []int{}
	var dfs func(*TreeNode)
	dfs = func(root *TreeNode) {
		if root == nil {
			return
		}
		dfs(root.Left)
		nums = append(nums, root.Val)
		dfs(root.Right)
	}
	dfs(root)
	for _, x := range queries {
		i := sort.SearchInts(nums, x+1) - 1
		j := sort.SearchInts(nums, x)
		mi, mx := -1, -1
		if i >= 0 {
			mi = nums[i]
		}
		if j < len(nums) {
			mx = nums[j]
		}
		ans = append(ans, []int{mi, mx})
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

function closestNodes(root: TreeNode | null, queries: number[]): number[][] {
    const nums: number[] = [];
    const dfs = (root: TreeNode | null) => {
        if (!root) {
            return;
        }
        dfs(root.left);
        nums.push(root.val);
        dfs(root.right);
    };
    const search = (x: number): number => {
        let [l, r] = [0, nums.length];
        while (l < r) {
            const mid = (l + r) >> 1;
            if (nums[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };
    dfs(root);
    const ans: number[][] = [];
    for (const x of queries) {
        const i = search(x + 1) - 1;
        const j = search(x);
        const mi = i >= 0 ? nums[i] : -1;
        const mx = j < nums.length ? nums[j] : -1;
        ans.push([mi, mx]);
    }
    return ans;
}
```

#### C#

```cs
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left;
 *     public TreeNode right;
 *     public TreeNode(int val=0, TreeNode left=null, TreeNode right=null) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
public class Solution {
    private List<int> nums = new List<int>();

    public IList<IList<int>> ClosestNodes(TreeNode root, IList<int> queries) {
        Dfs(root);
        List<IList<int>> ans = new List<IList<int>>();
        foreach (int x in queries) {
            int i = nums.BinarySearch(x + 1);
            int j = nums.BinarySearch(x);
            i = i < 0 ? -i - 2 : i - 1;
            j = j < 0 ? -j - 1 : j;
            int mi = i >= 0 && i < nums.Count ? nums[i] : -1;
            int mx = j >= 0 && j < nums.Count ? nums[j] : -1;
            ans.Add(new List<int> {mi, mx});
        }
        return ans;
    }

    private void Dfs(TreeNode root) {
        if (root == null) {
            return;
        }
        Dfs(root.left);
        nums.Add(root.val);
        Dfs(root.right);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
