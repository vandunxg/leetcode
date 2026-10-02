---
comments: true
difficulty: Medium
tags:
    - Tree
    - Array
    - Hash Table
    - Divide and Conquer
    - Binary Tree
---

<!-- problem:start -->

# [889. Construct Binary Tree from Preorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal)

[中文文档](/solution/0800-0899/0889.Construct%20Binary%20Tree%20from%20Preorder%20and%20Postorder%20Traversal/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>preorder</code> và <code>postorder</code>, trong đó <code>preorder</code> là thứ tự duyệt preorder của một binary tree có các giá trị <strong>khác nhau</strong>, còn <code>postorder</code> là thứ tự duyệt postorder của cùng cây đó. Hãy dựng lại và trả về <em>binary tree</em>.</p>

<p>Nếu có nhiều đáp án, bạn có thể <strong>trả về bất kỳ đáp án nào</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0889.Construct%20Binary%20Tree%20from%20Preorder%20and%20Postorder%20Traversal/images/lc-prepost.jpg" style="width: 304px; height: 265px;" />
<pre>
<strong>Đầu vào:</strong> preorder = [1,2,4,5,3,6,7], postorder = [4,5,2,6,7,3,1]
<strong>Đầu ra:</strong> [1,2,3,4,5,6,7]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> preorder = [1], postorder = [1]
<strong>Đầu ra:</strong> [1]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= preorder.length &lt;= 30</code></li>
	<li><code>1 &lt;= preorder[i] &lt;= preorder.length</code></li>
	<li>Tất cả giá trị trong <code>preorder</code> đều <strong>duy nhất</strong>.</li>
	<li><code>postorder.length == preorder.length</code></li>
	<li><code>1 &lt;= postorder[i] &lt;= postorder.length</code></li>
	<li>Tất cả giá trị trong <code>postorder</code> đều <strong>duy nhất</strong>.</li>
	<li>Đảm bảo <code>preorder</code> và <code>postorder</code> lần lượt là thứ tự duyệt preorder và postorder của cùng một binary tree.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Từ hai thứ tự preorder và postorder, ta có thể dựng lại cây (các giá trị là duy nhất). Giá trị tiếp theo trong preorder là gốc của cây con trái; vị trí của nó trong postorder giúp xác định đoạn cây con trái. Vì có nhiều nhất $30$ node nên có thể dùng đệ quy.
>
> Lập map từ giá trị sang chỉ số trong postorder. Mỗi lần gọi đệ quy sẽ chia các đoạn thành cây con trái và phải dựa trên gốc cây con trái. Nếu đoạn rỗng hoặc chỉ có một node thì trả về ngay.

<!-- thinking:end -->

Thứ tự duyệt preorder là: root node -> cây con trái -> cây con phải; còn thứ tự duyệt postorder là: cây con trái -> cây con phải -> root node.

Do đó, root node của binary tree là node đầu tiên trong preorder và node cuối cùng trong postorder.

Tiếp theo, ta cần xác định đoạn tương ứng với cây con trái và cây con phải.

Nếu binary tree có cây con trái, root node của cây con trái là node thứ hai trong preorder. Nếu không có cây con trái, node thứ hai trong preorder chính là root node của cây con phải. Vì postorder không phân biệt được hai trường hợp này, ta có thể xem node thứ hai trong preorder là root node của cây con trái, rồi tìm vị trí của nó trong postorder để xác định đoạn cây con trái.

Cụ thể, ta định nghĩa hàm đệ quy $dfs(a, b, c, d)$, trong đó $[a, b]$ là đoạn trong preorder và $[c, d]$ là đoạn trong postorder. Hàm dựng root node của binary tree dựa trên hai đoạn này. Kết quả là $dfs(0, n - 1, 0, n - 1)$, với $n$ là độ dài của preorder.

Hàm $dfs(a, b, c, d)$ thực hiện các bước sau:

1. Nếu $a > b$, đoạn đang xét rỗng nên trả về node null.
1. Nếu không, tạo node mới $root$ có giá trị bằng node đầu tiên trong preorder, tức $preorder[a]$.
1. Nếu $a=b$, $root$ không có cây con trái hay cây con phải, nên trả về $root$.
1. Nếu không, giá trị root node của cây con trái là $preorder[a + 1]$. Gọi $i$ là vị trí của $preorder[a + 1]$ trong postorder. Số node của cây con trái là $m = i - c + 1$. Vì vậy, đoạn cây con trái trong preorder là $[a + 1, a + m]$ và trong postorder là $[c, i]$; đoạn cây con phải trong preorder là $[a + m + 1, b]$ và trong postorder là $[i + 1, d - 1]$.
1. Sau khi xác định được các đoạn của hai cây con, ta đệ quy dựng lại chúng, rồi lần lượt gán root node của cây con trái và cây con phải làm node con trái và node con phải của $root$. Cuối cùng, trả về $root$.

Trong hàm $dfs(a, b, c, d)$, ta dùng hash table $pos$ để lưu vị trí của mỗi node trong postorder. Tạo hash table này trước khi gọi hàm để có thể tìm vị trí của một node trong postorder trong $O(1)$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, với $n$ là số node.

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
    def constructFromPrePost(
        self, preorder: List[int], postorder: List[int]
    ) -> Optional[TreeNode]:
        def dfs(a: int, b: int, c: int, d: int) -> Optional[TreeNode]:
            if a > b:
                return None
            root = TreeNode(preorder[a])
            if a == b:
                return root
            i = pos[preorder[a + 1]]
            m = i - c + 1
            root.left = dfs(a + 1, a + m, c, i)
            root.right = dfs(a + m + 1, b, i + 1, d - 1)
            return root

        pos = {x: i for i, x in enumerate(postorder)}
        return dfs(0, len(preorder) - 1, 0, len(postorder) - 1)
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
    private Map<Integer, Integer> pos = new HashMap<>();
    private int[] preorder;

    public TreeNode constructFromPrePost(int[] preorder, int[] postorder) {
        this.preorder = preorder;
        for (int i = 0; i < postorder.length; ++i) {
            pos.put(postorder[i], i);
        }
        return dfs(0, preorder.length - 1, 0, postorder.length - 1);
    }

    private TreeNode dfs(int a, int b, int c, int d) {
        if (a > b) {
            return null;
        }
        TreeNode root = new TreeNode(preorder[a]);
        if (a == b) {
            return root;
        }
        int i = pos.get(preorder[a + 1]);
        int m = i - c + 1;
        root.left = dfs(a + 1, a + m, c, i);
        root.right = dfs(a + m + 1, b, i + 1, d - 1);
        return root;
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
    TreeNode* constructFromPrePost(vector<int>& preorder, vector<int>& postorder) {
        unordered_map<int, int> pos;
        int n = postorder.size();
        for (int i = 0; i < n; ++i) {
            pos[postorder[i]] = i;
        }
        function<TreeNode*(int, int, int, int)> dfs = [&](int a, int b, int c, int d) -> TreeNode* {
            if (a > b) {
                return nullptr;
            }
            TreeNode* root = new TreeNode(preorder[a]);
            if (a == b) {
                return root;
            }
            int i = pos[preorder[a + 1]];
            int m = i - c + 1;
            root->left = dfs(a + 1, a + m, c, i);
            root->right = dfs(a + m + 1, b, i + 1, d - 1);
            return root;
        };
        return dfs(0, n - 1, 0, n - 1);
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
func constructFromPrePost(preorder []int, postorder []int) *TreeNode {
	pos := map[int]int{}
	for i, x := range postorder {
		pos[x] = i
	}
	var dfs func(int, int, int, int) *TreeNode
	dfs = func(a, b, c, d int) *TreeNode {
		if a > b {
			return nil
		}
		root := &TreeNode{Val: preorder[a]}
		if a == b {
			return root
		}
		i := pos[preorder[a+1]]
		m := i - c + 1
		root.Left = dfs(a+1, a+m, c, i)
		root.Right = dfs(a+m+1, b, i+1, d-1)
		return root
	}
	return dfs(0, len(preorder)-1, 0, len(postorder)-1)
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

function constructFromPrePost(preorder: number[], postorder: number[]): TreeNode | null {
    const pos: Map<number, number> = new Map();
    const n = postorder.length;
    for (let i = 0; i < n; ++i) {
        pos.set(postorder[i], i);
    }
    const dfs = (a: number, b: number, c: number, d: number): TreeNode | null => {
        if (a > b) {
            return null;
        }
        const root = new TreeNode(preorder[a]);
        if (a === b) {
            return root;
        }
        const i = pos.get(preorder[a + 1])!;
        const m = i - c + 1;
        root.left = dfs(a + 1, a + m, c, i);
        root.right = dfs(a + m + 1, b, i + 1, d - 1);
        return root;
    };
    return dfs(0, n - 1, 0, n - 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Một cách đệ quy khác

<!-- thinking:start -->

> **Tư duy**
>
> Cách 1 dùng bốn mốc biên. Ta cũng có thể chỉ dùng ba tham số — vị trí bắt đầu của preorder, vị trí bắt đầu của postorder và kích thước đoạn; kích thước cây con trái vẫn được tính từ vị trí của root cây con trái trong postorder.
>
> Cách đệ quy tương tự. Cây con phải bắt đầu tại $i+m+1$ trong preorder, tại $k+1$ trong postorder và có kích thước $n-m-1$.

<!-- thinking:end -->

Ta có thể định nghĩa hàm đệ quy $dfs(i, j, n)$, trong đó $i$ và $j$ lần lượt là vị trí bắt đầu của preorder và postorder, còn $n$ là số node. Hàm dựng root node của binary tree từ đoạn preorder $[i, i + n - 1]$ và postorder $[j, j + n - 1]$. Kết quả là $dfs(0, 0, n)$, với $n$ là độ dài của preorder.

Hàm $dfs(i, j, n)$ thực hiện các bước sau:

1. Nếu $n=0$, đoạn đang xét rỗng nên trả về node null.
2. Nếu không, tạo node mới $root$ có giá trị bằng node đầu tiên trong preorder, tức $preorder[i]$.
3. Nếu $n=1$, $root$ không có cây con trái hay cây con phải, nên trả về $root$.
4. Nếu không, giá trị root node của cây con trái là $preorder[i + 1]$. Gọi $k$ là vị trí của $preorder[i + 1]$ trong postorder. Khi đó, cây con trái có $m = k - j + 1$ node, còn cây con phải có $n - m - 1$ node. Ta đệ quy dựng lại hai cây con, rồi lần lượt gán root node của chúng làm node con trái và node con phải của $root$. Cuối cùng, trả về $root$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, với $n$ là số node.

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
    def constructFromPrePost(
        self, preorder: List[int], postorder: List[int]
    ) -> Optional[TreeNode]:
        def dfs(i: int, j: int, n: int) -> Optional[TreeNode]:
            if n <= 0:
                return None
            root = TreeNode(preorder[i])
            if n == 1:
                return root
            k = pos[preorder[i + 1]]
            m = k - j + 1
            root.left = dfs(i + 1, j, m)
            root.right = dfs(i + m + 1, k + 1, n - m - 1)
            return root

        pos = {x: i for i, x in enumerate(postorder)}
        return dfs(0, 0, len(preorder))
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
    private Map<Integer, Integer> pos = new HashMap<>();
    private int[] preorder;

    public TreeNode constructFromPrePost(int[] preorder, int[] postorder) {
        this.preorder = preorder;
        for (int i = 0; i < postorder.length; ++i) {
            pos.put(postorder[i], i);
        }
        return dfs(0, 0, preorder.length);
    }

    private TreeNode dfs(int i, int j, int n) {
        if (n <= 0) {
            return null;
        }
        TreeNode root = new TreeNode(preorder[i]);
        if (n == 1) {
            return root;
        }
        int k = pos.get(preorder[i + 1]);
        int m = k - j + 1;
        root.left = dfs(i + 1, j, m);
        root.right = dfs(i + m + 1, k + 1, n - m - 1);
        return root;
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
    TreeNode* constructFromPrePost(vector<int>& preorder, vector<int>& postorder) {
        unordered_map<int, int> pos;
        int n = postorder.size();
        for (int i = 0; i < n; ++i) {
            pos[postorder[i]] = i;
        }
        function<TreeNode*(int, int, int)> dfs = [&](int i, int j, int n) -> TreeNode* {
            if (n <= 0) {
                return nullptr;
            }
            TreeNode* root = new TreeNode(preorder[i]);
            if (n == 1) {
                return root;
            }
            int k = pos[preorder[i + 1]];
            int m = k - j + 1;
            root->left = dfs(i + 1, j, m);
            root->right = dfs(i + m + 1, k + 1, n - m - 1);
            return root;
        };
        return dfs(0, 0, n);
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
func constructFromPrePost(preorder []int, postorder []int) *TreeNode {
	pos := map[int]int{}
	for i, x := range postorder {
		pos[x] = i
	}
	var dfs func(int, int, int) *TreeNode
	dfs = func(i, j, n int) *TreeNode {
		if n <= 0 {
			return nil
		}
		root := &TreeNode{Val: preorder[i]}
		if n == 1 {
			return root
		}
		k := pos[preorder[i+1]]
		m := k - j + 1
		root.Left = dfs(i+1, j, m)
		root.Right = dfs(i+m+1, k+1, n-m-1)
		return root
	}
	return dfs(0, 0, len(preorder))
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

function constructFromPrePost(preorder: number[], postorder: number[]): TreeNode | null {
    const pos: Map<number, number> = new Map();
    const n = postorder.length;
    for (let i = 0; i < n; ++i) {
        pos.set(postorder[i], i);
    }
    const dfs = (i: number, j: number, n: number): TreeNode | null => {
        if (n <= 0) {
            return null;
        }
        const root = new TreeNode(preorder[i]);
        if (n === 1) {
            return root;
        }
        const k = pos.get(preorder[i + 1])!;
        const m = k - j + 1;
        root.left = dfs(i + 1, j, m);
        root.right = dfs(i + 1 + m, k + 1, n - 1 - m);
        return root;
    };
    return dfs(0, 0, n);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
