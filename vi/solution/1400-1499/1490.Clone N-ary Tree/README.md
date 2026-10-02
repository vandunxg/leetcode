---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Hash Table
---

<!-- problem:start -->

# [1490. Clone N-ary Tree 🔒](https://leetcode.com/problems/clone-n-ary-tree)

[中文文档](/solution/1400-1499/1490.Clone%20N-ary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một cây N-ary, hãy trả về một <a href="https://en.wikipedia.org/wiki/Object_copying#Deep_copy" target="_blank"><strong>bản sao sâu</strong></a> (clone) của cây.</p>

<p>Mỗi node trong cây n-ary chứa một giá trị val (<code>int</code>) và một danh sách (<code>List[Node]</code>) các node con.</p>

<pre>
class Node {
    public int val;
    public List&lt;Node&gt; children;
}
</pre>

<p>Tuần tự hóa đầu vào của cây N-ary được biểu diễn bằng phép duyệt theo thứ tự level, trong đó mỗi nhóm node con được phân tách bằng giá trị null (xem các ví dụ).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1490.Clone%20N-ary%20Tree/images/narytreeexample.png" style="width: 100%; max-width: 300px;" /></p>

<pre>
<strong>Đầu vào:</strong> root = [1,null,3,2,4,null,5,6]
<strong>Đầu ra:</strong> [1,null,3,2,4,null,5,6]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1490.Clone%20N-ary%20Tree/images/sample_4_964.png" style="width: 296px; height: 241px;" /></p>

<pre>
<strong>Đầu vào:</strong> root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
<strong>Đầu ra:</strong> [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Độ sâu của cây n-ary nhỏ hơn hoặc bằng <code>1000</code>.</li>
	<li>Tổng số node nằm trong khoảng <code>[0, 10<sup>4</sup>]</code>.</li>
</ul>

<p>&nbsp;</p>
<strong>Câu hỏi mở rộng: </strong>Lời giải của bạn có thể áp dụng cho <a href="https://leetcode.com/problems/clone-graph/" target="_blank">bài toán graph</a> không?

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Vì không có random pointer, bản sao sâu chỉ cần sao chép giá trị rồi đệ quy trên từng node con. Có nhiều nhất $10^4$ node nên chỉ cần duyệt cây một lần. Nếu root là null thì trả về null.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children if children is not None else []
"""


class Solution:
    def cloneTree(self, root: 'Node') -> 'Node':
        if root is None:
            return None
        children = [self.cloneTree(child) for child in root.children]
        return Node(root.val, children)
```

#### Java

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;


    public Node() {
        children = new ArrayList<Node>();
    }

    public Node(int _val) {
        val = _val;
        children = new ArrayList<Node>();
    }

    public Node(int _val,ArrayList<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
    public Node cloneTree(Node root) {
        if (root == null) {
            return null;
        }
        ArrayList<Node> children = new ArrayList<>();
        for (Node child : root.children) {
            children.add(cloneTree(child));
        }
        return new Node(root.val, children);
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
    Node* cloneTree(Node* root) {
        if (!root) {
            return root;
        }
        vector<Node*> children;
        for (Node* child : root->children) {
            children.emplace_back(cloneTree(child));
        }
        return new Node(root->val, children);
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

func cloneTree(root *Node) *Node {
	if root == nil {
		return nil
	}
	children := []*Node{}
	for _, child := range root.Children {
		children = append(children, cloneTree(child))
	}
	return &Node{root.Val, children}
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
