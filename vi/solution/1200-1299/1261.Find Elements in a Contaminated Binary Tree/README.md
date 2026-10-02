---
comments: true
difficulty: Medium
rating: 1439
source: Weekly Contest 163 Q2
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Design
    - Hash Table
    - Binary Tree
---

<!-- problem:start -->

# [1261. Find Elements in a Contaminated Binary Tree](https://leetcode.com/problems/find-elements-in-a-contaminated-binary-tree)

[中文文档](/solution/1200-1299/1261.Find%20Elements%20in%20a%20Contaminated%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một cây nhị phân thỏa các quy tắc sau:</p>

<ol>
	<li><code>root.val == 0</code></li>
	<li>Với mọi <code>treeNode</code>:
	<ol type="a">
		<li>Nếu <code>treeNode.val</code> có giá trị <code>x</code> và <code>treeNode.left != null</code>, thì <code>treeNode.left.val == 2 * x + 1</code></li>
		<li>Nếu <code>treeNode.val</code> có giá trị <code>x</code> và <code>treeNode.right != null</code>, thì <code>treeNode.right.val == 2 * x + 2</code></li>
	</ol>
	</li>
</ol>

<p>Cây nhị phân hiện đã bị làm nhiễm bẩn, nghĩa là mọi <code>treeNode.val</code> đều bị đổi thành <code>-1</code>.</p>

<p>Hãy triển khai class <code>FindElements</code>:</p>

<ul>
	<li><code>FindElements(TreeNode* root)</code> Khởi tạo object bằng một cây nhị phân bị làm nhiễm bẩn và khôi phục cây.</li>
	<li><code>bool find(int target)</code> Trả về <code>true</code> nếu giá trị <code>target</code> tồn tại trong cây nhị phân đã khôi phục.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1261.Find%20Elements%20in%20a%20Contaminated%20Binary%20Tree/images/untitled-diagram-4-1.jpg" style="width: 320px; height: 119px;" />
<pre>
<strong>Input</strong>
[&quot;FindElements&quot;,&quot;find&quot;,&quot;find&quot;]
[[[-1,null,-1]],[1],[2]]
<strong>Output</strong>
[null,false,true]
<strong>Giải thích</strong>
FindElements findElements = new FindElements([-1,null,-1]); 
findElements.find(1); // return False 
findElements.find(2); // return True </pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1261.Find%20Elements%20in%20a%20Contaminated%20Binary%20Tree/images/untitled-diagram-4.jpg" style="width: 400px; height: 198px;" />
<pre>
<strong>Input</strong>
[&quot;FindElements&quot;,&quot;find&quot;,&quot;find&quot;,&quot;find&quot;]
[[[-1,-1,-1,-1,-1]],[1],[3],[5]]
<strong>Output</strong>
[null,true,true,false]
<strong>Giải thích</strong>
FindElements findElements = new FindElements([-1,-1,-1,-1,-1]);
findElements.find(1); // return True
findElements.find(3); // return True
findElements.find(5); // return False</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1261.Find%20Elements%20in%20a%20Contaminated%20Binary%20Tree/images/untitled-diagram-4-1-1.jpg" style="width: 306px; height: 274px;" />
<pre>
<strong>Input</strong>
[&quot;FindElements&quot;,&quot;find&quot;,&quot;find&quot;,&quot;find&quot;,&quot;find&quot;]
[[[-1,null,-1,-1,null,-1]],[2],[3],[4],[5]]
<strong>Output</strong>
[null,true,false,false,true]
<strong>Giải thích</strong>
FindElements findElements = new FindElements([-1,null,-1,-1,null,-1]);
findElements.find(2); // return True
findElements.find(3); // return False
findElements.find(4); // return False
findElements.find(5); // return True
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>TreeNode.val == -1</code></li>
	<li>Chiều cao của cây nhị phân không vượt quá <code>20</code></li>
	<li>Tổng số node nằm trong khoảng <code>[1, 10<sup>4</sup>]</code></li>
	<li>Tổng số lần gọi <code>find()</code> nằm trong khoảng <code>[1, 10<sup>4</sup>]</code></li>
	<li><code>0 &lt;= target &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi cây bị làm nhiễm bẩn, mọi giá trị đều là $-1$, nhưng các node con vẫn tuân theo $left=2x+1$ và $right=2x+2$, với root bằng $0$. Hàm $find$ có thể được gọi tới $10^4$ lần, nên không nên tính lại đường đi từ root đến node cho từng query.
>
> DFS lúc khởi tạo khôi phục mọi giá trị và lưu chúng vào hash set; $find$ chỉ cần tra cứu trong set. Duyệt một lần để mỗi query có thời gian hằng số.

<!-- thinking:end -->

Đầu tiên, ta dùng DFS duyệt cây nhị phân, khôi phục giá trị ban đầu cho các node và lưu toàn bộ giá trị vào hash table. Khi tìm kiếm, ta chỉ cần kiểm tra target có trong hash table hay không.

Độ phức tạp thời gian là $O(n)$ để duyệt cây nhị phân khi khởi tạo và $O(1)$ để kiểm tra target có trong hash table khi tìm kiếm. Độ phức tạp không gian là $O(n)$, trong đó $n$ là số node của cây nhị phân.

<!-- tabs:start -->

#### Python3

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class FindElements:

    def __init__(self, root: Optional[TreeNode]):
        def dfs(root: Optional[TreeNode]):
            self.s.add(root.val)
            if root.left:
                root.left.val = root.val * 2 + 1
                dfs(root.left)
            if root.right:
                root.right.val = root.val * 2 + 2
                dfs(root.right)

        root.val = 0
        self.s = set()
        dfs(root)

    def find(self, target: int) -> bool:
        return target in self.s


# Your FindElements object will be instantiated and called as such:
# obj = FindElements(root)
# param_1 = obj.find(target)
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
class FindElements {
    private Set<Integer> s = new HashSet<>();

    public FindElements(TreeNode root) {
        root.val = 0;
        dfs(root);
    }

    public boolean find(int target) {
        return s.contains(target);
    }

    private void dfs(TreeNode root) {
        s.add(root.val);
        if (root.left != null) {
            root.left.val = root.val * 2 + 1;
            dfs(root.left);
        }
        if (root.right != null) {
            root.right.val = root.val * 2 + 2;
            dfs(root.right);
        }
    }
}

/**
 * Your FindElements object will be instantiated and called as such:
 * FindElements obj = new FindElements(root);
 * boolean param_1 = obj.find(target);
 */
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
class FindElements {
public:
    FindElements(TreeNode* root) {
        root->val = 0;
        dfs(root);
    }

    bool find(int target) {
        return s.contains(target);
    }

private:
    unordered_set<int> s;

    void dfs(TreeNode* root) {
        s.insert(root->val);
        if (root->left) {
            root->left->val = root->val * 2 + 1;
            dfs(root->left);
        }
        if (root->right) {
            root->right->val = root->val * 2 + 2;
            dfs(root->right);
        }
    };
};

/**
 * Your FindElements object will be instantiated and called as such:
 * FindElements* obj = new FindElements(root);
 * bool param_1 = obj->find(target);
 */
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
type FindElements struct {
	s map[int]bool
}

func Constructor(root *TreeNode) FindElements {
	root.Val = 0
	s := map[int]bool{}
	var dfs func(*TreeNode)
	dfs = func(root *TreeNode) {
		s[root.Val] = true
		if root.Left != nil {
			root.Left.Val = root.Val*2 + 1
			dfs(root.Left)
		}
		if root.Right != nil {
			root.Right.Val = root.Val*2 + 2
			dfs(root.Right)
		}
	}
	dfs(root)
	return FindElements{s}
}

func (this *FindElements) Find(target int) bool {
	return this.s[target]
}

/**
 * Your FindElements object will be instantiated and called as such:
 * obj := Constructor(root);
 * param_1 := obj.Find(target);
 */
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

class FindElements {
    readonly #s = new Set<number>();

    constructor(root: TreeNode | null) {
        root.val = 0;

        const dfs = (node: TreeNode | null, x = 0) => {
            if (!node) return;

            this.#s.add(x);
            dfs(node.left, x * 2 + 1);
            dfs(node.right, x * 2 + 2);
        };

        dfs(root);
    }

    find(target: number): boolean {
        return this.#s.has(target);
    }
}

/**
 * Your FindElements object will be instantiated and called as such:
 * var obj = new FindElements(root)
 * var param_1 = obj.find(target)
 */
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

const s = Symbol.for('s');

/**
 * @param {TreeNode} root
 */
var FindElements = function (root) {
    root.val = 0;
    this[s] = new Set();

    const dfs = (node, x = 0) => {
        if (!node) return;

        this[s].add(x);
        dfs(node.left, x * 2 + 1);
        dfs(node.right, x * 2 + 2);
    };

    dfs(root);
};

/**
 * @param {number} target
 * @return {boolean}
 */
FindElements.prototype.find = function (target) {
    return this[s].has(target);
};

/**
 * Your FindElements object will be instantiated and called as such:
 * var obj = new FindElements(root)
 * var param_1 = obj.find(target)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
