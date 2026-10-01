---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [04.12. Paths with Sum](https://leetcode.cn/problems/paths-with-sum-lcci)

[中文文档](/lcci/04.12.Paths%20with%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một cây nhị phân, trong đó mỗi node chứa một giá trị nguyên (có thể dương hoặc âm). Hãy thiết kế một thuật toán để đếm số đường đi có tổng bằng một giá trị cho trước. Đường đi không nhất thiết phải bắt đầu hoặc kết thúc ở root hay leaf, nhưng phải đi xuống (chỉ đi từ node cha đến node con).</p>

<p><strong>Ví dụ:</strong><br />

Cho cây sau và &nbsp;<code>sum = 22,</code></p>

<pre>

              5

             / \

            4   8

           /   / \

          11  13  4

         /  \    / \

        7    2  5   1

</pre>

<p>Đầu ra:</p>

<pre>

3

<strong>Giải thích: </strong>Các đường đi có tổng bằng 22 là: [5,4,11,2], [5,8,4,5], [4,11,7]</pre>

<p>Lưu ý:</p>

<ul>
	<li><code>node number &lt;= 10000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Prefix Sum + Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Một đường đi có thể bắt đầu và kết thúc ở bất kỳ cặp ancestor–descendant nào. Nếu bắt đầu lại việc tìm đường đi xuống từ mỗi node, ta sẽ đếm lại các cạnh và có thể đạt độ phức tạp bậc hai.
>
> Tổng của một đường đi là hiệu của hai prefix sum. Với prefix hiện tại $s$, các ancestor hữu ích là những node có prefix $s-sum$.
>
> $cnt$ lưu tần suất prefix sum từ root, với $cnt[0]=1$ cho prefix rỗng. Truy vấn $cnt[s-sum]$, tăng $s$, đệ quy, sau đó giảm để các nhánh sibling không ảnh hưởng lẫn nhau.

<!-- thinking:end -->

Ta có thể dùng ý tưởng prefix sum để duyệt đệ quy cây nhị phân, đồng thời dùng một hash table $cnt$ để đếm số lần xuất hiện của mỗi prefix sum trên đường đi từ root đến node hiện tại.

Ta xây dựng hàm đệ quy $dfs(node, s)$, trong đó node hiện tại đang được duyệt là $node$, còn prefix sum trên đường đi từ root đến node hiện tại là $s$. Giá trị trả về của hàm là số đường đi có tổng bằng $sum$ và kết thúc tại node $node$ hoặc một node trong subtree của nó. Vì vậy, đáp án là $dfs(root, 0)$.

Quy trình đệ quy của hàm $dfs(node, s)$ như sau:

- Nếu node hiện tại $node$ là null, trả về $0$.
- Tính prefix sum $s$ trên đường đi từ root đến node hiện tại.
- Dùng $cnt[s - sum]$ để biểu diễn số đường đi có tổng bằng $sum$ và kết thúc tại node hiện tại, trong đó $cnt[s - sum]$ là số lần prefix sum bằng $s - sum$ xuất hiện trong $cnt$.
- Tăng số lần xuất hiện của prefix sum $s$ lên $1$, tức là $cnt[s] = cnt[s] + 1$.
- Duyệt đệ quy node con trái và phải của node hiện tại, tức là gọi các hàm $dfs(node.left, s)$ và $dfs(node.right, s)$, rồi cộng các giá trị trả về.
- Sau khi tính xong giá trị trả về, giảm số lần xuất hiện của prefix sum $s$ của node hiện tại đi $1$, tức là thực hiện $cnt[s] = cnt[s] - 1$.
- Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số node trong cây nhị phân.

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
    def pathSum(self, root: Optional[TreeNode], sum: int) -> int:
        def dfs(root: Optional[TreeNode], s: int) -> int:
            if root is None:
                return 0
            s += root.val
            ans = cnt[s - sum]
            cnt[s] += 1
            ans += dfs(root.left, s)
            ans += dfs(root.right, s)
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
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    private Map<Long, Integer> cnt = new HashMap<>();
    private int target;

    public int pathSum(TreeNode root, int sum) {
        cnt.put(0L, 1);
        target = sum;
        return dfs(root, 0);
    }

    private int dfs(TreeNode root, long s) {
        if (root == null) {
            return 0;
        }
        s += root.val;
        int ans = cnt.getOrDefault(s - target, 0);
        cnt.merge(s, 1, Integer::sum);
        ans += dfs(root.left, s);
        ans += dfs(root.right, s);
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
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int pathSum(TreeNode* root, int sum) {
        unordered_map<long long, int> cnt{{0, 1}};
        auto dfs = [&](this auto&& dfs, TreeNode* root, long long s) -> int {
            if (!root) {
                return 0;
            }
            s += root->val;
            int ans = cnt[s - sum];
            ++cnt[s];
            ans += dfs(root->left, s);
            ans += dfs(root->right, s);
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
func pathSum(root *TreeNode, sum int) int {
	cnt := map[int]int{0: 1}
	var dfs func(*TreeNode, int) int
	dfs = func(root *TreeNode, s int) int {
		if root == nil {
			return 0
		}
		s += root.Val
		ans := cnt[s-sum]
		cnt[s]++
		ans += dfs(root.Left, s)
		ans += dfs(root.Right, s)
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

function pathSum(root: TreeNode | null, sum: number): number {
    const cnt: Map<number, number> = new Map();
    cnt.set(0, 1);
    const dfs = (root: TreeNode | null, s: number): number => {
        if (!root) {
            return 0;
        }
        s += root.val;
        let ans = cnt.get(s - sum) ?? 0;
        cnt.set(s, (cnt.get(s) ?? 0) + 1);
        ans += dfs(root.left, s);
        ans += dfs(root.right, s);
        cnt.set(s, (cnt.get(s) ?? 0) - 1);
        return ans;
    };
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
use std::cell::RefCell;
use std::collections::HashMap;
use std::rc::Rc;
impl Solution {
    pub fn path_sum(root: Option<Rc<RefCell<TreeNode>>>, sum: i32) -> i32 {
        let mut cnt = HashMap::new();
        cnt.insert(0, 1);
        return Self::dfs(root, sum, 0, &mut cnt);
    }

    fn dfs(
        root: Option<Rc<RefCell<TreeNode>>>,
        sum: i32,
        s: i32,
        cnt: &mut HashMap<i32, i32>,
    ) -> i32 {
        if let Some(node) = root {
            let node = node.borrow();
            let s = s + node.val;
            let mut ans = *cnt.get(&(s - sum)).unwrap_or(&0);
            *cnt.entry(s).or_insert(0) += 1;
            ans += Self::dfs(node.left.clone(), sum, s, cnt);
            ans += Self::dfs(node.right.clone(), sum, s, cnt);
            *cnt.entry(s).or_insert(0) -= 1;
            return ans;
        }
        return 0;
    }
}
```

#### Swift

```swift
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public var val: Int
 *     public var left: TreeNode?
 *     public var right: TreeNode?
 *     public init() { self.val = 0; self.left = nil; self.right = nil; }
 *     public init(_ val: Int) { self.val = val; self.left = nil; self.right = nil; }
 *     public init(_ val: Int, _ left: TreeNode?, _ right: TreeNode?) {
 *         self.val = val
 *         self.left = left
 *         self.right = right
 *     }
 * }
 */
class Solution {
    func pathSum(_ root: TreeNode?, _ sum: Int) -> Int {
        var cnt: [Int: Int] = [0: 1]

        func dfs(_ root: TreeNode?, _ s: Int) -> Int {
            guard let root = root else { return 0 }

            var s = s + root.val
            var ans = cnt[s - sum, default: 0]

            cnt[s, default: 0] += 1
            ans += dfs(root.left, s)
            ans += dfs(root.right, s)
            cnt[s, default: 0] -= 1

            return ans
        }

        return dfs(root, 0)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
