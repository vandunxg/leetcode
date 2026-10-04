---
comments: true
difficulty: Medium
rating: 1341
source: Weekly Contest 406 Q2
tags:
    - Array
    - Hash Table
    - Linked List
---

<!-- problem:start -->

# [3217. Delete Nodes From Linked List Present in Array](https://leetcode.com/problems/delete-nodes-from-linked-list-present-in-array)

[中文文档](/solution/3200-3299/3217.Delete%20Nodes%20From%20Linked%20List%20Present%20in%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và <code>head</code> của một linked list. Hãy trả về <code>head</code> của linked list đã được sửa đổi sau khi <strong>xóa</strong> tất cả các node trong linked list có giá trị tồn tại trong <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3], head = [1,2,3,4,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[4,5]</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3217.Delete%20Nodes%20From%20Linked%20List%20Present%20in%20Array/images/linkedlistexample0.png" style="width: 400px; height: 66px;" /></strong></p>

<p>Xóa các node có giá trị 1, 2 và 3.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1], head = [1,2,1,2,1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,2,2]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3217.Delete%20Nodes%20From%20Linked%20List%20Present%20in%20Array/images/linkedlistexample1.png" style="height: 62px; width: 450px;" /></p>

<p>Xóa các node có giá trị 1.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5], head = [1,2,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,2,3,4]</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3217.Delete%20Nodes%20From%20Linked%20List%20Present%20in%20Array/images/linkedlistexample2.png" style="width: 400px; height: 83px;" /></strong></p>

<p>Không có node nào có giá trị 5.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li>Tất cả các phần tử trong <code>nums</code> đều khác nhau.</li>
	<li>Số lượng node trong list đã cho nằm trong khoảng <code>[1, 10<sup>5</sup>]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
	<li>Dữ liệu đầu vào được tạo sao cho có ít nhất một node trong linked list có giá trị không xuất hiện trong <code>nums</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần xóa các node trong list có giá trị xuất hiện trong mảng $\textit{nums}$. Với $n,m\le 10^5$, việc duyệt mảng ở mỗi node sẽ có độ phức tạp bậc hai.
>
> Đưa $\textit{nums}$ vào một hash set để kiểm tra với độ phức tạp kỳ vọng $O(1)$. Một node giả trỏ đến $\textit{head}$; nếu giá trị của node kế tiếp nằm trong set thì bỏ qua node đó, ngược lại di chuyển con trỏ về phía trước. Cách duyệt một lượt này cũng xử lý được trường hợp node đầu bị xóa.

<!-- thinking:end -->

Ta có thể dùng một hash table $\textit{s}$ để lưu tất cả phần tử trong mảng $\textit{nums}$. Sau đó, ta tạo một node giả $\textit{dummy}$ và cho nó trỏ đến node đầu của list $\textit{head}$.

Tiếp theo, ta duyệt list bắt đầu từ node giả $\textit{dummy}$. Nếu giá trị của node kế tiếp node hiện tại nằm trong hash table $\textit{s}$, ta cho node hiện tại trỏ đến node sau node kế tiếp; nếu không, ta di chuyển con trỏ của node hiện tại đến node kế tiếp.

Cuối cùng, ta trả về node kế tiếp của node giả $\textit{dummy}$.

Độ phức tạp thời gian là $O(n + m)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$ và $m$ là độ dài của list $\textit{head}$.

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def modifiedList(
        self, nums: List[int], head: Optional[ListNode]
    ) -> Optional[ListNode]:
        s = set(nums)
        pre = dummy = ListNode(next=head)
        while pre.next:
            if pre.next.val in s:
                pre.next = pre.next.next
            else:
                pre = pre.next
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
    public ListNode modifiedList(int[] nums, ListNode head) {
        Set<Integer> s = new HashSet<>();
        for (int x : nums) {
            s.add(x);
        }
        ListNode dummy = new ListNode(0, head);
        for (ListNode pre = dummy; pre.next != null;) {
            if (s.contains(pre.next.val)) {
                pre.next = pre.next.next;
            } else {
                pre = pre.next;
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
    ListNode* modifiedList(vector<int>& nums, ListNode* head) {
        unordered_set<int> s(nums.begin(), nums.end());
        ListNode* dummy = new ListNode(0, head);
        for (ListNode* pre = dummy; pre->next;) {
            if (s.count(pre->next->val)) {
                pre->next = pre->next->next;
            } else {
                pre = pre->next;
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
func modifiedList(nums []int, head *ListNode) *ListNode {
	s := map[int]bool{}
	for _, x := range nums {
		s[x] = true
	}
	dummy := &ListNode{Next: head}
	for pre := dummy; pre.Next != nil; {
		if s[pre.Next.Val] {
			pre.Next = pre.Next.Next
		} else {
			pre = pre.Next
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

function modifiedList(nums: number[], head: ListNode | null): ListNode | null {
    const s: Set<number> = new Set(nums);
    const dummy = new ListNode(0, head);
    for (let pre = dummy; pre.next;) {
        if (s.has(pre.next.val)) {
            pre.next = pre.next.next;
        } else {
            pre = pre.next;
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

use std::collections::HashSet;

impl Solution {
    pub fn modified_list(nums: Vec<i32>, head: Option<Box<ListNode>>) -> Option<Box<ListNode>> {
        let s: HashSet<i32> = nums.into_iter().collect();
        let mut dummy = Box::new(ListNode { val: 0, next: head });
        let mut pre = &mut dummy;

        while let Some(ref mut node) = pre.next {
            if s.contains(&node.val) {
                pre.next = node.next.take();
            } else {
                pre = pre.next.as_mut().unwrap();
            }
        }

        dummy.next
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
