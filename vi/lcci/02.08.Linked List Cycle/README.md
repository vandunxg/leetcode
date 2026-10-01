---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [02.08. Linked List Cycle](https://leetcode.cn/problems/linked-list-cycle-lcci)

[中文文档](/lcci/02.08.Linked%20List%20Cycle/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một linked list vòng, hãy cài đặt một thuật toán trả về node ở đầu vòng lặp.</p>

<p>Linked list vòng: Một linked list (bị lỗi) trong đó con trỏ next của một node trỏ đến một node ở trước đó, từ đó tạo thành một vòng lặp trong linked list.</p>

<p><strong>Ví dụ 1: </strong></p>

<pre>

<strong>Đầu vào: </strong>head = [3,2,0,-4], pos = 1

<strong>Đầu ra: </strong>tail connects to node index 1</pre>

<p><strong>Ví dụ 2: </strong></p>

<pre>

<strong>Đầu vào: </strong>head = [1,2], pos = 0

<strong>Đầu ra: </strong>tail connects to node index 0</pre>

<p><strong>Ví dụ 3: </strong></p>

<pre>

<strong>Đầu vào: </strong>head = [1], pos = -1

<strong>Đầu ra: </strong>no cycle</pre>

<p><strong>Câu hỏi mở rộng: </strong><br />
Bạn có thể giải bài này mà không dùng thêm không gian không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Có thể tìm vị trí đầu vòng bằng một visited set, nhưng cách này dùng không gian tuyến tính.
>
> Nếu fast và slow gặp nhau, điểm gặp nằm trong vòng. Sau đó, một con trỏ thứ ba đi từ head sẽ gặp slow tại vị trí đầu vòng, vì $x = (k-1)(y+z)+z$.
>
> Cho `fast` di chuyển mỗi bước $2$ node và `slow` di chuyển mỗi bước $1$ node cho đến khi chúng gặp nhau (hoặc `fast` đi đến cuối, nghĩa là không có vòng); sau đó cho $ans$ đi từ head cùng với `slow`. Không cần bảng bổ sung.

<!-- thinking:end -->

Trước tiên, chúng ta dùng con trỏ nhanh và con trỏ chậm để xác định linked list có vòng hay không. Nếu có vòng, con trỏ nhanh và con trỏ chậm chắc chắn sẽ gặp nhau, và node gặp nhau phải nằm trong vòng.

Nếu không có vòng, con trỏ nhanh sẽ đi đến đuôi của linked list trước, khi đó trả về `null` ngay.

Nếu có vòng, chúng ta định nghĩa một con trỏ kết quả $ans$ trỏ đến head của linked list, sau đó cho $ans$ và con trỏ chậm cùng di chuyển về phía trước, mỗi lần đi một bước, cho đến khi $ans$ và con trỏ chậm gặp nhau; node gặp nhau chính là node đầu vòng.

Tại sao cách này có thể tìm được node đầu vòng?

Giả sử khoảng cách từ node head của linked list đến đầu vòng là $x$, khoảng cách từ đầu vòng đến node gặp nhau là $y$, và khoảng cách từ node gặp nhau đến đầu vòng là $z$. Khi đó, quãng đường con trỏ chậm đi được là $x + y$, còn quãng đường con trỏ nhanh đi được là $x + y + k \times (y + z)$, trong đó $k$ là số vòng mà con trỏ nhanh đi quanh vòng.

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/lcci/02.08.Linked%20List%20Cycle/images/linked-list-cycle-ii.png" /></p>

Vì tốc độ của con trỏ nhanh gấp đôi con trỏ chậm, ta có $2 \times (x + y) = x + y + k \times (y + z)$, từ đó suy ra $x + y = k \times (y + z)$, tức là $x = (k - 1) \times (y + z) + z$.

Nói cách khác, nếu ta định nghĩa một con trỏ kết quả $ans$ trỏ đến head của linked list, rồi cho $ans$ và con trỏ chậm cùng di chuyển về phía trước, chúng chắc chắn sẽ gặp nhau tại đầu vòng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số node trong linked list. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None


class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        fast = slow = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                ans = head
                while ans != slow:
                    ans = ans.next
                    slow = slow.next
                return ans
```

#### Java

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode fast = head, slow = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) {
                ListNode ans = head;
                while (ans != slow) {
                    ans = ans.next;
                    slow = slow.next;
                }
                return ans;
            }
        }
        return null;
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
    ListNode* detectCycle(ListNode* head) {
        ListNode* fast = head;
        ListNode* slow = head;
        while (fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
            if (slow == fast) {
                ListNode* ans = head;
                while (ans != slow) {
                    ans = ans->next;
                    slow = slow->next;
                }
                return ans;
            }
        }
        return nullptr;
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
func detectCycle(head *ListNode) *ListNode {
	fast, slow := head, head
	for fast != nil && fast.Next != nil {
		slow = slow.Next
		fast = fast.Next.Next
		if slow == fast {
			ans := head
			for ans != slow {
				ans = ans.Next
				slow = slow.Next
			}
			return ans
		}
	}
	return nil
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

function detectCycle(head: ListNode | null): ListNode | null {
    let [slow, fast] = [head, head];
    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow === fast) {
            let ans = head;
            while (ans !== slow) {
                ans = ans.next;
                slow = slow.next;
            }
            return ans;
        }
    }
    return null;
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
 * @return {ListNode}
 */
var detectCycle = function (head) {
    let [slow, fast] = [head, head];
    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow === fast) {
            let ans = head;
            while (ans !== slow) {
                ans = ans.next;
                slow = slow.next;
            }
            return ans;
        }
    }
    return null;
};
```

#### Swift

```swift
/*
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
    func detectCycle(_ head: ListNode?) -> ListNode? {
        var slow = head
        var fast = head

        while fast != nil && fast?.next != nil {
            slow = slow?.next
            fast = fast?.next?.next
            if slow === fast {
                var ans = head
                while ans !== slow {
                    ans = ans?.next
                    slow = slow?.next
                }
                return ans
            }
        }
        return nil
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
