---
comments: true
difficulty: Medium
tags:
    - Tree
    - Divide and Conquer
---

<!-- problem:start -->

# [558. Logical OR of Two Binary Grids Represented as Quad-Trees](https://leetcode.com/problems/logical-or-of-two-binary-grids-represented-as-quad-trees)

[中文文档](/solution/0500-0599/0558.Logical%20OR%20of%20Two%20Binary%20Grids%20Represented%20as%20Quad-Trees/README.md)

## Mô tả

<!-- description:start -->

<p>Ma trận nhị phân là ma trận mà mọi phần tử đều bằng <strong>0</strong> hoặc <strong>1</strong>.</p>

<p>Cho <code>quadTree1</code> và <code>quadTree2</code>. <code>quadTree1</code> biểu diễn một ma trận nhị phân kích thước <code>n * n</code>, còn <code>quadTree2</code> biểu diễn một ma trận nhị phân khác cũng có kích thước <code>n * n</code>.</p>

<p>Hãy trả về <em>một Quad-Tree</em> biểu diễn ma trận nhị phân kích thước <code>n * n</code> là kết quả của phép <strong>OR theo bit</strong> giữa hai ma trận nhị phân được biểu diễn bởi <code>quadTree1</code> và <code>quadTree2</code>.</p>

<p>Lưu ý, khi <code>isLeaf</code> là <strong>False</strong>, bạn có thể gán giá trị cho node là <strong>True</strong> hoặc <strong>False</strong>; cả hai đều được <strong>chấp nhận</strong> trong đáp án.</p>

<p>Quad-Tree là cấu trúc dữ liệu dạng cây, trong đó mỗi node nội bộ có chính xác bốn node con. Mỗi node có hai thuộc tính:</p>

<ul>
	<li><code>val</code>: True nếu node biểu diễn một vùng lưới toàn số 1, hoặc False nếu node biểu diễn một vùng lưới toàn số 0.</li>
	<li><code>isLeaf</code>: True nếu node là node lá, hoặc False nếu node có bốn node con.</li>
</ul>

<pre>
class Node {
    public boolean val;
    public boolean isLeaf;
    public Node topLeft;
    public Node topRight;
    public Node bottomLeft;
    public Node bottomRight;
}</pre>

<p>Có thể dựng Quad-Tree từ một vùng hai chiều theo các bước sau:</p>

<ol>
	<li>Nếu toàn bộ ô trong vùng lưới hiện tại có cùng giá trị (tức toàn là <code>1&#39;s</code> hoặc toàn là <code>0&#39;s</code>), đặt <code>isLeaf</code> thành True, đặt <code>val</code> thành giá trị của vùng lưới, đặt bốn node con thành Null rồi dừng.</li>
	<li>Nếu vùng lưới hiện tại có các giá trị khác nhau, đặt <code>isLeaf</code> thành False, đặt <code>val</code> thành giá trị bất kỳ rồi chia vùng lưới thành bốn vùng con như hình minh họa.</li>
	<li>Đệ quy trên từng node con với vùng lưới con tương ứng.</li>
</ol>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0558.Logical%20OR%20of%20Two%20Binary%20Grids%20Represented%20as%20Quad-Trees/images/new_top.png" style="width: 777px; height: 181px;" />
<p>Để tìm hiểu thêm về Quad-Tree, bạn có thể tham khảo <a href="https://en.wikipedia.org/wiki/Quadtree">wiki</a>.</p>

<p><strong>Định dạng Quad-Tree:</strong></p>

<p>Đầu vào/đầu ra biểu diễn Quad-Tree dưới dạng đã serialize bằng cách duyệt theo thứ tự level, trong đó <code>null</code> biểu thị điểm kết thúc của một nhánh, tức là không có node nào bên dưới.</p>

<p>Cách serialize này rất giống với cây nhị phân. Điểm khác biệt duy nhất là mỗi node được biểu diễn bằng danh sách <code>[isLeaf, val]</code>.</p>

<p>Nếu giá trị của <code>isLeaf</code> hoặc <code>val</code> là True, ta biểu diễn bằng <strong>1</strong> trong danh sách <code>[isLeaf, val]</code>; nếu là False, ta biểu diễn bằng <strong>0</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0558.Logical%20OR%20of%20Two%20Binary%20Grids%20Represented%20as%20Quad-Trees/images/qt1.png" style="width: 550px; height: 196px;" /> <img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0558.Logical%20OR%20of%20Two%20Binary%20Grids%20Represented%20as%20Quad-Trees/images/qt2.png" style="width: 550px; height: 278px;" />
<pre>
<strong>Đầu vào:</strong> quadTree1 = [[0,1],[1,1],[1,1],[1,0],[1,0]]
, quadTree2 = [[0,1],[1,1],[0,1],[1,1],[1,0],null,null,null,null,[1,0],[1,0],[1,1],[1,1]]
<strong>Đầu ra:</strong> [[0,0],[1,1],[1,1],[1,1],[1,0]]
<strong>Giải thích:</strong> quadTree1 và quadTree2 được minh họa phía trên. Bạn có thể xem ma trận nhị phân được biểu diễn bởi từng Quad-Tree.
Nếu áp dụng phép OR theo bit lên hai ma trận nhị phân, ta thu được ma trận bên dưới, được biểu diễn bởi Quad-Tree kết quả.
Lưu ý, các ma trận nhị phân chỉ nhằm minh họa; bạn không cần dựng ma trận nhị phân để tạo cây kết quả.
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0558.Logical%20OR%20of%20Two%20Binary%20Grids%20Represented%20as%20Quad-Trees/images/qtr.png" style="width: 777px; height: 222px;" />
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> quadTree1 = [[1,0]], quadTree2 = [[1,0]]
<strong>Đầu ra:</strong> [[1,0]]
<strong>Giải thích:</strong> Mỗi cây biểu diễn một ma trận nhị phân kích thước 1*1. Mỗi ma trận chỉ chứa số 0.
Ma trận kết quả cũng có kích thước 1*1 và chỉ chứa số 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>quadTree1</code> và <code>quadTree2</code> đều là Quad-Tree <strong>hợp lệ</strong>, lần lượt biểu diễn một lưới kích thước <code>n * n</code>.</li>
	<li><code>n == 2<sup>x</sup></code> trong đó <code>0 &lt;= x &lt;= 9</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Thực hiện phép OR theo bit trên hai Quad-Tree. Mở rộng thành từng pixel rồi nén lại sẽ làm mất cấu trúc sẵn có. Có thể xử lý nhanh node lá: node lá có giá trị true sẽ quyết định kết quả; nếu cả hai đều là node lá thì OR giá trị của chúng để tạo node lá kết quả.
>
> Nếu không, đệ quy trên bốn cặp node con. Nếu cả bốn node con đều là node lá và có cùng giá trị, gộp chúng lại thành một node lá để cây giữ dạng chuẩn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
"""
# Definition for a QuadTree node.
class Node:
    def __init__(self, val, isLeaf, topLeft, topRight, bottomLeft, bottomRight):
        self.val = val
        self.isLeaf = isLeaf
        self.topLeft = topLeft
        self.topRight = topRight
        self.bottomLeft = bottomLeft
        self.bottomRight = bottomRight
"""


class Solution:
    def intersect(self, quadTree1: "Node", quadTree2: "Node") -> "Node":
        def dfs(t1, t2):
            if t1.isLeaf and t2.isLeaf:
                return Node(t1.val or t2.val, True)
            if t1.isLeaf:
                return t1 if t1.val else t2
            if t2.isLeaf:
                return t2 if t2.val else t1
            res = Node()
            res.topLeft = dfs(t1.topLeft, t2.topLeft)
            res.topRight = dfs(t1.topRight, t2.topRight)
            res.bottomLeft = dfs(t1.bottomLeft, t2.bottomLeft)
            res.bottomRight = dfs(t1.bottomRight, t2.bottomRight)
            isLeaf = (
                res.topLeft.isLeaf
                and res.topRight.isLeaf
                and res.bottomLeft.isLeaf
                and res.bottomRight.isLeaf
            )
            sameVal = (
                res.topLeft.val
                == res.topRight.val
                == res.bottomLeft.val
                == res.bottomRight.val
            )
            if isLeaf and sameVal:
                res = res.topLeft
            return res

        return dfs(quadTree1, quadTree2)
```

#### Java

```java
/*
// Definition for a QuadTree node.
class Node {
    public boolean val;
    public boolean isLeaf;
    public Node topLeft;
    public Node topRight;
    public Node bottomLeft;
    public Node bottomRight;

    public Node() {}

    public Node(boolean _val,boolean _isLeaf,Node _topLeft,Node _topRight,Node _bottomLeft,Node
_bottomRight) { val = _val; isLeaf = _isLeaf; topLeft = _topLeft; topRight = _topRight; bottomLeft =
_bottomLeft; bottomRight = _bottomRight;
    }
};
*/

class Solution {
    public Node intersect(Node quadTree1, Node quadTree2) {
        return dfs(quadTree1, quadTree2);
    }

    private Node dfs(Node t1, Node t2) {
        if (t1.isLeaf && t2.isLeaf) {
            return new Node(t1.val || t2.val, true);
        }
        if (t1.isLeaf) {
            return t1.val ? t1 : t2;
        }
        if (t2.isLeaf) {
            return t2.val ? t2 : t1;
        }
        Node res = new Node();
        res.topLeft = dfs(t1.topLeft, t2.topLeft);
        res.topRight = dfs(t1.topRight, t2.topRight);
        res.bottomLeft = dfs(t1.bottomLeft, t2.bottomLeft);
        res.bottomRight = dfs(t1.bottomRight, t2.bottomRight);
        boolean isLeaf = res.topLeft.isLeaf && res.topRight.isLeaf && res.bottomLeft.isLeaf
            && res.bottomRight.isLeaf;
        boolean sameVal = res.topLeft.val == res.topRight.val
            && res.topRight.val == res.bottomLeft.val && res.bottomLeft.val == res.bottomRight.val;
        if (isLeaf && sameVal) {
            res = res.topLeft;
        }
        return res;
    }
}
```

#### C++

```cpp
/*
// Definition for a QuadTree node.
class Node {
public:
    bool val;
    bool isLeaf;
    Node* topLeft;
    Node* topRight;
    Node* bottomLeft;
    Node* bottomRight;

    Node() {
        val = false;
        isLeaf = false;
        topLeft = NULL;
        topRight = NULL;
        bottomLeft = NULL;
        bottomRight = NULL;
    }

    Node(bool _val, bool _isLeaf) {
        val = _val;
        isLeaf = _isLeaf;
        topLeft = NULL;
        topRight = NULL;
        bottomLeft = NULL;
        bottomRight = NULL;
    }

    Node(bool _val, bool _isLeaf, Node* _topLeft, Node* _topRight, Node* _bottomLeft, Node* _bottomRight) {
        val = _val;
        isLeaf = _isLeaf;
        topLeft = _topLeft;
        topRight = _topRight;
        bottomLeft = _bottomLeft;
        bottomRight = _bottomRight;
    }
};
*/

class Solution {
public:
    Node* intersect(Node* quadTree1, Node* quadTree2) {
        return dfs(quadTree1, quadTree2);
    }

    Node* dfs(Node* t1, Node* t2) {
        if (t1->isLeaf && t2->isLeaf) return new Node(t1->val || t2->val, true);
        if (t1->isLeaf) return t1->val ? t1 : t2;
        if (t2->isLeaf) return t2->val ? t2 : t1;
        Node* res = new Node();
        res->topLeft = dfs(t1->topLeft, t2->topLeft);
        res->topRight = dfs(t1->topRight, t2->topRight);
        res->bottomLeft = dfs(t1->bottomLeft, t2->bottomLeft);
        res->bottomRight = dfs(t1->bottomRight, t2->bottomRight);
        bool isLeaf = res->topLeft->isLeaf && res->topRight->isLeaf && res->bottomLeft->isLeaf && res->bottomRight->isLeaf;
        bool sameVal = res->topLeft->val == res->topRight->val && res->topRight->val == res->bottomLeft->val && res->bottomLeft->val == res->bottomRight->val;
        if (isLeaf && sameVal) res = res->topLeft;
        return res;
    }
};
```

#### Go

```go
/**
 * Definition for a QuadTree node.
 * type Node struct {
 *     Val bool
 *     IsLeaf bool
 *     TopLeft *Node
 *     TopRight *Node
 *     BottomLeft *Node
 *     BottomRight *Node
 * }
 */

func intersect(quadTree1 *Node, quadTree2 *Node) *Node {
	var dfs func(*Node, *Node) *Node
	dfs = func(t1, t2 *Node) *Node {
		if t1.IsLeaf && t2.IsLeaf {
			return &Node{Val: t1.Val || t2.Val, IsLeaf: true}
		}
		if t1.IsLeaf {
			if t1.Val {
				return t1
			}
			return t2
		}
		if t2.IsLeaf {
			if t2.Val {
				return t2
			}
			return t1
		}
		res := &Node{}
		res.TopLeft = dfs(t1.TopLeft, t2.TopLeft)
		res.TopRight = dfs(t1.TopRight, t2.TopRight)
		res.BottomLeft = dfs(t1.BottomLeft, t2.BottomLeft)
		res.BottomRight = dfs(t1.BottomRight, t2.BottomRight)
		isLeaf := res.TopLeft.IsLeaf && res.TopRight.IsLeaf && res.BottomLeft.IsLeaf && res.BottomRight.IsLeaf
		sameVal := res.TopLeft.Val == res.TopRight.Val && res.TopRight.Val == res.BottomLeft.Val && res.BottomLeft.Val == res.BottomRight.Val
		if isLeaf && sameVal {
			res = res.TopLeft
		}
		return res
	}

	return dfs(quadTree1, quadTree2)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
