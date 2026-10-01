---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [02.01. Remove Duplicate Node](https://leetcode.cn/problems/remove-duplicate-node-lcci)

[中文文档](/lcci/02.01.Remove%20Duplicate%20Node/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy viết code để loại bỏ các phần tử trùng lặp khỏi một linked list không được sắp xếp.</p>

<p><strong>Ví dụ 1:</strong></p>

<pre>

<strong>Đầu vào</strong>: [1, 2, 3, 3, 2, 1]

<strong>Đầu ra</strong>: [1, 2, 3]

</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>

<strong>Đầu vào</strong>: [1, 1, 1, 1, 2]

<strong>Đầu ra</strong>: [1, 2]

</pre>

<p><strong>Lưu ý: </strong></p>

<ol>
	<li>Độ dài của danh sách nằm trong khoảng[0, 20000].</li>
    <li>Giá trị của các phần tử trong danh sách nằm trong khoảng [0, 20000].</li>
</ol>

<p><strong>Câu hỏi mở rộng: </strong></p>

<p>Bạn sẽ giải bài toán này như thế nào nếu không được phép sử dụng bộ đệm tạm thời?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Các giá trị trùng lặp phải được loại bỏ nhưng vẫn giữ lại lần xuất hiện đầu tiên. Duyệt phần còn lại của danh sách cho mỗi node sẽ tốn $O(n^2)$, phù hợp với danh sách ngắn nhưng mỗi lần kiểm tra vẫn có thời gian tuyến tính.
>
> “Giá trị này đã xuất hiện chưa?” là một thao tác tra cứu; hash table giúp thao tác này có thời gian hằng số kỳ vọng.
>
> $vis$ lưu các giá trị được giữ lại, còn node giả $pre$ giúp đơn giản hóa thao tác xóa: nếu giá trị của node được trỏ tới bởi $pre.next$ đã có trong tập hợp thì bỏ qua nó, nếu không thì ghi nhận giá trị đó và di chuyển node giả. Duyệt một lượt là đủ để loại bỏ các phần tử trùng lặp.

<!-- thinking:end -->

Chúng ta tạo một hash table $vis$ để ghi lại các giá trị của những node đã được duyệt.

Tiếp theo, chúng ta tạo một node giả $pre$ sao cho $pre.next = head$.

Tiếp đó, chúng ta duyệt linked list. Nếu giá trị của node hiện tại đã có trong hash table, chúng ta xóa node hiện tại, tức là $pre.next = pre.next.next$; nếu không, chúng ta thêm giá trị của node hiện tại vào hash table và di chuyển $pre$ đến node tiếp theo.

Sau khi duyệt xong, chúng ta trả về head của linked list.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của linked list.

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None


class Solution:
    def removeDuplicateNodes(self, head: ListNode) -> ListNode:
        vis = set()
        pre = ListNode(0, head)
        while pre.next:
            if pre.next.val in vis:
                pre.next = pre.next.next
            else:
                vis.add(pre.next.val)
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
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode removeDuplicateNodes(ListNode head) {
        Set<Integer> vis = new HashSet<>();
        ListNode pre = new ListNode(0, head);
        while (pre.next != null) {
            if (vis.add(pre.next.val)) {
                pre = pre.next;
            } else {
                pre.next = pre.next.next;
            }
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
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* removeDuplicateNodes(ListNode* head) {
        unordered_set<int> vis;
        ListNode* pre = new ListNode(0, head);
        while (pre->next) {
            if (vis.count(pre->next->val)) {
                pre->next = pre->next->next;
            } else {
                vis.insert(pre->next->val);
                pre = pre->next;
            }
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
func removeDuplicateNodes(head *ListNode) *ListNode {
	vis := map[int]bool{}
	pre := &ListNode{0, head}
	for pre.Next != nil {
		if vis[pre.Next.Val] {
			pre.Next = pre.Next.Next
		} else {
			vis[pre.Next.Val] = true
			pre = pre.Next
		}
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

function removeDuplicateNodes(head: ListNode | null): ListNode | null {
    const vis: Set<number> = new Set();
    let pre: ListNode = new ListNode(0, head);
    while (pre.next) {
        if (vis.has(pre.next.val)) {
            pre.next = pre.next.next;
        } else {
            vis.add(pre.next.val);
            pre = pre.next;
        }
    }
    return head;
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
use std::collections::HashSet;

impl Solution {
    pub fn remove_duplicate_nodes(mut head: Option<Box<ListNode>>) -> Option<Box<ListNode>> {
        let mut vis = HashSet::new();
        let mut pre = ListNode::new(0);
        pre.next = head;
        let mut cur = &mut pre;
        while let Some(node) = cur.next.take() {
            if vis.contains(&node.val) {
                cur.next = node.next;
            } else {
                vis.insert(node.val);
                cur.next = Some(node);
                cur = cur.next.as_mut().unwrap();
            }
        }
        pre.next
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
 * @return {ListNode}
 */
var removeDuplicateNodes = function (head) {
    const vis = new Set();
    let pre = new ListNode(0, head);
    while (pre.next) {
        if (vis.has(pre.next.val)) {
            pre.next = pre.next.next;
        } else {
            vis.add(pre.next.val);
            pre = pre.next;
        }
    }
    return head;
};
```

#### Swift

```swift
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *   var val: Int
 *   var next: ListNode?
 *   init(_ x: Int, _ next: ListNode? = nil) {
 *       self.val = x
 *       self.next = next
 *   }
 * }
 */

class Solution {
    func removeDuplicateNodes(_ head: ListNode?) -> ListNode? {
        var vis = Set<Int>()
        let pre = ListNode(0, head)
        var current: ListNode? = pre

        while current?.next != nil {
            if vis.insert(current!.next!.val).inserted {
                current = current?.next
            } else {
                current?.next = current?.next?.next
            }
        }

        return head
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
