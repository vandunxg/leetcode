---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [04.02. Minimum Height Tree](https://leetcode.cn/problems/minimum-height-tree-lcci)

[中文文档](/lcci/04.02.Minimum%20Height%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng đã được sắp xếp (theo thứ tự tăng dần) gồm các phần tử nguyên phân biệt, hãy viết một algo&shy;rithm để tạo một cây tìm kiếm nhị phân có chiều cao nhỏ nhất.</p>

<p><strong>Ví dụ:</strong></p>

<pre>

Với mảng đã sắp xếp: [-10,-3,0,5,9],



Một đáp án có thể là: [0,-3,9,-10,null,5]，biểu diễn cây sau:



           0

          / \

        -3   9

        /   /

      -10  5

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng đã sắp xếp phải được chuyển thành BST có chiều cao nhỏ nhất. Nếu luôn chọn khóa nhỏ nhất hoặc lớn nhất, ta sẽ tạo thành một chuỗi có chiều cao $n$.
>
> Chiều cao được tối thiểu hóa khi hai cây con có kích thước cân bằng nhất có thể, vì vậy nút gốc nên là phần tử ở giữa đoạn.
>
> $dfs(l,r)$ trả về null khi $l>r$, nếu không sẽ tạo nút tại $mid=(l+r)//2$. Thứ tự inorder khớp với mảng, nên cây là một BST có chiều cao $\Theta(\log n)$.

<!-- thinking:end -->

Ta thiết kế một hàm `dfs(l, r)` để xây dựng một cây con từ `l` đến `r`. Vì vậy, đáp án là `dfs(0, len(nums) - 1)`.

Quá trình thực thi hàm `dfs(l, r)` như sau:

1. Nếu `l > r`, trả về `None`.
2. Nếu không, tính vị trí ở giữa `mid = (l + r) / 2`, sau đó tạo nút gốc; cây con trái là `dfs(l, mid - 1)`, còn cây con phải là `dfs(mid + 1, r)`.
3. Cuối cùng, trả về nút gốc.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None


class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> TreeNode:
        def dfs(l: int, r: int) -> TreeNode:
            if l > r:
                return None
            mid = (l + r) >> 1
            return TreeNode(nums[mid], dfs(l, mid - 1), dfs(mid + 1, r))

        return dfs(0, len(nums) - 1)
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
    private int[] nums;

    public TreeNode sortedArrayToBST(int[] nums) {
        this.nums = nums;
        return dfs(0, nums.length - 1);
    }

    private TreeNode dfs(int l, int r) {
        if (l > r) {
            return null;
        }
        int mid = (l + r) >> 1;
        return new TreeNode(nums[mid], dfs(l, mid - 1), dfs(mid + 1, r));
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
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        auto dfs = [&](this auto&& dfs, int l, int r) -> TreeNode* {
            if (l > r) {
                return nullptr;
            }
            int mid = l + r >> 1;
            return new TreeNode(nums[mid], dfs(l, mid - 1), dfs(mid + 1, r));
        };
        return dfs(0, nums.size() - 1);
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
func sortedArrayToBST(nums []int) *TreeNode {
	var dfs func(int, int) *TreeNode
	dfs = func(l, r int) *TreeNode {
		if l > r {
			return nil
		}
		mid := (l + r) >> 1
		return &TreeNode{nums[mid], dfs(l, mid-1), dfs(mid+1, r)}
	}

	return dfs(0, len(nums)-1)
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

function sortedArrayToBST(nums: number[]): TreeNode | null {
    const dfs = (l: number, r: number): TreeNode | null => {
        if (l > r) {
            return null;
        }
        const mid = (l + r) >> 1;
        return new TreeNode(nums[mid], dfs(l, mid - 1), dfs(mid + 1, r));
    };
    return dfs(0, nums.length - 1);
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
    fn dfs(nums: &Vec<i32>, l: usize, r: usize) -> Option<Rc<RefCell<TreeNode>>> {
        if l >= r {
            return None;
        }
        let mid = (l + r) >> 1;
        Some(Rc::new(RefCell::new(TreeNode {
            val: nums[mid],
            left: Self::dfs(nums, l, mid),
            right: Self::dfs(nums, mid + 1, r),
        })))
    }
    pub fn sorted_array_to_bst(nums: Vec<i32>) -> Option<Rc<RefCell<TreeNode>>> {
        Self::dfs(&nums, 0, nums.len())
    }
}
```

#### JavaScript

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {number[]} nums
 * @return {TreeNode}
 */
var sortedArrayToBST = function (nums) {
    function dfs(l, r) {
        if (l > r) {
            return null;
        }
        const mid = (l + r) >> 1;
        return new TreeNode(nums[mid], dfs(l, mid - 1), dfs(mid + 1, r));
    }

    return dfs(0, nums.length - 1);
};
```

#### Swift

```swift
/**
* class TreeNode {
*     var val: Int
*     var left: TreeNode?
*     var right: TreeNode?
*
*     init(_ val: Int, _ left: TreeNode? = nil, _ right: TreeNode? = nil) {
*         self.val = val
*         self.left = left
*         self.right = right
*     }
* }
*/

class Solution {
    private var nums: [Int]!

    func sortedArrayToBST(_ nums: [Int]) -> TreeNode? {
        self.nums = nums
        return dfs(0, nums.count - 1)
    }

    private func dfs(_ l: Int, _ r: Int) -> TreeNode? {
        if l > r {
            return nil
        }
        let mid = (l + r) / 2
        return TreeNode(nums[mid], dfs(l, mid - 1), dfs(mid + 1, r))
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
