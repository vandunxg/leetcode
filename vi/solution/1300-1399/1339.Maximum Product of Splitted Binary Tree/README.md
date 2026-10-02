---
comments: true
difficulty: Medium
rating: 1674
source: Weekly Contest 174 Q3
tags:
    - Tree
    - Depth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [1339. Maximum Product of Splitted Binary Tree](https://leetcode.com/problems/maximum-product-of-splitted-binary-tree)

[中文文档](/solution/1300-1399/1339.Maximum%20Product%20of%20Splitted%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một binary tree. Hãy xóa một cạnh để tách cây thành hai cây con sao cho tích tổng các node của hai cây con là lớn nhất.</p>

<p>Trả về <em>tích lớn nhất giữa tổng của hai cây con</em>. Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p><strong>Lưu ý</strong> rằng cần tối đa hóa đáp án trước khi lấy modulo, không phải sau đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1339.Maximum%20Product%20of%20Splitted%20Binary%20Tree/images/sample_1_1699.png" style="width: 500px; height: 167px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,2,3,4,5,6]
<strong>Đầu ra:</strong> 110
<strong>Giải thích:</strong> Xóa cạnh màu đỏ để thu được hai binary tree có tổng lần lượt là 11 và 10. Tích của chúng là 110 (11*10).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1339.Maximum%20Product%20of%20Splitted%20Binary%20Tree/images/sample_2_1699.png" style="width: 500px; height: 211px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,null,2,3,4,null,null,5,6]
<strong>Đầu ra:</strong> 90
<strong>Giải thích:</strong> Xóa cạnh màu đỏ để thu được hai binary tree có tổng lần lượt là 15 và 6. Tích của chúng là 90 (15*6).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[2, 5 * 10<sup>4</sup>]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai lượt DFS

<!-- thinking:start -->

> **Tư duy**
>
> Xóa một cạnh sẽ tách cây; ta cần tìm tích lớn nhất giữa tổng của hai cây con. Tính lại tổng hai phía cho từng cạnh sẽ mất thời gian bậc hai. Khi tổng toàn cây $s$ đã cố định, một cạnh tạo ra tích $t \times (s-t)$ với tổng cây con là $t$. Một lượt DFS tính $s$, lượt DFS thứ hai duyệt từng giá trị $t$ và cập nhật tích lớn nhất.

<!-- thinking:end -->

Ta có thể giải bài toán bằng hai lượt DFS.

Ở lượt duyệt thứ nhất, ta dùng hàm $\text{sum}(\text{root})$ để tính đệ quy tổng giá trị của tất cả node trong cây, ký hiệu là $s$.

Ở lượt duyệt thứ hai, ta dùng hàm $\text{dfs}(\text{root})$ để duyệt đệ quy từng node và tính tổng các node trong cây con có gốc tại node hiện tại, ký hiệu là $t$. Khi xóa cạnh nối node hiện tại với cha của nó, tổng của hai cây con lần lượt là $t$ và $s - t$, nên tích là $t \times (s - t)$. Ta duyệt tất cả node để tìm tích lớn nhất làm đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, với $n$ là số node trong binary tree.

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
    def maxProduct(self, root: Optional[TreeNode]) -> int:
        def sum(root: Optional[TreeNode]) -> int:
            if root is None:
                return 0
            return root.val + sum(root.left) + sum(root.right)

        def dfs(root: Optional[TreeNode]) -> int:
            if root is None:
                return 0
            t = root.val + dfs(root.left) + dfs(root.right)
            nonlocal ans, s
            if t < s:
                ans = max(ans, t * (s - t))
            return t

        mod = 10**9 + 7
        s = sum(root)
        ans = 0
        dfs(root)
        return ans % mod
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
    private long ans;
    private long s;

    public int maxProduct(TreeNode root) {
        final int mod = (int) 1e9 + 7;
        s = sum(root);
        dfs(root);
        return (int) (ans % mod);
    }

    private long dfs(TreeNode root) {
        if (root == null) {
            return 0;
        }
        long t = root.val + dfs(root.left) + dfs(root.right);
        if (t < s) {
            ans = Math.max(ans, t * (s - t));
        }
        return t;
    }

    private long sum(TreeNode root) {
        if (root == null) {
            return 0;
        }
        return root.val + sum(root.left) + sum(root.right);
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
    int maxProduct(TreeNode* root) {
        using ll = long long;
        ll ans = 0;
        const int mod = 1e9 + 7;

        auto sum = [&](this auto&& sum, TreeNode* root) -> ll {
            if (!root) {
                return 0;
            }
            return root->val + sum(root->left) + sum(root->right);
        };

        ll s = sum(root);

        auto dfs = [&](this auto&& dfs, TreeNode* root) -> ll {
            if (!root) {
                return 0;
            }
            ll t = root->val + dfs(root->left) + dfs(root->right);
            if (t < s) {
                ans = max(ans, t * (s - t));
            }
            return t;
        };

        dfs(root);
        return ans % mod;
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
func maxProduct(root *TreeNode) (ans int) {
	const mod = 1e9 + 7
	var sum func(*TreeNode) int
	sum = func(root *TreeNode) int {
		if root == nil {
			return 0
		}
		return root.Val + sum(root.Left) + sum(root.Right)
	}
	s := sum(root)
	var dfs func(*TreeNode) int
	dfs = func(root *TreeNode) int {
		if root == nil {
			return 0
		}
		t := root.Val + dfs(root.Left) + dfs(root.Right)
		if t < s {
			ans = max(ans, t*(s-t))
		}
		return t
	}
	dfs(root)
	ans %= mod
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

function maxProduct(root: TreeNode | null): number {
    const sum = (root: TreeNode | null): number => {
        if (!root) {
            return 0;
        }
        return root.val + sum(root.left) + sum(root.right);
    };
    const s = sum(root);
    let ans = 0;
    const mod = 1e9 + 7;
    const dfs = (root: TreeNode | null): number => {
        if (!root) {
            return 0;
        }
        const t = root.val + dfs(root.left) + dfs(root.right);
        if (t < s) {
            ans = Math.max(ans, t * (s - t));
        }
        return t;
    };
    dfs(root);
    return ans % mod;
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
    pub fn max_product(root: Option<Rc<RefCell<TreeNode>>>) -> i32 {
        const MOD: i64 = 1_000_000_007;
        let mut ans: i64 = 0;
        let s = Self::sum(&root);
        Self::dfs(&root, s, &mut ans);
        (ans % MOD) as i32
    }

    fn dfs(root: &Option<Rc<RefCell<TreeNode>>>, s: i64, ans: &mut i64) -> i64 {
        if root.is_none() {
            return 0;
        }
        let node = root.as_ref().unwrap().borrow();
        let t = node.val as i64 + Self::dfs(&node.left, s, ans) + Self::dfs(&node.right, s, ans);
        if t < s {
            *ans = (*ans).max(t * (s - t));
        }
        t
    }

    fn sum(root: &Option<Rc<RefCell<TreeNode>>>) -> i64 {
        if root.is_none() {
            return 0;
        }
        let node = root.as_ref().unwrap().borrow();
        node.val as i64 + Self::sum(&node.left) + Self::sum(&node.right)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
