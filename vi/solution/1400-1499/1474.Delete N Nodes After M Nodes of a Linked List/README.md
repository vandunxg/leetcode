---
comments: true
difficulty: Easy
tags:
    - Linked List
---

<!-- problem:start -->

# [1474. Delete N Nodes After M Nodes of a Linked List 🔒](https://leetcode.com/problems/delete-n-nodes-after-m-nodes-of-a-linked-list)

[中文文档](/solution/1400-1499/1474.Delete%20N%20Nodes%20After%20M%20Nodes%20of%20a%20Linked%20List/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>head</code> của một linked list và hai số nguyên <code>m</code> và <code>n</code>.</p>

<p>Duyệt linked list và xóa một số node theo cách sau:</p>

<ul>
	<li>Bắt đầu với head là node hiện tại.</li>
	<li>Giữ lại <code>m</code> node đầu tiên, bắt đầu từ node hiện tại.</li>
	<li>Xóa <code>n</code> node tiếp theo.</li>
	<li>Tiếp tục lặp lại bước 2 và 3 cho đến khi đến cuối list.</li>
</ul>

<p>Trả về <em>head của list sau khi đã xóa các node được nêu trên</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1474.Delete%20N%20Nodes%20After%20M%20Nodes%20of%20a%20Linked%20List/images/sample_1_1848.png" style="width: 600px; height: 95px;" />
<pre>
<strong>Đầu vào:</strong> head = [1,2,3,4,5,6,7,8,9,10,11,12,13], m = 2, n = 3
<strong>Đầu ra:</strong> [1,2,6,7,11,12]
<strong>Giải thích:</strong> Giữ lại (m = 2) node đầu tiên bắt đầu từ head của linked List (1 -&gt;2), được hiển thị bằng các node màu đen.
Xóa (n = 3) node tiếp theo (3 -&gt; 4 -&gt; 5), được hiển thị bằng các node màu đỏ.
Tiếp tục quy trình tương tự cho đến khi đến tail của linked List.
Trả về head của linked list sau khi xóa các node.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1474.Delete%20N%20Nodes%20After%20M%20Nodes%20of%20a%20Linked%20List/images/sample_2_1848.png" style="width: 600px; height: 123px;" />
<pre>
<strong>Đầu vào:</strong> head = [1,2,3,4,5,6,7,8,9,10,11], m = 1, n = 3
<strong>Đầu ra:</strong> [1,5,9]
<strong>Giải thích:</strong> Trả về head của linked list sau khi xóa các node.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong list nằm trong khoảng <code>[1, 10<sup>4</sup>]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= m, n &lt;= 1000</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể giải bài toán này bằng cách chỉnh sửa list in-place không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Liên tục giữ lại $m$ node và xóa $n$ node. Di chuyển $\textit{pre}$ qua đoạn được giữ lại, dùng $\textit{cur}$ để bỏ qua $n$ node, sau đó nối lại $\textit{pre.next}$. Dừng sớm nếu list đã hết.

<!-- thinking:end -->

Ta có thể mô phỏng toàn bộ quá trình xóa. Trước tiên, dùng một con trỏ $\textit{pre}$ trỏ đến head của linked list, sau đó duyệt linked list và di chuyển $m - 1$ bước. Nếu $\textit{pre}$ là null, điều đó có nghĩa số node tính từ node hiện tại nhỏ hơn $m$, nên ta trả về head ngay. Nếu không, dùng một con trỏ $\textit{cur}$ trỏ đến $\textit{pre}$, sau đó di chuyển $n$ bước. Nếu $\textit{cur}$ là null, điều đó có nghĩa số node tính từ $\textit{pre}$ nhỏ hơn $m + n$, nên ta đặt trực tiếp $\textit{next}$ của $\textit{pre}$ thành null. Nếu không, đặt $\textit{next}$ của $\textit{pre}$ bằng $\textit{next}$ của $\textit{cur}$, sau đó di chuyển $\textit{pre}$ đến $\textit{next}$ của nó. Tiếp tục duyệt linked list cho đến khi $\textit{pre}$ là null, rồi trả về head.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số node trong linked list. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def deleteNodes(self, head: ListNode, m: int, n: int) -> ListNode:
        pre = head
        while pre:
            for _ in range(m - 1):
                if pre:
                    pre = pre.next
            if pre is None:
                return head
            cur = pre
            for _ in range(n):
                if cur:
                    cur = cur.next
            pre.next = None if cur is None else cur.next
            pre = pre.next
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
    public ListNode deleteNodes(ListNode head, int m, int n) {
        ListNode pre = head;
        while (pre != null) {
            for (int i = 0; i < m - 1 && pre != null; ++i) {
                pre = pre.next;
            }
            if (pre == null) {
                return head;
            }
            ListNode cur = pre;
            for (int i = 0; i < n && cur != null; ++i) {
                cur = cur.next;
            }
            pre.next = cur == null ? null : cur.next;
            pre = pre.next;
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
    ListNode* deleteNodes(ListNode* head, int m, int n) {
        auto pre = head;
        while (pre) {
            for (int i = 0; i < m - 1 && pre; ++i) {
                pre = pre->next;
            }
            if (!pre) {
                return head;
            }
            auto cur = pre;
            for (int i = 0; i < n && cur; ++i) {
                cur = cur->next;
            }
            pre->next = cur ? cur->next : nullptr;
            pre = pre->next;
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
func deleteNodes(head *ListNode, m int, n int) *ListNode {
	pre := head
	for pre != nil {
		for i := 0; i < m-1 && pre != nil; i++ {
			pre = pre.Next
		}
		if pre == nil {
			return head
		}
		cur := pre
		for i := 0; i < n && cur != nil; i++ {
			cur = cur.Next
		}
		pre.Next = nil
		if cur != nil {
			pre.Next = cur.Next
		}
		pre = pre.Next
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

function deleteNodes(head: ListNode | null, m: number, n: number): ListNode | null {
    let pre = head;
    while (pre) {
        for (let i = 0; i < m - 1 && pre; ++i) {
            pre = pre.next;
        }
        if (!pre) {
            break;
        }
        let cur = pre;
        for (let i = 0; i < n && cur; ++i) {
            cur = cur.next;
        }
        pre.next = cur?.next || null;
        pre = pre.next;
    }
    return head;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
