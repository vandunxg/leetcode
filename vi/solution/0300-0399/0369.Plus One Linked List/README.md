---
comments: true
difficulty: Medium
tags:
    - Linked List
    - Math
---

<!-- problem:start -->

# [369. Plus One Linked List 🔒](https://leetcode.com/problems/plus-one-linked-list)

[中文文档](/solution/0300-0399/0369.Plus%20One%20Linked%20List/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên không âm được biểu diễn dưới dạng linked list các chữ số, hãy <em>cộng thêm một vào số nguyên đó</em>.</p>

<p>Các chữ số được lưu sao cho chữ số ở hàng cao nhất nằm tại <code>head</code> của danh sách.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> head = [1,2,3]
<strong>Đầu ra:</strong> [1,2,4]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> head = [0]
<strong>Đầu ra:</strong> [1]
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong linked list nằm trong khoảng <code>[1, 100]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 9</code></li>
	<li>Số được biểu diễn bởi linked list không có chữ số 0 ở đầu, trừ trường hợp số đó là 0.&nbsp;</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt linked list

<!-- thinking:start -->

> **Tư duy**
>
> Cộng một vào số nguyên không âm được lưu dưới dạng danh sách. Chuyển toàn bộ thành mảng sẽ tốn thêm $O(n)$ bộ nhớ. Phần cần xử lý chỉ là dãy số 9 ở cuối.
>
> Dùng dummy head để xử lý trường hợp phát sinh chữ số mới ở đầu. Duyệt một lượt để ghi nhớ node cuối cùng không phải 9, tăng giá trị node đó lên 1 rồi đổi các node phía sau thành 0. Nếu dummy có giá trị $1$, nó sẽ trở thành head mới.

<!-- thinking:end -->

Trước tiên, ta tạo dummy head node $\textit{dummy}$ có giá trị ban đầu là $0$, với node kế tiếp trỏ đến linked list $\textit{head}$.

Tiếp theo, ta duyệt linked list bắt đầu từ dummy head, tìm node cuối cùng không có giá trị $9$, tăng giá trị node đó lên $1$, rồi đặt giá trị của tất cả node phía sau thành $0$.

Cuối cùng, ta kiểm tra giá trị của dummy head node có bằng $1$ hay không. Nếu bằng $1$, trả về $\textit{dummy}$; nếu không, trả về node kế tiếp của $\textit{dummy}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài linked list. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def plusOne(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode(0, head)
        target = dummy
        while head:
            if head.val != 9:
                target = head
            head = head.next
        target.val += 1
        target = target.next
        while target:
            target.val = 0
            target = target.next
        return dummy if dummy.val else dummy.next
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
    public ListNode plusOne(ListNode head) {
        ListNode dummy = new ListNode(0, head);
        ListNode target = dummy;
        while (head != null) {
            if (head.val != 9) {
                target = head;
            }
            head = head.next;
        }
        ++target.val;
        target = target.next;
        while (target != null) {
            target.val = 0;
            target = target.next;
        }
        return dummy.val == 1 ? dummy : dummy.next;
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
    ListNode* plusOne(ListNode* head) {
        ListNode* dummy = new ListNode(0, head);
        ListNode* target = dummy;
        for (; head; head = head->next) {
            if (head->val != 9) {
                target = head;
            }
        }
        target->val++;
        for (target = target->next; target; target = target->next) {
            target->val = 0;
        }
        return dummy->val ? dummy : dummy->next;
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
func plusOne(head *ListNode) *ListNode {
	dummy := &ListNode{0, head}
	target := dummy
	for head != nil {
		if head.Val != 9 {
			target = head
		}
		head = head.Next
	}
	target.Val++
	for target = target.Next; target != nil; target = target.Next {
		target.Val = 0
	}
	if dummy.Val == 1 {
		return dummy
	}
	return dummy.Next
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

function plusOne(head: ListNode | null): ListNode | null {
    const dummy = new ListNode(0, head);
    let target = dummy;
    while (head) {
        if (head.val !== 9) {
            target = head;
        }
        head = head.next;
    }
    target.val++;
    for (target = target.next; target; target = target.next) {
        target.val = 0;
    }
    return dummy.val ? dummy : dummy.next;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
