---
comments: true
difficulty: Easy
tags:
    - Stack
    - Tree
    - Depth-First Search
---

<!-- problem:start -->

# [590. N-ary Tree Postorder Traversal](https://leetcode.com/problems/n-ary-tree-postorder-traversal)

[中文文档](/solution/0500-0599/0590.N-ary%20Tree%20Postorder%20Traversal/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của cây n-ary, hãy trả về <em>các giá trị node theo thứ tự duyệt hậu tự</em>.</p>

<p>Dữ liệu đầu vào của cây n-ary được serialize theo thứ tự duyệt level. Mỗi nhóm node con được ngăn cách bằng giá trị null (xem ví dụ).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0590.N-ary%20Tree%20Postorder%20Traversal/images/narytreeexample.png" style="width: 100%; max-width: 300px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,null,3,2,4,null,5,6]
<strong>Đầu ra:</strong> [5,6,3,2,4,1]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0590.N-ary%20Tree%20Postorder%20Traversal/images/sample_4_964.png" style="width: 296px; height: 241px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
<strong>Đầu ra:</strong> [2,6,14,11,7,3,12,8,4,13,9,10,5,1]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số lượng node trong cây nằm trong khoảng <code>[0, 10<sup>4</sup>]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 10<sup>4</sup></code></li>
	<li>Chiều cao của cây n-ary không vượt quá <code>1000</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Lời giải đệ quy khá đơn giản, bạn có thể giải bài toán bằng cách lặp không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Duyệt hậu tự cây n-ary là duyệt các node con từ trái sang phải, sau đó mới đến root. Đệ quy tự nhiên sẽ duyệt các node con trước.
>
> Gọi đệ quy trên từng node con, rồi thêm root vào kết quả. Cây rỗng trả về danh sách rỗng.

<!-- thinking:end -->

Ta có thể duyệt toàn bộ cây bằng đệ quy. Với mỗi node, trước tiên gọi đệ quy trên từng node con, sau đó thêm giá trị của node vào đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children
"""


class Solution:
    def postorder(self, root: 'Node') -> List[int]:
        def dfs(root):
            if root is None:
                return
            for child in root.children:
                dfs(child)
            ans.append(root.val)

        ans = []
        dfs(root)
        return ans
```

#### Java

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
    private List<Integer> ans = new ArrayList<>();

    public List<Integer> postorder(Node root) {
        dfs(root);
        return ans;
    }

    private void dfs(Node root) {
        if (root == null) {
            return;
        }
        for (Node child : root.children) {
            dfs(child);
        }
        ans.add(root.val);
    }
}
```

#### C++

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
public:
    vector<int> postorder(Node* root) {
        vector<int> ans;
        function<void(Node*)> dfs = [&](Node* root) {
            if (!root) {
                return;
            }
            for (auto& child : root->children) {
                dfs(child);
            }
            ans.push_back(root->val);
        };
        dfs(root);
        return ans;
    }
};
```

#### Go

```go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Children []*Node
 * }
 */

func postorder(root *Node) (ans []int) {
	var dfs func(*Node)
	dfs = func(root *Node) {
		if root == nil {
			return
		}
		for _, child := range root.Children {
			dfs(child)
		}
		ans = append(ans, root.Val)
	}
	dfs(root)
	return
}
```

#### TypeScript

```ts
/**
 * Definition for node.
 * class Node {
 *     val: number
 *     children: Node[]
 *     constructor(val?: number) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.children = []
 *     }
 * }
 */

function postorder(root: Node | null): number[] {
    const ans: number[] = [];
    const dfs = (root: Node | null) => {
        if (!root) {
            return;
        }
        for (const child of root.children) {
            dfs(child);
        }
        ans.push(root.val);
    };
    dfs(root);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Lặp (dùng stack)

<!-- thinking:start -->

> **Tư duy**
>
> Duyệt hậu tự bằng đệ quy cũng bị giới hạn bởi độ sâu. Nếu dùng stack, lấy root ra trước rồi push các node con từ trái sang phải, ta thu được thứ tự root-phải-trái; đảo ngược danh sách này sẽ thành trái-phải-root.
>
> Đây là mẹo đảo chiều preorder rồi đảo ngược kết quả. Chỉ cần một stack và mảng đáp án.

<!-- thinking:end -->

Ta cũng có thể giải bài toán bằng cách lặp.

Ta dùng stack để thực hiện duyệt hậu tự. Trước tiên, push root vào stack. Vì thứ tự hậu tự là cây con trái, cây con phải rồi đến root, còn stack hoạt động theo nguyên tắc vào trước ra sau, ta thêm giá trị node vào đáp án trước rồi push các node con từ trái sang phải. Như vậy, thứ tự thu được là root, cây con phải, cây con trái. Cuối cùng, đảo ngược đáp án để có kết quả duyệt hậu tự.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children
"""


class Solution:
    def postorder(self, root: 'Node') -> List[int]:
        ans = []
        if root is None:
            return ans
        stk = [root]
        while stk:
            node = stk.pop()
            ans.append(node.val)
            for child in node.children:
                stk.append(child)
        return ans[::-1]
```

#### Java

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
    public List<Integer> postorder(Node root) {
        LinkedList<Integer> ans = new LinkedList<>();
        if (root == null) {
            return ans;
        }
        Deque<Node> stk = new ArrayDeque<>();
        stk.offer(root);
        while (!stk.isEmpty()) {
            root = stk.pollLast();
            ans.addFirst(root.val);
            for (Node child : root.children) {
                stk.offer(child);
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
public:
    vector<int> postorder(Node* root) {
        vector<int> ans;
        if (!root) {
            return ans;
        }
        stack<Node*> stk{{root}};
        while (!stk.empty()) {
            root = stk.top();
            ans.push_back(root->val);
            stk.pop();
            for (Node* child : root->children) {
                stk.push(child);
            }
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```

#### Go

```go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Children []*Node
 * }
 */

func postorder(root *Node) []int {
	var ans []int
	if root == nil {
		return ans
	}
	stk := []*Node{root}
	for len(stk) > 0 {
		root = stk[len(stk)-1]
		stk = stk[:len(stk)-1]
		ans = append([]int{root.Val}, ans...)
		for _, child := range root.Children {
			stk = append(stk, child)
		}
	}
	return ans
}
```

#### TypeScript

```ts
/**
 * Definition for node.
 * class Node {
 *     val: number
 *     children: Node[]
 *     constructor(val?: number) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.children = []
 *     }
 * }
 */

function postorder(root: Node | null): number[] {
    const ans: number[] = [];
    if (!root) {
        return ans;
    }
    const stk: Node[] = [root];
    while (stk.length) {
        const { val, children } = stk.pop()!;
        ans.push(val);
        stk.push(...children);
    }
    return ans.reverse();
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
