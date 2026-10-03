---
comments: true
difficulty: Medium
rating: 1333
source: Weekly Contest 281 Q2
tags:
    - Linked List
    - Simulation
---

<!-- problem:start -->

# [2181. Merge Nodes in Between Zeros](https://leetcode.com/problems/merge-nodes-in-between-zeros)

[中文文档](/solution/2100-2199/2181.Merge%20Nodes%20in%20Between%20Zeros/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>head</code> của một linked list chứa một chuỗi số nguyên được <strong>ngăn cách</strong> bởi các số <code>0</code>. <strong>Phần đầu</strong> và <strong>phần cuối</strong> của linked list đều có <code>Node.val == 0</code>.</p>

<p>Với <strong>mỗi </strong>cặp hai số <code>0</code> liên tiếp, hãy <strong>gộp</strong> tất cả các node nằm giữa chúng thành một node duy nhất, có giá trị là <strong>tổng</strong> của tất cả các node được gộp. Linked list sau khi thay đổi không được chứa bất kỳ số <code>0</code> nào.</p>

<p>Trả về <em>node đầu</em> <code>head</code> <em>của linked list sau khi thay đổi</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2181.Merge%20Nodes%20in%20Between%20Zeros/images/ex1-1.png" style="width: 600px; height: 41px;" />
<pre>
<strong>Đầu vào:</strong> head = [0,3,1,0,4,5,2,0]
<strong>Đầu ra:</strong> [4,11]
<strong>Giải thích:</strong>
Hình trên biểu diễn linked list đã cho. Linked list sau khi thay đổi chứa:
- Tổng của các node được đánh dấu màu xanh lá: 3 + 1 = 4.
- Tổng của các node được đánh dấu màu đỏ: 4 + 5 + 2 = 11.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2181.Merge%20Nodes%20in%20Between%20Zeros/images/ex2-1.png" style="width: 600px; height: 41px;" />
<pre>
<strong>Đầu vào:</strong> head = [0,1,0,3,0,2,2,0]
<strong>Đầu ra:</strong> [1,3,4]
<strong>Giải thích:</strong>
Hình trên biểu diễn linked list đã cho. Linked list sau khi thay đổi chứa:
- Tổng của các node được đánh dấu màu xanh lá: 1 = 1.
- Tổng của các node được đánh dấu màu đỏ: 3 = 3.
- Tổng của các node được đánh dấu màu vàng: 2 + 2 = 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong linked list nằm trong khoảng <code>[3, 2 * 10<sup>5</sup>]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 1000</code></li>
	<li><strong>Không</strong> có hai node liên tiếp nào có <code>Node.val == 0</code>.</li>
	<li><strong>Phần đầu</strong> và <strong>phần cuối</strong> của linked list có <code>Node.val == 0</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Các giá trị nằm giữa hai số 0 liền kề phải được gộp thành một node. Chỉ cần duyệt linked list một lần từ đầu đến cuối.
>
> Một node đuôi giả sẽ cộng dồn $s$ giữa hai số 0 và thêm một node có giá trị $s$ khi gặp số 0.
>
> Trả về linked list bắt đầu sau node giả.

<!-- thinking:end -->

Ta định nghĩa một node đầu giả $\textit{dummy}$, một con trỏ $\textit{tail}$ trỏ tới node hiện tại, và một biến $\textit{s}$ để lưu tổng giá trị của các node hiện tại.

Tiếp theo, ta duyệt linked list bắt đầu từ node thứ hai. Nếu giá trị của node hiện tại khác 0, ta cộng nó vào $\textit{s}$. Ngược lại, ta gán $\textit{s}$ cho node sau $\textit{tail}$, đặt $\textit{s}$ về 0, rồi cập nhật $\textit{tail}$ thành node tiếp theo.

Cuối cùng, ta trả về node ngay sau $\textit{dummy}$.

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
    def mergeNodes(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy = tail = ListNode()
        s = 0
        cur = head.next
        while cur:
            if cur.val:
                s += cur.val
            else:
                tail.next = ListNode(s)
                tail = tail.next
                s = 0
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
    public ListNode mergeNodes(ListNode head) {
        ListNode dummy = new ListNode();
        int s = 0;
        ListNode tail = dummy;
        for (ListNode cur = head.next; cur != null; cur = cur.next) {
            if (cur.val != 0) {
                s += cur.val;
            } else {
                tail.next = new ListNode(s);
                tail = tail.next;
                s = 0;
            }
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
    ListNode* mergeNodes(ListNode* head) {
        ListNode* dummy = new ListNode();
        ListNode* tail = dummy;
        int s = 0;
        for (ListNode* cur = head->next; cur; cur = cur->next) {
            if (cur->val) {
                s += cur->val;
            } else {
                tail->next = new ListNode(s);
                tail = tail->next;
                s = 0;
            }
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
func mergeNodes(head *ListNode) *ListNode {
	dummy := &ListNode{}
	tail := dummy
	s := 0
	for cur := head.Next; cur != nil; cur = cur.Next {
		if cur.Val != 0 {
			s += cur.Val
		} else {
			tail.Next = &ListNode{Val: s}
			tail = tail.Next
			s = 0
		}
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

function mergeNodes(head: ListNode | null): ListNode | null {
    const dummy = new ListNode();
    let tail = dummy;
    let s = 0;
    for (let cur = head.next; cur; cur = cur.next) {
        if (cur.val) {
            s += cur.val;
        } else {
            tail.next = new ListNode(s);
            tail = tail.next;
            s = 0;
        }
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
    pub fn merge_nodes(head: Option<Box<ListNode>>) -> Option<Box<ListNode>> {
        let mut dummy = Box::new(ListNode::new(0));
        let mut tail = &mut dummy;
        let mut s = 0;
        let mut cur = head.unwrap().next;

        while let Some(mut node) = cur {
            if node.val != 0 {
                s += node.val;
            } else {
                tail.next = Some(Box::new(ListNode::new(s)));
                tail = tail.next.as_mut().unwrap();
                s = 0;
            }
            cur = node.next.take();
        }

        dummy.next
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

struct ListNode* mergeNodes(struct ListNode* head) {
    struct ListNode dummy;
    struct ListNode* cur = &dummy;
    int sum = 0;
    while (head) {
        if (head->val == 0 && sum != 0) {
            cur->next = malloc(sizeof(struct ListNode));
            cur->next->val = sum;
            cur->next->next = NULL;
            cur = cur->next;
            sum = 0;
        }
        sum += head->val;
        head = head->next;
    }
    return dummy.next;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
