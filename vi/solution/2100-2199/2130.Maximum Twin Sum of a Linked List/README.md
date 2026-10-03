---
comments: true
difficulty: Medium
rating: 1317
source: Biweekly Contest 69 Q2
tags:
    - Stack
    - Linked List
    - Two Pointers
---

<!-- problem:start -->

# [2130. Maximum Twin Sum of a Linked List](https://leetcode.com/problems/maximum-twin-sum-of-a-linked-list)

[中文文档](/solution/2100-2199/2130.Maximum%20Twin%20Sum%20of%20a%20Linked%20List/README.md)

## Mô tả

<!-- description:start -->

<p>Trong một linked list có kích thước <code>n</code>, với <code>n</code> là <strong>số chẵn</strong>, node thứ <code>i<sup>th</sup></code> (<strong>đánh chỉ số từ 0</strong>) của linked list được gọi là <strong>node song sinh</strong> của node thứ <code>(n-1-i)<sup>th</sup></code>, nếu <code>0 &lt;= i &lt;= (n / 2) - 1</code>.</p>

<ul>
	<li>Ví dụ, nếu <code>n = 4</code>, node <code>0</code> là node song sinh của node <code>3</code>, còn node <code>1</code> là node song sinh của node <code>2</code>. Đây là các node duy nhất có node song sinh khi <code>n = 4</code>.</li>
</ul>

<p><strong>Tổng song sinh</strong> được định nghĩa là tổng của một node và node song sinh của nó.</p>

<p>Cho <code>head</code> của một linked list có độ dài chẵn, hãy trả về <em><strong>tổng song sinh lớn nhất</strong> của linked list</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2130.Maximum%20Twin%20Sum%20of%20a%20Linked%20List/images/eg1drawio.png" style="width: 250px; height: 70px;" />
<pre>
<strong>Đầu vào:</strong> head = [5,4,2,1]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong>
Node 0 và node 1 lần lượt là node song sinh của node 3 và node 2. Tất cả đều có tổng song sinh = 6.
Không còn node nào khác có node song sinh trong linked list.
Vì vậy, tổng song sinh lớn nhất của linked list là 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2130.Maximum%20Twin%20Sum%20of%20a%20Linked%20List/images/eg2drawio.png" style="width: 250px; height: 70px;" />
<pre>
<strong>Đầu vào:</strong> head = [4,2,2,3]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong>
Các node có node song sinh trong linked list này là:
- Node 0 là node song sinh của node 3, có tổng song sinh là 4 + 3 = 7.
- Node 1 là node song sinh của node 2, có tổng song sinh là 2 + 2 = 4.
Vì vậy, tổng song sinh lớn nhất là max(7, 4) = 7.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2130.Maximum%20Twin%20Sum%20of%20a%20Linked%20List/images/eg3drawio.png" style="width: 200px; height: 88px;" />
<pre>
<strong>Đầu vào:</strong> head = [1,100000]
<strong>Đầu ra:</strong> 100001
<strong>Giải thích:</strong>
Chỉ có một node có node song sinh trong linked list, với tổng song sinh là 1 + 100000 = 100001.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong linked list là một số nguyên <strong>chẵn</strong> trong khoảng <code>[2, 10<sup>5</sup>]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Các node song sinh là các giá trị ở những chỉ số đối xứng; ta cần tổng lớn nhất của chúng. Linked list không hỗ trợ truy cập ngẫu nhiên, còn việc đi từ head cho từng cặp sẽ có độ phức tạp bậc hai.
>
> Đưa các giá trị vào một mảng sẽ giúp chỉ số $i$ ghép cặp với $n-1-i$. Vì $n$ là số chẵn và không vượt quá $10^5$, bộ nhớ bổ sung tuyến tính là chấp nhận được.
>
> Lưu các giá trị, sau đó lấy giá trị lớn nhất của $s[i]+s[-(i+1)]$ trên nửa đầu của mảng.

<!-- thinking:end -->

Ta có thể lưu các giá trị của node trong linked list vào một mảng, sau đó dùng hai con trỏ trỏ tới đầu và cuối mảng để tính tổng song sinh cho từng cặp node song sinh. Tổng song sinh lớn nhất chính là đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node trong linked list.

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def pairSum(self, head: Optional[ListNode]) -> int:
        s = []
        while head:
            s.append(head.val)
            head = head.next
        n = len(s)
        return max(s[i] + s[-(i + 1)] for i in range(n >> 1))
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
    public int pairSum(ListNode head) {
        List<Integer> s = new ArrayList<>();
        for (; head != null; head = head.next) {
            s.add(head.val);
        }
        int ans = 0, n = s.size();
        for (int i = 0; i < (n >> 1); ++i) {
            ans = Math.max(ans, s.get(i) + s.get(n - 1 - i));
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
    int pairSum(ListNode* head) {
        vector<int> s;
        for (; head != nullptr; head = head->next) s.push_back(head->val);
        int ans = 0, n = s.size();
        for (int i = 0; i < (n >> 1); ++i) ans = max(ans, s[i] + s[n - i - 1]);
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
func pairSum(head *ListNode) int {
	var s []int
	for ; head != nil; head = head.Next {
		s = append(s, head.Val)
	}
	ans, n := 0, len(s)
	for i := 0; i < (n >> 1); i++ {
		if ans < s[i]+s[n-i-1] {
			ans = s[i] + s[n-i-1]
		}
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

function pairSum(head: ListNode | null): number {
    const arr = [];
    let node = head;
    while (node) {
        arr.push(node.val);
        node = node.next;
    }
    const n = arr.length;
    let ans = 0;
    for (let i = 0; i < n >> 1; i++) {
        ans = Math.max(ans, arr[i] + arr[n - 1 - i]);
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
    pub fn pair_sum(head: Option<Box<ListNode>>) -> i32 {
        let mut arr = Vec::new();
        let mut node = &head;
        while node.is_some() {
            let t = node.as_ref().unwrap();
            arr.push(t.val);
            node = &t.next;
        }
        let n = arr.len();
        let mut ans = 0;
        for i in 0..n >> 1 {
            ans = ans.max(arr[i] + arr[n - 1 - i]);
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 sử dụng một mảng có kích thước $O(n)$. Để dùng bộ nhớ bổ sung hằng số, ta cần ghép cặp trực tiếp trên linked list.
>
> Dùng fast/slow để tìm điểm giữa; đảo ngược nửa sau rồi duyệt nó cùng với nửa đầu, tính các tổng theo từng cặp.
>
> Việc đảo ngược được thực hiện tại chỗ, nên bộ nhớ bổ sung là $O(1)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def pairSum(self, head: Optional[ListNode]) -> int:
        def reverse(head):
            dummy = ListNode()
            curr = head
            while curr:
                next = curr.next
                curr.next = dummy.next
                dummy.next = curr
                curr = next
            return dummy.next

        slow, fast = head, head.next
        while fast and fast.next:
            slow, fast = slow.next, fast.next.next
        pa = head
        q = slow.next
        slow.next = None
        pb = reverse(q)
        ans = 0
        while pa and pb:
            ans = max(ans, pa.val + pb.val)
            pa = pa.next
            pb = pb.next
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
    public int pairSum(ListNode head) {
        ListNode slow = head;
        ListNode fast = head.next;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode pa = head;
        ListNode q = slow.next;
        slow.next = null;
        ListNode pb = reverse(q);
        int ans = 0;
        while (pa != null) {
            ans = Math.max(ans, pa.val + pb.val);
            pa = pa.next;
            pb = pb.next;
        }
        return ans;
    }

    private ListNode reverse(ListNode head) {
        ListNode dummy = new ListNode();
        ListNode curr = head;
        while (curr != null) {
            ListNode next = curr.next;
            curr.next = dummy.next;
            dummy.next = curr;
            curr = next;
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
    int pairSum(ListNode* head) {
        ListNode* slow = head;
        ListNode* fast = head->next;
        while (fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
        }
        ListNode* pa = head;
        ListNode* q = slow->next;
        slow->next = nullptr;
        ListNode* pb = reverse(q);
        int ans = 0;
        while (pa) {
            ans = max(ans, pa->val + pb->val);
            pa = pa->next;
            pb = pb->next;
        }
        return ans;
    }

    ListNode* reverse(ListNode* head) {
        ListNode* dummy = new ListNode();
        ListNode* curr = head;
        while (curr) {
            ListNode* next = curr->next;
            curr->next = dummy->next;
            dummy->next = curr;
            curr = next;
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
func pairSum(head *ListNode) int {
	reverse := func(head *ListNode) *ListNode {
		dummy := &ListNode{}
		curr := head
		for curr != nil {
			next := curr.Next
			curr.Next = dummy.Next
			dummy.Next = curr
			curr = next
		}
		return dummy.Next
	}
	slow, fast := head, head.Next
	for fast != nil && fast.Next != nil {
		slow, fast = slow.Next, fast.Next.Next
	}
	pa := head
	q := slow.Next
	slow.Next = nil
	pb := reverse(q)
	ans := 0
	for pa != nil {
		ans = max(ans, pa.Val+pb.Val)
		pa = pa.Next
		pb = pb.Next
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

function pairSum(head: ListNode | null): number {
    let fast = head;
    let slow = head;
    while (fast) {
        fast = fast.next.next;
        slow = slow.next;
    }
    let prev = null;
    while (slow) {
        const next = slow.next;
        slow.next = prev;
        prev = slow;
        slow = next;
    }
    let left = head;
    let right = prev;
    let ans = 0;
    while (left && right) {
        ans = Math.max(ans, left.val + right.val);
        left = left.next;
        right = right.next;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
