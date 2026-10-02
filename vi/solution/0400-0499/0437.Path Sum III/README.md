---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [437. Path Sum III](https://leetcode.com/problems/path-sum-iii)

[中文文档](/solution/0400-0499/0437.Path%20Sum%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của cây nhị phân và số nguyên <code>targetSum</code>. Hãy trả về <em>số đường đi có tổng các giá trị trên đường đi bằng</em> <code>targetSum</code>.</p>

<p>Đường đi không nhất thiết phải bắt đầu hoặc kết thúc ở root hay node lá, nhưng phải đi theo hướng xuống (tức là chỉ đi từ node cha đến node con).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0437.Path%20Sum%20III/images/pathsum3-1-tree.jpg" style="width: 450px; height: 386px;" />
<pre>
<strong>Đầu vào:</strong> root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Các đường đi có tổng bằng 8 được minh họa trong hình.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
<strong>Đầu ra:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[0, 1000]</code>.</li>
	<li><code>-10<sup>9</sup> &lt;= Node.val &lt;= 10<sup>9</sup></code></li>
	<li><code>-1000 &lt;= targetSum &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Prefix Sum + Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Đường đi phải hướng xuống. Thử từng node tổ tiên rồi duyệt xuống sẽ tốn $O(n^2)$. Tổng trên một đường đi bằng prefix sum hiện tại trừ prefix sum tại một node tổ tiên nào đó.
>
> Tích lũy prefix sum $s$ khi duyệt preorder và lưu tần suất các prefix sum trên đường từ root đến node hiện tại. $\textit{cnt}[s-\textit{targetSum}]$ đếm số đường đi hợp lệ kết thúc tại đây. Khi quay lui, hoàn tác trạng thái để nhánh khác không bị ảnh hưởng.
>
> $\textit{cnt}[0]=1$ tính cả đường đi bắt đầu từ root. Tra cứu trước khi thêm để không tính node hiện tại hai lần.

<!-- thinking:end -->

Ta có thể dùng prefix sum để duyệt đệ quy cây nhị phân, đồng thời dùng hash table $\textit{cnt}$ đếm số lần xuất hiện của từng prefix sum trên đường từ root đến node hiện tại.

Định nghĩa hàm đệ quy $\textit{dfs(node, s)}$, trong đó $\textit{node}$ là node đang duyệt và $s$ là prefix sum trên đường từ root đến node đó. Hàm trả về số đường đi kết thúc tại $\textit{node}$ hoặc một node trong cây con của nó, có tổng bằng $\textit{targetSum}$. Đáp án cuối cùng là $\textit{dfs(root, 0)}$.

Quy trình đệ quy của $\textit{dfs(node, s)}$ như sau:

- Nếu node hiện tại $\textit{node}$ là null, trả về $0$.
- Tính prefix sum $s$ trên đường từ root đến node hiện tại.
- Dùng $\textit{cnt}[s - \textit{targetSum}]$ để biểu thị số đường đi kết thúc tại node hiện tại có tổng bằng $\textit{targetSum}$. Giá trị này là số lần prefix sum $s - \textit{targetSum}$ xuất hiện trong $\textit{cnt}$.
- Tăng số lần xuất hiện của prefix sum $s$ lên $1$, tức là $\textit{cnt}[s] = \textit{cnt}[s] + 1$.
- Gọi đệ quy để duyệt node con trái và phải bằng $\textit{dfs(node.left, s)}$ và $\textit{dfs(node.right, s)}$, rồi cộng các giá trị trả về.
- Sau khi tính xong kết quả, giảm số lần xuất hiện của prefix sum $s$ đi $1$, tức là $\textit{cnt}[s] = \textit{cnt}[s] - 1$.
- Cuối cùng, trả về kết quả.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node trong cây nhị phân.

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
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> int:
        def dfs(node, s):
            if node is None:
                return 0
            s += node.val
            ans = cnt[s - targetSum]
            cnt[s] += 1
            ans += dfs(node.left, s)
            ans += dfs(node.right, s)
            cnt[s] -= 1
            return ans

        cnt = Counter({0: 1})
        return dfs(root, 0)
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
    private Map<Long, Integer> cnt = new HashMap<>();
    private int targetSum;

    public int pathSum(TreeNode root, int targetSum) {
        cnt.put(0L, 1);
        this.targetSum = targetSum;
        return dfs(root, 0);
    }

    private int dfs(TreeNode node, long s) {
        if (node == null) {
            return 0;
        }
        s += node.val;
        int ans = cnt.getOrDefault(s - targetSum, 0);
        cnt.merge(s, 1, Integer::sum);
        ans += dfs(node.left, s);
        ans += dfs(node.right, s);
        cnt.merge(s, -1, Integer::sum);
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
    int pathSum(TreeNode* root, int targetSum) {
        unordered_map<long long, int> cnt;
        cnt[0] = 1;
        auto dfs = [&](this auto&& dfs, TreeNode* node, long long s) -> int {
            if (!node) {
                return 0;
            }
            s += node->val;
            int ans = cnt[s - targetSum];
            ++cnt[s];
            ans += dfs(node->left, s) + dfs(node->right, s);
            --cnt[s];
            return ans;
        };
        return dfs(root, 0);
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
func pathSum(root *TreeNode, targetSum int) int {
	cnt := map[int]int{0: 1}
	var dfs func(*TreeNode, int) int
	dfs = func(node *TreeNode, s int) int {
		if node == nil {
			return 0
		}
		s += node.Val
		ans := cnt[s-targetSum]
		cnt[s]++
		ans += dfs(node.Left, s) + dfs(node.Right, s)
		cnt[s]--
		return ans
	}
	return dfs(root, 0)
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

function pathSum(root: TreeNode | null, targetSum: number): number {
    const cnt: Map<number, number> = new Map();
    const dfs = (node: TreeNode | null, s: number): number => {
        if (!node) {
            return 0;
        }
        s += node.val;
        let ans = cnt.get(s - targetSum) ?? 0;
        cnt.set(s, (cnt.get(s) ?? 0) + 1);
        ans += dfs(node.left, s);
        ans += dfs(node.right, s);
        cnt.set(s, (cnt.get(s) ?? 0) - 1);
        return ans;
    };
    cnt.set(0, 1);
    return dfs(root, 0);
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
use std::rc::Rc;
use std::cell::RefCell;
use std::collections::HashMap;

impl Solution {
    pub fn path_sum(root: Option<Rc<RefCell<TreeNode>>>, target_sum: i32) -> i32 {
        let mut cnt = HashMap::new();
        cnt.insert(0, 1);

        fn dfs(node: Option<Rc<RefCell<TreeNode>>>, s: i64, target: i64, cnt: &mut HashMap<i64, i32>) -> i32 {
            if let Some(n) = node {
                let n = n.borrow();
                let s = s + n.val as i64;
                let ans = cnt.get(&(s - target)).copied().unwrap_or(0);
                *cnt.entry(s).or_insert(0) += 1;
                let ans = ans + dfs(n.left.clone(), s, target, cnt) + dfs(n.right.clone(), s, target, cnt);
                *cnt.get_mut(&s).unwrap() -= 1;
                ans
            } else {
                0
            }
        }

        dfs(root, 0, target_sum as i64, &mut cnt)
    }
}
```

#### JavaScript

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @param {number} targetSum
 * @return {number}
 */
var pathSum = function (root, targetSum) {
    const cnt = new Map();
    const dfs = (node, s) => {
        if (!node) {
            return 0;
        }
        s += node.val;
        let ans = cnt.get(s - targetSum) || 0;
        cnt.set(s, (cnt.get(s) || 0) + 1);
        ans += dfs(node.left, s);
        ans += dfs(node.right, s);
        cnt.set(s, cnt.get(s) - 1);
        return ans;
    };
    cnt.set(0, 1);
    return dfs(root, 0);
};
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
    public int PathSum(TreeNode root, int targetSum) {
        Dictionary<long, int> cnt = new Dictionary<long, int>();

        int Dfs(TreeNode node, long s) {
            if (node == null) {
                return 0;
            }
            s += node.val;
            int ans = cnt.GetValueOrDefault(s - targetSum, 0);
            cnt[s] = cnt.GetValueOrDefault(s, 0) + 1;
            ans += Dfs(node.left, s);
            ans += Dfs(node.right, s);
            cnt[s]--;
            return ans;
        }

        cnt[0] = 1;
        return Dfs(root, 0);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
