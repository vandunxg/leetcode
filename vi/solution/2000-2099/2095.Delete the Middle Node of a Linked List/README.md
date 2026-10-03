---
comments: true
difficulty: Medium
rating: 1324
source: Weekly Contest 270 Q2
tags:
    - Linked List
    - Two Pointers
---

<!-- problem:start -->

# [2095. Delete the Middle Node of a Linked List](https://leetcode.com/problems/delete-the-middle-node-of-a-linked-list)

[中文文档](/solution/2000-2099/2095.Delete%20the%20Middle%20Node%20of%20a%20Linked%20List/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho <code>head</code> của một linked list. Hãy <strong>xóa</strong> <strong>node ở giữa</strong>, rồi trả về <em>con trỏ</em> <code>head</code> <em>của linked list đã được chỉnh sửa</em>.</p>

<p><strong>Node ở giữa</strong> của linked list có kích thước <code>n</code> là node thứ <code>&lfloor;n / 2&rfloor;<sup>th</sup></code> tính từ <b>đầu danh sách</b> theo <strong>chỉ số bắt đầu từ 0</strong>, trong đó <code>&lfloor;x&rfloor;</code> là số nguyên lớn nhất nhỏ hơn hoặc bằng <code>x</code>.</p>

<ul>
	<li>Với <code>n</code> = <code>1</code>, <code>2</code>, <code>3</code>, <code>4</code> và <code>5</code>, node ở giữa lần lượt là <code>0</code>, <code>1</code>, <code>1</code>, <code>2</code> và <code>2</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2095.Delete%20the%20Middle%20Node%20of%20a%20Linked%20List/images/eg1drawio.png" style="width: 500px; height: 77px;" />
<pre>
<strong>Đầu vào:</strong> head = [1,3,4,7,1,2,6]
<strong>Đầu ra:</strong> [1,3,4,1,2,6]
<strong>Giải thích:</strong>
Hình trên biểu diễn linked list đã cho. Chỉ số của các node được ghi bên dưới.
Vì n = 7, node 3 có giá trị 7 là node ở giữa và được đánh dấu màu đỏ.
Ta trả về linked list mới sau khi xóa node này.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2095.Delete%20the%20Middle%20Node%20of%20a%20Linked%20List/images/eg2drawio.png" style="width: 250px; height: 43px;" />
<pre>
<strong>Đầu vào:</strong> head = [1,2,3,4]
<strong>Đầu ra:</strong> [1,2,4]
<strong>Giải thích:</strong>
Hình trên biểu diễn linked list đã cho.
Với n = 4, node 2 có giá trị 3 là node ở giữa và được đánh dấu màu đỏ.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2095.Delete%20the%20Middle%20Node%20of%20a%20Linked%20List/images/eg3drawio.png" style="width: 150px; height: 58px;" />
<pre>
<strong>Đầu vào:</strong> head = [2,1]
<strong>Đầu ra:</strong> [2]
<strong>Giải thích:</strong>
Hình trên biểu diễn linked list đã cho.
Với n = 2, node 1 có giá trị 1 là node ở giữa và được đánh dấu màu đỏ.
Node 0 có giá trị 2 là node duy nhất còn lại sau khi xóa node 1.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số lượng node trong danh sách nằm trong khoảng <code>[1, 10<sup>5</sup>]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Con trỏ nhanh và chậm

<!-- thinking:start -->

> **Tư duy**
>
> Việc xóa node ở giữa (nếu độ dài chẵn thì chọn node ở phía sau) cần biết node đứng trước nó. Ta đặt slow tại một node giả và fast tại head; cho chúng di chuyển lần lượt một và hai bước thì slow sẽ dừng ngay trước node ở giữa.
>
> Nối lại `slow.next`. Trường hợp độ dài bằng $1$ được xử lý vì node giả nằm trước node duy nhất.

<!-- thinking:end -->

Kỹ thuật sử dụng con trỏ nhanh và chậm là một phương pháp phổ biến để giải các bài toán liên quan đến linked list. Ta duy trì hai con trỏ, con trỏ chậm $\textit{slow}$ và con trỏ nhanh $\textit{fast}$. Ban đầu, $\textit{slow}$ trỏ đến một node giả, có con trỏ $\textit{next}$ trỏ đến node đầu $\textit{head}$ của danh sách, còn $\textit{fast}$ trỏ đến node đầu $\textit{head}$.

Sau đó, mỗi lần ta di chuyển con trỏ chậm tiến một vị trí và con trỏ nhanh tiến hai vị trí, cho đến khi con trỏ nhanh chạm đến cuối danh sách. Lúc này, node ngay sau node mà con trỏ chậm trỏ đến chính là node ở giữa danh sách. Ta có thể xóa node ở giữa bằng cách đặt con trỏ $\textit{next}$ của node mà con trỏ chậm trỏ đến, trỏ tới node kế tiếp node kế tiếp.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của danh sách. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def deleteMiddle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode(next=head)
        slow, fast = dummy, head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        slow.next = slow.next.next
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
    public ListNode deleteMiddle(ListNode head) {
        ListNode dummy = new ListNode(0, head);
        ListNode slow = dummy, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        slow.next = slow.next.next;
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
    ListNode* deleteMiddle(ListNode* head) {
        ListNode* dummy = new ListNode(0, head);
        ListNode* slow = dummy;
        ListNode* fast = head;
        while (fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
        }
        slow->next = slow->next->next;
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
func deleteMiddle(head *ListNode) *ListNode {
	dummy := &ListNode{Val: 0, Next: head}
	slow, fast := dummy, dummy.Next
	for fast != nil && fast.Next != nil {
		slow, fast = slow.Next, fast.Next.Next
	}
	slow.Next = slow.Next.Next
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

function deleteMiddle(head: ListNode | null): ListNode | null {
    const dummy = new ListNode(0, head);
    let [slow, fast] = [dummy, head];
    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
    }
    slow.next = slow.next.next;
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
    pub fn delete_middle(head: Option<Box<ListNode>>) -> Option<Box<ListNode>> {
        let mut slow = 0;
        let mut fast = head.as_ref();

        while let Some(node) = fast {
            if node.next.is_none() {
                break;
            }
            slow += 1;
            fast = node.next.as_ref().unwrap().next.as_ref();
        }

        let mut dummy = Some(Box::new(ListNode { val: 0, next: head }));
        let mut cur = dummy.as_mut();

        for _ in 0..slow {
            cur = cur.unwrap().next.as_mut();
        }

        let node = cur.unwrap();
        node.next = node.next.as_mut().unwrap().next.take();

        dummy.unwrap().next
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
