---
comments: true
difficulty: Easy
tags:
    - Tree
    - Depth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [872. Leaf-Similar Trees](https://leetcode.com/problems/leaf-similar-trees)

[中文文档](/solution/0800-0899/0872.Leaf-Similar%20Trees/README.md)

## Mô tả

<!-- description:start -->

<p>Xét các lá của cây nhị phân theo thứ tự từ trái sang phải; các giá trị của chúng tạo thành một <strong>dãy giá trị lá</strong><em>.</em></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0872.Leaf-Similar%20Trees/images/tree.png" style="width: 400px; height: 336px;" /></p>

<p>Ví dụ, trong cây được cho ở trên, dãy giá trị lá là <code>(6, 7, 4, 9, 8)</code>.</p>

<p>Hai cây nhị phân được xem là <em>tương đồng theo lá</em>&nbsp;nếu dãy giá trị lá của chúng giống nhau.</p>

<p>Hãy trả về <code>true</code> khi và chỉ khi hai cây đã cho, có node gốc là <code>root1</code> và <code>root2</code>, tương đồng theo lá.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0872.Leaf-Similar%20Trees/images/leaf-similar-1.jpg" style="width: 600px; height: 237px;" />
<pre>
<strong>Đầu vào:</strong> root1 = [3,5,1,6,2,9,8,null,null,7,4], root2 = [3,5,1,6,7,4,2,null,null,null,null,null,null,9,8]
<strong>Đầu ra:</strong> true
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0872.Leaf-Similar%20Trees/images/leaf-similar-2.jpg" style="width: 300px; height: 110px;" />
<pre>
<strong>Đầu vào:</strong> root1 = [1,2,3], root2 = [1,3,2]
<strong>Đầu ra:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong mỗi cây nằm trong khoảng <code>[1, 200]</code>.</li>
	<li>Giá trị node của cả hai cây nằm trong khoảng <code>[0, 200]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> So sánh dãy lá theo thứ tự trái sang phải của hai cây. Cả hai cây đều nhỏ, nên chỉ cần chạy DFS một lần cho mỗi cây.
>
> Node không có con là lá. Thu thập hai dãy rồi kiểm tra chúng có bằng nhau không.

<!-- thinking:end -->

Ta có thể dùng tìm kiếm theo chiều sâu (DFS) để duyệt các node lá của hai cây, lần lượt lưu giá trị lá vào hai list $l_1$ và $l_2$. Cuối cùng, so sánh hai list để kiểm tra chúng có bằng nhau không.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node trong cây.

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
    def leafSimilar(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> bool:
        def dfs(root: Optional[TreeNode], nums: List[int]) -> None:
            if root.left == root.right:
                nums.append(root.val)
                return
            if root.left:
                dfs(root.left, nums)
            if root.right:
                dfs(root.right, nums)

        l1, l2 = [], []
        dfs(root1, l1)
        dfs(root2, l2)
        return l1 == l2
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
    public boolean leafSimilar(TreeNode root1, TreeNode root2) {
        List<Integer> l1 = new ArrayList<>();
        List<Integer> l2 = new ArrayList<>();
        dfs(root1, l1);
        dfs(root2, l2);
        return l1.equals(l2);
    }

    private void dfs(TreeNode root, List<Integer> nums) {
        if (root.left == root.right) {
            nums.add(root.val);
            return;
        }
        if (root.left != null) {
            dfs(root.left, nums);
        }
        if (root.right != null) {
            dfs(root.right, nums);
        }
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
    bool leafSimilar(TreeNode* root1, TreeNode* root2) {
        vector<int> l1, l2;
        dfs(root1, l1);
        dfs(root2, l2);
        return l1 == l2;
    }

    void dfs(TreeNode* root, vector<int>& nums) {
        if (root->left == root->right) {
            nums.push_back(root->val);
            return;
        }
        if (root->left) {
            dfs(root->left, nums);
        }
        if (root->right) {
            dfs(root->right, nums);
        }
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
func leafSimilar(root1 *TreeNode, root2 *TreeNode) bool {
	l1, l2 := []int{}, []int{}
	var dfs func(*TreeNode, *[]int)
	dfs = func(root *TreeNode, nums *[]int) {
		if root.Left == root.Right {
			*nums = append(*nums, root.Val)
			return
		}
		if root.Left != nil {
			dfs(root.Left, nums)
		}
		if root.Right != nil {
			dfs(root.Right, nums)
		}
	}
	dfs(root1, &l1)
	dfs(root2, &l2)
	return reflect.DeepEqual(l1, l2)
}
```

#### Rust

```rust
// Definition for a binary tree node.
// #[derive(Debug, PartialEq, Eq)]
// pub struct TreeNode {
//   pub val: i32,
//   pub left: Option<Rc<RefCell<TreeNode>>>,
//   pub right: Option<Rc<RefCell<TreeNode>>>,
// }
//
// impl TreeNode {
//   #[inline]
//   pub fn new(val: i32) -> Self {
//     TreeNode {
//       val,
//       left: None,
//       right: None
//     }
//   }
// }
use std::cell::RefCell;
use std::rc::Rc;
impl Solution {
    pub fn leaf_similar(
        root1: Option<Rc<RefCell<TreeNode>>>,
        root2: Option<Rc<RefCell<TreeNode>>>,
    ) -> bool {
        let mut l1 = Vec::new();
        let mut l2 = Vec::new();
        Self::dfs(&root1, &mut l1);
        Self::dfs(&root2, &mut l2);
        l1 == l2
    }

    fn dfs(node: &Option<Rc<RefCell<TreeNode>>>, nums: &mut Vec<i32>) {
        if let Some(n) = node {
            let n = n.borrow();
            if n.left.is_none() && n.right.is_none() {
                nums.push(n.val);
                return;
            }
            if n.left.is_some() {
                Self::dfs(&n.left, nums);
            }
            if n.right.is_some() {
                Self::dfs(&n.right, nums);
            }
        }
    }
}
```

#### JavaScript

```js
var leafSimilar = function (root1, root2) {
    const l1 = [];
    const l2 = [];
    const dfs = (root, nums) => {
        if (root.left === root.right) {
            nums.push(root.val);
            return;
        }
        root.left && dfs(root.left, nums);
        root.right && dfs(root.right, nums);
    };
    dfs(root1, l1);
    dfs(root2, l2);
    return l1.join(',') === l2.join(',');
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
