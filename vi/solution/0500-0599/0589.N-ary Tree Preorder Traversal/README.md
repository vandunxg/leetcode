---
comments: true
difficulty: Easy
tags:
    - Stack
    - Tree
    - Depth-First Search
---

<!-- problem:start -->

# [589. N-ary Tree Preorder Traversal](https://leetcode.com/problems/n-ary-tree-preorder-traversal)

[中文文档](/solution/0500-0599/0589.N-ary%20Tree%20Preorder%20Traversal/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của cây N-ary, hãy trả về <em>các giá trị node theo thứ tự duyệt preorder</em>.</p>

<p>Dữ liệu đầu vào của cây N-ary được biểu diễn theo thứ tự duyệt từng level. Các nhóm node con được ngăn cách bằng giá trị null (xem ví dụ).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0589.N-ary%20Tree%20Preorder%20Traversal/images/narytreeexample.png" style="width: 100%; max-width: 300px;" /></p>

<pre>
<strong>Đầu vào:</strong> root = [1,null,3,2,4,null,5,6]
<strong>Đầu ra:</strong> [1,3,5,6,2,4]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0589.N-ary%20Tree%20Preorder%20Traversal/images/sample_4_964.png" style="width: 296px; height: 241px;" /></p>

<pre>
<strong>Đầu vào:</strong> root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
<strong>Đầu ra:</strong> [1,2,3,6,7,11,14,4,8,12,5,9,13,10]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[0, 10<sup>4</sup>]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 10<sup>4</sup></code></li>
	<li>Chiều cao của cây N-ary không vượt quá <code>1000</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Lời giải đệ quy khá đơn giản; bạn có thể giải bài này bằng cách lặp không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Preorder của cây N-ary duyệt root trước, sau đó đến các node con từ trái sang phải. Đệ quy khớp trực tiếp với định nghĩa này.
>
> Thêm root vào kết quả, rồi đệ quy trên từng node con. Cây rỗng trả về danh sách rỗng. Mỗi node chỉ được duyệt một lần.

<!-- thinking:end -->

Ta có thể duyệt toàn bộ cây bằng đệ quy. Với mỗi node, trước tiên thêm giá trị của node vào kết quả, sau đó gọi đệ quy cho từng node con.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node trong cây.

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
    def preorder(self, root: "Node") -> List[int]:
        def dfs(root):
            if root is None:
                return
            ans.append(root.val)
            for child in root.children:
                dfs(child)

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

    public List<Integer> preorder(Node root) {
        dfs(root);
        return ans;
    }

    private void dfs(Node root) {
        if (root == null) {
            return;
        }
        ans.add(root.val);
        for (Node child : root.children) {
            dfs(child);
        }
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
    vector<int> preorder(Node* root) {
        vector<int> ans;
        function<void(Node*)> dfs = [&](Node* root) {
            if (!root) {
                return;
            }
            ans.push_back(root->val);
            for (auto& child : root->children) {
                dfs(child);
            }
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

func preorder(root *Node) (ans []int) {
	var dfs func(*Node)
	dfs = func(root *Node) {
		if root == nil {
			return
		}
		ans = append(ans, root.Val)
		for _, child := range root.Children {
			dfs(child)
		}
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

function preorder(root: Node | null): number[] {
    const ans: number[] = [];
    const dfs = (root: Node | null) => {
        if (!root) {
            return;
        }
        ans.push(root.val);
        for (const child of root.children) {
            dfs(child);
        }
    };
    dfs(root);
    return ans;
}
```

#### C

```c
/**
 * Definition for a Node.
 * struct Node {
 *     int val;
 *     int numChildren;
 *     struct Node** children;
 * };
 */

/**
 * Note: The returned array must be malloced, assume caller calls free().
 */

void dfs(struct Node* root, int* ans, int* i) {
    if (!root) {
        return;
    }
    ans[(*i)++] = root->val;
    for (int j = 0; j < root->numChildren; j++) {
        dfs(root->children[j], ans, i);
    }
}

int* preorder(struct Node* root, int* returnSize) {
    int* ans = malloc(sizeof(int) * 10000);
    *returnSize = 0;
    dfs(root, ans, returnSize);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Lặp (dùng Stack)

<!-- thinking:start -->

> **Tư duy**
>
> Đệ quy sử dụng call stack. Với stack tường minh, cần push các node con từ phải sang trái để node con ngoài cùng bên trái được pop trước.
>
> Pop node ra và đưa vào kết quả, sau đó push các node con theo thứ tự ngược. Cách này giữ đúng thứ tự preorder như đệ quy mà không bị giới hạn call stack.

<!-- thinking:end -->

Ta cũng có thể giải bài này bằng cách lặp.

Ta dùng stack để thực hiện preorder. Trước tiên, push root vào stack. Vì preorder duyệt root rồi đến các cây con từ trái sang phải, còn stack có thứ tự vào sau ra trước, ta thêm giá trị node hiện tại vào kết quả rồi push các node con từ phải sang trái. Lặp lại cho đến khi stack rỗng.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node trong cây.

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
    def preorder(self, root: 'Node') -> List[int]:
        ans = []
        if root is None:
            return ans
        stk = [root]
        while stk:
            node = stk.pop()
            ans.append(node.val)
            for child in node.children[::-1]:
                stk.append(child)
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
    public List<Integer> preorder(Node root) {
        if (root == null) {
            return Collections.emptyList();
        }
        List<Integer> ans = new ArrayList<>();
        Deque<Node> stk = new ArrayDeque<>();
        stk.push(root);
        while (!stk.isEmpty()) {
            Node node = stk.pop();
            ans.add(node.val);
            List<Node> children = node.children;
            for (int i = children.size() - 1; i >= 0; --i) {
                stk.push(children.get(i));
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
    vector<int> preorder(Node* root) {
        if (!root) return {};
        vector<int> ans;
        stack<Node*> stk;
        stk.push(root);
        while (!stk.empty()) {
            Node* node = stk.top();
            ans.push_back(node->val);
            stk.pop();
            auto children = node->children;
            for (int i = children.size() - 1; i >= 0; --i) stk.push(children[i]);
        }
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

func preorder(root *Node) (ans []int) {
	if root == nil {
		return
	}
	stk := []*Node{root}
	for len(stk) > 0 {
		node := stk[len(stk)-1]
		ans = append(ans, node.Val)
		stk = stk[:len(stk)-1]
		children := node.Children
		for i := len(children) - 1; i >= 0; i-- {
			stk = append(stk, children[i])
		}
	}
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

function preorder(root: Node | null): number[] {
    const ans: number[] = [];
    if (!root) {
        return ans;
    }
    const stk: Node[] = [root];
    while (stk.length) {
        const { val, children } = stk.pop()!;
        ans.push(val);
        for (let i = children.length - 1; i >= 0; i--) {
            stk.push(children[i]);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
