---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [02.06. Palindrome Linked List](https://leetcode.cn/problems/palindrome-linked-list-lcci)

[中文文档](/lcci/02.06.Palindrome%20Linked%20List/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy triển khai một hàm để kiểm tra xem một linked list có phải là palindrome hay không.</p>

<p>&nbsp;</p>

<p><strong>Ví dụ 1: </strong></p>

<pre>

<strong>Đầu vào:  </strong>1-&gt;2

<strong>Đầu ra: </strong> false

</pre>

<p><strong>Ví dụ 2: </strong></p>

<pre>

<strong>Đầu vào:  </strong>1-&gt;2-&gt;2-&gt;1

<strong>Đầu ra: </strong> true

</pre>

<p>&nbsp;</p>

<p><b>Câu hỏi mở rộng:</b><br />

Bạn có thể giải bài toán trong thời gian O(n) và không gian O(1) không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Fast and Slow Pointers + Reverse List

<!-- thinking:start -->

> **Tư duy**
>
> Việc chép các giá trị vào một mảng rồi so sánh hai đầu cần $O(n)$ không gian. Nếu được phép thay đổi linked list, ta chỉ cần thêm không gian hằng số.
>
> Một palindrome có thể được kiểm tra bằng cách so sánh nửa đầu với nửa sau đã đảo ngược. Fast/slow pointers giúp tìm phần tử giữa, sau đó đảo ngược nửa sau ngay tại chỗ rồi so sánh hai nửa.
>
> Với độ dài chẵn, slow pointer dừng ở phần tử giữa bên trái; phép đảo ngược bắt đầu từ `next` của nó. Xóa `slow.next` để giữ cho hai chain tách rời.

<!-- thinking:end -->

Trước hết, ta kiểm tra xem linked list có rỗng hay không. Nếu rỗng, ta trả về `true` ngay.

Tiếp theo, ta dùng fast pointer và slow pointer để tìm phần tử giữa của linked list. Nếu độ dài linked list là lẻ, slow pointer trỏ đến phần tử giữa. Nếu độ dài là chẵn, slow pointer trỏ đến phần tử đầu tiên trong hai phần tử giữa.

Sau đó, ta đảo ngược phần linked list nằm sau slow pointer, thu được nửa sau của linked list với node đầu là $p$.

Cuối cùng, ta duyệt để so sánh nửa đầu và nửa sau của linked list. Nếu có bất kỳ node nào khác nhau, ta trả về `false` ngay. Nếu không, sau khi duyệt hết linked list, ta trả về `true`.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của linked list. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        if head is None:
            return True
        slow, fast = head, head.next
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        p = slow.next
        slow.next = None
        dummy = ListNode()
        while p:
            next = p.next
            p.next = dummy.next
            dummy.next = p
            p = next
        p = dummy.next
        while p:
            if head.val != p.val:
                return False
            head = head.next
            p = p.next
        return True
```

#### Java

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isPalindrome(ListNode head) {
        if (head == null) {
            return true;
        }
        ListNode slow = head;
        ListNode fast = head.next;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode p = slow.next;
        slow.next = null;
        ListNode dummy = new ListNode(0);
        while (p != null) {
            ListNode next = p.next;
            p.next = dummy.next;
            dummy.next = p;
            p = next;
        }
        p = dummy.next;
        while (p != null) {
            if (head.val != p.val) {
                return false;
            }
            head = head.next;
            p = p.next;
        }
        return true;
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
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if (!head) {
            return true;
        }
        ListNode* slow = head;
        ListNode* fast = head->next;
        while (fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
        }
        ListNode* p = slow->next;
        slow->next = nullptr;
        ListNode* dummy = new ListNode(0);
        while (p) {
            ListNode* next = p->next;
            p->next = dummy->next;
            dummy->next = p;
            p = next;
        }
        p = dummy->next;
        while (p) {
            if (head->val != p->val) {
                return false;
            }
            head = head->next;
            p = p->next;
        }
        return true;
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
func isPalindrome(head *ListNode) bool {
	if head == nil {
		return true
	}
	slow, fast := head, head.Next
	for fast != nil && fast.Next != nil {
		slow, fast = slow.Next, fast.Next.Next
	}
	p := slow.Next
	slow.Next = nil
	dummy := &ListNode{}
	for p != nil {
		next := p.Next
		p.Next = dummy.Next
		dummy.Next = p
		p = next
	}
	p = dummy.Next
	for p != nil {
		if head.Val != p.Val {
			return false
		}
		head = head.Next
		p = p.Next
	}
	return true
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

function isPalindrome(head: ListNode | null): boolean {
    if (!head) {
        return true;
    }
    let slow = head;
    let fast = head.next;
    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
    }
    let p = slow.next;
    slow.next = null;
    const dummy = new ListNode(0);
    while (p) {
        const next = p.next;
        p.next = dummy.next;
        dummy.next = p;
        p = next;
    }
    p = dummy.next;
    while (p) {
        if (head.val !== p.val) {
            return false;
        }
        head = head.next;
        p = p.next;
    }
    return true;
}
```

#### JavaScript

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @return {boolean}
 */
var isPalindrome = function (head) {
    if (!head) {
        return true;
    }
    let slow = head;
    let fast = head.next;
    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
    }
    let p = slow.next;
    slow.next = null;
    const dummy = new ListNode(0);
    while (p) {
        const next = p.next;
        p.next = dummy.next;
        dummy.next = p;
        p = next;
    }
    p = dummy.next;
    while (p) {
        if (head.val !== p.val) {
            return false;
        }
        head = head.next;
        p = p.next;
    }
    return true;
};
```

#### C#

```cs
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public int val;
 *     public ListNode next;
 *     public ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public bool IsPalindrome(ListNode head) {
        if (head == null) {
            return true;
        }
        ListNode slow = head;
        ListNode fast = head.next;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode p = slow.next;
        slow.next = null;
        ListNode dummy = new ListNode(0);
        while (p != null) {
            ListNode next = p.next;
            p.next = dummy.next;
            dummy.next = p;
            p = next;
        }
        p = dummy.next;
        while (p != null) {
            if (head.val != p.val) {
                return false;
            }
            head = head.next;
            p = p.next;
        }
        return true;
    }
}
```

#### Swift

```swift
/**
* public class ListNode {
*    var val: Int
*    var next: ListNode?
*    init(_ x: Int) {
*        self.val = x
*        self.next = nil
*    }
* }
*/

class Solution {
    func isPalindrome(_ head: ListNode?) -> Bool {
        if head == nil {
            return true
        }

        var slow = head
        var fast = head?.next
        while fast != nil && fast?.next != nil {
            slow = slow?.next
            fast = fast?.next?.next
        }

        var p = slow?.next
        slow?.next = nil
        var dummy = ListNode(0)

        while p != nil {
            let next = p?.next
            p?.next = dummy.next
            dummy.next = p
            p = next
        }

        p = dummy.next
        var currentHead = head
        while p != nil {
            if currentHead?.val != p?.val {
                return false
            }
            currentHead = currentHead?.next
            p = p?.next
        }

        return true
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
