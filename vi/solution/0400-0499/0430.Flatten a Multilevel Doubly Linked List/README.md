---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Linked List
    - Doubly-Linked List
---

<!-- problem:start -->

# [430. Flatten a Multilevel Doubly Linked List](https://leetcode.com/problems/flatten-a-multilevel-doubly-linked-list)

[中文文档](/solution/0400-0499/0430.Flatten%20a%20Multilevel%20Doubly%20Linked%20List/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một doubly linked list gồm các node có pointer <code>next</code>, pointer <code>prev</code> và thêm một <strong>child pointer</strong>. Pointer này có thể trỏ hoặc không trỏ đến một doubly linked list riêng, cũng chứa các node cùng loại. Các danh sách con này lại có thể có thêm danh sách con nữa, tạo thành một <strong>cấu trúc dữ liệu nhiều cấp</strong> như ví dụ bên dưới.</p>

<p>Cho <code>head</code> của danh sách ở cấp đầu tiên, hãy <strong>làm phẳng</strong> danh sách để tất cả node nằm trong một doubly linked list duy nhất. Giả sử <code>curr</code> là node có danh sách con. Trong danh sách sau khi làm phẳng, các node của danh sách con phải nằm <strong>sau</strong> <code>curr</code> và <strong>trước</strong> <code>curr.next</code>.</p>

<p>Trả về <em><code>head</code> </em>của danh sách đã làm phẳng. Tất cả child pointer của các node trong danh sách phải được đặt thành <code>null</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0430.Flatten%20a%20Multilevel%20Doubly%20Linked%20List/images/flatten11.jpg" style="width: 700px; height: 339px;" />
<pre>
<strong>Đầu vào:</strong> head = [1,2,3,4,5,6,null,null,null,7,8,9,10,null,null,11,12]
<strong>Đầu ra:</strong> [1,2,3,7,8,11,12,9,10,4,5,6]
<strong>Giải thích:</strong> Danh sách liên kết nhiều cấp trong đầu vào được minh họa bên dưới.
Sau khi làm phẳng, danh sách trở thành:
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0430.Flatten%20a%20Multilevel%20Doubly%20Linked%20List/images/flatten12.jpg" style="width: 1000px; height: 69px;" />
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0430.Flatten%20a%20Multilevel%20Doubly%20Linked%20List/images/flatten2.1jpg" style="width: 200px; height: 200px;" />
<pre>
<strong>Đầu vào:</strong> head = [1,2,null,3]
<strong>Đầu ra:</strong> [1,3,2]
<strong>Giải thích:</strong> Danh sách liên kết nhiều cấp trong đầu vào được minh họa bên dưới.
Sau khi làm phẳng, danh sách trở thành:
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0430.Flatten%20a%20Multilevel%20Doubly%20Linked%20List/images/list.jpg" style="width: 300px; height: 87px;" />
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> head = []
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> Danh sách đầu vào có thể rỗng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node không vượt quá <code>1000</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Cách biểu diễn danh sách liên kết nhiều cấp trong các test case:</strong></p>

<p>Ta dùng danh sách liên kết nhiều cấp trong <strong>Ví dụ 1</strong> ở trên:</p>

<pre>
 1---2---3---4---5---6--NULL
         |
         7---8---9---10--NULL
             |
             11--12--NULL</pre>

<p>Cách tuần tự hóa từng cấp như sau:</p>

<pre>
[1,2,3,4,5,6,null]
[7,8,9,10,null]
[11,12,null]
</pre>

<p>Để tuần tự hóa tất cả các cấp cùng nhau, ta thêm giá trị null vào mỗi cấp để biểu thị rằng không có node nào nối với node phía trên ở cấp trước đó. Khi ấy, kết quả tuần tự hóa là:</p>

<pre>
[1,    2,    3, 4, 5, 6, null]
             |
[null, null, 7,    8, 9, 10, null]
                   |
[            null, 11, 12, null]
</pre>

<p>Gộp phần tuần tự hóa của từng cấp và bỏ các giá trị null ở cuối, ta được:</p>

<pre>
[1,2,3,4,5,6,null,null,null,7,8,9,10,null,null,11,12]
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Làm phẳng danh sách nhiều cấp tương đương duyệt preorder: đi vào child trước node next ban đầu. Nếu duyệt next trước, danh sách con sẽ bị nối xuống cuối cùng.
>
> $\textit{preorder}(\textit{pre},\textit{cur})$ nối node hiện tại sau node predecessor, lưu lại next cũ, làm phẳng danh sách con (tail trả về trở thành predecessor mới), rồi làm phẳng next đã lưu và xóa $\textit{child}$.
>
> Dùng một dummy node để giữ head; sau đó xóa $\textit{prev}$ của head thật. Cần lưu next ban đầu trước khi duyệt danh sách con ghi đè lên nó.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val, prev, next, child):
        self.val = val
        self.prev = prev
        self.next = next
        self.child = child
"""


class Solution:
    def flatten(self, head: 'Node') -> 'Node':
        def preorder(pre, cur):
            if cur is None:
                return pre
            cur.prev = pre
            pre.next = cur

            t = cur.next
            tail = preorder(cur, cur.child)
            cur.child = None
            return preorder(tail, t)

        if head is None:
            return None
        dummy = Node(0, None, head, None)
        preorder(dummy, head)
        dummy.next.prev = None
        return dummy.next
```

#### Java

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node prev;
    public Node next;
    public Node child;
};
*/

class Solution {
    public Node flatten(Node head) {
        if (head == null) {
            return null;
        }
        Node dummy = new Node();
        dummy.next = head;
        preorder(dummy, head);
        dummy.next.prev = null;
        return dummy.next;
    }

    private Node preorder(Node pre, Node cur) {
        if (cur == null) {
            return pre;
        }
        cur.prev = pre;
        pre.next = cur;

        Node t = cur.next;
        Node tail = preorder(cur, cur.child);
        cur.child = null;
        return preorder(tail, t);
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
    Node* prev;
    Node* next;
    Node* child;
};
*/

class Solution {
public:
    Node* flatten(Node* head) {
        flattenGetTail(head);
        return head;
    }

    Node* flattenGetTail(Node* head) {
        Node* cur = head;
        Node* tail = nullptr;

        while (cur) {
            Node* next = cur->next;
            if (cur->child) {
                Node* child = cur->child;
                Node* childTail = flattenGetTail(cur->child);

                cur->child = nullptr;
                cur->next = child;
                child->prev = cur;
                childTail->next = next;

                if (next)
                    next->prev = childTail;

                tail = childTail;
            } else {
                tail = cur;
            }

            cur = next;
        }

        return tail;
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
