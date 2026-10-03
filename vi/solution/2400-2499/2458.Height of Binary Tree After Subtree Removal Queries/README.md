---
comments: true
difficulty: Hard
rating: 2298
source: Weekly Contest 317 Q4
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Array
    - Binary Tree
---

<!-- problem:start -->

# [2458. Height of Binary Tree After Subtree Removal Queries](https://leetcode.com/problems/height-of-binary-tree-after-subtree-removal-queries)

[中文文档](/solution/2400-2499/2458.Height%20of%20Binary%20Tree%20After%20Subtree%20Removal%20Queries/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho <code>root</code> của một <strong>cây nhị phân</strong> có <code>n</code> node. Mỗi node được gán một giá trị duy nhất từ <code>1</code> đến <code>n</code>. Bạn cũng được cho một mảng <code>queries</code> có kích thước <code>m</code>.</p>

<p>Bạn phải thực hiện <code>m</code> truy vấn <strong>độc lập</strong> trên cây, trong đó ở truy vấn <code>i<sup>th</sup></code>, bạn thực hiện việc sau:</p>

<ul>
	<li><strong>Xóa</strong> cây con bắt nguồn từ node có giá trị <code>queries[i]</code> khỏi cây. <strong>Đảm bảo</strong> rằng <code>queries[i]</code> sẽ <strong>không</strong> bằng giá trị của root.</li>
</ul>

<p>Trả về <em>một mảng </em><code>answer</code><em> có kích thước </em><code>m</code><em>, trong đó </em><code>answer[i]</code><em> là chiều cao của cây sau khi thực hiện </em><code>i<sup>th</sup></code><em> truy vấn.</em></p>

<p><strong>Lưu ý</strong>:</p>

<ul>
	<li>Các truy vấn độc lập với nhau, vì vậy cây trở về trạng thái <strong>ban đầu</strong> sau mỗi truy vấn.</li>
	<li>Chiều cao của một cây là <strong>số cạnh trong đường đi đơn dài nhất</strong> từ root đến một node bất kỳ trong cây.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2458.Height%20of%20Binary%20Tree%20After%20Subtree%20Removal%20Queries/images/binaryytreeedrawio-1.png" style="width: 495px; height: 281px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,3,4,2,null,6,5,null,null,null,null,null,7], queries = [4]
<strong>Đầu ra:</strong> [2]
<strong>Giải thích:</strong> Hình trên biểu diễn cây sau khi xóa cây con bắt nguồn từ node có giá trị 4.
Chiều cao của cây là 2 (đường đi 1 -&gt; 3 -&gt; 2).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2458.Height%20of%20Binary%20Tree%20After%20Subtree%20Removal%20Queries/images/binaryytreeedrawio-2.png" style="width: 301px; height: 284px;" />
<pre>
<strong>Đầu vào:</strong> root = [5,8,9,2,1,3,7,4,6], queries = [3,2,4,8]
<strong>Đầu ra:</strong> [3,2,3,2]
<strong>Giải thích:</strong> Ta có các truy vấn sau:
- Xóa cây con bắt nguồn từ node có giá trị 3. Chiều cao của cây trở thành 3 (đường đi 5 -&gt; 8 -&gt; 2 -&gt; 4).
- Xóa cây con bắt nguồn từ node có giá trị 2. Chiều cao của cây trở thành 2 (đường đi 5 -&gt; 8 -&gt; 1).
- Xóa cây con bắt nguồn từ node có giá trị 4. Chiều cao của cây trở thành 3 (đường đi 5 -&gt; 8 -&gt; 2 -&gt; 6).
- Xóa cây con bắt nguồn từ node có giá trị 8. Chiều cao của cây trở thành 2 (đường đi 5 -&gt; 9 -&gt; 3).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây là <code>n</code>.</li>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= Node.val &lt;= n</code></li>
	<li>Tất cả các giá trị trong cây đều <strong>duy nhất</strong>.</li>
	<li><code>m == queries.length</code></li>
	<li><code>1 &lt;= m &lt;= min(n, 10<sup>4</sup>)</code></li>
	<li><code>1 &lt;= queries[i] &lt;= n</code></li>
	<li><code>queries[i] != root.val</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai lần duyệt DFS

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi truy vấn xóa một cây con không chứa root; $n\le 10^5$ và $q\le 10^4$ khiến việc tính lại chiều cao là không thể. Sau khi xóa $x$, chiều cao là độ dài lớn nhất của đường đi từ root đến lá không đi qua $x$, tức là giá trị lớn hơn giữa độ sâu phía node anh em và các nhánh rẽ ở phía trên.
>
> Lượt DFS đầu tiên lưu chiều cao cây con $d$. Lượt DFS thứ hai truyền $\textit{rest}$, là chiều cao nếu xóa node hiện tại: khi đi sang trái, $\textit{rest}$ được so sánh với $\textit{depth}$ cộng chiều cao cây con bên phải (và ngược lại). Lưu giá trị vào $\textit{res}[val]$.

<!-- thinking:end -->

Trước hết, ta thực hiện một lượt duyệt DFS để xác định độ sâu của mỗi node và lưu vào hash table $d$, trong đó $d[x]$ biểu thị độ sâu của node $x$.

Sau đó, ta thiết kế hàm $dfs(root, depth, rest)$, trong đó:

- `root` biểu thị node hiện tại;
- `depth` biểu thị độ sâu của node hiện tại;
- `rest` biểu thị chiều cao của cây sau khi xóa node hiện tại.

Logic tính toán của hàm như sau:

Nếu node là null, trả về ngay. Nếu không, ta tăng `depth` lên $1$, sau đó lưu `rest` vào `res`.

Tiếp theo, ta đệ quy duyệt cây con trái và phải.

Trước khi đệ quy vào cây con trái, ta tính độ sâu từ node gốc đến node sâu nhất trong cây con phải của node hiện tại, tức là $depth+d[root.right]$, rồi so sánh với `rest` và chọn giá trị lớn hơn làm `rest` cho cây con trái.

Trước khi đệ quy vào cây con phải, ta tính độ sâu từ node gốc đến node sâu nhất trong cây con trái của node hiện tại, tức là $depth+d[root.left]$, rồi so sánh với `rest` và chọn giá trị lớn hơn làm `rest` cho cây con phải.

Cuối cùng, ta trả về các giá trị kết quả tương ứng với từng node trong truy vấn.

Độ phức tạp thời gian là $O(n+m)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ và $m$ lần lượt là số node trong cây và số truy vấn.

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
    def treeQueries(self, root: Optional[TreeNode], queries: List[int]) -> List[int]:
        def f(root):
            if root is None:
                return 0
            l, r = f(root.left), f(root.right)
            d[root] = 1 + max(l, r)
            return d[root]

        def dfs(root, depth, rest):
            if root is None:
                return
            depth += 1
            res[root.val] = rest
            dfs(root.left, depth, max(rest, depth + d[root.right]))
            dfs(root.right, depth, max(rest, depth + d[root.left]))

        d = defaultdict(int)
        f(root)
        res = [0] * (len(d) + 1)
        dfs(root, -1, 0)
        return [res[v] for v in queries]
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
    private Map<TreeNode, Integer> d = new HashMap<>();
    private int[] res;

    public int[] treeQueries(TreeNode root, int[] queries) {
        f(root);
        res = new int[d.size() + 1];
        d.put(null, 0);
        dfs(root, -1, 0);
        int m = queries.length;
        int[] ans = new int[m];
        for (int i = 0; i < m; ++i) {
            ans[i] = res[queries[i]];
        }
        return ans;
    }

    private void dfs(TreeNode root, int depth, int rest) {
        if (root == null) {
            return;
        }
        ++depth;
        res[root.val] = rest;
        dfs(root.left, depth, Math.max(rest, depth + d.get(root.right)));
        dfs(root.right, depth, Math.max(rest, depth + d.get(root.left)));
    }

    private int f(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int l = f(root.left), r = f(root.right);
        d.put(root, 1 + Math.max(l, r));
        return d.get(root);
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
    vector<int> treeQueries(TreeNode* root, vector<int>& queries) {
        unordered_map<TreeNode*, int> d;
        function<int(TreeNode*)> f = [&](TreeNode* root) -> int {
            if (!root) return 0;
            int l = f(root->left), r = f(root->right);
            d[root] = 1 + max(l, r);
            return d[root];
        };
        f(root);
        vector<int> res(d.size() + 1);
        function<void(TreeNode*, int, int)> dfs = [&](TreeNode* root, int depth, int rest) {
            if (!root) return;
            ++depth;
            res[root->val] = rest;
            dfs(root->left, depth, max(rest, depth + d[root->right]));
            dfs(root->right, depth, max(rest, depth + d[root->left]));
        };
        dfs(root, -1, 0);
        vector<int> ans;
        for (int v : queries) ans.emplace_back(res[v]);
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
func treeQueries(root *TreeNode, queries []int) (ans []int) {
	d := map[*TreeNode]int{}
	var f func(*TreeNode) int
	f = func(root *TreeNode) int {
		if root == nil {
			return 0
		}
		l, r := f(root.Left), f(root.Right)
		d[root] = 1 + max(l, r)
		return d[root]
	}
	f(root)
	res := make([]int, len(d)+1)
	var dfs func(*TreeNode, int, int)
	dfs = func(root *TreeNode, depth, rest int) {
		if root == nil {
			return
		}
		depth++
		res[root.Val] = rest
		dfs(root.Left, depth, max(rest, depth+d[root.Right]))
		dfs(root.Right, depth, max(rest, depth+d[root.Left]))
	}
	dfs(root, -1, 0)
	for _, v := range queries {
		ans = append(ans, res[v])
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Một lần DFS + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 điền chiều cao khi xóa của mọi node bằng hai lượt DFS. Ta có thể dùng một lượt DFS để ghi nhận node con sâu nhất của mỗi level rồi sắp xếp level đó. Với truy vấn $q$, nếu node đó giữ giá trị lớn nhất của level, ta dùng giá trị lớn thứ hai; nếu không thì giữ giá trị lớn nhất. Với level chỉ có một node, kết quả là $level-1$.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function treeQueries(root: TreeNode | null, queries: number[]): number[] {
    const ans: number[] = [];
    const levels: Map<number, [number, number][]> = new Map();
    const valToLevel = new Map<number, number>();

    const dfs = (node: TreeNode | null, level = 0): number => {
        if (!node) return level - 1;

        const max = Math.max(dfs(node.left, level + 1), dfs(node.right, level + 1));

        if (!levels.has(level)) {
            levels.set(level, []);
        }
        levels.get(level)?.push([max, node.val]);
        valToLevel.set(node.val, level);

        return max;
    };

    dfs(root, 0);

    for (const [_, l] of levels) {
        l.sort(([a], [b]) => b - a);
    }

    for (const q of queries) {
        const level = valToLevel.get(q)!;
        const maxes = levels.get(level)!;

        if (maxes.length === 1) {
            ans.push(level - 1);
        } else {
            const [val0, max0, max1] = [maxes[0][1], maxes[0][0], maxes[1][0]];
            const max = val0 === q ? max1 : max0;
            ans.push(max);
        }
    }

    return ans;
}
```

#### JavaScript

```js
function treeQueries(root, queries) {
    const ans = [];
    const levels = new Map();
    const valToLevel = new Map();

    const dfs = (node, level = 0) => {
        if (!node) return level - 1;

        const max = Math.max(dfs(node.left, level + 1), dfs(node.right, level + 1));

        if (!levels.has(level)) {
            levels.set(level, []);
        }
        levels.get(level)?.push([max, node.val]);
        valToLevel.set(node.val, level);

        return max;
    };

    dfs(root, 0);

    for (const [_, l] of levels) {
        l.sort(([a], [b]) => b - a);
    }

    for (const q of queries) {
        const level = valToLevel.get(q);
        const maxes = levels.get(level);

        if (maxes.length === 1) {
            ans.push(level - 1);
        } else {
            const [val0, max0, max1] = [maxes[0][1], maxes[0][0], maxes[1][0]];
            const max = val0 === q ? max1 : max0;
            ans.push(max);
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
