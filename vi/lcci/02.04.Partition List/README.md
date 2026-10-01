---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [02.04. Partition List](https://leetcode.cn/problems/partition-list-lcci)

[中文文档](/lcci/02.04.Partition%20List/README.md)

## Mô tả

<!-- description:start -->

<p>Viết code để phân hoạch một linked list quanh giá trị x, sao cho mọi node nhỏ hơn x đứng trước mọi node lớn hơn hoặc bằng x. Nếu x xuất hiện trong list, các giá trị của x chỉ cần đứng sau các phần tử nhỏ hơn x (xem bên dưới). Phần tử phân hoạch x có thể xuất hiện ở bất kỳ vị trí nào trong &quot;phân hoạch phải&quot;; không nhất thiết phải nằm giữa phân hoạch trái và phân hoạch phải.</p>

<p><strong>Ví dụ:</strong></p>

<pre>

<strong>Đầu vào:</strong> head = 3-&gt;5-&gt;8-&gt;5-&gt;10-&gt;2-&gt;1, <em>x</em> = 5

<strong>Đầu ra:</strong> 3-&gt;1-&gt;2-&gt;10-&gt;5-&gt;5-&gt;8

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nối các list

<!-- thinking:start -->

> **Tư duy**
>
> Các node nhỏ hơn $x$ phải đứng trước và giữ nguyên thứ tự tương đối. Nếu thu thập chúng vào một mảng thì sẽ tốn thêm bộ nhớ; nếu đổi giá trị ngay tại chỗ thì có thể làm mất thứ tự.
>
> Chỉ có hai thứ tự tương đối cần giữ: phần “nhỏ hơn” và phần “còn lại”, nên có thể tách list rồi nối hai phần lại.
>
> Hai list có dummy head $left$ và $right$ lần lượt nhận node ở cuối; sau đó đặt $p1.next = right.next$, ngắt $p2.next$ và trả về $left.next$. Chỉ các pointer bị thay đổi.

<!-- thinking:end -->

Chúng ta tạo hai list, `left` và `right`, để lưu các node nhỏ hơn `x` và các node lớn hơn hoặc bằng `x` tương ứng.

Sau đó, chúng ta dùng hai pointer `p1` và `p2` lần lượt trỏ đến node cuối của `left` và `right`; ban đầu, cả `p1` và `p2` đều trỏ đến một dummy head.

Tiếp theo, chúng ta duyệt list `head`. Nếu giá trị của node hiện tại nhỏ hơn `x`, chúng ta thêm node hiện tại vào cuối list `left`, tức là `p1.next = head`, rồi đặt `p1 = p1.next`; ngược lại, chúng ta thêm node hiện tại vào cuối list `right`, tức là `p2.next = head`, rồi đặt `p2 = p2.next`.

Sau khi duyệt xong, chúng ta trỏ node cuối của list `left` đến node hợp lệ đầu tiên của list `right`, tức là `p1.next = right.next`, rồi trỏ node cuối của list `right` đến node null, tức là `p2.next = null`.

Cuối cùng, chúng ta trả về node hợp lệ đầu tiên của list `left`.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của list. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None


class Solution:
    def partition(self, head: ListNode, x: int) -> ListNode:
        left, right = ListNode(0), ListNode(0)
        p1, p2 = left, right
        while head:
            if head.val < x:
                p1.next = head
                p1 = p1.next
            else:
                p2.next = head
                p2 = p2.next
            head = head.next
        p1.next = right.next
        p2.next = None
        return left.next
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
    public ListNode partition(ListNode head, int x) {
        ListNode left = new ListNode(0);
        ListNode right = new ListNode(0);
        ListNode p1 = left;
        ListNode p2 = right;
        for (; head != null; head = head.next) {
            if (head.val < x) {
                p1.next = head;
                p1 = p1.next;
            } else {
                p2.next = head;
                p2 = p2.next;
            }
        }
        p1.next = right.next;
        p2.next = null;
        return left.next;
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
    ListNode* partition(ListNode* head, int x) {
        ListNode* left = new ListNode(0);
        ListNode* right = new ListNode(0);
        ListNode* p1 = left;
        ListNode* p2 = right;
        for (; head; head = head->next) {
            if (head->val < x) {
                p1->next = head;
                p1 = p1->next;
            } else {
                p2->next = head;
                p2 = p2->next;
            }
        }
        p1->next = right->next;
        p2->next = nullptr;
        return left->next;
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
func partition(head *ListNode, x int) *ListNode {
	left, right := &ListNode{}, &ListNode{}
	p1, p2 := left, right
	for ; head != nil; head = head.Next {
		if head.Val < x {
			p1.Next = head
			p1 = p1.Next
		} else {
			p2.Next = head
			p2 = p2.Next
		}
	}
	p1.Next = right.Next
	p2.Next = nil
	return left.Next
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

function partition(head: ListNode | null, x: number): ListNode | null {
    const [left, right] = [new ListNode(), new ListNode()];
    let [p1, p2] = [left, right];
    for (; head; head = head.next) {
        if (head.val < x) {
            p1.next = head;
            p1 = p1.next;
        } else {
            p2.next = head;
            p2 = p2.next;
        }
    }
    p1.next = right.next;
    p2.next = null;
    return left.next;
}
```

#### Swift

```swift
/** public class ListNode {
*    var val: Int
*    var next: ListNode?
*    init(_ x: Int) {
*        self.val = x
*        self.next = nil
*    }
* }
*/

class Solution {
    func partition(_ head: ListNode?, _ x: Int) -> ListNode? {
        let leftDummy = ListNode(0)
        let rightDummy = ListNode(0)
        var left = leftDummy
        var right = rightDummy
        var head = head

        while let current = head {
            if current.val < x {
                left.next = current
                left = left.next!
            } else {
                right.next = current
                right = right.next!
            }
            head = head?.next
        }

        right.next = nil
        left.next = rightDummy.next

        return leftDummy.next
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
