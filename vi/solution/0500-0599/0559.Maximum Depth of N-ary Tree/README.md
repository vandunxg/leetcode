---
comments: true
difficulty: Easy
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
---

<!-- problem:start -->

# [559. Maximum Depth of N-ary Tree](https://leetcode.com/problems/maximum-depth-of-n-ary-tree)

[中文文档](/solution/0500-0599/0559.Maximum%20Depth%20of%20N-ary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một cây N-ary, hãy tìm độ sâu lớn nhất của cây.</p>

<p>Độ sâu lớn nhất là số node trên đường đi dài nhất từ node gốc đến node lá xa nhất.</p>

<p><em>Dữ liệu đầu vào của cây N-ary được biểu diễn theo thứ tự duyệt theo tầng; các nhóm node con được ngăn cách bằng giá trị null (xem ví dụ).</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0559.Maximum%20Depth%20of%20N-ary%20Tree/images/narytreeexample.png" style="width: 100%; max-width: 300px;" /></p>

<pre>
<strong>Đầu vào:</strong> root = [1,null,3,2,4,null,5,6]
<strong>Đầu ra:</strong> 3
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0559.Maximum%20Depth%20of%20N-ary%20Tree/images/sample_4_964.png" style="width: 296px; height: 241px;" /></p>

<pre>
<strong>Đầu vào:</strong> root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
<strong>Đầu ra:</strong> 5
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Tổng số node nằm trong khoảng <code>[0, 10<sup>4</sup>]</code>.</li>
	<li>Độ sâu của cây N-ary không vượt quá <code>1000</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Độ sâu của cây $N$-ary bằng một cộng với độ sâu lớn nhất trong các cây con. Có thể dùng cùng công thức truy hồi như với cây nhị phân.
>
> Node rỗng có độ sâu $0$. Đệ quy trên từng node con rồi lấy giá trị lớn nhất. Mỗi node chỉ được duyệt một lần.

<!-- thinking:end -->

Trước tiên, ta kiểm tra $\textit{root}$ có null hay không. Nếu có, trả về 0. Ngược lại, ta khởi tạo biến $\textit{mx}$ để lưu độ sâu lớn nhất của các node con, duyệt tất cả node con của $\textit{root}$, gọi đệ quy hàm $\text{maxDepth}$ rồi cập nhật $\textit{mx}$. Cuối cùng, trả về $\textit{mx} + 1$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val: Optional[int] = None, children: Optional[List['Node']] = None):
        self.val = val
        self.children = children
"""


class Solution:
    def maxDepth(self, root: "Node") -> int:
        if root is None:
            return 0
        mx = 0
        for child in root.children:
            mx = max(mx, self.maxDepth(child))
        return 1 + mx
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
    public int maxDepth(Node root) {
        if (root == null) {
            return 0;
        }
        int mx = 0;
        for (Node child : root.children) {
            mx = Math.max(mx, maxDepth(child));
        }
        return 1 + mx;
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
    int maxDepth(Node* root) {
        if (!root) {
            return 0;
        }
        int mx = 0;
        for (Node* child : root->children) {
            mx = max(mx, maxDepth(child));
        }
        return mx + 1;
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

func maxDepth(root *Node) int {
	if root == nil {
		return 0
	}
	mx := 0
	for _, child := range root.Children {
		mx = max(mx, maxDepth(child))
	}
	return 1 + mx
}
```

#### TypeScript

```ts
/**
 * Definition for _Node.
 * class _Node {
 *     val: number
 *     children: _Node[]
 *
 *     constructor(val?: number, children?: _Node[]) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.children = (children===undefined ? [] : children)
 *     }
 * }
 */

function maxDepth(root: _Node | null): number {
    if (!root) {
        return 0;
    }
    return 1 + Math.max(...root.children.map(child => maxDepth(child)), 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
