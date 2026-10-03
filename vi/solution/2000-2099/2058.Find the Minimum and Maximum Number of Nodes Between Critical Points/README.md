---
comments: true
difficulty: Medium
rating: 1310
source: Weekly Contest 265 Q2
tags:
    - Linked List
---

<!-- problem:start -->

# [2058. Find the Minimum and Maximum Number of Nodes Between Critical Points](https://leetcode.com/problems/find-the-minimum-and-maximum-number-of-nodes-between-critical-points)

[中文文档](/solution/2000-2099/2058.Find%20the%20Minimum%20and%20Maximum%20Number%20of%20Nodes%20Between%20Critical%20Points/README.md)

## Mô tả

<!-- description:start -->

<p>Một <strong>điểm tới hạn</strong> trong danh sách liên kết được định nghĩa là <strong>một trong hai</strong>: <strong>cực đại địa phương</strong> hoặc <strong>cực tiểu địa phương</strong>.</p>

<p>Một node là <strong>cực đại địa phương</strong> nếu giá trị của node hiện tại <strong>lớn hơn nghiêm ngặt</strong> node trước và node sau.</p>

<p>Một node là <strong>cực tiểu địa phương</strong> nếu giá trị của node hiện tại <strong>nhỏ hơn nghiêm ngặt</strong> node trước và node sau.</p>

<p>Lưu ý rằng một node chỉ có thể là cực đại/cực tiểu địa phương khi tồn tại <strong>cả</strong> node trước và node sau.</p>

<p>Cho một danh sách liên kết có <code>head</code>, hãy trả về <em>một mảng có độ dài 2 chứa </em><code>[minDistance, maxDistance]</code><em>, trong đó </em><code>minDistance</code><em> là <strong>khoảng cách nhỏ nhất</strong> giữa <strong>hai</strong> điểm tới hạn khác nhau bất kỳ và </em><code>maxDistance</code><em> là <strong>khoảng cách lớn nhất</strong> giữa <strong>hai</strong> điểm tới hạn khác nhau bất kỳ. Nếu có <strong>ít hơn</strong> hai điểm tới hạn, hãy trả về </em><code>[-1, -1]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2058.Find%20the%20Minimum%20and%20Maximum%20Number%20of%20Nodes%20Between%20Critical%20Points/images/a1.png" style="width: 148px; height: 55px;" />
<pre>
<strong>Đầu vào:</strong> head = [3,1]
<strong>Đầu ra:</strong> [-1,-1]
<strong>Giải thích:</strong> Không có điểm tới hạn nào trong [3,1].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2058.Find%20the%20Minimum%20and%20Maximum%20Number%20of%20Nodes%20Between%20Critical%20Points/images/a2.png" style="width: 624px; height: 46px;" />
<pre>
<strong>Đầu vào:</strong> head = [5,3,1,2,5,1,2]
<strong>Đầu ra:</strong> [1,3]
<strong>Giải thích:</strong> Có ba điểm tới hạn:
- [5,3,<strong><u>1</u></strong>,2,5,1,2]: Node thứ ba là cực tiểu địa phương vì 1 nhỏ hơn 3 và 2.
- [5,3,1,2,<u><strong>5</strong></u>,1,2]: Node thứ năm là cực đại địa phương vì 5 lớn hơn 2 và 1.
- [5,3,1,2,5,<u><strong>1</strong></u>,2]: Node thứ sáu là cực tiểu địa phương vì 1 nhỏ hơn 5 và 2.
Khoảng cách nhỏ nhất là giữa node thứ năm và node thứ sáu. minDistance = 6 - 5 = 1.
Khoảng cách lớn nhất là giữa node thứ ba và node thứ sáu. maxDistance = 6 - 3 = 3.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2058.Find%20the%20Minimum%20and%20Maximum%20Number%20of%20Nodes%20Between%20Critical%20Points/images/a5.png" style="width: 624px; height: 39px;" />
<pre>
<strong>Đầu vào:</strong> head = [1,3,2,2,3,2,2,2,7]
<strong>Đầu ra:</strong> [3,3]
<strong>Giải thích:</strong> Có hai điểm tới hạn:
- [1,<u><strong>3</strong></u>,2,2,3,2,2,2,7]: Node thứ hai là cực đại địa phương vì 3 lớn hơn 1 và 2.
- [1,3,2,2,<u><strong>3</strong></u>,2,2,2,7]: Node thứ năm là cực đại địa phương vì 3 lớn hơn 2 và 2.
Cả khoảng cách nhỏ nhất và lớn nhất đều là khoảng cách giữa node thứ hai và node thứ năm.
Do đó, minDistance và maxDistance là 5 - 2 = 3.
Lưu ý rằng node cuối cùng không được xem là cực đại địa phương vì nó không có node sau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số lượng node trong danh sách nằm trong khoảng <code>[2, 10<sup>5</sup>]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt trực tiếp

<!-- thinking:start -->

> **Tư duy**
>
> Các điểm tới hạn là các đỉnh hoặc đáy cục bộ. Với $n \le 10^5$, chỉ cần một cửa sổ trượt gồm ba node. Khoảng cách lớn nhất là vị trí cuối cùng trừ vị trí đầu tiên; khoảng cách nhỏ nhất là khoảng cách giữa một cặp điểm tới hạn liền kề gần nhau nhất.
>
> Theo dõi chỉ số của điểm tới hạn đầu tiên và điểm tới hạn trước đó trong quá trình duyệt. Nếu có ít hơn hai điểm tới hạn, kết quả là $[-1,-1]$.

<!-- thinking:end -->

Dựa trên mô tả bài toán, ta cần tìm vị trí của điểm tới hạn đầu tiên và cuối cùng trong danh sách liên kết, lần lượt là $\textit{first}$ và $\textit{last}$. Nhờ đó, ta có thể tính khoảng cách lớn nhất $\textit{maxDistance} = \textit{last} - \textit{first}$. Để tính khoảng cách nhỏ nhất $\textit{minDistance}$, ta cần duyệt danh sách liên kết, tính khoảng cách giữa hai điểm tới hạn liền kề và lấy giá trị nhỏ nhất.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của danh sách liên kết. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def nodesBetweenCriticalPoints(self, head: Optional[ListNode]) -> List[int]:
        ans = [inf, -inf]
        first = last = -1
        i = 0
        while head.next.next:
            a, b, c = head.val, head.next.val, head.next.next.val
            if a > b < c or a < b > c:
                if last == -1:
                    first = last = i
                else:
                    ans[0] = min(ans[0], i - last)
                    last = i
                    ans[1] = max(ans[1], last - first)
            i += 1
            head = head.next
        return [-1, -1] if first == last else ans
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
    public int[] nodesBetweenCriticalPoints(ListNode head) {
        int[] ans = {1 << 30, 0};
        int first = -1, last = -1;
        for (int i = 0; head.next.next != null; head = head.next, ++i) {
            int a = head.val, b = head.next.val, c = head.next.next.val;
            if (b < Math.min(a, c) || b > Math.max(a, c)) {
                if (last == -1) {
                    first = i;
                    last = i;
                } else {
                    ans[0] = Math.min(ans[0], i - last);
                    last = i;
                    ans[1] = Math.max(ans[1], last - first);
                }
            }
        }
        return first == last ? new int[] {-1, -1} : ans;
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
    vector<int> nodesBetweenCriticalPoints(ListNode* head) {
        vector<int> ans = {1 << 30, 0};
        int first = -1, last = -1;
        for (int i = 0; head->next->next; head = head->next, ++i) {
            int a = head->val, b = head->next->val, c = head->next->next->val;
            if (b < min(a, c) || b > max(a, c)) {
                if (last == -1) {
                    first = i;
                    last = i;
                } else {
                    ans[0] = min(ans[0], i - last);
                    last = i;
                    ans[1] = max(ans[1], last - first);
                }
            }
        }
        return first == last ? vector<int>{-1, -1} : ans;
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
func nodesBetweenCriticalPoints(head *ListNode) []int {
	ans := []int{1 << 30, 0}
	first, last := -1, -1
	for i := 0; head.Next.Next != nil; head, i = head.Next, i+1 {
		a, b, c := head.Val, head.Next.Val, head.Next.Next.Val
		if b < min(a, c) || b > max(a, c) {
			if last == -1 {
				first, last = i, i
			} else {
				ans[0] = min(ans[0], i-last)
				last = i
				ans[1] = max(ans[1], last-first)
			}
		}
	}
	if first == last {
		return []int{-1, -1}
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

function nodesBetweenCriticalPoints(head: ListNode | null): number[] {
    const ans: number[] = [Infinity, 0];
    let [first, last] = [-1, -1];
    for (let i = 0; head.next.next; head = head.next, ++i) {
        const [a, b, c] = [head.val, head.next.val, head.next.next.val];
        if (b < Math.min(a, c) || b > Math.max(a, c)) {
            if (last < 0) {
                first = i;
                last = i;
            } else {
                ans[0] = Math.min(ans[0], i - last);
                last = i;
                ans[1] = Math.max(ans[1], last - first);
            }
        }
    }
    return first === last ? [-1, -1] : ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
