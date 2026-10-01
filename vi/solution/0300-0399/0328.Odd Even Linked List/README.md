---
comments: true
difficulty: Medium
tags:
    - Linked List
---

<!-- problem:start -->

# [328. Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list)

[中文文档](/solution/0300-0399/0328.Odd%20Even%20Linked%20List/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>head</code> của một singly linked list. Hãy nhóm các node ở vị trí lẻ lại với nhau, sau đó đến các node ở vị trí chẵn, rồi trả về <em>linked list sau khi sắp xếp lại</em>.</p>

<p>Node <strong>đầu tiên</strong> được xem là ở vị trí <strong>lẻ</strong>, node <strong>thứ hai</strong> ở vị trí <strong>chẵn</strong>, cứ thế tiếp tục.</p>

<p>Lưu ý rằng thứ tự tương đối bên trong cả nhóm node ở vị trí chẵn lẫn nhóm ở vị trí lẻ phải được giữ nguyên như trong input.</p>

<p>Bạn phải giải bài này với độ phức tạp không gian phụ <code>O(1)</code> và độ phức tạp thời gian <code>O(n)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0328.Odd%20Even%20Linked%20List/images/oddeven-linked-list.jpg" style="width: 300px; height: 123px;" />
<pre>
<strong>Đầu vào:</strong> head = [1,2,3,4,5]
<strong>Đầu ra:</strong> [1,3,5,2,4]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0328.Odd%20Even%20Linked%20List/images/oddeven2-linked-list.jpg" style="width: 500px; height: 142px;" />
<pre>
<strong>Đầu vào:</strong> head = [2,1,3,5,6,4,7]
<strong>Đầu ra:</strong> [2,3,6,7,1,5,4]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong linked list nằm trong khoảng <code>[0, 10<sup>4</sup>]</code>.</li>
	<li><code>-10<sup>6</sup> &lt;= Node.val &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lượt

<!-- thinking:start -->

> **Tư duy**
>
> Đưa các node ở vị trí lẻ lên trước các node ở vị trí chẵn, đồng thời giữ nguyên thứ tự. Dùng thêm một linked list thì đơn giản, nhưng yêu cầu là không gian $O(1)$.
>
> Dùng $a$ và $b$ lần lượt trỏ đến node cuối của chuỗi node ở vị trí lẻ và chẵn; $c$ giữ lại head của chuỗi node chẵn. Liên tục nối $b.next$ sau $a$, rồi nối node kế tiếp mới sau $b$, cho đến khi hết chuỗi node chẵn. Cuối cùng, đặt $a.next=c$.

<!-- thinking:end -->

Ta dùng hai pointer $a$ và $b$ lần lượt đại diện cho node cuối của nhóm node ở vị trí lẻ và chẵn. Ban đầu, $a$ trỏ đến node head $head$ của list, còn $b$ trỏ đến node thứ hai $head.next$. Ngoài ra, pointer $c$ trỏ đến head $head.next$ của nhóm node chẵn, cũng là vị trí ban đầu của $b$.

Ta duyệt list, cho $a$ trỏ đến node kế tiếp của $b$, tức $a.next = b.next$, rồi tiến $a$ lên một node bằng cách đặt $a = a.next$. Tiếp đó, cho $b$ trỏ đến node kế tiếp của $a$, tức $b.next = a.next$, rồi tiến $b$ lên một node bằng cách đặt $b = b.next$. Tiếp tục cho đến khi $b$ đến cuối list.

Cuối cùng, cho node cuối $a$ của nhóm node lẻ trỏ đến head $c$ của nhóm node chẵn, tức $a.next = c$, rồi trả về node head $head$ của list.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài list, vì ta chỉ cần duyệt list một lần. Độ phức tạp không gian là $O(1)$; ta chỉ cần duy trì một số lượng pointer cố định.

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def oddEvenList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head is None:
            return None
        a = head
        b = c = head.next
        while b and b.next:
            a.next = b.next
            a = a.next
            b.next = a.next
            b = b.next
        a.next = c
        return head
```

#### Java

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode oddEvenList(ListNode head) {
        if (head == null) {
            return null;
        }
        ListNode a = head;
        ListNode b = head.next, c = b;
        while (b != null && b.next != null) {
            a.next = b.next;
            a = a.next;
            b.next = a.next;
            b = b.next;
        }
        a.next = c;
        return head;
    }
}
```

#### C++

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        if (!head) {
            return nullptr;
        }
        ListNode* a = head;
        ListNode *b = head->next, *c = b;
        while (b && b->next) {
            a->next = b->next;
            a = a->next;
            b->next = a->next;
            b = b->next;
        }
        a->next = c;
        return head;
    }
};
```

#### Go

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func oddEvenList(head *ListNode) *ListNode {
	if head == nil {
		return nil
	}
	a := head
	b, c := head.Next, head.Next
	for b != nil && b.Next != nil {
		a.Next = b.Next
		a = a.Next
		b.Next = a.Next
		b = b.Next
	}
	a.Next = c
	return head
}
```

#### TypeScript

```ts
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     val: number
 *     next: ListNode | null
 *     constructor(val?: number, next?: ListNode | null) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.next = (next===undefined ? null : next)
 *     }
 * }
 */

function oddEvenList(head: ListNode | null): ListNode | null {
    if (!head) {
        return null;
    }
    let [a, b, c] = [head, head.next, head.next];
    while (b && b.next) {
        a.next = b.next;
        a = a.next;
        b.next = a.next;
        b = b.next;
    }
    a.next = c;
    return head;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
