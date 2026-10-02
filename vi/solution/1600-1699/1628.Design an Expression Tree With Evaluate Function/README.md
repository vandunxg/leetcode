---
comments: true
difficulty: Medium
tags:
    - Stack
    - Tree
    - Design
    - Array
    - Math
    - Binary Tree
---

<!-- problem:start -->

# [1628. Design an Expression Tree With Evaluate Function 🔒](https://leetcode.com/problems/design-an-expression-tree-with-evaluate-function)

[中文文档](/solution/1600-1699/1628.Design%20an%20Expression%20Tree%20With%20Evaluate%20Function/README.md)

## Mô tả

<!-- description:start -->

<p>Cho các token <code>postfix</code> của một biểu thức số học, hãy xây dựng và trả về <em>cây biểu thức nhị phân biểu diễn biểu thức này.</em></p>

<p>Ký pháp <b>Postfix</b> là cách viết biểu thức số học trong đó toán hạng (các số) xuất hiện trước toán tử. Ví dụ, các token postfix của biểu thức <code>4*(5-(7+2))</code> được biểu diễn trong mảng <code>postfix = [&quot;4&quot;,&quot;5&quot;,&quot;7&quot;,&quot;2&quot;,&quot;+&quot;,&quot;-&quot;,&quot;*&quot;]</code>.</p>

<p>Lớp <code>Node</code> là interface bạn cần dùng để cài đặt cây biểu thức nhị phân. Cây được trả về sẽ được kiểm tra bằng hàm <code>evaluate</code>, dùng để tính giá trị của cây. Không được xóa lớp <code>Node</code>; tuy nhiên, bạn có thể sửa nó tùy ý và định nghĩa thêm các lớp khác nếu cần.</p>

<p><strong><a href="https://en.wikipedia.org/wiki/Binary_expression_tree" target="_blank">Cây biểu thức nhị phân</a></strong> là một loại cây nhị phân dùng để biểu diễn biểu thức số học. Mỗi nút của cây biểu thức nhị phân có không hoặc hai nút con. Nút lá (nút có 0 nút con) tương ứng với toán hạng (các số), còn nút trong (nút có hai nút con) tương ứng với các toán tử <code>&#39;+&#39;</code> (cộng), <code>&#39;-&#39;</code> (trừ), <code>&#39;*&#39;</code> (nhân) và <code>&#39;/&#39;</code> (chia).</p>

<p>Đảm bảo không có cây con nào cho giá trị có trị tuyệt đối vượt quá <code>10<sup>9</sup></code>, và mọi phép toán đều hợp lệ (tức là không chia cho không).</p>

<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể thiết kế cây biểu thức theo hướng module hóa hơn không? Ví dụ, thiết kế của bạn có hỗ trợ thêm toán tử mà không cần sửa cách cài đặt <code>evaluate</code> hiện tại không?</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1628.Design%20an%20Expression%20Tree%20With%20Evaluate%20Function/images/untitled-diagram.png" style="width: 242px; height: 241px;" />
<pre>
<strong>Input:</strong> s = [&quot;3&quot;,&quot;4&quot;,&quot;+&quot;,&quot;2&quot;,&quot;*&quot;,&quot;7&quot;,&quot;/&quot;]
<strong>Output:</strong> 2
<strong>Explanation:</strong> this expression evaluates to the above binary tree with expression (<code>(3+4)*2)/7) = 14/7 = 2.</code>
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1628.Design%20an%20Expression%20Tree%20With%20Evaluate%20Function/images/untitled-diagram2.png" style="width: 222px; height: 232px;" />
<pre>
<strong>Input:</strong> s = [&quot;4&quot;,&quot;5&quot;,&quot;2&quot;,&quot;7&quot;,&quot;+&quot;,&quot;-&quot;,&quot;*&quot;]
<strong>Output:</strong> -16
<strong>Explanation:</strong> this expression evaluates to the above binary tree with expression 4*(5-<code>(2+7)) = 4*(-4) = -16.</code>
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt; 100</code></li>
	<li><code>s.length</code> là số lẻ.</li>
	<li><code>s</code> chỉ gồm các số và ký tự <code>&#39;+&#39;</code>, <code>&#39;-&#39;</code>, <code>&#39;*&#39;</code>, <code>&#39;/&#39;</code>.</li>
	<li>Nếu <code>s[i]</code> là một số, biểu diễn nguyên của nó không lớn hơn <code>10<sup>5</sup></code>.</li>
	<li>Đảm bảo <code>s</code> là một biểu thức hợp lệ.</li>
	<li>Trị tuyệt đối của kết quả và các giá trị trung gian không vượt quá <code>10<sup>9</sup></code>.</li>
	<li>Đảm bảo không có biểu thức nào chứa phép chia cho không.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đầu vào ở dạng postfix, nên một toán tử luôn đứng sau hai toán hạng của nó và stack có thể dựng lại cây.
>
> Các chữ số được push; khi gặp toán tử, pop nút con phải rồi nút con trái, nối chúng và push nút mới. Nút còn lại là root.
>
> Khi tính giá trị, trả về số nguyên tại nút lá và áp dụng toán tử lên hai nút con, dùng phép chia nguyên.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
import abc
from abc import ABC, abstractmethod

"""
This is the interface for the expression tree Node.
You should not remove it, and you can define some classes to implement it.
"""


class Node(ABC):
    @abstractmethod
    # define your fields here
    def evaluate(self) -> int:
        pass


class MyNode(Node):
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None

    def evaluate(self) -> int:
        x = self.val
        if x.isdigit():
            return int(x)

        left, right = self.left.evaluate(), self.right.evaluate()
        if x == '+':
            return left + right
        if x == '-':
            return left - right
        if x == '*':
            return left * right
        if x == '/':
            return left // right


"""
This is the TreeBuilder class.
You can treat it as the driver code that takes the postinfix input
and returns the expression tree represnting it as a Node.
"""


class TreeBuilder(object):
    def buildTree(self, postfix: List[str]) -> 'Node':
        stk = []
        for s in postfix:
            node = MyNode(s)
            if not s.isdigit():
                node.right = stk.pop()
                node.left = stk.pop()
            stk.append(node)
        return stk[-1]


"""
Your TreeBuilder object will be instantiated and called as such:
obj = TreeBuilder();
expTree = obj.buildTree(postfix);
ans = expTree.evaluate();
"""
```

#### Java

```java
/**
 * This is the interface for the expression tree Node.
 * You should not remove it, and you can define some classes to implement it.
 */

abstract class Node {
    public abstract int evaluate();
    // define your fields here
    protected String val;
    protected Node left;
    protected Node right;
};

class MyNode extends Node {
    public MyNode(String val) {
        this.val = val;
    }

    public int evaluate() {
        if (isNumeric()) {
            return Integer.parseInt(val);
        }
        int leftVal = left.evaluate();
        int rightVal = right.evaluate();
        if (Objects.equals(val, "+")) {
            return leftVal + rightVal;
        }
        if (Objects.equals(val, "-")) {
            return leftVal - rightVal;
        }
        if (Objects.equals(val, "*")) {
            return leftVal * rightVal;
        }
        if (Objects.equals(val, "/")) {
            return leftVal / rightVal;
        }
        return 0;
    }

    public boolean isNumeric() {
        for (char c : val.toCharArray()) {
            if (!Character.isDigit(c)) {
                return false;
            }
        }
        return true;
    }
}

/**
 * This is the TreeBuilder class.
 * You can treat it as the driver code that takes the postinfix input
 * and returns the expression tree represnting it as a Node.
 */

class TreeBuilder {
    Node buildTree(String[] postfix) {
        Deque<MyNode> stk = new ArrayDeque<>();
        for (String s : postfix) {
            MyNode node = new MyNode(s);
            if (!node.isNumeric()) {
                node.right = stk.pop();
                node.left = stk.pop();
            }
            stk.push(node);
        }
        return stk.peek();
    }
};

/**
 * Your TreeBuilder object will be instantiated and called as such:
 * TreeBuilder obj = new TreeBuilder();
 * Node expTree = obj.buildTree(postfix);
 * int ans = expTree.evaluate();
 */
```

#### C++

```cpp
/**
 * This is the interface for the expression tree Node.
 * You should not remove it, and you can define some classes to implement it.
 */

class Node {
public:
    virtual ~Node(){};
    virtual int evaluate() const = 0;

protected:
    // define your fields here
    string val;
    Node* left;
    Node* right;
};

class MyNode : public Node {
public:
    MyNode(string val) {
        this->val = val;
    }

    MyNode(string val, Node* left, Node* right) {
        this->val = val;
        this->left = left;
        this->right = right;
    }

    int evaluate() const {
        if (!(val == "+" || val == "-" || val == "*" || val == "/")) return stoi(val);
        auto leftVal = left->evaluate(), rightVal = right->evaluate();
        if (val == "+") return leftVal + rightVal;
        if (val == "-") return leftVal - rightVal;
        if (val == "*") return leftVal * rightVal;
        if (val == "/") return leftVal / rightVal;
        return 0;
    }
};

/**
 * This is the TreeBuilder class.
 * You can treat it as the driver code that takes the postinfix input
 * and returns the expression tree represnting it as a Node.
 */

class TreeBuilder {
public:
    Node* buildTree(vector<string>& postfix) {
        stack<MyNode*> stk;
        for (auto s : postfix) {
            MyNode* node;
            if (s == "+" || s == "-" || s == "*" || s == "/") {
                auto right = stk.top();
                stk.pop();
                auto left = stk.top();
                stk.pop();
                node = new MyNode(s, left, right);
            } else {
                node = new MyNode(s);
            }
            stk.push(node);
        }
        return stk.top();
    }
};

/**
 * Your TreeBuilder object will be instantiated and called as such:
 * TreeBuilder* obj = new TreeBuilder();
 * Node* expTree = obj->buildTree(postfix);
 * int ans = expTree->evaluate();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
