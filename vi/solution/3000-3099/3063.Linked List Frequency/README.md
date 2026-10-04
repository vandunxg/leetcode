---
comments: true
difficulty: Easy
tags:
    - Hash Table
    - Linked List
    - Counting
---

<!-- problem:start -->

# [3063. Linked List Frequency 🔒](https://leetcode.com/problems/linked-list-frequency)

[中文文档](/solution/3000-3099/3063.Linked%20List%20Frequency/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>head</code> của một linked list chứa <code>k</code> phần tử <strong>phân biệt</strong>, hãy trả về <em>head của một linked list có độ dài </em><code>k</code><em>, chứa <span data-keyword="frequency-linkedlist">tần suất</span> của mỗi phần tử <strong>phân biệt</strong> trong linked list đã cho theo <strong>bất kỳ thứ tự nào</strong>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1: </strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> head = [1,1,2,1,2,3] </span></p>

<p><strong>Đầu ra: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> [3,2,1] </span></p>

<p><strong>Giải thích: </strong> Có <code>3</code> phần tử phân biệt trong danh sách. Tần suất của <code>1</code> là <code>3</code>, tần suất của <code>2</code> là <code>2</code> và tần suất của <code>3</code> là <code>1</code>. Vì vậy, ta trả về <code>3 -&gt; 2 -&gt; 1</code>.</p>

<p>Lưu ý rằng <code>1 -&gt; 2 -&gt; 3</code>, <code>1 -&gt; 3 -&gt; 2</code>, <code>2 -&gt; 1 -&gt; 3</code>, <code>2 -&gt; 3 -&gt; 1</code> và <code>3 -&gt; 1 -&gt; 2</code> cũng là các đáp án hợp lệ.</p>
</div>

<p><strong class="example">Ví dụ 2: </strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> head = [1,1,2,2,2] </span></p>

<p><strong>Đầu ra: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> [2,3] </span></p>

<p><strong>Giải thích: </strong> Có <code>2</code> phần tử phân biệt trong danh sách. Tần suất của <code>1</code> là <code>2</code> và tần suất của <code>2</code> là <code>3</code>. Vì vậy, ta trả về <code>2 -&gt; 3</code>.</p>
</div>

<p><strong class="example">Ví dụ 3: </strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> head = [6,5,4,3,2,1] </span></p>

<p><strong>Đầu ra: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> [1,1,1,1,1,1] </span></p>

<p><strong>Giải thích: </strong> Có <code>6</code> phần tử phân biệt trong danh sách. Tần suất của mỗi phần tử là <code>1</code>. Vì vậy, ta trả về <code>1 -&gt; 1 -&gt; 1 -&gt; 1 -&gt; 1 -&gt; 1</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li>Số node trong danh sách nằm trong khoảng <code>[1, 10<sup>5</sup>]</code>.</li>
    <li><code>1 &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Tần suất của mỗi giá trị phân biệt sẽ trở thành một node trong linked list mới. Chỉ có thể biết các tần suất sau khi duyệt toàn bộ danh sách.
>
> Ta đếm bằng hash map, sau đó dựng danh sách từ các tần suất đó. Vì thứ tự không quan trọng, ta chèn node vào đầu danh sách.

<!-- thinking:end -->

Ta dùng hash table `cnt` để ghi lại số lần xuất hiện của mỗi giá trị phần tử trong linked list, sau đó duyệt qua các giá trị của hash table để tạo một linked list mới.

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
    def frequenciesOfElements(self, head: Optional[ListNode]) -> Optional[ListNode]:
        cnt = Counter()
        while head:
            cnt[head.val] += 1
            head = head.next
        dummy = ListNode()
        for val in cnt.values():
            dummy.next = ListNode(val, dummy.next)
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
    public ListNode frequenciesOfElements(ListNode head) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (; head != null; head = head.next) {
            cnt.merge(head.val, 1, Integer::sum);
        }
        ListNode dummy = new ListNode();
        for (int val : cnt.values()) {
            dummy.next = new ListNode(val, dummy.next);
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
    ListNode* frequenciesOfElements(ListNode* head) {
        unordered_map<int, int> cnt;
        for (; head; head = head->next) {
            cnt[head->val]++;
        }
        ListNode* dummy = new ListNode();
        for (auto& [_, val] : cnt) {
            dummy->next = new ListNode(val, dummy->next);
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
func frequenciesOfElements(head *ListNode) *ListNode {
    cnt := map[int]int{}
    for ; head != nil; head = head.Next {
        cnt[head.Val]++
    }
    dummy := &ListNode{}
    for _, val := range cnt {
        dummy.Next = &ListNode{val, dummy.Next}
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

function frequenciesOfElements(head: ListNode | null): ListNode | null {
    const cnt: Map<number, number> = new Map();
    for (; head; head = head.next) {
        cnt.set(head.val, (cnt.get(head.val) || 0) + 1);
    }
    const dummy = new ListNode();
    for (const val of cnt.values()) {
        dummy.next = new ListNode(val, dummy.next);
    }
    return dummy.next;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
