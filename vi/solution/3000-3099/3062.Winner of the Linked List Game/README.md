---
comments: true
difficulty: Easy
tags:
    - Linked List
---

<!-- problem:start -->

# [3062. Winner of the Linked List Game 🔒](https://leetcode.com/problems/winner-of-the-linked-list-game)

[中文文档](/solution/3000-3099/3062.Winner%20of%20the%20Linked%20List%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho <code>head</code> của một linked list có độ dài <strong>chẵn</strong>, chứa các số nguyên.</p>

<p>Mỗi node ở chỉ số <strong>lẻ</strong> chứa một số nguyên lẻ và mỗi node ở chỉ số <strong>chẵn</strong> chứa một số nguyên chẵn.</p>

<p>Ta gọi mỗi node ở chỉ số chẵn và node tiếp theo của nó là một <strong>cặp</strong>, ví dụ, các node ở chỉ số <code>0</code> và <code>1</code> là một cặp, các node ở chỉ số <code>2</code> và <code>3</code> là một cặp, và cứ tiếp tục như vậy.</p>

<p>Với mỗi <strong>cặp</strong>, ta so sánh giá trị của hai node trong cặp:</p>

<ul>
    <li>Nếu node ở chỉ số lẻ lớn hơn, đội <code>&quot;Odd&quot;</code> được một điểm.</li>
    <li>Nếu node ở chỉ số chẵn lớn hơn, đội <code>&quot;Even&quot;</code> được một điểm.</li>
</ul>

<p>Trả về <em>tên của đội có số điểm <strong>cao hơn</strong>; nếu số điểm bằng nhau, trả về</em> <code>&quot;Tie&quot;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1: </strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> head = [2,1] </span></p>

<p><strong>Đầu ra: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> &quot;Even&quot; </span></p>

<p><strong>Giải thích: </strong> Linked list này chỉ có một cặp là <code>(2,1)</code>. Vì <code>2 &gt; 1</code>, đội Even được một điểm.</p>

<p>Do đó, đáp án là <code>&quot;Even&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2: </strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> head = [2,5,4,7,20,5] </span></p>

<p><strong>Đầu ra: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> &quot;Odd&quot; </span></p>

<p><strong>Giải thích: </strong> Linked list này có <code>3</code> cặp. Hãy xét từng cặp:</p>

<p><code>(2,5)</code> -&gt; Vì <code>2 &lt; 5</code>, đội Odd được một điểm.</p>

<p><code>(4,7)</code> -&gt; Vì <code>4 &lt; 7</code>, đội Odd được một điểm.</p>

<p><code>(20,5)</code> -&gt; Vì <code>20 &gt; 5</code>, đội Even được một điểm.</p>

<p>Đội Odd được <code>2</code> điểm, trong khi đội Even được <code>1</code> điểm, nên đội Odd có số điểm cao hơn.</p>

<p>Do đó, đáp án là <code>&quot;Odd&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 3: </strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> head = [4,5,2,1] </span></p>

<p><strong>Đầu ra: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> &quot;Tie&quot; </span></p>

<p><strong>Giải thích: </strong> Linked list này có <code>2</code> cặp. Hãy xét từng cặp:</p>

<p><code>(4,5)</code> -&gt; Vì <code>4 &lt; 5</code>, đội Odd được một điểm.</p>

<p><code>(2,1)</code> -&gt; Vì <code>2 &gt; 1</code>, đội Even được một điểm.</p>

<p>Cả hai đội đều được <code>1</code> điểm.</p>

<p>Do đó, đáp án là <code>&quot;Tie&quot;</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li>Số node trong list nằm trong khoảng <code>[2, 100]</code>.</li>
    <li>Số node trong list là số chẵn.</li>
    <li><code>1 &lt;= Node.val &lt;= 100</code></li>
    <li>Giá trị của mỗi node ở chỉ số lẻ là số lẻ.</li>
    <li>Giá trị của mỗi node ở chỉ số chẵn là số chẵn.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Với list có độ dài chẵn, mỗi cặp node lẻ/chẵn liền kề sẽ giúp bên có giá trị lớn hơn được một điểm. Độ dài tối đa là $100$, nên ta duyệt theo từng cặp.
>
> Mỗi cặp cộng điểm cho bên lẻ hoặc bên chẵn, sau đó ta so sánh tổng điểm của hai bên.
>
> Chỉ cần duyệt một lần, mỗi bước nhảy qua hai node.

<!-- thinking:end -->

Duyệt linked list, mỗi lần lấy ra hai node, so sánh giá trị của chúng, sau đó cập nhật điểm của đội Odd và Even dựa trên kết quả so sánh. Cuối cùng, so sánh điểm của hai đội và trả về kết quả.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của linked list. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def gameResult(self, head: Optional[ListNode]) -> str:
        odd = even = 0
        while head:
            a = head.val
            b = head.next.val
            odd += a < b
            even += a > b
            head = head.next.next
        if odd > even:
            return "Odd"
        if odd < even:
            return "Even"
        return "Tie"
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
    public String gameResult(ListNode head) {
        int odd = 0, even = 0;
        for (; head != null; head = head.next.next) {
            int a = head.val;
            int b = head.next.val;
            odd += a < b ? 1 : 0;
            even += a > b ? 1 : 0;
        }
        if (odd > even) {
            return "Odd";
        }
        if (odd < even) {
            return "Even";
        }
        return "Tie";
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
    string gameResult(ListNode* head) {
        int odd = 0, even = 0;
        for (; head != nullptr; head = head->next->next) {
            int a = head->val;
            int b = head->next->val;
            odd += a < b;
            even += a > b;
        }
        if (odd > even) {
            return "Odd";
        }
        if (odd < even) {
            return "Even";
        }
        return "Tie";
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
func gameResult(head *ListNode) string {
    var odd, even int
    for ; head != nil; head = head.Next.Next {
        a, b := head.Val, head.Next.Val
        if a < b {
            odd++
        }
        if a > b {
            even++
        }
    }
    if odd > even {
        return "Odd"
    }
    if odd < even {
        return "Even"
    }
    return "Tie"
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

function gameResult(head: ListNode | null): string {
    let [odd, even] = [0, 0];
    for (; head; head = head.next.next) {
        const [a, b] = [head.val, head.next.val];
        odd += a < b ? 1 : 0;
        even += a > b ? 1 : 0;
    }
    if (odd > even) {
        return 'Odd';
    }
    if (odd < even) {
        return 'Even';
    }
    return 'Tie';
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
