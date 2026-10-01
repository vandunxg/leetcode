---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [02.05. Sum Lists](https://leetcode.cn/problems/sum-lists-lcci)

[中文文档](/lcci/02.05.Sum%20Lists/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có hai số được biểu diễn bằng một linked list, trong đó mỗi node chứa một chữ số. Các chữ số được lưu theo thứ tự ngược, sao cho chữ số hàng đơn vị nằm ở đầu linked list. Hãy viết một hàm cộng hai số và trả về tổng dưới dạng một linked list.</p>

<p>&nbsp;</p>

<p><strong>Ví dụ: </strong></p>

<pre>

<strong>Đầu vào: </strong>(7 -&gt; 1 -&gt; 6) + (5 -&gt; 9 -&gt; 2). Tức là, 617 + 295.

<strong>Đầu ra: </strong>2 -&gt; 1 -&gt; 9. Tức là, 912.

</pre>

<p><strong>Câu hỏi mở rộng:&nbsp;</strong>Giả sử các chữ số được lưu theo thứ tự xuôi. Hãy lặp lại bài toán trên.</p>

<p><strong>Ví dụ: </strong></p>

<pre>

<strong>Đầu vào: </strong>(6 -&gt; 1 -&gt; 7) + (2 -&gt; 9 -&gt; 5). Tức là, 617 + 295.

<strong>Đầu ra: </strong>9 -&gt; 1 -&gt; 2. Tức là, 912.

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Các chữ số được lưu từ chữ số hàng đơn vị trước, vì vậy phép cộng tuân theo quy tắc tính tay thông thường. Việc chuyển thành số nguyên sẽ gây tràn số khi các linked list dài.
>
> Duyệt đồng thời hai linked list và duy trì một biến carry. Vòng lặp phải tiếp tục khi vẫn còn $l_1$, $l_2$ hoặc $carry$.
>
> Mỗi bước dùng `divmod` để lấy chữ số và carry tiếp theo, rồi nối thêm node sau một dummy head. Không tạo toàn bộ số nguyên trong bộ nhớ.

<!-- thinking:end -->

Chúng ta duyệt đồng thời hai linked list $l_1$ và $l_2$, đồng thời dùng một biến $carry$ để biểu thị hiện có carry hay không.

Trong mỗi lần duyệt, chúng ta lấy chữ số hiện tại của linked list tương ứng, tính tổng của chúng và carry-over $carry$, sau đó cập nhật giá trị của carry-over, rồi thêm giá trị của chữ số hiện tại vào linked list kết quả. Việc duyệt kết thúc khi cả hai linked list đã được duyệt hết và carry-over là $0$.

Cuối cùng, chúng ta trả về node head của linked list kết quả.

Độ phức tạp thời gian là $O(\max(m, n))$, trong đó $m$ và $n$ lần lượt là độ dài của hai linked list. Chúng ta cần duyệt qua mọi vị trí của hai linked list và chỉ mất $O(1)$ thời gian để xử lý mỗi vị trí. Không tính phần không gian dùng cho kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None


class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        dummy = ListNode()
        carry, curr = 0, dummy
        while l1 or l2 or carry:
            s = (l1.val if l1 else 0) + (l2.val if l2 else 0) + carry
            carry, val = divmod(s, 10)
            curr.next = ListNode(val)
            curr = curr.next
            l1 = l1.next if l1 else None
            l2 = l2.next if l2 else None
        return dummy.next
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
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        int carry = 0;
        ListNode cur = dummy;
        while (l1 != null || l2 != null || carry != 0) {
            int s = (l1 == null ? 0 : l1.val) + (l2 == null ? 0 : l2.val) + carry;
            carry = s / 10;
            cur.next = new ListNode(s % 10);
            cur = cur.next;
            l1 = l1 == null ? null : l1.next;
            l2 = l2 == null ? null : l2.next;
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
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* dummy = new ListNode(0);
        ListNode* cur = dummy;
        int carry = 0;
        while (l1 || l2 || carry) {
            carry += (!l1 ? 0 : l1->val) + (!l2 ? 0 : l2->val);
            cur->next = new ListNode(carry % 10);
            cur = cur->next;
            carry /= 10;
            l1 = l1 ? l1->next : l1;
            l2 = l2 ? l2->next : l2;
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
func addTwoNumbers(l1 *ListNode, l2 *ListNode) *ListNode {
	dummy := &ListNode{}
	cur := dummy
	carry := 0
	for l1 != nil || l2 != nil || carry > 0 {
		if l1 != nil {
			carry += l1.Val
			l1 = l1.Next
		}
		if l2 != nil {
			carry += l2.Val
			l2 = l2.Next
		}
		cur.Next = &ListNode{Val: carry % 10}
		cur = cur.Next
		carry /= 10
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

function addTwoNumbers(l1: ListNode | null, l2: ListNode | null): ListNode | null {
    if (l1 == null || l2 == null) {
        return l1 && l2;
    }
    const dummy = new ListNode(0);
    let cur = dummy;
    while (l1 != null || l2 != null) {
        let val = 0;
        if (l1 != null) {
            val += l1.val;
            l1 = l1.next;
        }
        if (l2 != null) {
            val += l2.val;
            l2 = l2.next;
        }
        if (cur.val >= 10) {
            cur.val %= 10;
            val++;
        }
        cur.next = new ListNode(val);
        cur = cur.next;
    }
    if (cur.val >= 10) {
        cur.val %= 10;
        cur.next = new ListNode(1);
    }
    return dummy.next;
}
```

#### Rust

```rust
impl Solution {
    pub fn add_two_numbers(
        mut l1: Option<Box<ListNode>>,
        mut l2: Option<Box<ListNode>>,
    ) -> Option<Box<ListNode>> {
        let mut dummy = Some(Box::new(ListNode::new(0)));
        let mut cur = dummy.as_mut();
        while l1.is_some() || l2.is_some() {
            let mut val = 0;
            if let Some(node) = l1 {
                val += node.val;
                l1 = node.next;
            }
            if let Some(node) = l2 {
                val += node.val;
                l2 = node.next;
            }
            if let Some(node) = cur {
                if node.val >= 10 {
                    val += 1;
                    node.val %= 10;
                }
                node.next = Some(Box::new(ListNode::new(val)));
                cur = node.next.as_mut();
            }
        }
        if let Some(node) = cur {
            if node.val >= 10 {
                node.val %= 10;
                node.next = Some(Box::new(ListNode::new(1)));
            }
        }
        dummy.unwrap().next
    }
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
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var addTwoNumbers = function (l1, l2) {
    let carry = 0;
    const dummy = new ListNode(0);
    let cur = dummy;
    while (l1 || l2 || carry) {
        carry += (l1?.val || 0) + (l2?.val || 0);
        cur.next = new ListNode(carry % 10);
        carry = Math.floor(carry / 10);
        cur = cur.next;
        l1 = l1?.next;
        l2 = l2?.next;
    }
    return dummy.next;
};
```

#### Swift

```swift
/**
* Definition for singly-linked list.
*    class ListNode {
*        var val: Int
*        var next: ListNode?
*        init(_ val: Int) {
*            self.val = val
*            self.next = nil
*        }
*    }
*/

class Solution {
    func addTwoNumbers(_ l1: ListNode?, _ l2: ListNode?) -> ListNode? {
        var carry = 0
        let dummy = ListNode(0)
        var current: ListNode? = dummy
        var l1 = l1, l2 = l2

        while l1 != nil || l2 != nil || carry != 0 {
            let sum = (l1?.val ?? 0) + (l2?.val ?? 0) + carry
            carry = sum / 10
            current?.next = ListNode(sum % 10)
            current = current?.next
            l1 = l1?.next
            l2 = l2?.next
        }

        return dummy.next
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
