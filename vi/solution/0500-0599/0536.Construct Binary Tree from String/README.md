---
comments: true
difficulty: Medium
tags:
    - Stack
    - Tree
    - Depth-First Search
    - String
    - Binary Tree
---

<!-- problem:start -->

# [536. Construct Binary Tree from String 🔒](https://leetcode.com/problems/construct-binary-tree-from-string)

[中文文档](/solution/0500-0599/0536.Construct%20Binary%20Tree%20from%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy dựng một cây nhị phân từ chuỗi gồm dấu ngoặc đơn và các số nguyên.</p>

<p>Toàn bộ input biểu diễn một cây nhị phân. Chuỗi gồm một số nguyên, theo sau là không, một hoặc hai cặp dấu ngoặc đơn. Số nguyên biểu diễn giá trị của root, còn mỗi cặp dấu ngoặc chứa một cây con có cấu trúc tương tự.</p>

<p>Nếu có node con <b>trái</b>, bạn luôn dựng node đó trước.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0536.Construct%20Binary%20Tree%20from%20String/images/butree.jpg" style="width: 382px; height: 322px;" />
<pre>
<strong>Đầu vào:</strong> s = &quot;4(2(3)(1))(6(5))&quot;
<strong>Đầu ra:</strong> [4,2,6,3,1,5]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;4(2(3)(1))(6(5)(7))&quot;
<strong>Đầu ra:</strong> [4,2,6,3,1,5,7]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;-4(2(3)(1))(6(5)(7))&quot;
<strong>Đầu ra:</strong> [-4,2,6,3,1,5,7]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= s.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>s</code> chỉ gồm chữ số, <code>&#39;(&#39;</code>, <code>&#39;)&#39;</code> và <code>&#39;-&#39;</code>.</li>
	<li>Giá trị của mọi số trong cây <strong>không vượt quá</strong> <code>2<sup>30</sup></code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi mã hóa gồm một giá trị, tiếp theo là cây con trái trong ngoặc nếu có, rồi đến cây con phải trong ngoặc nếu có. Khi duyệt chuỗi, cần tách giá trị khỏi các khối ngoặc cân bằng.
>
> Nếu không có dấu `'('`, toàn bộ token biểu diễn một node lá. Nếu có, giá trị là phần tiền tố trước dấu ngoặc đầu tiên; biến đếm độ sâu giúp tìm khối ngoặc cân bằng đầu tiên (cây con trái) và khối tiếp theo (cây con phải), rồi đệ quy phân tích từng khối. Độ sâu dùng để theo dõi mức lồng nhau.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def str2tree(self, s: str) -> TreeNode:
        def dfs(s):
            if not s:
                return None
            p = s.find('(')
            if p == -1:
                return TreeNode(int(s))
            root = TreeNode(int(s[:p]))
            start = p
            cnt = 0
            for i in range(p, len(s)):
                if s[i] == '(':
                    cnt += 1
                elif s[i] == ')':
                    cnt -= 1
                if cnt == 0:
                    if start == p:
                        root.left = dfs(s[start + 1 : i])
                        start = i + 1
                    else:
                        root.right = dfs(s[start + 1 : i])
            return root

        return dfs(s)
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
class Solution {
    public TreeNode str2tree(String s) {
        return dfs(s);
    }

    private TreeNode dfs(String s) {
        if ("".equals(s)) {
            return null;
        }
        int p = s.indexOf("(");
        if (p == -1) {
            return new TreeNode(Integer.parseInt(s));
        }
        TreeNode root = new TreeNode(Integer.parseInt(s.substring(0, p)));
        int start = p;
        int cnt = 0;
        for (int i = p; i < s.length(); ++i) {
            if (s.charAt(i) == '(') {
                ++cnt;
            } else if (s.charAt(i) == ')') {
                --cnt;
            }
            if (cnt == 0) {
                if (start == p) {
                    root.left = dfs(s.substring(start + 1, i));
                    start = i + 1;
                } else {
                    root.right = dfs(s.substring(start + 1, i));
                }
            }
        }
        return root;
    }
}
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
class Solution {
public:
    TreeNode* str2tree(string s) {
        return dfs(s);
    }

    TreeNode* dfs(string s) {
        if (s == "") return nullptr;
        int p = s.find("(");
        if (p == s.npos) return new TreeNode(stoi(s));
        TreeNode* root = new TreeNode(stoi(s.substr(0, p)));
        int start = p;
        int cnt = 0;
        for (int i = p; i < s.size(); ++i) {
            if (s[i] == '(')
                ++cnt;
            else if (s[i] == ')')
                --cnt;
            if (cnt == 0) {
                if (start == p) {
                    root->left = dfs(s.substr(start + 1, i - start - 1));
                    start = i + 1;
                } else
                    root->right = dfs(s.substr(start + 1, i - start - 1));
            }
        }
        return root;
    }
};
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
func str2tree(s string) *TreeNode {
	var dfs func(s string) *TreeNode
	dfs = func(s string) *TreeNode {
		if s == "" {
			return nil
		}
		p := strings.IndexAny(s, "(")
		if p == -1 {
			v, _ := strconv.Atoi(s)
			return &TreeNode{Val: v}
		}
		v, _ := strconv.Atoi(s[:p])
		root := &TreeNode{Val: v}
		start := p
		cnt := 0
		for i := p; i < len(s); i++ {
			if s[i] == '(' {
				cnt++
			} else if s[i] == ')' {
				cnt--
			}
			if cnt == 0 {
				if p == start {
					root.Left = dfs(s[start+1 : i])
					start = i + 1
				} else {
					root.Right = dfs(s[start+1 : i])
				}
			}
		}
		return root
	}
	return dfs(s)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
