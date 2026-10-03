---
comments: true
difficulty: Medium
tags:
    - Linked List
    - Two Pointers
    - Sorting
---

<!-- problem:start -->

# [2046. Sort Linked List Already Sorted Using Absolute Values 🔒](https://leetcode.com/problems/sort-linked-list-already-sorted-using-absolute-values)

[中文文档](/solution/2000-2099/2046.Sort%20Linked%20List%20Already%20Sorted%20Using%20Absolute%20Values/README.md)

## Mô tả

<!-- description:start -->

Cho <code>head</code> của một danh sách liên kết đơn được sắp xếp theo thứ tự <strong>không giảm</strong> dựa trên <strong>giá trị tuyệt đối</strong> của các node, hãy trả về <em>danh sách được sắp xếp theo thứ tự <strong>không giảm</strong> dựa trên <strong>giá trị thực</strong> của các node</em>.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2046.Sort%20Linked%20List%20Already%20Sorted%20Using%20Absolute%20Values/images/image-20211017201240-3.png" style="width: 621px; height: 250px;" />
<pre>
<strong>Đầu vào:</strong> head = [0,2,-5,5,10,-10]
<strong>Đầu ra:</strong> [-10,-5,0,2,5,10]
<strong>Giải thích:</strong>
Danh sách được sắp xếp theo thứ tự không giảm dựa trên giá trị tuyệt đối của các node là [0,2,-5,5,10,-10].
Danh sách được sắp xếp theo thứ tự không giảm dựa trên giá trị thực là [-10,-5,0,2,5,10].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2046.Sort%20Linked%20List%20Already%20Sorted%20Using%20Absolute%20Values/images/image-20211017201318-4.png" style="width: 338px; height: 250px;" />
<pre>
<strong>Đầu vào:</strong> head = [0,1,2]
<strong>Đầu ra:</strong> [0,1,2]
<strong>Giải thích:</strong>
Danh sách liên kết đã được sắp xếp theo thứ tự không giảm.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> head = [1]
<strong>Đầu ra:</strong> [1]
<strong>Giải thích:</strong>
Danh sách liên kết đã được sắp xếp theo thứ tự không giảm.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số lượng node trong danh sách nằm trong khoảng <code>[1, 10<sup>5</sup>]</code>.</li>
	<li><code>-5000 &lt;= Node.val &lt;= 5000</code></li>
	<li><code>head</code> được sắp xếp theo thứ tự không giảm dựa trên giá trị tuyệt đối của các node.</li>
</ul>

<p>&nbsp;</p>
<strong>Câu hỏi mở rộng:</strong>
<ul>
	<li>Bạn có thể nghĩ ra lời giải có độ phức tạp thời gian <code>O(n)</code> không?</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phương pháp chèn vào đầu

<!-- thinking:start -->

> **Tư duy**
>
> Danh sách được sắp xếp theo giá trị tuyệt đối, vì vậy các số âm phải được đưa lên đầu theo thứ tự ngược với thứ tự xuất hiện. Chỉ cần duyệt một lần: chèn node âm vào đầu danh sách, còn với các node không âm thì tiếp tục tiến lên.
>
> $O(1)$ bộ nhớ bổ sung; không cần dựng lại mảng.

<!-- thinking:end -->

Trước hết, ta giả sử node đầu tiên đã được sắp xếp đúng vị trí. Bắt đầu từ node thứ hai, khi gặp một node có giá trị âm, ta sử dụng phương pháp chèn vào đầu. Với các giá trị không âm, ta tiếp tục duyệt xuống dưới.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của danh sách liên kết. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def sortLinkedList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        prev, curr = head, head.next
        while curr:
            if curr.val < 0:
                t = curr.next
                prev.next = t
                curr.next = head
                head = curr
                curr = t
            else:
                prev, curr = curr, curr.next
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
    public ListNode sortLinkedList(ListNode head) {
        ListNode prev = head, curr = head.next;
        while (curr != null) {
            if (curr.val < 0) {
                ListNode t = curr.next;
                prev.next = t;
                curr.next = head;
                head = curr;
                curr = t;
            } else {
                prev = curr;
                curr = curr.next;
            }
        }
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
    ListNode* sortLinkedList(ListNode* head) {
        ListNode* prev = head;
        ListNode* curr = head->next;
        while (curr) {
            if (curr->val < 0) {
                auto t = curr->next;
                prev->next = t;
                curr->next = head;
                head = curr;
                curr = t;
            } else {
                prev = curr;
                curr = curr->next;
            }
        }
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
func sortLinkedList(head *ListNode) *ListNode {
	prev, curr := head, head.Next
	for curr != nil {
		if curr.Val < 0 {
			t := curr.Next
			prev.Next = t
			curr.Next = head
			head = curr
			curr = t
		} else {
			prev, curr = curr, curr.Next
		}
	}
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

function sortLinkedList(head: ListNode | null): ListNode | null {
    let [prev, curr] = [head, head.next];
    while (curr !== null) {
        if (curr.val < 0) {
            const t = curr.next;
            prev.next = t;
            curr.next = head;
            head = curr;
            curr = t;
        } else {
            [prev, curr] = [curr, curr.next];
        }
    }
    return head;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
