---
comments: true
difficulty: Medium
rating: 1782
source: Weekly Contest 151 Q3
tags:
    - Hash Table
    - Linked List
---

<!-- problem:start -->

# [1171. Remove Zero Sum Consecutive Nodes from Linked List](https://leetcode.com/problems/remove-zero-sum-consecutive-nodes-from-linked-list)

[中文文档](/solution/1100-1199/1171.Remove%20Zero%20Sum%20Consecutive%20Nodes%20from%20Linked%20List/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>head</code> của một linked list, liên tục xóa các dãy node liên tiếp có tổng bằng <code>0</code> cho đến khi không còn dãy nào như vậy.</p>

<p>Sau đó, trả về head của linked list cuối cùng. Bạn có thể trả về bất kỳ đáp án hợp lệ nào.</p>

<p>&nbsp;</p>
<p>(Lưu ý rằng trong các ví dụ dưới đây, mọi dãy đều là dạng tuần tự hóa của các đối tượng <code>ListNode</code>.)</p>

<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> head = [1,2,-3,3,1]
<strong>Đầu ra:</strong> [3,1]
<strong>Lưu ý:</strong> Đáp án [1,2,1] cũng được chấp nhận.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> head = [1,2,3,-3,4]
<strong>Đầu ra:</strong> [1,2,4]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> head = [1,2,3,-3,-2]
<strong>Đầu ra:</strong> [1]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Linked list đã cho có từ <code>1</code> đến <code>1000</code> node.</li>
	<li>Mỗi node trong linked list có <code>-1000 &lt;= node.val &lt;= 1000</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Một đoạn có tổng bằng 0 tương ứng với hai prefix sum bằng nhau. Xóa từng đoạn rồi quét lại có thể khiến ta phải duyệt linked list nhiều lần. Lượt đầu lưu node cuối cùng ứng với mỗi prefix sum; lượt thứ hai gán $cur.next$ bằng node đứng sau node cuối đó để bỏ qua cả đoạn giữa chỉ trong một lần. Node giả xử lý trường hợp đoạn có tổng bằng 0 bắt đầu ngay từ head.

<!-- thinking:end -->

Nếu hai prefix sum của linked list bằng nhau, tổng dãy node liên tiếp nằm giữa chúng bằng $0$, vì vậy ta có thể xóa dãy node đó.

Trước tiên, ta duyệt linked list và dùng hash table $last$ để lưu prefix sum cùng node tương ứng. Với cùng prefix sum $s$, node xuất hiện sau sẽ ghi đè node trước đó.

Tiếp theo, ta duyệt linked list lần nữa. Nếu node hiện tại $cur$ có prefix sum $s$ đã có trong $last$, điều đó có nghĩa tổng các node nằm sau $cur$ đến $last[s]$ bằng $0$. Ta cập nhật trực tiếp pointer của $cur$ thành $last[s].next$, qua đó xóa đoạn node liên tiếp có tổng bằng $0$. Tiếp tục duyệt và xóa mọi đoạn như vậy.

Cuối cùng, ta trả về head của linked list là $dummy.next$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của linked list.

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeZeroSumSublists(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode(next=head)
        last = {}
        s, cur = 0, dummy
        while cur:
            s += cur.val
            last[s] = cur
            cur = cur.next
        s, cur = 0, dummy
        while cur:
            s += cur.val
            cur.next = last[s].next
            cur = cur.next
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
    public ListNode removeZeroSumSublists(ListNode head) {
        ListNode dummy = new ListNode(0, head);
        Map<Integer, ListNode> last = new HashMap<>();
        int s = 0;
        ListNode cur = dummy;
        while (cur != null) {
            s += cur.val;
            last.put(s, cur);
            cur = cur.next;
        }
        s = 0;
        cur = dummy;
        while (cur != null) {
            s += cur.val;
            cur.next = last.get(s).next;
            cur = cur.next;
        }
        return dummy.next;
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
    ListNode* removeZeroSumSublists(ListNode* head) {
        ListNode* dummy = new ListNode(0, head);
        unordered_map<int, ListNode*> last;
        ListNode* cur = dummy;
        int s = 0;
        while (cur) {
            s += cur->val;
            last[s] = cur;
            cur = cur->next;
        }
        s = 0;
        cur = dummy;
        while (cur) {
            s += cur->val;
            cur->next = last[s]->next;
            cur = cur->next;
        }
        return dummy->next;
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
func removeZeroSumSublists(head *ListNode) *ListNode {
	dummy := &ListNode{0, head}
	last := map[int]*ListNode{}
	cur := dummy
	s := 0
	for cur != nil {
		s += cur.Val
		last[s] = cur
		cur = cur.Next
	}
	s = 0
	cur = dummy
	for cur != nil {
		s += cur.Val
		cur.Next = last[s].Next
		cur = cur.Next
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

function removeZeroSumSublists(head: ListNode | null): ListNode | null {
    const dummy = new ListNode(0, head);
    const last = new Map<number, ListNode>();
    let s = 0;
    for (let cur = dummy; cur; cur = cur.next) {
        s += cur.val;
        last.set(s, cur);
    }
    s = 0;
    for (let cur = dummy; cur; cur = cur.next) {
        s += cur.val;
        cur.next = last.get(s)!.next;
    }
    return dummy.next;
}
```

#### Rust

```rust
// Definition for singly-linked list.
// #[derive(PartialEq, Eq, Clone, Debug)]
// pub struct ListNode {
//   pub val: i32,
//   pub next: Option<Box<ListNode>>
// }
//
// impl ListNode {
//   #[inline]
//   fn new(val: i32) -> Self {
//     ListNode {
//       next: None,
//       val
//     }
//   }
// }
impl Solution {
    pub fn remove_zero_sum_sublists(head: Option<Box<ListNode>>) -> Option<Box<ListNode>> {
        let dummy = Some(Box::new(ListNode { val: 0, next: head }));
        let mut last = std::collections::HashMap::new();
        let mut s = 0;
        let mut p = dummy.as_ref();
        while let Some(node) = p {
            s += node.val;
            last.insert(s, node);
            p = node.next.as_ref();
        }

        let mut dummy = Some(Box::new(ListNode::new(0)));
        let mut q = dummy.as_mut();
        s = 0;
        while let Some(cur) = q {
            s += cur.val;
            if let Some(node) = last.get(&s) {
                cur.next = node.next.clone();
            }
            q = cur.next.as_mut();
        }
        dummy.unwrap().next
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
