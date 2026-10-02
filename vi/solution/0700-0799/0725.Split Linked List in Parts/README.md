---
comments: true
difficulty: Medium
tags:
    - Linked List
---

<!-- problem:start -->

# [725. Split Linked List in Parts](https://leetcode.com/problems/split-linked-list-in-parts)

[中文文档](/solution/0700-0799/0725.Split%20Linked%20List%20in%20Parts/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>head</code> của một singly linked list và số nguyên <code>k</code>, hãy chia linked list thành <code>k</code> phần liên tiếp.</p>

<p>Độ dài các phần cần cân bằng nhất có thể: chênh lệch kích thước giữa bất kỳ hai phần nào không quá một. Vì vậy, một số phần có thể là null.</p>

<p>Các phần phải giữ nguyên thứ tự xuất hiện trong linked list đầu vào; phần đứng trước luôn có kích thước lớn hơn hoặc bằng phần đứng sau.</p>

<p>Trả về <em>mảng gồm </em><code>k</code><em> phần</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0725.Split%20Linked%20List%20in%20Parts/images/split1-lc.jpg" style="width: 400px; height: 134px;" />
<pre>
<strong>Đầu vào:</strong> head = [1,2,3], k = 5
<strong>Đầu ra:</strong> [[1],[2],[3],[],[]]
<strong>Giải thích:</strong>
Phần tử đầu tiên output[0] có output[0].val = 1 và output[0].next = null.
Phần tử cuối cùng output[4] là null, nhưng biểu diễn dưới dạng ListNode là [].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0725.Split%20Linked%20List%20in%20Parts/images/split2-lc.jpg" style="width: 600px; height: 60px;" />
<pre>
<strong>Đầu vào:</strong> head = [1,2,3,4,5,6,7,8,9,10], k = 3
<strong>Đầu ra:</strong> [[1,2,3,4],[5,6,7],[8,9,10]]
<strong>Giải thích:</strong>
Danh sách đầu vào được chia thành các phần liên tiếp có kích thước chênh lệch tối đa 1; các phần đứng trước có kích thước lớn hơn các phần đứng sau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong list nằm trong khoảng <code>[0, 1000]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 1000</code></li>
	<li><code>1 &lt;= k &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Chia list thành $k$ phần cân bằng nhất có thể, phân các node dư cho những phần đứng trước. Vì $n\le 1000$, ta có thể đếm độ dài rồi cắt list.
>
> Mỗi phần có $\lfloor n/k\rfloor$ node; $n\bmod k$ phần đầu tiên nhận thêm một node. Các slot không được dùng sẽ giữ giá trị null.
>
> Ở lượt duyệt thứ hai, đi theo độ dài đã tính, ngắt liên kết $\textit{next}$ và lưu lại head tiếp theo. Tổng cộng cần hai lượt duyệt.

<!-- thinking:end -->

Trước tiên, duyệt linked list để tính độ dài $n$, sau đó tính độ dài cơ bản $\textit{cnt} = \lfloor \frac{n}{k} \rfloor$ và số dư $\textit{mod} = n \bmod k$. Mỗi phần trong $\textit{mod}$ phần đầu có độ dài $\textit{cnt} + 1$; các phần còn lại có độ dài $\textit{cnt}$.

Tiếp theo, chỉ cần duyệt linked list và chia thành $k$ phần.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(k)$, trong đó $n$ là độ dài linked list.

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def splitListToParts(
        self, head: Optional[ListNode], k: int
    ) -> List[Optional[ListNode]]:
        n = 0
        cur = head
        while cur:
            n += 1
            cur = cur.next
        cnt, mod = divmod(n, k)
        ans = [None] * k
        cur = head
        for i in range(k):
            if cur is None:
                break
            ans[i] = cur
            m = cnt + int(i < mod)
            for _ in range(1, m):
                cur = cur.next
            nxt = cur.next
            cur.next = None
            cur = nxt
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
    public ListNode[] splitListToParts(ListNode head, int k) {
        int n = 0;
        for (ListNode cur = head; cur != null; cur = cur.next) {
            ++n;
        }
        int cnt = n / k, mod = n % k;
        ListNode[] ans = new ListNode[k];
        ListNode cur = head;
        for (int i = 0; i < k && cur != null; ++i) {
            ans[i] = cur;
            int m = cnt + (i < mod ? 1 : 0);
            for (int j = 1; j < m; ++j) {
                cur = cur.next;
            }
            ListNode nxt = cur.next;
            cur.next = null;
            cur = nxt;
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
    vector<ListNode*> splitListToParts(ListNode* head, int k) {
        int n = 0;
        for (ListNode* cur = head; cur != nullptr; cur = cur->next) {
            ++n;
        }
        int cnt = n / k, mod = n % k;
        vector<ListNode*> ans(k, nullptr);
        ListNode* cur = head;
        for (int i = 0; i < k && cur != nullptr; ++i) {
            ans[i] = cur;
            int m = cnt + (i < mod ? 1 : 0);
            for (int j = 1; j < m; ++j) {
                cur = cur->next;
            }
            ListNode* nxt = cur->next;
            cur->next = nullptr;
            cur = nxt;
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
func splitListToParts(head *ListNode, k int) []*ListNode {
	n := 0
	for cur := head; cur != nil; cur = cur.Next {
		n++
	}

	cnt := n / k
	mod := n % k
	ans := make([]*ListNode, k)
	cur := head

	for i := 0; i < k && cur != nil; i++ {
		ans[i] = cur
		m := cnt
		if i < mod {
			m++
		}
		for j := 1; j < m; j++ {
			cur = cur.Next
		}
		next := cur.Next
		cur.Next = nil
		cur = next
	}

	return ans
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

function splitListToParts(head: ListNode | null, k: number): Array<ListNode | null> {
    let n = 0;
    for (let cur = head; cur !== null; cur = cur.next) {
        n++;
    }
    const cnt = (n / k) | 0;
    const mod = n % k;
    const ans: Array<ListNode | null> = Array(k).fill(null);
    let cur = head;
    for (let i = 0; i < k && cur !== null; i++) {
        ans[i] = cur;
        let m = cnt + (i < mod ? 1 : 0);
        for (let j = 1; j < m; j++) {
            cur = cur.next!;
        }
        let next = cur.next;
        cur.next = null;
        cur = next;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
