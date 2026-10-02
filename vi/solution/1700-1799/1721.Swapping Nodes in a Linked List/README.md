---
comments: true
difficulty: Medium
rating: 1386
source: Weekly Contest 223 Q2
tags:
    - Linked List
    - Two Pointers
---

<!-- problem:start -->

# [1721. Swapping Nodes in a Linked List](https://leetcode.com/problems/swapping-nodes-in-a-linked-list)

[中文文档](/solution/1700-1799/1721.Swapping%20Nodes%20in%20a%20Linked%20List/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>head</code> của một linked list và một số nguyên <code>k</code>.</p>

<p>Trả về <em>head của linked list sau khi <strong>hoán đổi</strong> giá trị của </em><code>k<sup>th</sup></code> <em>node tính từ đầu và </em><code>k<sup>th</sup></code> <em>node tính từ cuối (list được đánh chỉ số từ <strong>1</strong>).</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1700-1799/1721.Swapping%20Nodes%20in%20a%20Linked%20List/images/linked1.jpg" style="width: 400px; height: 112px;" />
<pre>
<strong>Đầu vào:</strong> head = [1,2,3,4,5], k = 2
<strong>Đầu ra:</strong> [1,4,3,2,5]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> head = [7,9,6,6,7,8,3,0,9,5], k = 5
<strong>Đầu ra:</strong> [7,9,6,6,8,7,3,0,9,5]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong list là <code>n</code>.</li>
	<li><code>1 &lt;= k &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= Node.val &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Ta hoán đổi giá trị của node thứ $k$ tính từ đầu và từ cuối. Có thể tính độ dài rồi duyệt hai lần, nhưng chỉ cần một lần duyệt.
>
> Tiến con trỏ nhanh $k-1$ bước đến node thứ $k$ là $p$, sau đó di chuyển fast và slow cùng nhau. Khi fast đến node cuối, slow là node thứ $k$ tính từ cuối.
>
> Hoán đổi hai giá trị rồi trả về head.

<!-- thinking:end -->

Trước hết, ta dùng con trỏ nhanh `fast` để tìm node thứ $k$ của linked list và dùng con trỏ `p` trỏ đến node đó. Sau đó, ta dùng con trỏ chậm `slow` bắt đầu từ node head của linked list và di chuyển cả hai con trỏ cùng lúc. Khi con trỏ nhanh đến node cuối của linked list, con trỏ chậm `slow` trỏ đến node thứ $k$ tính từ cuối, và ta dùng con trỏ `q` trỏ đến node đó. Lúc này, chỉ cần hoán đổi giá trị của `p` và `q`.

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
    def swapNodes(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        fast = slow = head
        for _ in range(k - 1):
            fast = fast.next
        p = fast
        while fast.next:
            fast, slow = fast.next, slow.next
        q = slow
        p.val, q.val = q.val, p.val
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
    public ListNode swapNodes(ListNode head, int k) {
        ListNode fast = head;
        while (--k > 0) {
            fast = fast.next;
        }
        ListNode p = fast;
        ListNode slow = head;
        while (fast.next != null) {
            fast = fast.next;
            slow = slow.next;
        }
        ListNode q = slow;
        int t = p.val;
        p.val = q.val;
        q.val = t;
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
    ListNode* swapNodes(ListNode* head, int k) {
        ListNode* fast = head;
        while (--k) {
            fast = fast->next;
        }
        ListNode* slow = head;
        ListNode* p = fast;
        while (fast->next) {
            fast = fast->next;
            slow = slow->next;
        }
        ListNode* q = slow;
        swap(p->val, q->val);
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
func swapNodes(head *ListNode, k int) *ListNode {
	fast := head
	for ; k > 1; k-- {
		fast = fast.Next
	}
	p := fast
	slow := head
	for fast.Next != nil {
		fast, slow = fast.Next, slow.Next
	}
	q := slow
	p.Val, q.Val = q.Val, p.Val
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

function swapNodes(head: ListNode | null, k: number): ListNode | null {
    let [fast, slow] = [head, head];
    while (--k) {
        fast = fast.next;
    }
    const p = fast;
    while (fast.next) {
        fast = fast.next;
        slow = slow.next;
    }
    const q = slow;
    [p.val, q.val] = [q.val, p.val];
    return head;
}
```

#### C#

```cs
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public int val;
 *     public ListNode next;
 *     public ListNode(int val=0, ListNode next=null) {
 *         this.val = val;
 *         this.next = next;
 *     }
 * }
 */
public class Solution {
    public ListNode SwapNodes(ListNode head, int k) {
        ListNode fast = head;
        while (--k > 0) {
            fast = fast.next;
        }
        ListNode p = fast;
        ListNode slow = head;
        while (fast.next != null) {
            fast = fast.next;
            slow = slow.next;
        }
        ListNode q = slow;
        int t = p.val;
        p.val = q.val;
        q.val = t;
        return head;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
