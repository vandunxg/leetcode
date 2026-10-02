---
comments: true
difficulty: Easy
rating: 1151
source: Weekly Contest 167 Q1
tags:
    - Linked List
    - Math
---

<!-- problem:start -->

# [1290. Convert Binary Number in a Linked List to Integer](https://leetcode.com/problems/convert-binary-number-in-a-linked-list-to-integer)

[中文文档](/solution/1200-1299/1290.Convert%20Binary%20Number%20in%20a%20Linked%20List%20to%20Integer/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>head</code> là node tham chiếu đến một singly linked list. Giá trị của mỗi node là <code>0</code> hoặc <code>1</code>. Linked list biểu diễn một số ở dạng nhị phân.</p>

<p>Trả về <em>giá trị thập phân</em> của số được biểu diễn trong linked list.</p>

<p><strong>Bit có trọng số cao nhất</strong> nằm ở head của linked list.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1290.Convert%20Binary%20Number%20in%20a%20Linked%20List%20to%20Integer/images/graph-1.png" style="width: 426px; height: 108px;" />
<pre>
<strong>Đầu vào:</strong> head = [1,0,1]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> (101) in base 2 = (5) in base 10
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> head = [0]
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Linked List không rỗng.</li>
	<li>Số lượng node không vượt quá <code>30</code>.</li>
	<li>Giá trị của mỗi node là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt Linked List

<!-- thinking:start -->

> **Tư duy**
>
> Danh sách biểu diễn số nhị phân từ bit cao xuống bit thấp. Dịch giá trị đang có sang trái rồi OR với bit hiện tại sẽ tích lũy giá trị số nguyên. Độ dài tối đa là $30$, nên giá trị vẫn nằm trong phạm vi cho phép. Không cần thu thập các bit trước.

<!-- thinking:end -->

Ta dùng biến $\textit{ans}$ để lưu giá trị thập phân hiện tại, ban đầu bằng $0$.

Duyệt linked list. Với mỗi node, dịch trái $\textit{ans}$ một bit rồi thực hiện phép OR theo bit với giá trị của node hiện tại. Sau khi duyệt xong, $\textit{ans}$ là giá trị thập phân cần tìm.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài linked list. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def getDecimalValue(self, head: ListNode) -> int:
        ans = 0
        while head:
            ans = ans << 1 | head.val
            head = head.next
        return ans
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
    public int getDecimalValue(ListNode head) {
        int ans = 0;
        for (; head != null; head = head.next) {
            ans = ans << 1 | head.val;
        }
        return ans;
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
    int getDecimalValue(ListNode* head) {
        int ans = 0;
        for (; head; head = head->next) {
            ans = ans << 1 | head->val;
        }
        return ans;
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
func getDecimalValue(head *ListNode) (ans int) {
	for ; head != nil; head = head.Next {
		ans = ans<<1 | head.Val
	}
	return
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

function getDecimalValue(head: ListNode | null): number {
    let ans = 0;
    for (; head; head = head.next) {
        ans = (ans << 1) | head.val;
    }
    return ans;
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
    pub fn get_decimal_value(mut head: Option<Box<ListNode>>) -> i32 {
        let mut ans = 0;
        while let Some(node) = head {
            ans = (ans << 1) | node.val;
            head = node.next;
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @return {number}
 */
var getDecimalValue = function (head) {
    let ans = 0;
    for (; head; head = head.next) {
        ans = (ans << 1) | head.val;
    }
    return ans;
};
```

#### C#

```cs
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public int val;
 *     public ListNode next;
 *     public ListNode(int val=0, ListNode next=null) {
 *         this.val = val;
 *         this.next = next;
 *     }
 * }
 */
public class Solution {
    public int GetDecimalValue(ListNode head) {
        int ans = 0;
        for (; head != null; head = head.next) {
            ans = ans << 1 | head.val;
        }
        return ans;
    }
}
```

#### PHP

```php
/**
 * Definition for a singly-linked list.
 * class ListNode {
 *     public $val = 0;
 *     public $next = null;
 *     function __construct($val = 0, $next = null) {
 *         $this->val = $val;
 *         $this->next = $next;
 *     }
 * }
 */
class Solution {
    /**
     * @param ListNode $head
     * @return Integer
     */
    function getDecimalValue($head) {
        $ans = 0;
        while ($head !== null) {
            $ans = ($ans << 1) | $head->val;
            $head = $head->next;
        }
        return $ans;
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

int getDecimalValue(struct ListNode* head) {
    int ans = 0;
    struct ListNode* cur = head;
    while (cur) {
        ans = (ans << 1) | cur->val;
        cur = cur->next;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
