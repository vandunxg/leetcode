---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [02.02. Kth Node From End of List](https://leetcode.cn/problems/kth-node-from-end-of-list-lcci)

[中文文档](/lcci/02.02.Kth%20Node%20From%20End%20of%20List/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy cài đặt một thuật toán để tìm node thứ k tính từ cuối của một linked list đơn. Trả về giá trị của node đó.</p>

<p><strong>Lưu ý: </strong>Bài toán này hơi khác so với phiên bản gốc trong sách.</p>

<p><strong>Ví dụ: </strong></p>

<pre>

<strong>Đầu vào: </strong> 1-&gt;2-&gt;3-&gt;4-&gt;5 和 <em>k</em> = 2

<strong>Đầu ra:  </strong>4</pre>

<p><strong>Lưu ý: </strong></p>

<p>k luôn hợp lệ.</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Node thứ $k$ tính từ cuối là node thứ $(n-k+1)$ tính từ đầu. Một lượt duyệt để đếm rồi đi qua lần thứ hai là đúng; nhưng chỉ cần một lượt duyệt.
>
> Nếu con trỏ nhanh bắt đầu trước $k$ bước, nó chạm cuối đúng lúc con trỏ chậm đang ở node thứ $k$ tính từ cuối.
>
> Cả hai đều bắt đầu từ head; `fast` tiến lên $k$ lần, sau đó cùng di chuyển cho đến khi `fast` là null, rồi trả về `slow.val`.

<!-- thinking:end -->

Chúng ta định nghĩa hai con trỏ `slow` và `fast`, ban đầu đều trỏ tới node đầu `head`. Sau đó, con trỏ `fast` di chuyển trước $k$ bước, rồi hai con trỏ `slow` và `fast` cùng di chuyển cho đến khi con trỏ `fast` trỏ tới cuối linked list. Lúc này, node mà con trỏ `slow` trỏ tới là node thứ $k$ tính từ cuối của linked list.

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
    def kthToLast(self, head: ListNode, k: int) -> int:
        slow = fast = head
        for _ in range(k):
            fast = fast.next
        while fast:
            slow = slow.next
            fast = fast.next
        return slow.val
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
    public int kthToLast(ListNode head, int k) {
        ListNode slow = head, fast = head;
        while (k-- > 0) {
            fast = fast.next;
        }
        while (fast != null) {
            slow = slow.next;
            fast = fast.next;
        }
        return slow.val;
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
    int kthToLast(ListNode* head, int k) {
        ListNode* fast = head;
        ListNode* slow = head;
        while (k--) {
            fast = fast->next;
        }
        while (fast) {
            slow = slow->next;
            fast = fast->next;
        }
        return slow->val;
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
func kthToLast(head *ListNode, k int) int {
	slow, fast := head, head
	for ; k > 0; k-- {
		fast = fast.Next
	}
	for fast != nil {
		slow = slow.Next
		fast = fast.Next
	}
	return slow.Val
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

function kthToLast(head: ListNode | null, k: number): number {
    let [slow, fast] = [head, head];
    while (k--) {
        fast = fast.next;
    }
    while (fast !== null) {
        slow = slow.next;
        fast = fast.next;
    }
    return slow.val;
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
    pub fn kth_to_last(head: Option<Box<ListNode>>, k: i32) -> i32 {
        let mut fast = &head;
        for _ in 0..k {
            fast = &fast.as_ref().unwrap().next;
        }
        let mut slow = &head;
        while let (Some(f), Some(s)) = (fast, slow) {
            fast = &f.next;
            slow = &s.next;
        }
        slow.as_ref().unwrap().val
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
 * @param {ListNode} head
 * @param {number} k
 * @return {number}
 */
var kthToLast = function (head, k) {
    let [slow, fast] = [head, head];
    while (k--) {
        fast = fast.next;
    }
    while (fast !== null) {
        slow = slow.next;
        fast = fast.next;
    }
    return slow.val;
};
```

#### Swift

```swift
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     var val: Int
 *     var next: ListNode?
 *     init(_ x: Int, _ next: ListNode? = nil) {
 *         self.val = x
 *         self.next = next
 *     }
 * }
 */

class Solution {
    func kthToLast(_ head: ListNode?, _ k: Int) -> Int {
        var slow = head
        var fast = head
        var k = k

        while k > 0 {
            fast = fast?.next
            k -= 1
        }

        while fast != nil {
            slow = slow?.next
            fast = fast?.next
        }

        return slow?.val ?? 0
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
