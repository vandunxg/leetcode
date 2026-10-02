---
comments: true
difficulty: Medium
tags:
    - Tree
    - Array
    - Divide and Conquer
    - Matrix
---

<!-- problem:start -->

# [427. Construct Quad Tree](https://leetcode.com/problems/construct-quad-tree)

[中文文档](/solution/0400-0499/0427.Construct%20Quad%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận <code>n * n</code> <code>grid</code> chỉ gồm các giá trị <code>0&#39;s</code> và <code>1&#39;s</code>. Hãy biểu diễn <code>grid</code> bằng Quad-Tree.</p>

<p>Trả về root của Quad-Tree biểu diễn <code>grid</code>.</p>

<p>Quad-Tree là cấu trúc dữ liệu dạng cây, trong đó mỗi node nội bộ có đúng bốn node con. Ngoài ra, mỗi node có hai thuộc tính:</p>

<ul>
	<li><code>val</code>: True nếu node biểu diễn một vùng gồm toàn <code>1&#39;s</code>, hoặc False nếu node biểu diễn một vùng gồm toàn <code>0&#39;s</code>. Khi <code>isLeaf</code> là False, bạn có thể gán <code>val</code> là True hoặc False; cả hai giá trị đều được chấp nhận.</li>
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

<p>Có thể xây dựng Quad-Tree từ một vùng hai chiều theo các bước sau:</p>

<ol>
	<li>Nếu vùng hiện tại có cùng một giá trị (tức là toàn <code>1&#39;s</code> hoặc toàn <code>0&#39;s</code>), đặt <code>isLeaf</code> thành True, đặt <code>val</code> bằng giá trị của vùng, đặt bốn node con thành Null rồi dừng.</li>
	<li>Nếu vùng hiện tại có các giá trị khác nhau, đặt <code>isLeaf</code> thành False, đặt <code>val</code> bằng giá trị bất kỳ rồi chia vùng thành bốn vùng con như trong hình.</li>
	<li>Đệ quy cho từng node con với vùng con tương ứng.</li>
</ol>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0427.Construct%20Quad%20Tree/images/new_top.png" style="width: 777px; height: 181px;" />
<p>Nếu muốn tìm hiểu thêm về Quad-Tree, bạn có thể xem <a href="https://en.wikipedia.org/wiki/Quadtree">wiki</a>.</p>

<p><strong>Định dạng Quad-Tree:</strong></p>

<p>Bạn không cần đọc phần này để giải bài toán. Phần này chỉ giúp hiểu định dạng output. Output biểu diễn Quad-Tree đã được serialize bằng cách duyệt theo thứ tự level-order; <code>null</code> đánh dấu điểm kết thúc của một nhánh không còn node nào bên dưới.</p>

<p>Cách này rất giống với serialization của binary tree. Điểm khác duy nhất là mỗi node được biểu diễn bằng danh sách <code>[isLeaf, val]</code>.</p>

<p>Nếu <code>isLeaf</code> hoặc <code>val</code> có giá trị True, ta biểu diễn bằng <strong>1</strong> trong danh sách <code>[isLeaf, val]</code>; nếu có giá trị False thì biểu diễn bằng <strong>0</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0427.Construct%20Quad%20Tree/images/grid1.png" style="width: 777px; height: 99px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,1],[1,0]]
<strong>Đầu ra:</strong> [[0,1],[1,0],[1,1],[1,1],[1,0]]
<strong>Giải thích:</strong> Hình bên dưới minh họa ví dụ này:
Lưu ý: trong hình minh họa Quad-Tree, 0 biểu diễn False và 1 biểu diễn True.
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0427.Construct%20Quad%20Tree/images/e1tree.png" style="width: 777px; height: 186px;" />
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0427.Construct%20Quad%20Tree/images/e2mat.png" style="width: 777px; height: 343px;" /></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,1,1,1,1],[1,1,1,1,1,1,1,1],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0]]
<strong>Đầu ra:</strong> [[0,1],[1,1],[0,1],[1,1],[1,0],null,null,null,null,[1,0],[1,0],[1,1],[1,1]]
<strong>Giải thích:</strong> Các giá trị trong grid không giống nhau. Ta chia grid thành bốn vùng con.
Các vùng topLeft, bottomLeft và bottomRight đều có cùng một giá trị.
Vùng topRight có các giá trị khác nhau nên ta chia vùng này thành bốn vùng con, mỗi vùng con có cùng một giá trị.
Hình bên dưới minh họa phần giải thích:
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0427.Construct%20Quad%20Tree/images/e2tree.png" style="width: 777px; height: 328px;" />
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == grid.length == grid[i].length</code></li>
	<li><code>n == 2<sup>x</sup></code> where <code>0 &lt;= x &lt;= 6</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Solution 1

<!-- thinking:start -->

> **Tư duy**
>
> Quad-Tree lưu một vùng đồng nhất dưới dạng node lá; nếu vùng không đồng nhất thì chia thành bốn phần. Quét lại mọi hình chữ nhật con sẽ lặp lại công việc, nhưng với $n\le 64$, việc chia bốn nhánh vẫn giúp tổng lượng quét ở mức chấp nhận được.
>
> DFS kiểm tra hình chữ nhật hiện tại có chứa cả $0$ lẫn $1$ hay không. Nếu chỉ có một giá trị thì tạo node lá; nếu không, đệ quy trên bốn góc phần tư.
>
> $\textit{val}$ của node lá là màu của vùng tương ứng. Kiểm tra vùng có đồng nhất hay không trước khi chia giúp tránh tách một vùng đồng màu.

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
    def construct(self, grid: List[List[int]]) -> 'Node':
        def dfs(a, b, c, d):
            zero = one = 0
            for i in range(a, c + 1):
                for j in range(b, d + 1):
                    if grid[i][j] == 0:
                        zero = 1
                    else:
                        one = 1
            isLeaf = zero + one == 1
            val = isLeaf and one
            if isLeaf:
                return Node(grid[a][b], True)
            topLeft = dfs(a, b, (a + c) // 2, (b + d) // 2)
            topRight = dfs(a, (b + d) // 2 + 1, (a + c) // 2, d)
            bottomLeft = dfs((a + c) // 2 + 1, b, c, (b + d) // 2)
            bottomRight = dfs((a + c) // 2 + 1, (b + d) // 2 + 1, c, d)
            return Node(val, isLeaf, topLeft, topRight, bottomLeft, bottomRight)

        return dfs(0, 0, len(grid) - 1, len(grid[0]) - 1)
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


    public Node() {
        this.val = false;
        this.isLeaf = false;
        this.topLeft = null;
        this.topRight = null;
        this.bottomLeft = null;
        this.bottomRight = null;
    }

    public Node(boolean val, boolean isLeaf) {
        this.val = val;
        this.isLeaf = isLeaf;
        this.topLeft = null;
        this.topRight = null;
        this.bottomLeft = null;
        this.bottomRight = null;
    }

    public Node(boolean val, boolean isLeaf, Node topLeft, Node topRight, Node bottomLeft, Node
bottomRight) { this.val = val; this.isLeaf = isLeaf; this.topLeft = topLeft; this.topRight =
topRight; this.bottomLeft = bottomLeft; this.bottomRight = bottomRight;
    }
};
*/

class Solution {
    public Node construct(int[][] grid) {
        return dfs(0, 0, grid.length - 1, grid[0].length - 1, grid);
    }

    private Node dfs(int a, int b, int c, int d, int[][] grid) {
        int zero = 0, one = 0;
        for (int i = a; i <= c; ++i) {
            for (int j = b; j <= d; ++j) {
                if (grid[i][j] == 0) {
                    zero = 1;
                } else {
                    one = 1;
                }
            }
        }
        boolean isLeaf = zero + one == 1;
        boolean val = isLeaf && one == 1;
        Node node = new Node(val, isLeaf);
        if (isLeaf) {
            return node;
        }
        node.topLeft = dfs(a, b, (a + c) / 2, (b + d) / 2, grid);
        node.topRight = dfs(a, (b + d) / 2 + 1, (a + c) / 2, d, grid);
        node.bottomLeft = dfs((a + c) / 2 + 1, b, c, (b + d) / 2, grid);
        node.bottomRight = dfs((a + c) / 2 + 1, (b + d) / 2 + 1, c, d, grid);
        return node;
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
    Node* construct(vector<vector<int>>& grid) {
        return dfs(0, 0, grid.size() - 1, grid[0].size() - 1, grid);
    }

    Node* dfs(int a, int b, int c, int d, vector<vector<int>>& grid) {
        int zero = 0, one = 0;
        for (int i = a; i <= c; ++i) {
            for (int j = b; j <= d; ++j) {
                if (grid[i][j])
                    one = 1;
                else
                    zero = 1;
            }
        }
        bool isLeaf = zero + one == 1;
        bool val = isLeaf && one;
        Node* node = new Node(val, isLeaf);
        if (isLeaf) return node;
        node->topLeft = dfs(a, b, (a + c) / 2, (b + d) / 2, grid);
        node->topRight = dfs(a, (b + d) / 2 + 1, (a + c) / 2, d, grid);
        node->bottomLeft = dfs((a + c) / 2 + 1, b, c, (b + d) / 2, grid);
        node->bottomRight = dfs((a + c) / 2 + 1, (b + d) / 2 + 1, c, d, grid);
        return node;
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

func construct(grid [][]int) *Node {
	var dfs func(a, b, c, d int) *Node
	dfs = func(a, b, c, d int) *Node {
		zero, one := 0, 0
		for i := a; i <= c; i++ {
			for j := b; j <= d; j++ {
				if grid[i][j] == 0 {
					zero = 1
				} else {
					one = 1
				}
			}
		}
		isLeaf := zero+one == 1
		val := isLeaf && one == 1
		node := &Node{Val: val, IsLeaf: isLeaf}
		if isLeaf {
			return node
		}
		node.TopLeft = dfs(a, b, (a+c)/2, (b+d)/2)
		node.TopRight = dfs(a, (b+d)/2+1, (a+c)/2, d)
		node.BottomLeft = dfs((a+c)/2+1, b, c, (b+d)/2)
		node.BottomRight = dfs((a+c)/2+1, (b+d)/2+1, c, d)
		return node
	}
	return dfs(0, 0, len(grid)-1, len(grid[0])-1)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
