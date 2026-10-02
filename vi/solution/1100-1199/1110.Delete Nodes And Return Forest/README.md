---
comments: true
difficulty: Medium
rating: 1511
source: Weekly Contest 144 Q3
tags:
    - Tree
    - Depth-First Search
    - Array
    - Hash Table
    - Binary Tree
---

<!-- problem:start -->

# [1110. Delete Nodes And Return Forest](https://leetcode.com/problems/delete-nodes-and-return-forest)

[中文文档](/solution/1100-1199/1110.Delete%20Nodes%20And%20Return%20Forest/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một cây nhị phân, trong đó mỗi node có giá trị riêng biệt.</p>

<p>Sau khi xóa tất cả node có giá trị nằm trong <code>to_delete</code>, phần còn lại là một forest (tập hợp rời nhau gồm nhiều cây).</p>

<p>Hãy trả về các root của những cây trong forest còn lại. Có thể trả kết quả theo thứ tự bất kỳ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1100-1199/1110.Delete%20Nodes%20And%20Return%20Forest/images/screen-shot-2019-07-01-at-53836-pm.png" style="width: 237px; height: 150px;" />
<pre>
<strong>Input:</strong> root = [1,2,3,4,5,6,7], to_delete = [3,5]
<strong>Output:</strong> [[1,2,null,4],[6],[7]]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> root = [1,2,4,null,3], to_delete = [3]
<strong>Output:</strong> [[1,2,4]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây đã cho nhiều nhất là <code>1000</code>.</li>
	<li>Mỗi node có một giá trị riêng biệt trong khoảng từ <code>1</code> đến <code>1000</code>.</li>
	<li><code>to_delete.length &lt;= 1000</code></li>
	<li><code>to_delete</code> chứa các giá trị riêng biệt trong khoảng từ <code>1</code> đến <code>1000</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Việc xóa node sẽ chia cây thành một forest. Một root mới là node còn lại có node cha bị xóa (hoặc là root ban đầu nếu nó còn tồn tại). DFS hậu tự xử lý hai node con trước, sau đó xóa node hiện tại nếu cần và đưa các node con còn lại lên làm root mới.
>
> Dùng hash set giúp kiểm tra có cần xóa node hay không trong thời gian kỳ vọng O(1). Nếu root ban đầu còn lại, ta thêm nó vào kết quả sau cùng.

<!-- thinking:end -->

Trước tiên, dùng hash table hoặc mảng độ dài 1001 tên `s` để lưu các giá trị node cần xóa.

Tiếp theo, xây dựng hàm `dfs(root)` trả về root của cây con bắt đầu từ `root` sau khi xóa các node cần xóa. Hàm `dfs(root)` thực hiện như sau:

- Nếu `root` là null, trả về null;
- Nếu không, gọi đệ quy `dfs(root.left)` và `dfs(root.right)`, rồi lần lượt gán giá trị trả về cho `root.left` và `root.right`. Nếu không cần xóa `root`, trả về `root`. Nếu cần xóa `root`, kiểm tra `root.left` và `root.right`; node con nào khác null thì thêm vào mảng kết quả. Cuối cùng, trả về null.

Trong hàm chính, gọi `dfs(root)`. Nếu kết quả khác null, root không cần bị xóa nên ta thêm root vào mảng kết quả. Cuối cùng, trả về mảng kết quả.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, với $n$ là số node trong cây.

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
    def delNodes(
        self, root: Optional[TreeNode], to_delete: List[int]
    ) -> List[TreeNode]:
        def dfs(root: Optional[TreeNode]) -> Optional[TreeNode]:
            if root is None:
                return None
            root.left, root.right = dfs(root.left), dfs(root.right)
            if root.val not in s:
                return root
            if root.left:
                ans.append(root.left)
            if root.right:
                ans.append(root.right)
            return None

        s = set(to_delete)
        ans = []
        if dfs(root):
            ans.append(root)
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
    private boolean[] s = new boolean[1001];
    private List<TreeNode> ans = new ArrayList<>();

    public List<TreeNode> delNodes(TreeNode root, int[] to_delete) {
        for (int x : to_delete) {
            s[x] = true;
        }
        if (dfs(root) != null) {
            ans.add(root);
        }
        return ans;
    }

    private TreeNode dfs(TreeNode root) {
        if (root == null) {
            return null;
        }
        root.left = dfs(root.left);
        root.right = dfs(root.right);
        if (!s[root.val]) {
            return root;
        }
        if (root.left != null) {
            ans.add(root.left);
        }
        if (root.right != null) {
            ans.add(root.right);
        }
        return null;
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
    vector<TreeNode*> delNodes(TreeNode* root, vector<int>& to_delete) {
        bool s[1001];
        memset(s, 0, sizeof(s));
        for (int x : to_delete) {
            s[x] = true;
        }
        vector<TreeNode*> ans;
        function<TreeNode*(TreeNode*)> dfs = [&](TreeNode* root) -> TreeNode* {
            if (!root) {
                return nullptr;
            }
            root->left = dfs(root->left);
            root->right = dfs(root->right);
            if (!s[root->val]) {
                return root;
            }
            if (root->left) {
                ans.push_back(root->left);
            }
            if (root->right) {
                ans.push_back(root->right);
            }
            return nullptr;
        };
        if (dfs(root)) {
            ans.push_back(root);
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
func delNodes(root *TreeNode, to_delete []int) (ans []*TreeNode) {
	s := make([]bool, 1001)
	for _, x := range to_delete {
		s[x] = true
	}
	var dfs func(*TreeNode) *TreeNode
	dfs = func(root *TreeNode) *TreeNode {
		if root == nil {
			return nil
		}
		root.Left = dfs(root.Left)
		root.Right = dfs(root.Right)
		if !s[root.Val] {
			return root
		}
		if root.Left != nil {
			ans = append(ans, root.Left)
		}
		if root.Right != nil {
			ans = append(ans, root.Right)
		}
		return nil
	}
	if dfs(root) != nil {
		ans = append(ans, root)
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

function delNodes(root: TreeNode | null, to_delete: number[]): Array<TreeNode | null> {
    const s: boolean[] = Array(1001).fill(false);
    for (const x of to_delete) {
        s[x] = true;
    }
    const ans: Array<TreeNode | null> = [];
    const dfs = (root: TreeNode | null): TreeNode | null => {
        if (!root) {
            return null;
        }
        root.left = dfs(root.left);
        root.right = dfs(root.right);
        if (!s[root.val]) {
            return root;
        }
        if (root.left) {
            ans.push(root.left);
        }
        if (root.right) {
            ans.push(root.right);
        }
        return null;
    };
    if (dfs(root)) {
        ans.push(root);
    }
    return ans;
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
 * @param {number[]} to_delete
 * @return {TreeNode[]}
 */
var delNodes = function (root, to_delete) {
    const s = Array(1001).fill(false);
    for (const x of to_delete) {
        s[x] = true;
    }
    const ans = [];
    const dfs = root => {
        if (!root) {
            return null;
        }
        root.left = dfs(root.left);
        root.right = dfs(root.right);
        if (!s[root.val]) {
            return root;
        }
        if (root.left) {
            ans.push(root.left);
        }
        if (root.right) {
            ans.push(root.right);
        }
        return null;
    };
    if (dfs(root)) {
        ans.push(root);
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Cách 1 dùng recursion stack để xóa theo thứ tự hậu tự. BFS duyệt từ trên xuống: ngắt liên kết tới node con cần xóa; nếu node hiện tại bị xóa, ghi nhận các node con vẫn còn được nối với nó làm root mới. Duyệt theo từng level sẽ hoàn tất việc cắt mà không cần giá trị trả về; nếu root ban đầu còn lại thì thêm nó vào kết quả.

<!-- thinking:end -->

<!-- tabs:start -->

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
export function delNodes(root: T, to_delete: number[]): Array<T> {
    if (!root) return [];

    const del = new Set(to_delete);
    const res: T[] = [];
    let q: TreeNode[] = [root];

    while (q.length) {
        const qNext: TreeNode[] = [];

        for (const node of q) {
            if (node.left) {
                qNext.push(node.left);

                if (del.has(node.left.val)) {
                    node.left = null;
                }
            }

            if (node.right) {
                qNext.push(node.right);

                if (del.has(node.right.val)) {
                    node.right = null;
                }
            }

            if (del.has(node.val)) {
                if (node.left) res.push(node.left);
                if (node.right) res.push(node.right);
            }
        }

        q = qNext;
    }

    if (!del.has(root.val)) res.push(root);

    return res;
}

type T = TreeNode | null;
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
 * @param {number[]} to_delete
 * @return {TreeNode[]}
 */
var delNodes = function (root, to_delete) {
    if (!root) return [];

    const del = new Set(to_delete);
    const res = [];
    let q = [root];

    while (q.length) {
        const qNext = [];

        for (const node of q) {
            if (node.left) {
                qNext.push(node.left);

                if (del.has(node.left.val)) {
                    node.left = null;
                }
            }

            if (node.right) {
                qNext.push(node.right);

                if (del.has(node.right.val)) {
                    node.right = null;
                }
            }

            if (del.has(node.val)) {
                if (node.left) res.push(node.left);
                if (node.right) res.push(node.right);
            }
        }

        q = qNext;
    }

    if (!del.has(root.val)) res.push(root);

    return res;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
