---
comments: true
difficulty: Medium
tags:
    - Linked List
---

<!-- problem:start -->

# [237. Delete Node in a Linked List](https://leetcode.com/problems/delete-node-in-a-linked-list)

[中文文档](/solution/0200-0299/0237.Delete%20Node%20in%20a%20Linked%20List/README.md)

## Mô tả

<!-- description:start -->

<p>Có một singly-linked list <code>head</code> và chúng ta muốn xóa một node <code>node</code> trong đó.</p>

<p>Bạn được cung cấp node cần xóa <code>node</code>. Bạn <strong>sẽ không được truy cập</strong> vào node đầu tiên của <code>head</code>.</p>

<p>Tất cả các giá trị trong linked list đều <strong>khác nhau</strong>, và node được cung cấp <code>node</code> được đảm bảo không phải là node cuối cùng trong linked list.</p>

<p>Hãy xóa node đã cho. Lưu ý rằng xóa node không có nghĩa là loại bỏ nó khỏi bộ nhớ. Ý nghĩa của việc này là:</p>

<ul>
	<li>Giá trị của node đã cho không được tồn tại trong linked list.</li>
	<li>Số node trong linked list phải giảm đi một.</li>
	<li>Tất cả các giá trị trước <code>node</code> phải giữ nguyên thứ tự.</li>
	<li>Tất cả các giá trị sau <code>node</code> phải giữ nguyên thứ tự.</li>
</ul>

<p><strong>Kiểm thử tùy chỉnh:</strong></p>

<ul>
	<li>Đối với input, bạn cần cung cấp toàn bộ linked list <code>head</code> và node cần truyền vào <code>node</code>. <code>node</code> không được là node cuối cùng của list và phải là một node thực sự trong list.</li>
	<li>Chúng tôi sẽ tạo linked list và truyền node vào hàm của bạn.</li>
	<li>Output sẽ là toàn bộ list sau khi gọi hàm của bạn.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0200-0299/0237.Delete%20Node%20in%20a%20Linked%20List/images/node1.jpg" style="width: 400px; height: 286px;" />
<pre>
<strong>Đầu vào:</strong> head = [4,5,1,9], node = 5
<strong>Đầu ra:</strong> [4,1,9]
<strong>Giải thích: </strong>Bạn được cung cấp node thứ hai có giá trị 5, linked list phải trở thành 4 -&gt; 1 -&gt; 9 sau khi gọi hàm của bạn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0200-0299/0237.Delete%20Node%20in%20a%20Linked%20List/images/node2.jpg" style="width: 400px; height: 315px;" />
<pre>
<strong>Đầu vào:</strong> head = [4,5,1,9], node = 1
<strong>Đầu ra:</strong> [4,5,9]
<strong>Giải thích: </strong>Bạn được cung cấp node thứ ba có giá trị 1, linked list phải trở thành 4 -&gt; 5 -&gt; 9 sau khi gọi hàm của bạn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong list đã cho nằm trong khoảng <code>[2, 1000]</code>.</li>
	<li><code>-1000 &lt;= Node.val &lt;= 1000</code></li>
	<li>Giá trị của mỗi node trong list là <strong>duy nhất</strong>.</li>
	<li>Node <code>node</code> cần xóa <strong>nằm trong list</strong> và <strong>không phải</strong> là node tail.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Gán node

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta chỉ được cung cấp node cần xóa, nên không thể ghi đè $next$ của node đứng trước. Hãy sao chép giá trị của node kế tiếp vào node hiện tại rồi bỏ qua node kế tiếp; thao tác này tương đương với việc xóa nó.

<!-- thinking:end -->

Chúng ta có thể thay giá trị của node hiện tại bằng giá trị của node kế tiếp, sau đó xóa node kế tiếp. Như vậy, mục đích xóa node hiện tại sẽ đạt được.

Độ phức tạp thời gian là $O(1)$, độ phức tạp không gian là $O(1)$.

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
        """
        :type node: ListNode
        :rtype: void Do not return anything, modify node in-place instead.
        """
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

/**
  Do not return anything, modify it in-place instead.
  */
function deleteNode(node: ListNode | null): void {
    node.val = node.next.val;
    node.next = node.next.next;
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

#### C#

```cs
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public int val;
 *     public ListNode next;
 *     public ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void DeleteNode(ListNode node) {
        node.val = node.next.val;
        node.next = node.next.next;
    }
}
```

#### C

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
void deleteNode(struct ListNode* node) {
    node->val = node->next->val;
    node->next = node->next->next;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
