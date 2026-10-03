---
comments: true
difficulty: Hard
tags:
    - Tree
    - Depth-First Search
    - Dynamic Programming
    - Binary Tree
---

<!-- problem:start -->

# [2313. Minimum Flips in Binary Tree to Get Result 🔒](https://leetcode.com/problems/minimum-flips-in-binary-tree-to-get-result)

[中文文档](/solution/2300-2399/2313.Minimum%20Flips%20in%20Binary%20Tree%20to%20Get%20Result/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho <code>root</code> của một <strong>cây nhị phân</strong> có các tính chất sau:</p>

<ul>
	<li><strong>Node lá</strong> có giá trị <code>0</code> hoặc <code>1</code>, lần lượt biểu diễn <code>false</code> và <code>true</code>.</li>
	<li><strong>Node không phải lá</strong> có giá trị <code>2</code>, <code>3</code>, <code>4</code> hoặc <code>5</code>, lần lượt biểu diễn các phép toán Boolean <code>OR</code>, <code>AND</code>, <code>XOR</code> và <code>NOT</code>.</li>
</ul>

<p>Bạn cũng được cho một giá trị Boolean <code>result</code>, là kết quả mong muốn của phép <strong>đánh giá</strong> node <code>root</code>.</p>

<p>Việc đánh giá một node được thực hiện như sau:</p>

<ul>
	<li>Nếu node là node lá, kết quả đánh giá là <strong>giá trị</strong> của node, tức <code>true</code> hoặc <code>false</code>.</li>
	<li>Nếu không, hãy <strong>đánh giá</strong> các node con rồi <strong>áp dụng</strong> phép toán Boolean tương ứng với giá trị của node lên kết quả đánh giá của các node con.</li>
</ul>

<p>Trong một thao tác, bạn có thể <strong>lật</strong> một node lá, khiến node <code>false</code> trở thành <code>true</code> và node <code>true</code> trở thành <code>false</code>.</p>

<p>Trả về<em> số thao tác nhỏ nhất cần thực hiện để phép đánh giá của </em><code>root</code><em> cho ra </em><code>result</code>. Có thể chứng minh rằng luôn có cách đạt được <code>result</code>.</p>

<p><strong>Node lá</strong> là node không có node con.</p>

<p>Lưu ý: các node <code>NOT</code> có node con trái hoặc node con phải, còn các node không phải lá khác đều có cả node con trái và node con phải.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2313.Minimum%20Flips%20in%20Binary%20Tree%20to%20Get%20Result/images/operationstree.png" style="width: 500px; height: 179px;" />
<pre>
<strong>Đầu vào:</strong> root = [3,5,4,2,null,1,1,1,0], result = true
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Có thể chứng minh rằng cần lật ít nhất 2 node để root của cây
cho kết quả đánh giá là true. Một cách thực hiện được minh họa trong sơ đồ trên.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [0], result = false
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Root của cây đã có kết quả đánh giá là false, nên không cần lật node nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 10<sup>5</sup>]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 5</code></li>
	<li>Các node <code>OR</code>, <code>AND</code> và <code>XOR</code> có <code>2</code> node con.</li>
	<li>Các node <code>NOT</code> có <code>1</code> node con.</li>
	<li>Node lá có giá trị <code>0</code> hoặc <code>1</code>.</li>
	<li>Node không phải lá có giá trị <code>2</code>, <code>3</code>, <code>4</code> hoặc <code>5</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tree DP + Phân tích trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Các node lá có thể được lật giữa $0$ và $1$; các node bên trong là các toán tử Boolean. Thử lật trên mọi node lá có độ phức tạp hàm mũ và không đáp ứng được với cây có tới một nghìn node.
>
> Một cây con chỉ cần được biểu diễn bằng số lần lật nhỏ nhất để biến nó thành false hoặc true. Ta trả về cặp giá trị đó từ dưới lên. Kết hợp các node con theo loại node (node lá, $OR$, $AND$, $XOR$, $NOT$), sau đó lấy phần tử tương ứng với giá trị cần đạt của root.

<!-- thinking:end -->

Ta định nghĩa hàm $dfs(root)$, hàm này trả về một mảng có độ dài 2. Phần tử thứ nhất biểu diễn số lần lật nhỏ nhất cần thiết để thay đổi giá trị của node $root$ thành `false`, còn phần tử thứ hai biểu diễn số lần lật nhỏ nhất cần thiết để thay đổi giá trị của node $root$ thành `true`. Đáp án là $dfs(root)[result]$.

Cách cài đặt hàm $dfs(root)$ như sau:

Nếu $root$ là null, trả về $[+\infty, +\infty]$.

Ngược lại, gọi $x$ là giá trị của $root$, $l$ là giá trị trả về của cây con bên trái và $r$ là giá trị trả về của cây con bên phải. Khi đó, ta xét các trường hợp sau:

- Nếu $x \in \{0, 1\}$, trả về $[x, x \oplus 1]$.
- Nếu $x = 2$, nghĩa là phép toán Boolean là `OR`, để làm cho giá trị của $root$ là `false`, ta cần làm cho cả cây con bên trái và bên phải đều là `false`. Vì vậy, phần tử thứ nhất của giá trị trả về là $l[0] + r[0]$. Để làm cho giá trị của $root$ là `true`, chỉ cần ít nhất một trong hai cây con trái hoặc phải là `true`. Vì vậy, phần tử thứ hai của giá trị trả về là $\min(l[0] + r[1], l[1] + r[0], l[1] + r[1])$.
- Nếu $x = 3$, nghĩa là phép toán Boolean là `AND`, để làm cho giá trị của $root$ là `false`, chỉ cần ít nhất một trong hai cây con trái hoặc phải là `false`. Vì vậy, phần tử thứ nhất của giá trị trả về là $\min(l[0] + r[0], l[0] + r[1], l[1] + r[0])$. Để làm cho giá trị của $root$ là `true`, ta cần cả cây con trái và phải đều là `true`. Vì vậy, phần tử thứ hai của giá trị trả về là $l[1] + r[1]$.
- Nếu $x = 4$, nghĩa là phép toán Boolean là `XOR`, để làm cho giá trị của $root$ là `false`, ta cần cả cây con trái và phải đều là `false` hoặc đều là `true`. Vì vậy, phần tử thứ nhất của giá trị trả về là $\min(l[0] + r[0], l[1] + r[1])$. Để làm cho giá trị của $root$ là `true`, ta cần cây con trái và phải khác nhau. Vì vậy, phần tử thứ hai của giá trị trả về là $\min(l[0] + r[1], l[1] + r[0])$.
- Nếu $x = 5$, nghĩa là phép toán Boolean là `NOT`, để làm cho giá trị của $root$ là `false`, chỉ cần ít nhất một trong hai cây con trái hoặc phải là `true`. Vì vậy, phần tử thứ nhất của giá trị trả về là $\min(l[1], r[1])$. Để làm cho giá trị của $root$ là `true`, chỉ cần ít nhất một trong hai cây con trái hoặc phải là `false`. Vì vậy, phần tử thứ hai của giá trị trả về là $\min(l[0], r[0])$.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$. Trong đó $n$ là số node trong cây nhị phân.

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
    def minimumFlips(self, root: Optional[TreeNode], result: bool) -> int:
        def dfs(root: Optional[TreeNode]) -> (int, int):
            if root is None:
                return inf, inf
            x = root.val
            if x in (0, 1):
                return x, x ^ 1
            l, r = dfs(root.left), dfs(root.right)
            if x == 2:
                return l[0] + r[0], min(l[0] + r[1], l[1] + r[0], l[1] + r[1])
            if x == 3:
                return min(l[0] + r[0], l[0] + r[1], l[1] + r[0]), l[1] + r[1]
            if x == 4:
                return min(l[0] + r[0], l[1] + r[1]), min(l[0] + r[1], l[1] + r[0])
            return min(l[1], r[1]), min(l[0], r[0])

        return dfs(root)[int(result)]
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
    public int minimumFlips(TreeNode root, boolean result) {
        return dfs(root)[result ? 1 : 0];
    }

    private int[] dfs(TreeNode root) {
        if (root == null) {
            return new int[] {1 << 30, 1 << 30};
        }
        int x = root.val;
        if (x < 2) {
            return new int[] {x, x ^ 1};
        }
        var l = dfs(root.left);
        var r = dfs(root.right);
        int a = 0, b = 0;
        if (x == 2) {
            a = l[0] + r[0];
            b = Math.min(l[0] + r[1], Math.min(l[1] + r[0], l[1] + r[1]));
        } else if (x == 3) {
            a = Math.min(l[0] + r[0], Math.min(l[0] + r[1], l[1] + r[0]));
            b = l[1] + r[1];
        } else if (x == 4) {
            a = Math.min(l[0] + r[0], l[1] + r[1]);
            b = Math.min(l[0] + r[1], l[1] + r[0]);
        } else {
            a = Math.min(l[1], r[1]);
            b = Math.min(l[0], r[0]);
        }
        return new int[] {a, b};
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
    int minimumFlips(TreeNode* root, bool result) {
        function<pair<int, int>(TreeNode*)> dfs = [&](TreeNode* root) -> pair<int, int> {
            if (!root) {
                return {1 << 30, 1 << 30};
            }
            int x = root->val;
            if (x < 2) {
                return {x, x ^ 1};
            }
            auto [l0, l1] = dfs(root->left);
            auto [r0, r1] = dfs(root->right);
            int a = 0, b = 0;
            if (x == 2) {
                a = l0 + r0;
                b = min({l0 + r1, l1 + r0, l1 + r1});
            } else if (x == 3) {
                a = min({l0 + r0, l0 + r1, l1 + r0});
                b = l1 + r1;
            } else if (x == 4) {
                a = min(l0 + r0, l1 + r1);
                b = min(l0 + r1, l1 + r0);
            } else {
                a = min(l1, r1);
                b = min(l0, r0);
            }
            return {a, b};
        };
        auto [a, b] = dfs(root);
        return result ? b : a;
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
func minimumFlips(root *TreeNode, result bool) int {
	var dfs func(*TreeNode) (int, int)
	dfs = func(root *TreeNode) (int, int) {
		if root == nil {
			return 1 << 30, 1 << 30
		}
		x := root.Val
		if x < 2 {
			return x, x ^ 1
		}
		l0, l1 := dfs(root.Left)
		r0, r1 := dfs(root.Right)
		var a, b int
		if x == 2 {
			a = l0 + r0
			b = min(l0+r1, min(l1+r0, l1+r1))
		} else if x == 3 {
			a = min(l0+r0, min(l0+r1, l1+r0))
			b = l1 + r1
		} else if x == 4 {
			a = min(l0+r0, l1+r1)
			b = min(l0+r1, l1+r0)
		} else {
			a = min(l1, r1)
			b = min(l0, r0)
		}
		return a, b
	}
	a, b := dfs(root)
	if result {
		return b
	}
	return a
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

function minimumFlips(root: TreeNode | null, result: boolean): number {
    const dfs = (root: TreeNode | null): [number, number] => {
        if (!root) {
            return [1 << 30, 1 << 30];
        }
        const x = root.val;
        if (x < 2) {
            return [x, x ^ 1];
        }
        const [l0, l1] = dfs(root.left);
        const [r0, r1] = dfs(root.right);
        if (x === 2) {
            return [l0 + r0, Math.min(l0 + r1, l1 + r0, l1 + r1)];
        }
        if (x === 3) {
            return [Math.min(l0 + r0, l0 + r1, l1 + r0), l1 + r1];
        }
        if (x === 4) {
            return [Math.min(l0 + r0, l1 + r1), Math.min(l0 + r1, l1 + r0)];
        }
        return [Math.min(l1, r1), Math.min(l0, r0)];
    };
    return dfs(root)[result ? 1 : 0];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
