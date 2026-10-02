---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Tree
    - Depth-First Search
    - Hash Table
---

<!-- problem:start -->

# [1506. Find Root of N-Ary Tree 🔒](https://leetcode.com/problems/find-root-of-n-ary-tree)

[中文文档](/solution/1500-1599/1506.Find%20Root%20of%20N-Ary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho tất cả các node của một <strong><a href="https://leetcode.com/explore/learn/card/n-ary-tree/">cây N-ary</a></strong> dưới dạng một mảng các đối tượng <code>Node</code>, trong đó mỗi node có một <strong>giá trị duy nhất</strong>.</p>

<p>Hãy trả về <em><strong>root</strong> của cây N-ary</em>.</p>

<p><strong>Kiểm thử tùy chỉnh:</strong></p>

<p>Một cây N-ary có thể được tuần tự hóa theo biểu diễn của phép duyệt theo level order, trong đó mỗi nhóm node con được ngăn cách bằng giá trị <code>null</code> (xem các ví dụ).</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1506.Find%20Root%20of%20N-Ary%20Tree/images/sample_4_964.png" style="width: 296px; height: 241px;" /></p>

<p>Ví dụ, cây ở trên được tuần tự hóa thành <code>[1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]</code>.</p>

<p>Việc kiểm thử sẽ được thực hiện như sau:</p>

<ol>
	<li><strong>Dữ liệu đầu vào</strong> phải được cung cấp dưới dạng tuần tự hóa của cây.</li>
	<li>Code điều khiển sẽ dựng cây từ dữ liệu đầu vào đã tuần tự hóa và đưa mỗi đối tượng <code>Node</code> vào một mảng theo <strong>thứ tự bất kỳ</strong>.</li>
	<li>Code điều khiển sẽ truyền mảng đó vào <code>findRoot</code>, và hàm của bạn phải tìm rồi trả về đối tượng <code>Node</code> root trong mảng.</li>
	<li>Code điều khiển sẽ lấy đối tượng <code>Node</code> được trả về và tuần tự hóa nó. Nếu giá trị đã tuần tự hóa và dữ liệu đầu vào <strong>giống nhau</strong>, bài kiểm thử <strong>đạt</strong>.</li>
</ol>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1506.Find%20Root%20of%20N-Ary%20Tree/images/narytreeexample.png" style="width: 100%; max-width: 300px;" /></p>

<pre>
<strong>Đầu vào:</strong> tree = [1,null,3,2,4,null,5,6]
<strong>Đầu ra:</strong> [1,null,3,2,4,null,5,6]
<strong>Giải thích:</strong> Cây từ dữ liệu đầu vào được hiển thị ở trên.
Code điều khiển tạo cây và cung cấp cho findRoot các đối tượng Node theo thứ tự bất kỳ.
Ví dụ, mảng được truyền vào có thể là [Node(5),Node(4),Node(3),Node(6),Node(2),Node(1)] hoặc [Node(2),Node(6),Node(1),Node(3),Node(5),Node(4)].
Hàm findRoot phải trả về root Node(1), sau đó code điều khiển sẽ tuần tự hóa nó và so sánh với dữ liệu đầu vào.
Dữ liệu đầu vào và Node(1) đã tuần tự hóa giống nhau, nên bài kiểm thử đạt.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1506.Find%20Root%20of%20N-Ary%20Tree/images/sample_4_964.png" style="width: 296px; height: 241px;" /></p>

<pre>
<strong>Đầu vào:</strong> tree = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
<strong>Đầu ra:</strong> [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Tổng số node nằm trong khoảng <code>[1, 5 * 10<sup>4</sup>]</code>.</li>
	<li>Mỗi node có một giá trị <strong>duy nhất</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong></p>

<ul>
	<li>Bạn có thể giải bài toán này với độ phức tạp không gian hằng số và thuật toán thời gian tuyến tính không?</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cho trước mọi node của một cây $N$-ary, chúng ta cần khôi phục root. Root xuất hiện một lần trong danh sách; mọi node khác cũng xuất hiện với tư cách là node con của một node nào đó. Lưu tất cả node con vào một hash set rồi lấy node không có trong set sẽ cho kết quả đúng, nhưng sử dụng thêm không gian tuyến tính.
>
> Các giá trị không phải root xuất hiện số lần chẵn còn root xuất hiện số lần lẻ, nên XOR sẽ triệt tiêu các cặp. XOR giá trị của mọi node với giá trị của tất cả node con, sau đó tìm node có giá trị bằng kết quả XOR đó. Không gian bổ sung là hằng số.

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
    def findRoot(self, tree: List['Node']) -> 'Node':
        x = 0
        for node in tree:
            x ^= node.val
            for child in node.children:
                x ^= child.val
        return next(node for node in tree if node.val == x)
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
    public Node findRoot(List<Node> tree) {
        int x = 0;
        for (Node node : tree) {
            x ^= node.val;
            for (Node child : node.children) {
                x ^= child.val;
            }
        }
        for (int i = 0;; ++i) {
            if (tree.get(i).val == x) {
                return tree.get(i);
            }
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
    Node* findRoot(vector<Node*> tree) {
        int x = 0;
        for (Node* node : tree) {
            x ^= node->val;
            for (Node* child : node->children) {
                x ^= child->val;
            }
        }
        for (int i = 0;; ++i) {
            if (tree[i]->val == x) {
                return tree[i];
            }
        }
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

func findRoot(tree []*Node) *Node {
	x := 0
	for _, node := range tree {
		x ^= node.Val
		for _, child := range node.Children {
			x ^= child.Val
		}
	}
	for i := 0; ; i++ {
		if tree[i].Val == x {
			return tree[i]
		}
	}
}
```

#### TypeScript

```ts
/**
 * Definition for Node.
 * class Node {
 *     val: number
 *     children: Node[]
 *     constructor(val?: number, children?: Node[]) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.children = (children===undefined ? [] : children)
 *     }
 * }
 */

function findRoot(tree: Node[]): Node | null {
    let x = 0;
    for (const node of tree) {
        x ^= node.val;
        for (const child of node.children) {
            x ^= child.val;
        }
    }
    return tree.find(node => node.val === x) || null;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
