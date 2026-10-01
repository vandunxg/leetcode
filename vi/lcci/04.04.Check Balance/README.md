---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [04.04. Check Balance](https://leetcode.cn/problems/check-balance-lcci)

[Tài liệu tiếng Trung](/lcci/04.04.Check%20Balance/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy triển khai một hàm để kiểm tra xem một cây nhị phân có cân bằng hay không. Trong bài này, một cây cân bằng được định nghĩa là cây mà độ cao của hai cây con tại mọi nút không chênh lệch quá một.</p>

<p><br />

<strong>Ví dụ 1:</strong></p>

<pre>

Với cây [3,9,20,null,null,15,7]

    3

   / \

  9  20

    /  \

   15   7

trả về true.</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>

Với [1,2,2,3,3,null,null,4,4]

      1

     / \

    2   2

   / \

  3   3

 / \

4   4

trả về&nbsp;false.</pre>

<p>&nbsp;</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy (Duyệt post-order)

<!-- thinking:start -->

> **Tư duy**
>
> Cây cân bằng yêu cầu độ cao của hai cây con tại mọi nút chênh lệch không quá $1$. Nếu tính lại độ cao tại mỗi nút, các cây con sẽ bị duyệt lại và độ phức tạp có thể lên đến bậc hai.
>
> Khi duyệt post-order, ta đã biết độ cao của cả hai cây con, nên đồng thời có thể báo hiệu rằng cây đã mất cân bằng.
>
> $dfs$ trả về độ cao hoặc $-1$ nếu mất cân bằng; nếu một cây con trả về $-1$ hoặc $|l-r|>1$ thì giá trị $-1$ được truyền lên. Kết quả tại gốc là kiểm tra giá trị đó có không âm hay không, chỉ trong một lần duyệt.

<!-- thinking:end -->

Ta thiết kế hàm $dfs(root)$, hàm này trả về độ cao của cây có nút gốc là $root$. Nếu cây có nút gốc là $root$ cân bằng, hàm trả về độ cao của cây; ngược lại, hàm trả về $-1$.

Logic thực thi của hàm $dfs(root)$ như sau:

- Nếu $root$ là null, trả về $0$.
- Nếu không, ta gọi đệ quy $dfs(root.left)$ và $dfs(root.right)$, đồng thời kiểm tra xem giá trị trả về của $dfs(root.left)$ và $dfs(root.right)$ có phải là $-1$ hay không. Nếu không phải, ta kiểm tra điều kiện $abs(dfs(root.left) - dfs(root.right)) \leq 1$. Nếu điều kiện đúng, trả về $max(dfs(root.left), dfs(root.right)) + 1$; nếu không, trả về $-1$.

Trong hàm chính, ta chỉ cần gọi $dfs(root)$ và kiểm tra xem giá trị trả về có phải là $-1$ hay không. Nếu không phải, trả về `true`; ngược lại, trả về `false`.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là số nút trong cây nhị phân.

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
    def isBalanced(self, root: TreeNode) -> bool:
        def dfs(root: TreeNode):
            if root is None:
                return 0
            l, r = dfs(root.left), dfs(root.right)
            if l == -1 or r == -1 or abs(l - r) > 1:
                return -1
            return max(l, r) + 1

        return dfs(root) >= 0
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
    public boolean isBalanced(TreeNode root) {
        return dfs(root) >= 0;
    }

    private int dfs(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int l = dfs(root.left);
        int r = dfs(root.right);
        if (l < 0 || r < 0 || Math.abs(l - r) > 1) {
            return -1;
        }
        return Math.max(l, r) + 1;
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
    bool isBalanced(TreeNode* root) {
        function<int(TreeNode*)> dfs = [&](TreeNode* root) {
            if (!root) {
                return 0;
            }
            int l = dfs(root->left);
            int r = dfs(root->right);
            if (l == -1 || r == -1 || abs(l - r) > 1) {
                return -1;
            }
            return max(l, r) + 1;
        };
        return dfs(root) >= 0;
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
func isBalanced(root *TreeNode) bool {
	var dfs func(*TreeNode) int
	dfs = func(root *TreeNode) int {
		if root == nil {
			return 0
		}
		l, r := dfs(root.Left), dfs(root.Right)
		if l == -1 || r == -1 || abs(l-r) > 1 {
			return -1
		}
		return max(l, r) + 1
	}
	return dfs(root) >= 0
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
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

function isBalanced(root: TreeNode | null): boolean {
    const dfs = (root: TreeNode | null): number => {
        if (!root) {
            return 0;
        }
        const l = dfs(root.left);
        const r = dfs(root.right);
        if (l === -1 || r === -1 || Math.abs(l - r) > 1) {
            return -1;
        }
        return Math.max(l, r) + 1;
    };
    return dfs(root) >= 0;
}
```

#### Swift

```swift
/* class TreeNode {
*    var val: Int
*    var left: TreeNode?
*    var right: TreeNode?
*
*    init(_ val: Int) {
*        self.val = val
*        self.left = nil
*        self.right = nil
*    }
*  }
*/

class Solution {
    func isBalanced(_ root: TreeNode?) -> Bool {
        return dfs(root) >= 0
    }

    private func dfs(_ root: TreeNode?) -> Int {
        guard let root = root else {
            return 0
        }

        let leftHeight = dfs(root.left)
        let rightHeight = dfs(root.right)
        if leftHeight < 0 || rightHeight < 0 || abs(leftHeight - rightHeight) > 1 {
            return -1
        }
        return max(leftHeight, rightHeight) + 1
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
