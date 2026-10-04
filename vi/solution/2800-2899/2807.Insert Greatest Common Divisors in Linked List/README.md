---
comments: true
difficulty: Medium
rating: 1279
source: Biweekly Contest 110 Q2
tags:
    - Linked List
    - Math
    - Number Theory
---

<!-- problem:start -->

# [2807. Insert Greatest Common Divisors in Linked List](https://leetcode.com/problems/insert-greatest-common-divisors-in-linked-list)

[中文文档](/solution/2800-2899/2807.Insert%20Greatest%20Common%20Divisors%20in%20Linked%20List/README.md)

## Mô tả

<!-- description:start -->

<p>Cho đầu của một danh sách liên kết <code>head</code>, trong đó mỗi node chứa một giá trị số nguyên.</p>

<p>Giữa mỗi cặp node liền kề, hãy chèn một node mới có giá trị bằng <strong>ước chung lớn nhất</strong> của hai node đó.</p>

<p>Trả về <em>danh sách liên kết sau khi chèn</em>.</p>

<p><strong>Ước chung lớn nhất</strong> của hai số là số nguyên dương lớn nhất chia hết cho cả hai số.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2800-2899/2807.Insert%20Greatest%20Common%20Divisors%20in%20Linked%20List/images/ex1_copy.png" style="width: 641px; height: 181px;" />
<pre>
<strong>Input:</strong> head = [18,6,10,3]
<strong>Output:</strong> [18,6,6,2,10,1,3]
<strong>Giải thích:</strong> Sơ đồ thứ <sup>1</sup> biểu diễn danh sách liên kết ban đầu và sơ đồ thứ <sup>2</sup> biểu diễn danh sách liên kết sau khi chèn các node mới (các node màu xanh là những node được chèn vào).
- Chèn ước chung lớn nhất của 18 và 6 = 6 giữa node thứ <sup>1</sup> và node thứ <sup>2</sup>.
- Chèn ước chung lớn nhất của 6 và 10 = 2 giữa node thứ <sup>2</sup> và node thứ <sup>3</sup>.
- Chèn ước chung lớn nhất của 10 và 3 = 1 giữa node thứ <sup>3</sup> và node thứ <sup>4</sup>.
Không còn node liền kề nào khác, nên ta trả về danh sách liên kết.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2800-2899/2807.Insert%20Greatest%20Common%20Divisors%20in%20Linked%20List/images/ex2_copy1.png" style="width: 51px; height: 191px;" />
<pre>
<strong>Input:</strong> head = [7]
<strong>Output:</strong> [7]
<strong>Giải thích:</strong> Sơ đồ thứ <sup>1</sup> biểu diễn danh sách liên kết ban đầu và sơ đồ thứ <sup>2</sup> biểu diễn danh sách liên kết sau khi chèn các node mới.
Không có cặp node liền kề nào, nên ta trả về danh sách liên kết ban đầu.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số lượng node trong danh sách nằm trong khoảng <code>[1, 5000]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Ta chỉ cần chèn $\gcd(pre,cur)$ giữa mỗi cặp node liền kề, việc này có thể thực hiện trong một lần duyệt. Dùng hai con trỏ, nối một node mới vào danh sách, sau đó đưa $pre$ đến $cur$ ban đầu cho đến khi kết thúc danh sách.

<!-- thinking:end -->

Ta dùng hai con trỏ $pre$ và $cur$ lần lượt trỏ đến node hiện tại và node tiếp theo. Ta chỉ cần chèn một node mới giữa $pre$ và $cur$. Vì vậy, mỗi lần ta tính ước chung lớn nhất $x$ của $pre$ và $cur$, rồi chèn một node mới có giá trị $x$ giữa $pre$ và $cur$. Sau đó cập nhật $pre = cur$ và $cur = cur.next$, tiếp tục duyệt danh sách liên kết cho đến khi $cur$ là null.

Độ phức tạp thời gian là $O(n \times \log M)$, trong đó $n$ là độ dài danh sách liên kết và $M$ là giá trị lớn nhất của các node trong danh sách liên kết. Không tính phần bộ nhớ dành cho danh sách liên kết kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def insertGreatestCommonDivisors(
        self, head: Optional[ListNode]
    ) -> Optional[ListNode]:
        pre, cur = head, head.next
        while cur:
            x = gcd(pre.val, cur.val)
            pre.next = ListNode(x, cur)
            pre, cur = cur, cur.next
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
    public ListNode insertGreatestCommonDivisors(ListNode head) {
        for (ListNode pre = head, cur = head.next; cur != null; cur = cur.next) {
            int x = gcd(pre.val, cur.val);
            pre.next = new ListNode(x, cur);
            pre = cur;
        }
        return head;
    }

    private int gcd(int a, int b) {
        if (b == 0) {
            return a;
        }
        return gcd(b, a % b);
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
    ListNode* insertGreatestCommonDivisors(ListNode* head) {
        ListNode* pre = head;
        for (ListNode* cur = head->next; cur; cur = cur->next) {
            int x = gcd(pre->val, cur->val);
            pre->next = new ListNode(x, cur);
            pre = cur;
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
func insertGreatestCommonDivisors(head *ListNode) *ListNode {
	for pre, cur := head, head.Next; cur != nil; cur = cur.Next {
		x := gcd(pre.Val, cur.Val)
		pre.Next = &ListNode{x, cur}
		pre = cur
	}
	return head
}

func gcd(a, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
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

function insertGreatestCommonDivisors(head: ListNode | null): ListNode | null {
    for (let pre = head, cur = head.next; cur; cur = cur.next) {
        const x = gcd(pre.val, cur.val);
        pre.next = new ListNode(x, cur);
        pre = cur;
    }
    return head;
}

function gcd(a: number, b: number): number {
    if (b === 0) {
        return a;
    }
    return gcd(b, a % b);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
