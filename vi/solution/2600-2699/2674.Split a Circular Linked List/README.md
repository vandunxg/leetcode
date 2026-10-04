---
comments: true
difficulty: Medium
tags:
    - Linked List
    - Two Pointers
---

<!-- problem:start -->

# [2674. Split a Circular Linked List 🔒](https://leetcode.com/problems/split-a-circular-linked-list)

[中文文档](/solution/2600-2699/2674.Split%20a%20Circular%20Linked%20List/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một <strong>danh sách liên kết vòng</strong> <code>list</code> gồm các số nguyên dương, hãy chia nó thành 2 <strong>danh sách liên kết vòng</strong> sao cho danh sách thứ nhất chứa <strong>nửa đầu</strong> các node trong <code>list</code> (chính xác là <code>ceil(list.length / 2)</code> node) theo đúng thứ tự xuất hiện trong <code>list</code>, còn danh sách thứ hai chứa <strong>phần còn lại</strong> của các node trong <code>list</code> theo đúng thứ tự xuất hiện trong <code>list</code>.</p>

<p>Trả về <em>một mảng answer có độ dài 2, trong đó phần tử thứ nhất là một <strong>danh sách liên kết vòng</strong> biểu diễn <strong>nửa đầu</strong>, còn phần tử thứ hai là một <strong>danh sách liên kết vòng</strong> biểu diễn <strong>nửa còn lại</strong>.</em></p>

<div><strong>Danh sách liên kết vòng</strong> là một danh sách liên kết thông thường, với điểm khác biệt duy nhất là node next của node cuối cùng là node đầu tiên.</div>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,5,7]
<strong>Đầu ra:</strong> [[1,5],[7]]
<strong>Giải thích:</strong> Danh sách ban đầu có 3 node, nên nửa đầu sẽ gồm 2 phần tử đầu tiên vì ceil(3 / 2) = 2, còn 1 node còn lại nằm trong nửa thứ hai.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,6,1,5]
<strong>Đầu ra:</strong> [[2,6],[1,5]]
<strong>Giải thích:</strong> Danh sách ban đầu có 4 node, nên nửa đầu sẽ gồm 2 phần tử đầu tiên vì ceil(4 / 2) = 2, còn 2 node còn lại nằm trong nửa thứ hai.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong <code>list</code> nằm trong khoảng <code>[2, 10<sup>5</sup>]</code></li>
	<li><code>0 &lt;= Node.val &lt;= 10<sup>9</sup></code></li>
	<li><font face="monospace"><code>LastNode.next = FirstNode</code></font>, trong đó <code>LastNode</code> là node cuối cùng của danh sách và <code>FirstNode</code> là node đầu tiên</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Con trỏ nhanh và chậm

<!-- thinking:start -->

> **Tư duy**
>
> Danh sách vòng phải được chia sao cho nửa đầu có độ dài ít nhất bằng nửa sau. Nếu đếm độ dài trước thì cần duyệt hai lần. Hai con trỏ nhanh và chậm giúp đưa con trỏ chậm đến cuối nửa đầu chỉ trong một lần duyệt.
>
> Khi con trỏ nhanh sắp quay lại node đầu, con trỏ chậm đang ở giữa danh sách; khi đó ta đóng nửa thứ hai, rồi nối lại nửa thứ nhất với node đầu ban đầu.

<!-- thinking:end -->

Ta sử dụng hai con trỏ $a$ và $b$, ban đầu cả hai đều trỏ đến node đầu của danh sách liên kết. Trong mỗi vòng lặp, con trỏ $a$ tiến một bước, còn con trỏ $b$ tiến hai bước, cho đến khi con trỏ $b$ đi đến cuối danh sách liên kết. Lúc này, con trỏ $a$ đang trỏ đến node ở giữa danh sách, và ta ngắt danh sách tại con trỏ $a$, từ đó thu được node đầu của hai danh sách liên kết.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của danh sách liên kết. Thuật toán chỉ cần duyệt qua danh sách liên kết một lần. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def splitCircularLinkedList(
        self, list: Optional[ListNode]
    ) -> List[Optional[ListNode]]:
        a = b = list
        while b.next != list and b.next.next != list:
            a = a.next
            b = b.next.next
        if b.next != list:
            b = b.next
        list2 = a.next
        b.next = list2
        a.next = list
        return [list, list2]
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
    public ListNode[] splitCircularLinkedList(ListNode list) {
        ListNode a = list, b = list;
        while (b.next != list && b.next.next != list) {
            a = a.next;
            b = b.next.next;
        }
        if (b.next != list) {
            b = b.next;
        }
        ListNode list2 = a.next;
        b.next = list2;
        a.next = list;
        return new ListNode[] {list, list2};
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
    vector<ListNode*> splitCircularLinkedList(ListNode* list) {
        ListNode* a = list;
        ListNode* b = list;
        while (b->next != list && b->next->next != list) {
            a = a->next;
            b = b->next->next;
        }
        if (b->next != list) {
            b = b->next;
        }
        ListNode* list2 = a->next;
        b->next = list2;
        a->next = list;
        return {list, list2};
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
func splitCircularLinkedList(list *ListNode) []*ListNode {
	a, b := list, list
	for b.Next != list && b.Next.Next != list {
		a = a.Next
		b = b.Next.Next
	}
	if b.Next != list {
		b = b.Next
	}
	list2 := a.Next
	b.Next = list2
	a.Next = list
	return []*ListNode{list, list2}
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

function splitCircularLinkedList(list: ListNode | null): Array<ListNode | null> {
    let a = list;
    let b = list;
    while (b.next !== list && b.next.next !== list) {
        a = a.next;
        b = b.next.next;
    }
    if (b.next !== list) {
        b = b.next;
    }
    const list2 = a.next;
    b.next = list2;
    a.next = list;
    return [list, list2];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
