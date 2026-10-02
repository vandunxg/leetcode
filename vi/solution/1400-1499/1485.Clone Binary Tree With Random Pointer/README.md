---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Hash Table
    - Binary Tree
---

<!-- problem:start -->

# [1485. Clone Binary Tree With Random Pointer 🔒](https://leetcode.com/problems/clone-binary-tree-with-random-pointer)

[中文文档](/solution/1400-1499/1485.Clone%20Binary%20Tree%20With%20Random%20Pointer/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một cây nhị phân, trong đó mỗi node có thêm một con trỏ random có thể trỏ đến bất kỳ node nào trong cây hoặc <code>null</code>.</p>

<p>Hãy trả về một <a href="https://en.wikipedia.org/wiki/Object_copying#Deep_copy" target="_blank"><strong>bản sao sâu</strong></a> của cây.</p>

<p>Cây được biểu diễn theo cùng cách nhập/xuất như cây nhị phân thông thường, trong đó mỗi node được biểu diễn bởi một cặp <code>[val, random_index]</code> với:</p>

<ul>
	<li><code>val</code>: một số nguyên biểu diễn <code>Node.val</code></li>
	<li><code>random_index</code>: chỉ số của node (trong input) mà con trỏ random trỏ tới, hoặc <code>null</code> nếu nó không trỏ tới node nào.</li>
</ul>

<p>Bạn sẽ nhận được cây trong class <code>Node</code> và cần trả về cây đã clone trong class <code>NodeCopy</code>. Class <code>NodeCopy</code> chỉ là bản clone của class <code>Node</code> với cùng các thuộc tính và constructor.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1485.Clone%20Binary%20Tree%20With%20Random%20Pointer/images/clone_1.png" style="width: 500px; height: 473px;" />
<pre>
<strong>Đầu vào:</strong> root = [[1,null],null,[4,3],[7,0]]
<strong>Đầu ra:</strong> [[1,null],null,[4,3],[7,0]]
<strong>Giải thích:</strong> Cây nhị phân ban đầu là [1,null,4,7].
Con trỏ random của node một là null, nên nó được biểu diễn là [1, null].
Con trỏ random của node 4 là node 7, nên nó được biểu diễn là [4, 3], trong đó 3 là chỉ số của node 7 trong mảng biểu diễn cây.
Con trỏ random của node 7 là node 1, nên nó được biểu diễn là [7, 0], trong đó 0 là chỉ số của node 1 trong mảng biểu diễn cây.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1485.Clone%20Binary%20Tree%20With%20Random%20Pointer/images/clone_2.png" style="width: 500px; height: 540px;" />
<pre>
<strong>Đầu vào:</strong> root = [[1,4],null,[1,0],null,[1,5],[1,5]]
<strong>Đầu ra:</strong> [[1,4],null,[1,0],null,[1,5],[1,5]]
<strong>Giải thích:</strong> Con trỏ random của một node có thể trỏ tới chính node đó.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1485.Clone%20Binary%20Tree%20With%20Random%20Pointer/images/clone_3.png" style="width: 500px; height: 426px;" />
<pre>
<strong>Đầu vào:</strong> root = [[1,6],[2,5],[3,4],[4,3],[5,2],[6,1],[7,0]]
<strong>Đầu ra:</strong> [[1,6],[2,5],[3,4],[4,3],[5,2],[6,1],[7,0]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số lượng node trong <code>tree</code> nằm trong khoảng <code>[0, 1000].</code></li>
	<li><code>1 &lt;= Node.val &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + DFS

<!-- thinking:start -->

> **Tư duy**
>
> Ngoài left và right, node còn có một con trỏ $\textit{random}$ có thể tạo thành chu kỳ, vì vậy đệ quy đơn giản có thể lặp vô hạn hoặc sao chép một node nhiều lần.
>
> Ánh xạ mỗi node gốc với bản sao của nó: tạo bản sao trước, sau đó đệ quy trên left, right và $\textit{random}$. Nếu node đã có trong map, trả về bản sao đã tồn tại.

<!-- thinking:end -->

Chúng ta sử dụng một hash table $\textit{seen}$ để lưu quan hệ tương ứng giữa mỗi node trong cây ban đầu và bản sao của nó, sau đó thực hiện tìm kiếm theo chiều sâu.

Định nghĩa $\text{dfs}(root)$ trả về bản sao của $\textit{root}$:

- Nếu $\textit{root}$ là null, trả về null;
- Nếu $\textit{root}$ đã có trong $\textit{seen}$, trả về $\textit{seen}[root]$;
- Nếu không, tạo một node bản sao $\textit{copy}$, đặt $\textit{seen}[root] = copy$, sau đó đệ quy trên node con left, node con right và con trỏ $\textit{random}$;
- Cuối cùng trả về $\textit{copy}$.

Hàm chính trả về $\text{dfs}(root)$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là số lượng node.

<!-- tabs:start -->

#### Python3

```python
# Definition for Node.
# class Node:
#     def __init__(self, val=0, left=None, right=None, random=None):
#         self.val = val
#         self.left = left
#         self.right = right
#         self.random = random


class Solution:
    def copyRandomBinaryTree(self, root: "Optional[Node]") -> "Optional[NodeCopy]":
        def dfs(root: Optional[Node]) -> Optional[NodeCopy]:
            if root is None:
                return None
            if root in seen:
                return seen[root]
            copy = NodeCopy(root.val)
            seen[root] = copy
            copy.left = dfs(root.left)
            copy.right = dfs(root.right)
            copy.random = dfs(root.random)
            return copy

        seen = {}
        return dfs(root)
```

#### Java

```java
/**
 * Definition for Node.
 * public class Node {
 *     int val;
 *     Node left;
 *     Node right;
 *     Node random;
 *     Node() {}
 *     Node(int val) { this.val = val; }
 *     Node(int val, Node left, Node right, Node random) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *         this.random = random;
 *     }
 * }
 */

class Solution {
    private Map<Node, NodeCopy> seen;

    public NodeCopy copyRandomBinaryTree(Node root) {
        seen = new HashMap<>();
        return dfs(root);
    }

    private NodeCopy dfs(Node root) {
        if (root == null) {
            return null;
        }
        if (seen.containsKey(root)) {
            return seen.get(root);
        }
        NodeCopy copy = new NodeCopy(root.val);
        seen.put(root, copy);
        copy.left = dfs(root.left);
        copy.right = dfs(root.right);
        copy.random = dfs(root.random);
        return copy;
    }
}
```

#### C++

```cpp
/**
 * Definition for a Node.
 * struct Node {
 *     int val;
 *     Node *left;
 *     Node *right;
 *     Node *random;
 *     Node() : val(0), left(nullptr), right(nullptr), random(nullptr) {}
 *     Node(int x) : val(x), left(nullptr), right(nullptr), random(nullptr) {}
 *     Node(int x, Node *left, Node *right, Node *random) : val(x), left(left), right(right), random(random) {}
 * };
 */

class Solution {
public:
    NodeCopy* copyRandomBinaryTree(Node* root) {
        unordered_map<Node*, NodeCopy*> seen;
        auto dfs = [&](this auto&& dfs, Node* root) -> NodeCopy* {
            if (!root) {
                return nullptr;
            }
            if (seen.contains(root)) {
                return seen[root];
            }
            NodeCopy* copy = new NodeCopy(root->val);
            seen[root] = copy;
            copy->left = dfs(root->left);
            copy->right = dfs(root->right);
            copy->random = dfs(root->random);
            return copy;
        };
        return dfs(root);
    }
};
```

#### Go

```go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Left *Node
 *     Right *Node
 *     Random *Node
 * }
 */

func copyRandomBinaryTree(root *Node) *NodeCopy {
	seen := make(map[*Node]*NodeCopy)
	var dfs func(root *Node) *NodeCopy
	dfs = func(root *Node) *NodeCopy {
		if root == nil {
			return nil
		}
		if v, ok := seen[root]; ok {
			return v
		}
		copy := &NodeCopy{Val: root.Val}
		seen[root] = copy
		copy.Left = dfs(root.Left)
		copy.Right = dfs(root.Right)
		copy.Random = dfs(root.Random)
		return copy
	}
	return dfs(root)
}
```

#### TypeScript

```ts
/**
 * Definition for Node.
 * class Node {
 *     val: number
 *     left: Node | null
 *     right: Node | null
 *     random: Node | null
 *     constructor(val?: number, left?: Node | null, right?: Node | null, random?: Node | null) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.left = (left===undefined ? null : left)
 *         this.right = (right===undefined ? null : right)
 *         this.random = (random===undefined ? null : random)
 *     }
 * }
 */

function copyRandomBinaryTree(root: Node | null): NodeCopy | null {
    const seen = new Map<Node, NodeCopy>();
    const dfs = (root: Node | null): NodeCopy | null => {
        if (root === null) {
            return null;
        }
        if (seen.has(root)) {
            return seen.get(root)!;
        }
        const copy = new NodeCopy(root.val);
        seen.set(root, copy);
        copy.left = dfs(root.left);
        copy.right = dfs(root.right);
        copy.random = dfs(root.random);
        return copy;
    };
    return dfs(root);
}
```

#### C#

```cs
/*
// Definition for a Node.
public class Node {
    public int val;
    public Node left;
    public Node right;
    public Node random;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, Node _left, Node _right, Node _random) {
        val = _val;
        left = _left;
        right = _right;
        random = _random;
    }
}
*/

public class Solution {
    public NodeCopy CopyRandomBinaryTree(Node root) {
        var seen = new Dictionary<Node, NodeCopy>();
        NodeCopy Dfs(Node root) {
            if (root == null) {
                return null;
            }
            if (seen.ContainsKey(root)) {
                return seen[root];
            }
            var copy = new NodeCopy(root.val);
            seen[root] = copy;
            copy.left = Dfs(root.left);
            copy.right = Dfs(root.right);
            copy.random = Dfs(root.random);
            return copy;
        }
        return Dfs(root);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
