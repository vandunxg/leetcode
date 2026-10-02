---
comments: true
difficulty: Medium
rating: 1745
source: Weekly Contest 199 Q3
tags:
    - Tree
    - Depth-First Search
    - Binary Tree
    - Tree DP
---

<!-- problem:start -->

# [1530. Number of Good Leaf Nodes Pairs](https://leetcode.com/problems/number-of-good-leaf-nodes-pairs)

[中文文档](/solution/1500-1599/1530.Number%20of%20Good%20Leaf%20Nodes%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một cây nhị phân và một số nguyên <code>distance</code>. Một cặp gồm hai node <strong>lá</strong> khác nhau của cây nhị phân được gọi là tốt nếu độ dài của <strong>đường đi ngắn nhất</strong> giữa chúng nhỏ hơn hoặc bằng <code>distance</code>.</p>

<p>Trả về <em>số lượng cặp node lá tốt</em> trong cây.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1530.Number%20of%20Good%20Leaf%20Nodes%20Pairs/images/e1.jpg" style="width: 250px; height: 250px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,2,3,null,4], distance = 3
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Các node lá của cây là 3 và 4, độ dài đường đi ngắn nhất giữa chúng là 3. Đây là cặp tốt duy nhất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1530.Number%20of%20Good%20Leaf%20Nodes%20Pairs/images/e2.jpg" style="width: 250px; height: 182px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,2,3,4,5,6,7], distance = 3
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Các cặp tốt là [4,5] và [6,7], với đường đi ngắn nhất = 2. Cặp [4,6] không tốt vì độ dài đường đi ngắn nhất giữa chúng là 4.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [7,1,4,6,null,5,3,null,null,null,null,null,2], distance = 3
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Cặp tốt duy nhất là [2,5].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>The number of nodes in the <code>tree</code> is in the range <code>[1, 2<sup>10</sup>].</code></li>
	<li><code>1 &lt;= Node.val &lt;= 100</code></li>
	<li><code>1 &lt;= distance &lt;= 10</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các cặp node lá có khoảng cách không vượt quá $distance$. Truy vấn LCA cho từng cặp tốn khoảng (số node lá)$^2$ nhân với chiều cao, quá nặng với một cây dạng chuỗi. Một cặp tốt hoặc nằm hoàn toàn trong một cây con, hoặc đi qua root hiện tại.
>
> Chia đáp án thành các cặp trong cây con trái, các cặp trong cây con phải và các cặp đi qua root. Với phần đi qua root, ở mỗi phía chỉ cần histogram khoảng cách từ các node lá đến root này; các bucket có tổng khoảng cách không vượt quá giới hạn sẽ đóng góp một tích. Vì $distance\le 10$, các histogram luôn rất nhỏ.

<!-- thinking:end -->

Bài toán yêu cầu số lượng cặp node lá tốt trong một cây nhị phân. Đáp án có thể chia thành ba phần: số cặp node lá tốt trong cây con trái, số cặp node lá tốt trong cây con phải và số cặp node lá được tạo bởi một node ở cây con trái cùng một node ở cây con phải.

Ta có thể giải bài toán bằng đệ quy.

Độ phức tạp thời gian là $O(n \times d^2 \times h)$, trong đó $n$ là số node trong cây nhị phân, còn $h$ và $d$ lần lượt là chiều cao cây nhị phân và giới hạn khoảng cách. Độ phức tạp không gian là $O(h)$ cho recursion stack.

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
    def countPairs(self, root: TreeNode, distance: int) -> int:
        def dfs(root, cnt, i):
            if root is None or i >= distance:
                return
            if root.left is None and root.right is None:
                cnt[i] += 1
                return
            dfs(root.left, cnt, i + 1)
            dfs(root.right, cnt, i + 1)

        if root is None:
            return 0
        ans = self.countPairs(root.left, distance) + self.countPairs(
            root.right, distance
        )
        cnt1 = Counter()
        cnt2 = Counter()
        dfs(root.left, cnt1, 1)
        dfs(root.right, cnt2, 1)

        for k1, v1 in cnt1.items():
            for k2, v2 in cnt2.items():
                if k1 + k2 <= distance:
                    ans += v1 * v2
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
    public int countPairs(TreeNode root, int distance) {
        if (root == null) {
            return 0;
        }
        int ans = countPairs(root.left, distance) + countPairs(root.right, distance);
        int[] cnt1 = new int[distance];
        int[] cnt2 = new int[distance];
        dfs(root.left, cnt1, 1);
        dfs(root.right, cnt2, 1);
        for (int i = 0; i < distance; ++i) {
            for (int j = 0; j < distance; ++j) {
                if (i + j <= distance) {
                    ans += cnt1[i] * cnt2[j];
                }
            }
        }
        return ans;
    }

    void dfs(TreeNode root, int[] cnt, int i) {
        if (root == null || i >= cnt.length) {
            return;
        }
        if (root.left == null && root.right == null) {
            ++cnt[i];
            return;
        }
        dfs(root.left, cnt, i + 1);
        dfs(root.right, cnt, i + 1);
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
    int countPairs(TreeNode* root, int distance) {
        if (!root) return 0;
        int ans = countPairs(root->left, distance) + countPairs(root->right, distance);
        vector<int> cnt1(distance);
        vector<int> cnt2(distance);
        dfs(root->left, cnt1, 1);
        dfs(root->right, cnt2, 1);
        for (int i = 0; i < distance; ++i) {
            for (int j = 0; j < distance; ++j) {
                if (i + j <= distance) {
                    ans += cnt1[i] * cnt2[j];
                }
            }
        }
        return ans;
    }

    void dfs(TreeNode* root, vector<int>& cnt, int i) {
        if (!root || i >= cnt.size()) return;
        if (!root->left && !root->right) {
            ++cnt[i];
            return;
        }
        dfs(root->left, cnt, i + 1);
        dfs(root->right, cnt, i + 1);
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
func countPairs(root *TreeNode, distance int) int {
	if root == nil {
		return 0
	}
	ans := countPairs(root.Left, distance) + countPairs(root.Right, distance)
	cnt1 := make([]int, distance)
	cnt2 := make([]int, distance)
	dfs(root.Left, cnt1, 1)
	dfs(root.Right, cnt2, 1)
	for i, v1 := range cnt1 {
		for j, v2 := range cnt2 {
			if i+j <= distance {
				ans += v1 * v2
			}
		}
	}
	return ans
}

func dfs(root *TreeNode, cnt []int, i int) {
	if root == nil || i >= len(cnt) {
		return
	}
	if root.Left == nil && root.Right == nil {
		cnt[i]++
		return
	}
	dfs(root.Left, cnt, i+1)
	dfs(root.Right, cnt, i+1)
}
```

#### TypeScript

```ts
function countPairs(root: TreeNode | null, distance: number): number {
    const pairs: number[][] = [];

    const dfs = (node: TreeNode | null): number[][] => {
        if (!node) return [];
        if (!node.left && !node.right) return [[node.val, 1]];

        const left = dfs(node.left);
        const right = dfs(node.right);

        for (const [x, dx] of left) {
            for (const [y, dy] of right) {
                if (dx + dy <= distance) {
                    pairs.push([x, y]);
                }
            }
        }

        const res: number[][] = [];
        for (const arr of [left, right]) {
            for (const x of arr) {
                if (++x[1] <= distance) res.push(x);
            }
        }

        return res;
    };

    dfs(root);

    return pairs.length;
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
 * @param {number} distance
 * @return {number}
 */
var countPairs = function (root, distance) {
    const pairs = [];

    const dfs = node => {
        if (!node) return [];
        if (!node.left && !node.right) return [[node.val, 1]];

        const left = dfs(node.left);
        const right = dfs(node.right);

        for (const [x, dx] of left) {
            for (const [y, dy] of right) {
                if (dx + dy <= distance) {
                    pairs.push([x, y]);
                }
            }
        }

        const res = [];
        for (const arr of [left, right]) {
            for (const x of arr) {
                if (++x[1] <= distance) res.push(x);
            }
        }

        return res;
    };

    dfs(root);

    return pairs.length;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
