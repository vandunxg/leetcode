---
comments: true
difficulty: Medium
rating: 1685
source: Weekly Contest 267 Q2
tags:
    - Linked List
---

<!-- problem:start -->

# [2074. Reverse Nodes in Even Length Groups](https://leetcode.com/problems/reverse-nodes-in-even-length-groups)

[中文文档](/solution/2000-2099/2074.Reverse%20Nodes%20in%20Even%20Length%20Groups/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho <code>head</code> của một linked list.</p>

<p>Các node trong linked list được <strong>phân lần lượt</strong> vào các <strong>nhóm không rỗng</strong> có độ dài tạo thành dãy số tự nhiên (<code>1, 2, 3, 4, ...</code>). <strong>Độ dài</strong> của một nhóm là số node được phân vào nhóm đó. Nói cách khác,</p>

<ul>
	<li>Node <code>1<sup>st</sup></code> được phân vào nhóm đầu tiên.</li>
	<li>Các node <code>2<sup>nd</sup></code> và <code>3<sup>rd</sup></code> được phân vào nhóm thứ hai.</li>
	<li>Các node <code>4<sup>th</sup></code>, <code>5<sup>th</sup></code> và <code>6<sup>th</sup></code> được phân vào nhóm thứ ba, v.v.</li>
</ul>

<p>Lưu ý rằng độ dài của nhóm cuối cùng có thể nhỏ hơn hoặc bằng <code>1 + the length of the second to last group</code>.</p>

<p><strong>Đảo ngược</strong> các node trong mỗi nhóm có độ dài <strong>chẵn</strong>, rồi trả về <em>con trỏ</em> <code>head</code> <em>của linked list sau khi được chỉnh sửa</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2074.Reverse%20Nodes%20in%20Even%20Length%20Groups/images/eg1.png" style="width: 699px; height: 124px;" />
<pre>
<strong>Đầu vào:</strong> head = [5,2,6,3,9,1,7,3,8,4]
<strong>Đầu ra:</strong> [5,6,2,3,9,1,4,8,3,7]
<strong>Giải thích:</strong>
- Độ dài của nhóm đầu tiên là 1, tức là lẻ, nên không đảo ngược.
- Độ dài của nhóm thứ hai là 2, tức là chẵn, nên các node được đảo ngược.
- Độ dài của nhóm thứ ba là 3, tức là lẻ, nên không đảo ngược.
- Độ dài của nhóm cuối cùng là 4, tức là chẵn, nên các node được đảo ngược.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2074.Reverse%20Nodes%20in%20Even%20Length%20Groups/images/eg2.png" style="width: 284px; height: 114px;" />
<pre>
<strong>Đầu vào:</strong> head = [1,1,0,6]
<strong>Đầu ra:</strong> [1,0,1,6]
<strong>Giải thích:</strong>
- Độ dài của nhóm đầu tiên là 1. Không đảo ngược.
- Độ dài của nhóm thứ hai là 2. Các node được đảo ngược.
- Độ dài của nhóm cuối cùng là 1. Không đảo ngược.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2074.Reverse%20Nodes%20in%20Even%20Length%20Groups/images/ex3.png" style="width: 348px; height: 114px;" />
<pre>
<strong>Đầu vào:</strong> head = [1,1,0,6,5]
<strong>Đầu ra:</strong> [1,0,1,5,6]
<strong>Giải thích:</strong>
- Độ dài của nhóm đầu tiên là 1. Không đảo ngược.
- Độ dài của nhóm thứ hai là 2. Các node được đảo ngược.
- Độ dài của nhóm cuối cùng là 2. Các node được đảo ngược.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong list nằm trong khoảng <code>[1, 10<sup>5</sup>]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Nhóm $t$ có độ dài mục tiêu là $t$, còn nhóm cuối cùng có thể ngắn hơn. Với $n \le 10^5$, ta đảo các nhóm chẵn ngay trên linked list. Đếm số node, sau đó duyệt lần lượt từng nhóm.
>
> `reverse(head,l)` đảo ngược tối đa $l$ node và nối lại phần đuôi. Đảo ngược cả nhóm khi $l$ chẵn, và đảo ngược phần còn lại khi độ dài của nó chẵn. Dùng một node giả để xử lý phần đầu dễ hơn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseEvenLengthGroups(self, head: Optional[ListNode]) -> Optional[ListNode]:
        def reverse(head, l):
            prev, cur, tail = None, head, head
            i = 0
            while cur and i < l:
                t = cur.next
                cur.next = prev
                prev = cur
                cur = t
                i += 1
            tail.next = cur
            return prev

        n = 0
        t = head
        while t:
            t = t.next
            n += 1
        dummy = ListNode(0, head)
        prev = dummy
        l = 1
        while (1 + l) * l // 2 <= n and prev:
            if l % 2 == 0:
                prev.next = reverse(prev.next, l)
            i = 0
            while i < l and prev:
                prev = prev.next
                i += 1
            l += 1
        left = n - l * (l - 1) // 2
        if left > 0 and left % 2 == 0:
            prev.next = reverse(prev.next, left)
        return dummy.next
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
    public ListNode reverseEvenLengthGroups(ListNode head) {
        int n = 0;
        for (ListNode t = head; t != null; t = t.next) {
            ++n;
        }
        ListNode dummy = new ListNode(0, head);
        ListNode prev = dummy;
        int l = 1;
        for (; (1 + l) * l / 2 <= n && prev != null; ++l) {
            if (l % 2 == 0) {
                ListNode node = prev.next;
                prev.next = reverse(node, l);
            }
            for (int i = 0; i < l && prev != null; ++i) {
                prev = prev.next;
            }
        }
        int left = n - l * (l - 1) / 2;
        if (left > 0 && left % 2 == 0) {
            ListNode node = prev.next;
            prev.next = reverse(node, left);
        }
        return dummy.next;
    }

    private ListNode reverse(ListNode head, int l) {
        ListNode prev = null;
        ListNode cur = head;
        ListNode tail = cur;
        int i = 0;
        while (cur != null && i < l) {
            ListNode t = cur.next;
            cur.next = prev;
            prev = cur;
            cur = t;
            ++i;
        }
        tail.next = cur;
        return prev;
    }
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

function reverseEvenLengthGroups(head: ListNode | null): ListNode | null {
    let nums = [];
    let cur = head;
    while (cur) {
        nums.push(cur.val);
        cur = cur.next;
    }

    const n = nums.length;
    for (let i = 0, k = 1; i < n; i += k, k++) {
        // 最后一组， 可能出现不足
        k = Math.min(n - i, k);
        if (!(k & 1)) {
            let tmp = nums.splice(i, k);
            tmp.reverse();
            nums.splice(i, 0, ...tmp);
        }
    }

    cur = head;
    for (let num of nums) {
        cur.val = num;
        cur = cur.next;
    }
    return head;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
