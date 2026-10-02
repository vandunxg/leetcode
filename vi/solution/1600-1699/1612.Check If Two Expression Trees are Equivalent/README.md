---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Hash Table
    - Binary Tree
    - Counting
---

<!-- problem:start -->

# [1612. Check If Two Expression Trees are Equivalent 🔒](https://leetcode.com/problems/check-if-two-expression-trees-are-equivalent)

[中文文档](/solution/1600-1699/1612.Check%20If%20Two%20Expression%20Trees%20are%20Equivalent/README.md)

## Mô tả

<!-- description:start -->

<p><strong><a href="https://en.wikipedia.org/wiki/Binary_expression_tree" target="_blank">Cây biểu thức nhị phân</a></strong> là một loại cây nhị phân dùng để biểu diễn biểu thức số học. Mỗi node có 0 hoặc 2 node con. Node lá tương ứng với toán hạng (biến), còn node trong tương ứng với toán tử. Trong bài này, ta chỉ xét toán tử <code>&#39;+&#39;</code> (phép cộng).</p>

<p>Cho gốc của hai cây biểu thức nhị phân là <code>root1</code> và <code>root2</code>. Trả về <code>true</code><em> nếu hai cây tương đương</em>; ngược lại trả về <code>false</code>.</p>

<p>Hai cây biểu thức nhị phân tương đương nếu chúng <strong>cho cùng giá trị</strong> bất kể các biến được gán giá trị nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> root1 = [x], root2 = [x]
<strong>Output:</strong> true
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1612.Check%20If%20Two%20Expression%20Trees%20are%20Equivalent/images/tree1.png" style="width: 211px; height: 131px;" /></strong></p>

<pre>
<strong>Input:</strong> root1 = [+,a,+,null,null,b,c], root2 = [+,+,a,b,c]
<strong>Output:</strong> true
<strong>Giải thích</strong><strong>:</strong> <code>a + (b + c) == (b + c) + a</code></pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1612.Check%20If%20Two%20Expression%20Trees%20are%20Equivalent/images/tree2.png" style="width: 211px; height: 131px;" /></strong></p>

<pre>
<strong>Input:</strong> root1 = [+,a,+,null,null,b,c], root2 = [+,+,a,b,d]
<strong>Output:</strong> false
<strong>Giải thích</strong><strong>:</strong> <code>a + (b + c) != (b + d) + a</code>
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong hai cây bằng nhau, là số lẻ và nằm trong khoảng <code>[1, 4999]</code>.</li>
	<li><code>Node.val</code> là <code>&#39;+&#39;</code> hoặc một chữ cái tiếng Anh viết thường.</li>
	<li><strong>Đảm bảo</strong> cây được cho là cây biểu thức nhị phân hợp lệ.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn sẽ thay đổi lời giải thế nào nếu cây cũng hỗ trợ toán tử <code>&#39;-&#39;</code> (phép trừ)?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Cây chỉ chứa phép cộng và các lá là chữ cái. Phép cộng có tính giao hoán và kết hợp, nên hai biểu thức tương đương khi mỗi chữ cái xuất hiện cùng số lần. Khoảng $5000$ node đủ để duyệt một lần.
>
> Cộng một với mỗi chữ cái trong cây thứ nhất và trừ một trong cây thứ hai; mọi giá trị đều bằng 0 nghĩa là tương đương.
>
> DFS bỏ qua node `+` và chỉ cập nhật bộ đếm $cnt$ tại các node lá.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
# Definition for a binary tree node.
# class Node(object):
#     def __init__(self, val=" ", left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def checkEquivalence(self, root1: 'Node', root2: 'Node') -> bool:
        def dfs(root, v):
            if root is None:
                return
            if root.val != '+':
                cnt[root.val] += v
            dfs(root.left, v)
            dfs(root.right, v)

        cnt = Counter()
        dfs(root1, 1)
        dfs(root2, -1)
        return all(x == 0 for x in cnt.values())
```

#### Java

```java
/**
 * Definition for a binary tree node.
 * class Node {
 *     char val;
 *     Node left;
 *     Node right;
 *     Node() {this.val = ' ';}
 *     Node(char val) { this.val = val; }
 *     Node(char val, Node left, Node right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    private int[] cnt = new int[26];

    public boolean checkEquivalence(Node root1, Node root2) {
        dfs(root1, 1);
        dfs(root2, -1);
        for (int x : cnt) {
            if (x != 0) {
                return false;
            }
        }
        return true;
    }

    private void dfs(Node root, int v) {
        if (root == null) {
            return;
        }
        if (root.val != '+') {
            cnt[root.val - 'a'] += v;
        }
        dfs(root.left, v);
        dfs(root.right, v);
    }
}
```

#### C++

```cpp
/**
 * Definition for a binary tree node.
 * struct Node {
 *     char val;
 *     Node *left;
 *     Node *right;
 *     Node() : val(' '), left(nullptr), right(nullptr) {}
 *     Node(char x) : val(x), left(nullptr), right(nullptr) {}
 *     Node(char x, Node *left, Node *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool checkEquivalence(Node* root1, Node* root2) {
        int cnt[26]{};
        function<void(Node*, int)> dfs = [&](Node* root, int v) {
            if (!root) {
                return;
            }
            if (root->val != '+') {
                cnt[root->val - 'a'] += v;
            }
            dfs(root->left, v);
            dfs(root->right, v);
        };
        dfs(root1, 1);
        dfs(root2, -1);
        for (int& x : cnt) {
            if (x) {
                return false;
            }
        }
        return true;
    }
};
```

#### JavaScript

```js
/**
 * Definition for a binary tree node.
 * function Node(val, left, right) {
 *     this.val = (val===undefined ? " " : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {Node} root1
 * @param {Node} root2
 * @return {boolean}
 */
var checkEquivalence = function (root1, root2) {
    const cnt = new Array(26).fill(0);
    const dfs = (root, v) => {
        if (!root) {
            return;
        }
        if (root.val !== '+') {
            cnt[root.val.charCodeAt(0) - 'a'.charCodeAt(0)] += v;
        }
        dfs(root.left, v);
        dfs(root.right, v);
    };
    dfs(root1, 1);
    dfs(root2, -1);
    for (const x of cnt) {
        if (x) {
            return false;
        }
    }
    return true;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Đệ quy (Vector hệ số)

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 chỉ xử lý phép cộng. Với phép trừ, các chữ cái trở thành hệ số có dấu thay vì chỉ là số lần xuất hiện.
>
> Đệ quy xây dựng vector hệ số độ dài $26$ cho mỗi cây: lá có hệ số $1$, cộng hai vector với `+`, và đổi dấu vector con phải với `-`. Hai vector bằng nhau thì hai cây tương đương.

<!-- thinking:end -->

Đệ quy tính vector hệ số độ dài $26$ cho mỗi cây. Node lá tăng hệ số của chữ cái tương ứng; `+` cộng hai vector con, còn `-` lấy vector trái trừ vector phải. Hai cây tương đương khi và chỉ khi hai vector bằng nhau. Cách này cũng xử lý được toán tử trừ trong câu hỏi mở rộng.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
# Definition for a binary tree node.
# class Node(object):
#     def __init__(self, val=" ", left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def checkEquivalence(self, root1: 'Node', root2: 'Node') -> bool:
        def dfs(root):
            cnt = [0] * 26
            if root is None:
                return cnt
            if root.val in '+-':
                l, r = dfs(root.left), dfs(root.right)
                k = 1 if root.val == '+' else -1
                for i in range(26):
                    cnt[i] += l[i] + r[i] * k
            else:
                cnt[ord(root.val) - ord('a')] += 1
            return cnt

        return dfs(root1) == dfs(root2)
```

#### Java

```java
/**
 * Definition for a binary tree node.
 * class Node {
 *     char val;
 *     Node left;
 *     Node right;
 *     Node() {this.val = ' ';}
 *     Node(char val) { this.val = val; }
 *     Node(char val, Node left, Node right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean checkEquivalence(Node root1, Node root2) {
        int[] cnt1 = dfs(root1);
        int[] cnt2 = dfs(root2);
        for (int i = 0; i < 26; ++i) {
            if (cnt1[i] != cnt2[i]) {
                return false;
            }
        }
        return true;
    }

    private int[] dfs(Node root) {
        int[] cnt = new int[26];
        if (root == null) {
            return cnt;
        }
        if (root.val == '+' || root.val == '-') {
            int[] l = dfs(root.left);
            int[] r = dfs(root.right);
            int k = root.val == '+' ? 1 : -1;
            for (int i = 0; i < 26; ++i) {
                cnt[i] += l[i] + r[i] * k;
            }
        } else {
            cnt[root.val - 'a']++;
        }
        return cnt;
    }
}
```

#### C++

```cpp
/**
 * Definition for a binary tree node.
 * struct Node {
 *     char val;
 *     Node *left;
 *     Node *right;
 *     Node() : val(' '), left(nullptr), right(nullptr) {}
 *     Node(char x) : val(x), left(nullptr), right(nullptr) {}
 *     Node(char x, Node *left, Node *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool checkEquivalence(Node* root1, Node* root2) {
        function<vector<int>(Node*)> dfs = [&](Node* root) -> vector<int> {
            vector<int> cnt(26);
            if (!root) {
                return cnt;
            }
            if (root->val == '+' || root->val == '-') {
                auto l = dfs(root->left);
                auto r = dfs(root->right);
                int k = root->val == '+' ? 1 : -1;
                for (int i = 0; i < 26; ++i) {
                    cnt[i] += l[i] + r[i] * k;
                }
            } else {
                cnt[root->val - 'a']++;
            }
            return cnt;
        };
        return dfs(root1) == dfs(root2);
    }
};
```

#### JavaScript

```js
/**
 * Definition for a binary tree node.
 * function Node(val, left, right) {
 *     this.val = (val===undefined ? " " : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {Node} root1
 * @param {Node} root2
 * @return {boolean}
 */
var checkEquivalence = function (root1, root2) {
    const dfs = root => {
        const cnt = new Array(26).fill(0);
        if (!root) {
            return cnt;
        }
        if (root.val === '+' || root.val === '-') {
            const l = dfs(root.left);
            const r = dfs(root.right);
            const k = root.val === '+' ? 1 : -1;
            for (let i = 0; i < 26; ++i) {
                cnt[i] = l[i] + k * r[i];
            }
        } else {
            cnt[root.val.charCodeAt(0) - 'a'.charCodeAt(0)]++;
        }
        return cnt;
    };
    const cnt1 = dfs(root1);
    const cnt2 = dfs(root2);
    for (let i = 0; i < 26; ++i) {
        if (cnt1[i] !== cnt2[i]) {
            return false;
        }
    }
    return true;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
