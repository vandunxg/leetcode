---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [02.07. Intersection of Two Linked Lists](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci)

[中文文档](/lcci/02.07.Intersection%20of%20Two%20Linked%20Lists/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai linked list đơn, hãy xác định xem hai list có giao nhau không. Trả về node giao nhau. Lưu ý rằng giao nhau được xác định dựa trên reference, không phải value. Nghĩa là, nếu node thứ k của linked list thứ nhất là chính xác cùng một node (theo reference) với node thứ j của linked list thứ hai, thì hai list giao nhau.</p>

<p><strong>Ví dụ 1: </strong></p>

<pre>

<strong>Đầu vào: </strong>intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3

<strong>Đầu ra: </strong>Reference of the node with value = 8

<strong>Giải thích đầu vào:</strong> Giá trị của node giao nhau là 8 (lưu ý rằng giá trị này không được là 0 nếu hai list giao nhau). Bắt đầu từ head của A, ta đọc được [4,1,8,4,5]. Bắt đầu từ head của B, ta đọc được [5,0,1,8,4,5]. Có 2 node trước node giao nhau trong A; có 3 node trước node giao nhau trong B.</pre>

<p><strong>Ví dụ 2: </strong></p>

<pre>

<strong>Đầu vào: </strong>intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1

<strong>Đầu ra: </strong>Reference of the node with value = 2

<strong>Giải thích đầu vào:</strong>&nbsp;Giá trị của node giao nhau là 2 (lưu ý rằng giá trị này không được là 0 nếu hai list giao nhau). Bắt đầu từ head của A, ta đọc được [0,9,1,2,4]. Bắt đầu từ head của B, ta đọc được [3,2,4]. Có 3 node trước node giao nhau trong A; có 1 node trước node giao nhau trong B.</pre>

<p><strong>Ví dụ 3: </strong></p>

<pre>

<strong>Đầu vào: </strong>intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2

<strong>Đầu ra: </strong>null

<strong>Giải thích đầu vào:</strong> Bắt đầu từ head của A, ta đọc được [2,6,4]. Bắt đầu từ head của B, ta đọc được [1,5]. Vì hai list không giao nhau nên intersectVal phải bằng 0, còn skipA và skipB có thể nhận các giá trị bất kỳ.

<strong>Giải thích:</strong> Hai list không giao nhau, vì vậy trả về null.</pre>

<p><b>Lưu ý:</b></p>

- Nếu hai linked list hoàn toàn không giao nhau, trả về&nbsp;<code>null</code>.
- Các linked list phải giữ nguyên cấu trúc ban đầu sau khi hàm trả về.
- Có thể giả sử rằng không có cycle nào ở bất kỳ đâu trong toàn bộ cấu trúc liên kết.
- Code tốt nhất nên chạy trong thời gian O(n) và chỉ sử dụng O(1) bộ nhớ.

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Intersection là một suffix được chia sẻ. Lưu node của một list vào set là đúng, nhưng sử dụng không gian tuyến tính.
>
> Sau khi căn hai list theo tail, việc duyệt lockstep sẽ tìm được node chung đầu tiên. Nối “A rồi B” và “B rồi A” tạo ra hai đường đi có cùng độ dài.
>
> Các pointer $a$ và $b$ chuyển sang head còn lại khi gặp null; chúng sẽ gặp nhau tại intersection hoặc cùng trở thành null. Không cần tính độ dài trước.

<!-- thinking:end -->

Ta dùng hai pointer $a$ và $b$ lần lượt trỏ tới hai linked list $headA$ và $headB$.

Ta duyệt đồng thời hai linked list. Khi $a$ đi đến cuối linked list $headA$, nó được chuyển về node head của linked list $headB$. Khi $b$ đi đến cuối linked list $headB$, nó được chuyển về node head của linked list $headA$.

Nếu hai pointer gặp nhau, node mà chúng trỏ tới là node chung đầu tiên. Nếu chúng không gặp nhau, nghĩa là hai linked list không có node chung. Khi đó, cả hai pointer đều trỏ tới `null`, và ta có thể trả về một trong hai pointer.

Độ phức tạp thời gian là $O(m+n)$, trong đó $m$ và $n$ lần lượt là độ dài của linked list $headA$ và $headB$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None


class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        a, b = headA, headB
        while a != b:
            a = a.next if a else headB
            b = b.next if b else headA
        return a
```

#### Java

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode a = headA, b = headB;
        while (a != b) {
            a = a == null ? headB : a.next;
            b = b == null ? headA : b.next;
        }
        return a;
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
    ListNode* getIntersectionNode(ListNode* headA, ListNode* headB) {
        ListNode *a = headA, *b = headB;
        while (a != b) {
            a = a ? a->next : headB;
            b = b ? b->next : headA;
        }
        return a;
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
func getIntersectionNode(headA, headB *ListNode) *ListNode {
	a, b := headA, headB
	for a != b {
		if a == nil {
			a = headB
		} else {
			a = a.Next
		}
		if b == nil {
			b = headA
		} else {
			b = b.Next
		}
	}
	return a
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

function getIntersectionNode(headA: ListNode | null, headB: ListNode | null): ListNode | null {
    let a = headA;
    let b = headB;
    while (a != b) {
        a = a ? a.next : headB;
        b = b ? b.next : headA;
    }
    return a;
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
 * @param {ListNode} headA
 * @param {ListNode} headB
 * @return {ListNode}
 */
var getIntersectionNode = function (headA, headB) {
    let a = headA;
    let b = headB;
    while (a != b) {
        a = a ? a.next : headB;
        b = b ? b.next : headA;
    }
    return a;
};
```

#### Swift

```swift
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public var val: Int
 *     public var next: ListNode?
 *     public init(_ val: Int) {
 *         self.val = val
 *         self.next = nil
 *     }
 * }
 */

class Solution {
    func getIntersectionNode(_ headA: ListNode?, _ headB: ListNode?) -> ListNode? {
        var a = headA
        var b = headB
        while a !== b {
            a = a == nil ? headB : a?.next
            b = b == nil ? headA : b?.next
        }
        return a
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
