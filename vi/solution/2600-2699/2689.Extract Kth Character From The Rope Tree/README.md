---
comments: true
difficulty: Easy
tags:
    - Tree
    - Depth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [2689. Extract Kth Character From The Rope Tree 🔒](https://leetcode.com/problems/extract-kth-character-from-the-rope-tree)

[中文文档](/solution/2600-2699/2689.Extract%20Kth%20Character%20From%20The%20Rope%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một cây nhị phân và một số nguyên <code>k</code>. Ngoài các node con trái và phải, mỗi node của cây còn có hai thuộc tính khác: một <strong>chuỗi</strong> <code>node.val</code> chỉ chứa các chữ cái tiếng Anh viết thường (có thể rỗng) và một số nguyên không âm <code>node.len</code>. Cây này có hai loại node:</p>

<ul>
	<li><strong>Lá</strong>: Các node này không có node con, <code>node.len = 0</code>, và <code>node.val</code> là một chuỗi <strong>không rỗng</strong>.</li>
	<li><strong>Nội bộ</strong>: Các node này có ít nhất một node con (và nhiều nhất là hai node con), <code>node.len &gt; 0</code>, và <code>node.val</code> là một chuỗi <strong>rỗng</strong>.</li>
</ul>

<p>Cây được mô tả ở trên được gọi là cây nhị phân <em>Rope</em>. Bây giờ, ta định nghĩa đệ quy <code>S[node]</code> như sau:</p>

<ul>
	<li>Nếu <code>node</code> là một node lá, <code>S[node] = node.val</code>,</li>
	<li>Ngược lại, nếu <code>node</code> là một node nội bộ, <code>S[node] = concat(S[node.left], S[node.right])</code> và <code>S[node].length = node.len</code>.</li>
</ul>

<p>Trả về <em>ký tự thứ k của chuỗi</em> <code>S[root]</code>.</p>

<p><strong>Lưu ý:</strong> Nếu <code>s</code> và <code>p</code> là hai chuỗi, <code>concat(s, p)</code> là chuỗi thu được bằng cách nối <code>p</code> vào sau <code>s</code>. Ví dụ, <code>concat(&quot;ab&quot;, &quot;zz&quot;) = &quot;abzz&quot;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [10,4,&quot;abcpoe&quot;,&quot;g&quot;,&quot;rta&quot;], k = 6
<strong>Đầu ra:</strong> &quot;b&quot;
<strong>Giải thích:</strong> Trong hình bên dưới, ta đặt một số nguyên trên các node nội bộ để biểu diễn node.len, và một chuỗi trên các node lá để biểu diễn node.val.
Ta có thể thấy S[root] = concat(concat(&quot;g&quot;, &quot;rta&quot;), &quot;abcpoe&quot;) = &quot;grtaabcpoe&quot;. Vì vậy, S[root][5], tức ký tự thứ 6 của chuỗi, bằng &quot;b&quot;.
</pre>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2689.Extract%20Kth%20Character%20From%20The%20Rope%20Tree/images/example1.png" style="width: 300px; height: 213px; margin-left: 280px; margin-right: 280px;" /></p>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [12,6,6,&quot;abc&quot;,&quot;efg&quot;,&quot;hij&quot;,&quot;klm&quot;], k = 3
<strong>Đầu ra:</strong> &quot;c&quot;
<strong>Giải thích:</strong> Trong hình bên dưới, ta đặt một số nguyên trên các node nội bộ để biểu diễn node.len, và một chuỗi trên các node lá để biểu diễn node.val.
Ta có thể thấy S[root] = concat(concat(&quot;abc&quot;, &quot;efg&quot;), concat(&quot;hij&quot;, &quot;klm&quot;)) = &quot;abcefghijklm&quot;. Vì vậy, S[root][2], tức ký tự thứ 3 của chuỗi, bằng &quot;c&quot;.
</pre>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2689.Extract%20Kth%20Character%20From%20The%20Rope%20Tree/images/example2.png" style="width: 400px; height: 232px; margin-left: 255px; margin-right: 255px;" /></p>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [&quot;ropetree&quot;], k = 8
<strong>Đầu ra:</strong> &quot;e&quot;
<strong>Giải thích:</strong> Trong hình bên dưới, ta đặt một số nguyên trên các node nội bộ để biểu diễn node.len, và một chuỗi trên các node lá để biểu diễn node.val.
Ta có thể thấy S[root] = &quot;ropetree&quot;. Vì vậy, S[root][7], tức ký tự thứ 8 của chuỗi, bằng &quot;e&quot;.
</pre>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2689.Extract%20Kth%20Character%20From%20The%20Rope%20Tree/images/example3.png" style="width: 80px; height: 78px; margin-left: 400px; margin-right: 400px;" /></p>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 10<sup>3</sup>]</code></li>
	<li><code>node.val</code> chỉ chứa các chữ cái tiếng Anh viết thường</li>
	<li><code>0 &lt;= node.val.length &lt;= 50</code></li>
	<li><code>0 &lt;= node.len &lt;= 10<sup>4</sup></code></li>
	<li>đối với node lá, <code>node.len = 0</code> và <code>node.val</code> không rỗng</li>
	<li>đối với node nội bộ, <code>node.len &gt; 0</code> và <code>node.val</code> rỗng</li>
	<li><code>1 &lt;= k &lt;= S[root].length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Rope lưu các mảnh chuỗi ở các node lá và độ dài ở các node nội bộ. Ta cần tìm ký tự thứ $k$. Cây đủ nhỏ để ta dựng lại toàn bộ chuỗi, dù cũng có thể duyệt cây dựa trên độ dài.
>
> DFS trả về `val` tại node lá và phép nối hai node con trong các trường hợp còn lại; đáp án là chỉ số $k-1$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
# Definition for a rope tree node.
# class RopeTreeNode(object):
#     def __init__(self, len=0, val="", left=None, right=None):
#         self.len = len
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def getKthCharacter(self, root: Optional[object], k: int) -> str:
        def dfs(root):
            if root is None:
                return ""
            if root.len == 0:
                return root.val
            return dfs(root.left) + dfs(root.right)

        return dfs(root)[k - 1]
```

#### Java

```java
/**
 * Definition for a rope tree node.
 * class RopeTreeNode {
 *     int len;
 *     String val;
 *     RopeTreeNode left;
 *     RopeTreeNode right;
 *     RopeTreeNode() {}
 *     RopeTreeNode(String val) {
 *         this.len = 0;
 *         this.val = val;
 *     }
 *     RopeTreeNode(int len) {
 *         this.len = len;
 *         this.val = "";
 *     }
 *     RopeTreeNode(int len, TreeNode left, TreeNode right) {
 *         this.len = len;
 *         this.val = "";
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public char getKthCharacter(RopeTreeNode root, int k) {
        return dfs(root).charAt(k - 1);
    }

    private String dfs(RopeTreeNode root) {
        if (root == null) {
            return "";
        }
        if (root.val.length() > 0) {
            return root.val;
        }
        String left = dfs(root.left);
        String right = dfs(root.right);
        return left + right;
    }
}
```

#### C++

```cpp
/**
 * Definition for a rope tree node.
 * struct RopeTreeNode {
 *     int len;
 *     string val;
 *     RopeTreeNode *left;
 *     RopeTreeNode *right;
 *     RopeTreeNode() : len(0), val(""), left(nullptr), right(nullptr) {}
 *     RopeTreeNode(string s) : len(0), val(std::move(s)), left(nullptr), right(nullptr) {}
 *     RopeTreeNode(int x) : len(x), val(""), left(nullptr), right(nullptr) {}
 *     RopeTreeNode(int x, RopeTreeNode *left, RopeTreeNode *right) : len(x), val(""), left(left), right(right) {}
 * };
 */
class Solution {
public:
    char getKthCharacter(RopeTreeNode* root, int k) {
        function<string(RopeTreeNode * root)> dfs = [&](RopeTreeNode* root) -> string {
            if (root == nullptr) {
                return "";
            }
            if (root->len == 0) {
                return root->val;
            }
            string left = dfs(root->left);
            string right = dfs(root->right);
            return left + right;
        };
        return dfs(root)[k - 1];
    }
};
```

#### Go

```go
/**
 * Definition for a rope tree node.
 * type RopeTreeNode struct {
 * 	   len   int
 * 	   val   string
 * 	   left  *RopeTreeNode
 * 	   right *RopeTreeNode
 * }
 */
func getKthCharacter(root *RopeTreeNode, k int) byte {
	var dfs func(root *RopeTreeNode) string
	dfs = func(root *RopeTreeNode) string {
		if root == nil {
			return ""
		}
		if root.len == 0 {
			return root.val
		}
		left, right := dfs(root.left), dfs(root.right)
		return left + right
	}
	return dfs(root)[k-1]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
