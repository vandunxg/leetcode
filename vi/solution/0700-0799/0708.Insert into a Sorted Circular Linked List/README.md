---
comments: true
difficulty: Medium
tags:
    - Linked List
---

<!-- problem:start -->

# [708. Insert into a Sorted Circular Linked List 🔒](https://leetcode.com/problems/insert-into-a-sorted-circular-linked-list)

[中文文档](/solution/0700-0799/0708.Insert%20into%20a%20Sorted%20Circular%20Linked%20List/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một node trong circular linked list đã được sắp xếp theo thứ tự không giảm. Hãy viết hàm chèn giá trị <code>insertVal</code> vào list sao cho list vẫn được sắp xếp và giữ tính tuần hoàn. Node được cho có thể là tham chiếu đến bất kỳ node nào trong list, không nhất thiết là node có giá trị nhỏ nhất.</p>

<p>Nếu có nhiều vị trí phù hợp để chèn, bạn có thể chọn bất kỳ vị trí nào. Sau khi chèn, circular list vẫn phải được sắp xếp.</p>

<p>Nếu list rỗng (tức node được cho là <code>null</code>), hãy tạo một circular list chỉ có một node và trả về tham chiếu đến node đó. Nếu không, hãy trả về node ban đầu được cung cấp.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0708.Insert%20into%20a%20Sorted%20Circular%20Linked%20List/images/example_1_before_65p.jpg" style="width: 250px; height: 149px;" /><br />
&nbsp;
<pre>
<strong>Đầu vào:</strong> head = [3,4,1], insertVal = 2
<strong>Đầu ra:</strong> [3,4,1,2]
<strong>Giải thích:</strong> Hình trên minh họa circular list đã sắp xếp gồm ba phần tử. Bạn được cung cấp tham chiếu đến node có giá trị 3 và cần chèn 2 vào list. Node mới nên được chèn giữa node 1 và node 3. Sau khi chèn, list có dạng như hình dưới đây và ta vẫn trả về node 3.

<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0708.Insert%20into%20a%20Sorted%20Circular%20Linked%20List/images/example_1_after_65p.jpg" style="width: 250px; height: 149px;" />

</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> head = [], insertVal = 1
<strong>Đầu ra:</strong> [1]
<strong>Giải thích:</strong> List rỗng (head được cho là <code>null</code>). Ta tạo circular list chỉ có một node rồi trả về tham chiếu đến node đó.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> head = [1], insertVal = 0
<strong>Đầu ra:</strong> [1,0]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong list nằm trong khoảng <code>[0, 5 * 10<sup>4</sup>]</code>.</li>
	<li><code>-10<sup>6</sup> &lt;= Node.val, insertVal &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Solution 1

<!-- thinking:start -->

> **Tư duy**
>
> Chèn vào circular list không giảm. List có thể có đến $5 \times 10^4$ node, nên cần tìm vị trí chèn chỉ sau tối đa một vòng duyệt. Nếu list rỗng, node mới sẽ trỏ đến chính nó.
>
> Trong một vòng đã sắp xếp, chỉ có một chỗ giảm từ giá trị lớn nhất về nhỏ nhất. Giá trị cần chèn nằm trong một đoạn không giảm, hoặc nằm qua chỗ giảm đó nếu nó lớn hơn hoặc bằng giá trị lớn nhất hay nhỏ hơn hoặc bằng giá trị nhỏ nhất.
>
> Duyệt qua các cặp node kề nhau $\textit{prev}$ và $\textit{curr}$ cho đến khi gặp một trong các điều kiện trên; nếu list không giảm trên toàn bộ một vòng thì ta sẽ đến cặp node cuối cùng. Nối node mới vào rồi trả về head ban đầu.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, next=None):
        self.val = val
        self.next = next
"""


class Solution:
    def insert(self, head: 'Optional[Node]', insertVal: int) -> 'Node':
        node = Node(insertVal)
        if head is None:
            node.next = node
            return node
        prev, curr = head, head.next
        while curr != head:
            if prev.val <= insertVal <= curr.val or (
                prev.val > curr.val and (insertVal >= prev.val or insertVal <= curr.val)
            ):
                break
            prev, curr = curr, curr.next
        prev.next = node
        node.next = curr
        return head
```

#### Java

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node next;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, Node _next) {
        val = _val;
        next = _next;
    }
};
*/

class Solution {
    public Node insert(Node head, int insertVal) {
        Node node = new Node(insertVal);
        if (head == null) {
            node.next = node;
            return node;
        }
        Node prev = head, curr = head.next;
        while (curr != head) {
            if ((prev.val <= insertVal && insertVal <= curr.val)
                || (prev.val > curr.val && (insertVal >= prev.val || insertVal <= curr.val))) {
                break;
            }
            prev = curr;
            curr = curr.next;
        }
        prev.next = node;
        node.next = curr;
        return head;
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
    Node* next;

    Node() {}

    Node(int _val) {
        val = _val;
        next = NULL;
    }

    Node(int _val, Node* _next) {
        val = _val;
        next = _next;
    }
};
*/

class Solution {
public:
    Node* insert(Node* head, int insertVal) {
        Node* node = new Node(insertVal);
        if (!head) {
            node->next = node;
            return node;
        }
        Node *prev = head, *curr = head->next;
        while (curr != head) {
            if ((prev->val <= insertVal && insertVal <= curr->val) || (prev->val > curr->val && (insertVal >= prev->val || insertVal <= curr->val))) break;
            prev = curr;
            curr = curr->next;
        }
        prev->next = node;
        node->next = curr;
        return head;
    }
};
```

#### Go

```go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Next *Node
 * }
 */

func insert(head *Node, x int) *Node {
	node := &Node{Val: x}
	if head == nil {
		node.Next = node
		return node
	}
	prev, curr := head, head.Next
	for curr != head {
		if (prev.Val <= x && x <= curr.Val) || (prev.Val > curr.Val && (x >= prev.Val || x <= curr.Val)) {
			break
		}
		prev, curr = curr, curr.Next
	}
	prev.Next = node
	node.Next = curr
	return head
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
