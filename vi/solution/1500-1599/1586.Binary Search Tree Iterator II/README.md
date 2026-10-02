---
comments: true
difficulty: Medium
tags:
    - Stack
    - Tree
    - Design
    - Binary Search Tree
    - Binary Tree
    - Iterator
---

<!-- problem:start -->

# [1586. Binary Search Tree Iterator II 🔒](https://leetcode.com/problems/binary-search-tree-iterator-ii)

[中文文档](/solution/1500-1599/1586.Binary%20Search%20Tree%20Iterator%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Triển khai lớp <code>BSTIterator</code> biểu diễn iterator duyệt <strong><a href="https://en.wikipedia.org/wiki/Tree_traversal#In-order_(LNR)">in-order</a></strong> trên cây tìm kiếm nhị phân (BST):</p>

<ul>
	<li><code>BSTIterator(TreeNode root)</code> Khởi tạo đối tượng lớp <code>BSTIterator</code>. <code>root</code> của BST được truyền vào constructor. Con trỏ phải được khởi tạo tại một số không tồn tại và nhỏ hơn mọi phần tử trong BST.</li>
	<li><code>boolean hasNext()</code> Trả về <code>true</code> nếu có số nằm bên phải con trỏ trong phép duyệt, ngược lại trả về <code>false</code>.</li>
	<li><code>int next()</code> Di chuyển con trỏ sang phải rồi trả về số tại vị trí đó.</li>
	<li><code>boolean hasPrev()</code> Trả về <code>true</code> nếu có số nằm bên trái con trỏ trong phép duyệt, ngược lại trả về <code>false</code>.</li>
	<li><code>int prev()</code> Di chuyển con trỏ sang trái rồi trả về số tại vị trí đó.</li>
</ul>

<p>Lưu ý rằng vì con trỏ được khởi tạo tại một số nhỏ nhất không tồn tại, lần gọi <code>next()</code> đầu tiên sẽ trả về phần tử nhỏ nhất trong BST.</p>

<p>Có thể giả sử các lần gọi <code>next()</code> và <code>prev()</code> luôn hợp lệ. Nghĩa là luôn có ít nhất một số tiếp theo/trước đó trong phép duyệt in-order khi gọi <code>next()</code>/<code>prev()</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1586.Binary%20Search%20Tree%20Iterator%20II/images/untitled-diagram-1.png" style="width: 201px; height: 201px;" /></strong></p>

<pre>
<strong>Input</strong>
[&quot;BSTIterator&quot;, &quot;next&quot;, &quot;next&quot;, &quot;prev&quot;, &quot;next&quot;, &quot;hasNext&quot;, &quot;next&quot;, &quot;next&quot;, &quot;next&quot;, &quot;hasNext&quot;, &quot;hasPrev&quot;, &quot;prev&quot;, &quot;prev&quot;]
[[[7, 3, 15, null, null, 9, 20]], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null]]
<strong>Output</strong>
[null, 3, 7, 3, 7, true, 9, 15, 20, false, true, 15, 9]

<strong>Giải thích</strong>
// The underlined element is where the pointer currently is.
BSTIterator bSTIterator = new BSTIterator([7, 3, 15, null, null, 9, 20]); // state is <u> </u> [3, 7, 9, 15, 20]
bSTIterator.next(); // state becomes [<u>3</u>, 7, 9, 15, 20], return 3
bSTIterator.next(); // state becomes [3, <u>7</u>, 9, 15, 20], return 7
bSTIterator.prev(); // state becomes [<u>3</u>, 7, 9, 15, 20], return 3
bSTIterator.next(); // state becomes [3, <u>7</u>, 9, 15, 20], return 7
bSTIterator.hasNext(); // return true
bSTIterator.next(); // state becomes [3, 7, <u>9</u>, 15, 20], return 9
bSTIterator.next(); // state becomes [3, 7, 9, <u>15</u>, 20], return 15
bSTIterator.next(); // state becomes [3, 7, 9, 15, <u>20</u>], return 20
bSTIterator.hasNext(); // return false
bSTIterator.hasPrev(); // return true
bSTIterator.prev(); // state becomes [3, 7, 9, <u>15</u>, 20], return 15
bSTIterator.prev(); // state becomes [3, 7, <u>9</u>, 15, 20], return 9
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số nút trong cây nằm trong khoảng <code>[1, 10<sup>5</sup>]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 10<sup>6</sup></code></li>
	<li>Có nhiều nhất <code>10<sup>5</sup></code> lần gọi các hàm <code>hasNext</code>, <code>next</code>, <code>hasPrev</code> và <code>prev</code>.</li>
</ul>

<p>&nbsp;</p>
<strong>Câu hỏi mở rộng:</strong> Bạn có thể giải bài toán mà không tính trước các giá trị của cây không?

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt In-order + Mảng

<!-- thinking:start -->

> **Tư duy**
>
> Iterator phải hỗ trợ cả $next$ và $prev$ trong tối đa $10^5$ lần gọi. Một stack chỉ lưu đường đi đến nút hiện tại có thể đi tới, nhưng đi lùi cần tìm predecessor.
>
> Phép duyệt inorder trải phẳng BST thành một mảng đã sắp xếp; chỉ số $i$ là con trỏ. Next và prev trở thành $i\pm 1$, còn việc kiểm tra phần tử tồn tại chỉ cần kiểm tra biên. Khởi tạo mất thời gian tuyến tính; mỗi lần gọi sau đó có thời gian hằng số.

<!-- thinking:end -->

Ta có thể dùng phép duyệt in-order để lưu giá trị của mọi nút trong cây tìm kiếm nhị phân vào mảng $nums$, rồi dùng mảng này để triển khai iterator. Ta định nghĩa con trỏ $i$, ban đầu $i = -1$, trỏ đến một phần tử trong mảng $nums$. Mỗi lần gọi $next()$, ta tăng $i$ lên $1$ và trả về $nums[i]$; mỗi lần gọi $prev()$, ta giảm $i$ đi $1$ và trả về $nums[i]$.

Về độ phức tạp thời gian, khởi tạo iterator cần $O(n)$, trong đó $n$ là số nút của cây tìm kiếm nhị phân. Mỗi lần gọi $next()$ hoặc $prev()$ cần $O(1)$. Độ phức tạp không gian là $O(n)$ để lưu giá trị của mọi nút.

<!-- tabs:start -->

#### Python3

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class BSTIterator:
    def __init__(self, root: Optional[TreeNode]):
        self.nums = []

        def dfs(root):
            if root is None:
                return
            dfs(root.left)
            self.nums.append(root.val)
            dfs(root.right)

        dfs(root)
        self.i = -1

    def hasNext(self) -> bool:
        return self.i < len(self.nums) - 1

    def next(self) -> int:
        self.i += 1
        return self.nums[self.i]

    def hasPrev(self) -> bool:
        return self.i > 0

    def prev(self) -> int:
        self.i -= 1
        return self.nums[self.i]


# Your BSTIterator object will be instantiated and called as such:
# obj = BSTIterator(root)
# param_1 = obj.hasNext()
# param_2 = obj.next()
# param_3 = obj.hasPrev()
# param_4 = obj.prev()
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
class BSTIterator {
    private List<Integer> nums = new ArrayList<>();
    private int i = -1;

    public BSTIterator(TreeNode root) {
        dfs(root);
    }

    public boolean hasNext() {
        return i < nums.size() - 1;
    }

    public int next() {
        return nums.get(++i);
    }

    public boolean hasPrev() {
        return i > 0;
    }

    public int prev() {
        return nums.get(--i);
    }

    private void dfs(TreeNode root) {
        if (root == null) {
            return;
        }
        dfs(root.left);
        nums.add(root.val);
        dfs(root.right);
    }
}

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator obj = new BSTIterator(root);
 * boolean param_1 = obj.hasNext();
 * int param_2 = obj.next();
 * boolean param_3 = obj.hasPrev();
 * int param_4 = obj.prev();
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
class BSTIterator {
public:
    BSTIterator(TreeNode* root) {
        dfs(root);
        n = nums.size();
    }

    bool hasNext() {
        return i < n - 1;
    }

    int next() {
        return nums[++i];
    }

    bool hasPrev() {
        return i > 0;
    }

    int prev() {
        return nums[--i];
    }

private:
    vector<int> nums;
    int i = -1;
    int n;

    void dfs(TreeNode* root) {
        if (!root) {
            return;
        }
        dfs(root->left);
        nums.push_back(root->val);
        dfs(root->right);
    }
};

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator* obj = new BSTIterator(root);
 * bool param_1 = obj->hasNext();
 * int param_2 = obj->next();
 * bool param_3 = obj->hasPrev();
 * int param_4 = obj->prev();
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
type BSTIterator struct {
	nums []int
	i, n int
}

func Constructor(root *TreeNode) BSTIterator {
	nums := []int{}
	var dfs func(*TreeNode)
	dfs = func(root *TreeNode) {
		if root == nil {
			return
		}
		dfs(root.Left)
		nums = append(nums, root.Val)
		dfs(root.Right)
	}
	dfs(root)
	return BSTIterator{nums, -1, len(nums)}
}

func (this *BSTIterator) HasNext() bool {
	return this.i < this.n-1
}

func (this *BSTIterator) Next() int {
	this.i++
	return this.nums[this.i]
}

func (this *BSTIterator) HasPrev() bool {
	return this.i > 0
}

func (this *BSTIterator) Prev() int {
	this.i--
	return this.nums[this.i]
}

/**
 * Your BSTIterator object will be instantiated and called as such:
 * obj := Constructor(root);
 * param_1 := obj.HasNext();
 * param_2 := obj.Next();
 * param_3 := obj.HasPrev();
 * param_4 := obj.Prev();
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

class BSTIterator {
    private nums: number[];
    private n: number;
    private i: number;

    constructor(root: TreeNode | null) {
        this.nums = [];
        const dfs = (root: TreeNode | null) => {
            if (!root) {
                return;
            }
            dfs(root.left);
            this.nums.push(root.val);
            dfs(root.right);
        };
        dfs(root);
        this.n = this.nums.length;
        this.i = -1;
    }

    hasNext(): boolean {
        return this.i < this.n - 1;
    }

    next(): number {
        return this.nums[++this.i];
    }

    hasPrev(): boolean {
        return this.i > 0;
    }

    prev(): number {
        return this.nums[--this.i];
    }
}

/**
 * Your BSTIterator object will be instantiated and called as such:
 * var obj = new BSTIterator(root)
 * var param_1 = obj.hasNext()
 * var param_2 = obj.next()
 * var param_3 = obj.hasPrev()
 * var param_4 = obj.prev()
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
