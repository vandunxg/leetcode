---
comments: true
difficulty: Medium
tags:
    - Hash Table
    - Linked List
---

<!-- problem:start -->

# [1836. Remove Duplicates From an Unsorted Linked List 🔒](https://leetcode.com/problems/remove-duplicates-from-an-unsorted-linked-list)

[中文文档](/solution/1800-1899/1836.Remove%20Duplicates%20From%20an%20Unsorted%20Linked%20List/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>head</code> của một linked list, hãy tìm tất cả các giá trị xuất hiện <strong>nhiều hơn một lần</strong> trong danh sách và xóa các node có một trong những giá trị đó.</p>

<p>Trả về <em>linked list sau khi xóa.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1836.Remove%20Duplicates%20From%20an%20Unsorted%20Linked%20List/images/tmp-linked-list.jpg" style="width: 422px; height: 222px;" />
<pre>
<strong>Đầu vào:</strong> head = [1,2,3,2]
<strong>Đầu ra:</strong> [1,3]
<strong>Giải thích:</strong> 2 xuất hiện hai lần trong linked list, nên cần xóa tất cả các phần tử 2. Sau khi xóa toàn bộ 2, ta còn lại [1,3].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1836.Remove%20Duplicates%20From%20an%20Unsorted%20Linked%20List/images/tmp-linked-list-1.jpg" style="width: 422px; height: 151px;" />
<pre>
<strong>Đầu vào:</strong> head = [2,1,1,2]
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> Cả 2 và 1 đều xuất hiện hai lần. Tất cả phần tử đều phải bị xóa.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1836.Remove%20Duplicates%20From%20an%20Unsorted%20Linked%20List/images/tmp-linked-list-2.jpg" style="width: 500px; height: 142px;" />
<pre>
<strong>Đầu vào:</strong> head = [3,2,2,1,3,2,4]
<strong>Đầu ra:</strong> [1,4]
<strong>Giải thích: </strong>3 xuất hiện hai lần và 2 xuất hiện ba lần. Sau khi xóa toàn bộ 3 và 2, ta còn lại [1,4].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong danh sách nằm trong khoảng&nbsp;<code>[1, 10<sup>5</sup>]</code></li>
	<li><code>1 &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Mọi giá trị xuất hiện nhiều hơn một lần đều phải bị xóa khỏi danh sách chưa sắp xếp. Một lượt duyệt không thể biết tần suất trên toàn danh sách, và linked list không hỗ trợ truy cập ngẫu nhiên.
>
> Đếm các giá trị trong một lượt duyệt, sau đó duyệt lại và bỏ qua các node có $cnt>1$. Dummy head giúp xử lý cả việc xóa ở đầu danh sách.

<!-- thinking:end -->

Ta có thể dùng hash table $cnt$ để đếm số lần xuất hiện của mỗi phần tử trong linked list, sau đó duyệt linked list để xóa các phần tử xuất hiện nhiều hơn một lần.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài linked list.

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def deleteDuplicatesUnsorted(self, head: ListNode) -> ListNode:
        cnt = Counter()
        cur = head
        while cur:
            cnt[cur.val] += 1
            cur = cur.next
        dummy = ListNode(0, head)
        pre, cur = dummy, head
        while cur:
            if cnt[cur.val] > 1:
                pre.next = cur.next
            else:
                pre = cur
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
    public ListNode deleteDuplicatesUnsorted(ListNode head) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (ListNode cur = head; cur != null; cur = cur.next) {
            cnt.put(cur.val, cnt.getOrDefault(cur.val, 0) + 1);
        }
        ListNode dummy = new ListNode(0, head);
        for (ListNode pre = dummy, cur = head; cur != null; cur = cur.next) {
            if (cnt.get(cur.val) > 1) {
                pre.next = cur.next;
            } else {
                pre = cur;
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
    ListNode* deleteDuplicatesUnsorted(ListNode* head) {
        unordered_map<int, int> cnt;
        for (ListNode* cur = head; cur; cur = cur->next) {
            cnt[cur->val]++;
        }
        ListNode* dummy = new ListNode(0, head);
        for (ListNode *pre = dummy, *cur = head; cur; cur = cur->next) {
            if (cnt[cur->val] > 1) {
                pre->next = cur->next;
            } else {
                pre = cur;
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
func deleteDuplicatesUnsorted(head *ListNode) *ListNode {
	cnt := map[int]int{}
	for cur := head; cur != nil; cur = cur.Next {
		cnt[cur.Val]++
	}
	dummy := &ListNode{0, head}
	for pre, cur := dummy, head; cur != nil; cur = cur.Next {
		if cnt[cur.Val] > 1 {
			pre.Next = cur.Next
		} else {
			pre = cur
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

function deleteDuplicatesUnsorted(head: ListNode | null): ListNode | null {
    const cnt: Map<number, number> = new Map();
    for (let cur = head; cur; cur = cur.next) {
        const x = cur.val;
        cnt.set(x, (cnt.get(x) ?? 0) + 1);
    }
    const dummy = new ListNode(0, head);
    for (let pre = dummy, cur = head; cur; cur = cur.next) {
        if (cnt.get(cur.val)! > 1) {
            pre.next = cur.next;
        } else {
            pre = cur;
        }
    }
    return dummy.next;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
