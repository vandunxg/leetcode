---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [02.03. Delete Middle Node](https://leetcode.cn/problems/delete-middle-node-lcci)

[中文文档](/lcci/02.03.Delete%20Middle%20Node/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy cài đặt một thuật toán để xóa một node ở giữa (tức là bất kỳ node nào không phải node đầu tiên hoặc node cuối cùng, không nhất thiết là node chính giữa) của một danh sách liên kết đơn, khi chỉ được truy cập node đó.</p>

<p>&nbsp;</p>

<p><strong>Ví dụ: </strong></p>

<pre>

<strong>Đầu vào: </strong>the node c from the linked list a-&gt;b-&gt;c-&gt;d-&gt;e-&gt;f

<strong>Đầu ra: </strong>nothing is returned, but the new linked list looks like a-&gt;b-&gt;d-&gt;e-&gt;f

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Gán node

<!-- thinking:start -->

> **Tư duy**
>
> Thông thường, để xóa một node cần có node đứng trước nó. Ở đây chỉ được cung cấp node đích, và node đó không phải tail, nên không có cách nào để duyệt ngược.
>
> Hiệu ứng có thể quan sát được là value tại vị trí này và liên kết đến successor biến mất. Copy value của node kế tiếp vào node hiện tại rồi bỏ qua node kế tiếp sẽ tạo ra hiệu ứng giống như xóa node đối với caller.
>
> Chỉ cần hai phép gán `node.val = node.next.val` và `node.next = node.next.next` là đủ, với thời gian và không gian hằng số.

<!-- thinking:end -->

Chúng ta có thể thay value của node hiện tại bằng value của node kế tiếp, sau đó xóa node kế tiếp. Bằng cách này, ta có thể đạt được mục đích xóa node hiện tại.

Độ phức tạp thời gian là $O(1)$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None


class Solution:
    def deleteNode(self, node):
        node.val = node.next.val
        node.next = node.next.next
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
    public void deleteNode(ListNode node) {
        node.val = node.next.val;
        node.next = node.next.next;
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
    void deleteNode(ListNode* node) {
        node->val = node->next->val;
        node->next = node->next->next;
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
func deleteNode(node *ListNode) {
	node.Val = node.Next.Val
	node.Next = node.Next.Next
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
 * @param {ListNode} node
 * @return {void} Do not return anything, modify node in-place instead.
 */
var deleteNode = function (node) {
    node.val = node.next.val;
    node.next = node.next.next;
};
```

#### Swift

```swift
/**
*    public class ListNode {
*        var val: Int
*        var next: ListNode?
*        init(_ x: Int) {
*            self.val = x
*            self.next = nil
*        }
*    }
*/
class Solution {
    func deleteNode(_ node: ListNode?) {
        guard let node = node, let next = node.next else { return }
        node.val = next.val
        node.next = next.next
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
